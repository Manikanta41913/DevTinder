# DevTinder Deployment Architecture on Render

## Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                         USER'S BROWSER                          │
│                                                                 │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  React App (Frontend)                                    │  │
│  │  - Components, Redux Store, Socket.io Client            │  │
│  │  - Uses VITE_API_URL for backend connection             │  │
│  └──────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │ HTTPS Requests
                              │ WebSocket Connection
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    RENDER STATIC SITE                           │
│                                                                 │
│  Frontend: https://devtinder-frontend.onrender.com             │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  Static Files (HTML, CSS, JS)                           │  │
│  │  - Built from: npm run build                            │  │
│  │  - Serves: dist/ folder                                 │  │
│  │  - No server needed (just static hosting)               │  │
│  └──────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │ API Calls
                              │ Socket.io
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    RENDER WEB SERVICE                           │
│                                                                 │
│  Backend: https://devtinder-backend.onrender.com               │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  Node.js + Express Server                               │  │
│  │  - Port: 10000                                           │  │
│  │  - Health Check: /health                                │  │
│  │  - API Routes: /signup, /login, /feed, etc.             │  │
│  │  - Socket.io Server (Real-time chat)                    │  │
│  │  - CORS: Allows frontend URL                            │  │
│  └──────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │ Database Queries
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    MONGODB ATLAS (Cloud)                        │
│                                                                 │
│  Database: devtinder                                            │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  Collections:                                            │  │
│  │  - users                                                 │  │
│  │  - connectionrequests                                    │  │
│  │  - chats                                                 │  │
│  │  - payments                                              │  │
│  │                                                          │  │
│  │  Network Access: 0.0.0.0/0 (Allow from anywhere)        │  │
│  └──────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
```

---

## Request Flow Examples

### 1. User Login Flow

```
User enters credentials
    ↓
Frontend (React)
    ↓ POST /login
Backend (Express) → Verify credentials
    ↓ Query
MongoDB Atlas → Find user
    ↓ Return user data
Backend → Generate JWT token
    ↓ Set cookie + return user
Frontend → Store in Redux
    ↓
User sees dashboard
```

### 2. Real-time Chat Flow

```
User A sends message
    ↓
Frontend → socket.emit("sendMessage")
    ↓ WebSocket
Backend Socket.io → Receive message
    ↓ Save to DB
MongoDB Atlas → Store in chats collection
    ↓ Broadcast
Backend → io.to(roomId).emit("messageReceived")
    ↓ WebSocket
Frontend (User A & B) → Update chat UI
    ↓
Both users see message instantly
```

### 3. Feed/Browse Flow

```
User opens feed page
    ↓
Frontend → GET /user/feed?page=1
    ↓ HTTP Request
Backend → Check authentication (JWT)
    ↓ Query
MongoDB → Find users (exclude connected)
    ↓ Return users
Backend → Send user list
    ↓ JSON Response
Frontend → Display user cards
    ↓
User sees profiles to swipe
```

---

## Environment Variables Flow

### Build Time (Frontend)

```
Render reads: VITE_API_URL
    ↓
Vite build process
    ↓
Replaces: import.meta.env.VITE_API_URL
    ↓
With: "https://devtinder-backend.onrender.com"
    ↓
Bundles into: dist/assets/*.js
    ↓
Static files served to browser
```

### Runtime (Backend)

```
Render sets: Environment Variables
    ↓
Node.js process.env
    ↓
Used in: app.js, database.js, socket.js
    ↓
Examples:
- process.env.PORT → 10000
- process.env.DB_CONNECTION_SECRET → MongoDB URL
- process.env.FRONTEND_URL → CORS origin
```

---

## CORS Configuration

```
Browser (https://devtinder-frontend.onrender.com)
    ↓ Request with credentials
Backend checks CORS
    ↓
cors({
  origin: [
    "http://localhost:5173",
    process.env.FRONTEND_URL  ← Must match!
  ],
  credentials: true  ← Allows cookies
})
    ↓
If origin matches → Allow request
If not → Block with CORS error
```

---

## Socket.io Connection

```
Frontend loads
    ↓
import socket from './utils/socket'
    ↓
io(BASE_URL, { withCredentials: true })
    ↓ WebSocket Handshake
Backend Socket.io
    ↓
Check CORS origin
    ↓
If allowed → Establish connection
    ↓
socket.on("connection", (socket) => { ... })
    ↓
Persistent bidirectional connection
    ↓
Real-time events: sendMessage, receiveMessage
```

---

## Health Check Flow

```
Render Platform
    ↓ Every 30 seconds
GET /health
    ↓
Backend Express
    ↓
app.get("/health", (req, res) => {
  res.status(200).json({ status: "ok" })
})
    ↓
Returns: 200 OK
    ↓
Render marks service as "Healthy"
    ↓
If fails → Render marks as "Unhealthy"
```

---

## Deployment Process

### Backend Deployment

```
1. Push to GitHub
    ↓
2. Render detects changes
    ↓
3. Pulls code from repo
    ↓
4. Runs: npm install (backend/)
    ↓
5. Starts: npm start
    ↓
6. Executes: node src/app.js
    ↓
7. Server listens on 0.0.0.0:10000
    ↓
8. Health check: /health
    ↓
9. Service marked as "Live"
```

### Frontend Deployment

```
1. Push to GitHub
    ↓
2. Render detects changes
    ↓
3. Pulls code from repo
    ↓
4. Runs: npm install (frontend/)
    ↓
5. Runs: npm run build
    ↓
6. Vite builds: dist/ folder
    ↓
7. Render serves static files
    ↓
8. Site available immediately
```

---

## Free Tier Behavior

### Backend (Web Service)

```
User makes request
    ↓
Is service awake?
    ├─ Yes → Process immediately (fast)
    └─ No → Wake up service (30-50 seconds)
        ↓
    Start Node.js process
        ↓
    Connect to MongoDB
        ↓
    Initialize Socket.io
        ↓
    Process request
        ↓
    Service stays awake for 15 minutes
        ↓
    No requests for 15 min → Sleep again
```

### Frontend (Static Site)

```
User visits site
    ↓
Always available (never sleeps)
    ↓
Serves static files instantly
    ↓
No wake-up time
```

---

## Security Layers

```
1. HTTPS (Render provides SSL)
    ↓
2. CORS (Only allow frontend origin)
    ↓
3. JWT Authentication (Verify token)
    ↓
4. HTTP-only Cookies (Prevent XSS)
    ↓
5. Bcrypt Password Hashing
    ↓
6. MongoDB Atlas (Network restrictions)
    ↓
7. Environment Variables (Secrets not in code)
```

---

## Troubleshooting Map

```
Issue: Frontend loads but can't connect to backend
    ↓
Check: Browser console for errors
    ├─ CORS error → Check FRONTEND_URL in backend
    ├─ Network error → Check VITE_API_URL in frontend
    └─ 404 error → Check backend is deployed

Issue: Backend shows "Service Unavailable"
    ↓
Check: Render logs
    ├─ MongoDB error → Check connection string
    ├─ Port error → Verify PORT=10000
    └─ Startup error → Check package.json scripts

Issue: Chat not working
    ↓
Check: Socket.io connection
    ├─ Console error → Check BASE_URL
    ├─ CORS error → Check FRONTEND_URL
    └─ Connection refused → Backend might be sleeping
```

---

## Performance Optimization

```
Frontend:
- Vite build optimization
- Code splitting
- Lazy loading components
- Static file caching

Backend:
- MongoDB indexes
- Connection pooling
- Efficient queries
- Socket.io rooms

Database:
- Compound indexes
- Query optimization
- Proper schema design
```

---

## Monitoring Points

```
1. Backend Health: /health endpoint
2. Frontend Availability: Site loads
3. Database Connection: MongoDB Atlas metrics
4. API Response Times: Render logs
5. Socket.io Connections: Console logs
6. Error Rates: Browser console + Render logs
```

This architecture ensures your DevTinder app runs smoothly on Render's free tier! 🚀
