# DevTinder - Full Stack Application

[![Live Demo](https://img.shields.io/badge/Live%20Demo-Visit%20App-success?style=for-the-badge&logo=render)](https://devtinder-frontend1.onrender.com)
[![Backend API](https://img.shields.io/badge/Backend%20API-Active-blue?style=for-the-badge&logo=node.js)](https://devtinder-db0t.onrender.com/health)
[![MongoDB](https://img.shields.io/badge/Database-MongoDB%20Atlas-green?style=for-the-badge&logo=mongodb)](https://www.mongodb.com/cloud/atlas)

A Tinder-like application for developers to connect and collaborate.

## 🌐 Live Demo

**🚀 [Try the App Now!](https://devtinder-frontend1.onrender.com)**

- **Frontend**: [https://devtinder-frontend1.onrender.com](https://devtinder-frontend1.onrender.com)
- **Backend API**: [https://devtinder-db0t.onrender.com](https://devtinder-db0t.onrender.com)
- **Health Check**: [https://devtinder-db0t.onrender.com/health](https://devtinder-db0t.onrender.com/health)

> ⚠️ **Note**: Free tier may take 30-50 seconds to wake up on first request.

## 🎯 Project Status

✅ **Fully Functional** - Ready for interviews and demonstrations!
✅ **Deployed on Render** - Live and accessible
✅ **Production Ready** - Working authentication, real-time chat, and all features

## ✨ Key Highlights

- 🔐 **Secure Authentication** - JWT tokens with HTTP-only cookies
- 💬 **Real-time Chat** - Instant messaging with Socket.io
- 🎨 **Modern UI** - Responsive design with Tailwind CSS & DaisyUI
- 🗄️ **Cloud Database** - MongoDB Atlas for scalability
- 🚀 **Production Deployed** - Live on Render with proper CORS and security

### Working Features

- ✅ User authentication (signup/login/logout with JWT)
- ✅ Profile management (view/edit)
- ✅ Feed with user browsing
- ✅ Connection requests (interested/ignored)
- ✅ Accept/reject requests
- ✅ View connections
- ✅ Real-time chat with Socket.io
- ✅ MongoDB Atlas cloud database
- ⚠️ Premium membership UI (payment integration optional)

## 🎮 Try It Out

### Quick Demo Flow

1. **Visit the app**: [DevTinder Live Demo](https://devtinder-frontend1.onrender.com)
2. **Sign up** with a test account
3. **Edit your profile** - Add skills, photo, bio
4. **Browse the feed** - See other developers
5. **Send connection requests** - Click "Interested" or "Ignore"
6. **Accept requests** - Check the "Requests" page
7. **Start chatting** - Real-time messaging with connections!

> 💡 **Tip**: Create two accounts in different browsers to test the real-time chat feature!

## 🚀 Quick Start

**The project is already configured and ready to run!**

```bash
# Start both frontend and backend
npm run dev:all
```

- Frontend: http://localhost:5173
- Backend: http://localhost:7777

## 📁 Project Structure

```
DevTinder/
├── backend/              # Express.js API server
│   ├── src/
│   │   ├── config/      # Database configuration
│   │   ├── middlewares/ # Auth middleware
│   │   ├── models/      # Mongoose schemas
│   │   ├── routes/      # API endpoints
│   │   ├── utils/       # Socket.io, validation, helpers
│   │   └── app.js       # Main server file
│   ├── .env             # Environment variables (configured)
│   └── package.json
│
├── frontend/            # React application
│   ├── src/
│   │   ├── components/  # React components
│   │   ├── utils/       # Redux store, slices
│   │   ├── App.jsx
│   │   └── main.jsx
│   └── package.json
│
└── package.json         # Root scripts
```

## 🛠 Tech Stack

### Backend

- **Node.js** v20.20.0 + **Express.js** - Server framework
- **MongoDB Atlas** + **Mongoose** - Cloud database
- **JWT** + **bcrypt** - Authentication & password hashing
- **Socket.io** - Real-time bidirectional communication
- **Razorpay** - Payment gateway integration
- **AWS SES** - Email service
- **CORS**, **cookie-parser**, **validator**

### Frontend

- **React 18** - UI library
- **Vite** - Build tool & dev server
- **Redux Toolkit** - State management
- **React Router** - Client-side routing
- **Axios** - HTTP client
- **Socket.io Client** - Real-time messaging
- **Tailwind CSS** + **DaisyUI** - Styling

## 📦 Installation (If Cloning Fresh)

1. **Clone the repository**

   ```bash
   git clone https://github.com/Manikanta41913/DevTinder.git
   cd DevTinder
   ```

2. **Install all dependencies**

   ```bash
   npm run install:all
   ```

3. **Configure environment variables**

   The `backend/.env` file is already configured with:
   - MongoDB Atlas connection
   - JWT secret
   - Razorpay keys (optional)
   - AWS SES keys (optional)

4. **Start the application**
   ```bash
   npm run dev:all
   ```

## 🎮 Available Scripts

```bash
# Development
npm run dev:all          # Run both frontend & backend
npm run dev              # Run backend only
npm run client           # Run frontend only

# Installation
npm run install:all      # Install all dependencies
npm run install:backend  # Install backend dependencies
npm run install:frontend # Install frontend dependencies

# Production
npm run build:frontend   # Build frontend for production
npm start                # Start backend in production mode
```

## 🔐 Environment Variables

Located in `backend/.env`:

```env
PORT=7777
DB_CONNECTION_SECRET=mongodb+srv://...  # MongoDB Atlas connection
JWT_SECRET=your_jwt_secret              # JWT signing key
RAZORPAY_KEY_ID=...                     # Optional: Payment gateway
RAZORPAY_KEY_SECRET=...
RAZORPAY_WEBHOOK_SECRET=...
AWS_ACCESS_KEY=...                      # Optional: Email service
AWS_SECRET_KEY=...
```

## 📚 API Endpoints

### Authentication

- `POST /signup` - Register new user
- `POST /login` - Login user
- `POST /logout` - Logout user

### Profile

- `GET /profile/view` - Get user profile
- `PATCH /profile/edit` - Update profile
- `PATCH /profile/password` - Reset password

### Connections

- `POST /request/send/:status/:userId` - Send connection request
- `POST /request/review/:status/:requestId` - Accept/reject request
- `GET /user/requests/received` - Get pending requests
- `GET /user/connections` - Get matched connections
- `GET /user/feed` - Browse user profiles

### Chat

- `GET /chat/:userId` - Get chat history
- Socket.io events for real-time messaging

See `backend/apiList.md` for complete documentation.

## 🎯 How to Use

1. **Sign up** - Create a new account
2. **Edit profile** - Add your details, photo, skills
3. **Browse feed** - See other developers
4. **Send requests** - Click "Interested" or "Ignore"
5. **Accept requests** - Check "Requests" page
6. **View connections** - See matched users
7. **Start chatting** - Real-time messaging with connections

## 🔧 Troubleshooting

### Backend won't start

- Check MongoDB Atlas IP whitelist (should include 0.0.0.0/0)
- Verify `backend/.env` file exists
- Ensure port 7777 is available

### Frontend errors

- Clear browser cookies
- Logout and login again
- Check if backend is running

### Chat not working

- Ensure both users have accepted connection
- Check browser console for Socket.io errors

## 🎓 For Interview Preparation

This project demonstrates:

- ✅ Full-stack MERN development
- ✅ RESTful API design
- ✅ JWT authentication & security
- ✅ Real-time features with WebSockets
- ✅ State management with Redux
- ✅ Database design & relationships
- ✅ Payment gateway integration
- ✅ Professional Git workflow (14 meaningful commits)

**Key talking points:**

- Monorepo structure for better organization
- JWT-based stateless authentication
- Socket.io for real-time bidirectional communication
- MongoDB compound indexes for performance
- Redux Toolkit for predictable state management
- Tailwind CSS for rapid UI development

## 📊 Project Statistics

- **Total Files**: 50+ files
- **Lines of Code**: ~5000+ lines
- **API Endpoints**: 15+
- **Database Models**: 4 (User, ConnectionRequest, Payment, Chat)
- **React Components**: 12+
- **Redux Slices**: 4
- **Git Commits**: 14+ meaningful commits
- **Deployment**: Render (Production-ready)

## 🎓 Learning Outcomes

This project demonstrates:

- ✅ Full-stack MERN development
- ✅ RESTful API design
- ✅ JWT authentication & security
- ✅ Real-time features with WebSockets
- ✅ State management with Redux
- ✅ Database design & relationships
- ✅ Payment gateway integration
- ✅ Production deployment & DevOps
- ✅ Cross-origin cookie authentication
- ✅ Professional Git workflow

## 👨‍💻 Author

**Manikanta**

- GitHub: [@Manikanta41913](https://github.com/Manikanta41913)
- Project: [DevTinder](https://github.com/Manikanta41913/DevTinder)

## 📝 License

ISC

---

**Built with ❤️ using MERN Stack**
