# Quick Deployment Checklist

## 🚀 Pre-Deployment

- [ ] Code pushed to GitHub
- [ ] MongoDB Atlas allows 0.0.0.0/0 connections
- [ ] All changes committed

## 📋 Backend Deployment (Web Service)

### Configuration

- **Type**: Web Service
- **Name**: `devtinder-backend`
- **Root Directory**: `backend`
- **Build Command**: `npm install`
- **Start Command**: `npm start`
- **Health Check Path**: `/health`

### Environment Variables

```
PORT=10000
DB_CONNECTION_SECRET=mongodb+srv://manikanta31914_db_user:wzGaJx4oQBZnZslc@cluster0.iescdku.mongodb.net/devtinder?retryWrites=true&w=majority&appName=Cluster0
JWT_SECRET=DevTinder2024_SecureJWT_Key_Manikanta_Random_String_9x7z3k5m
NODE_ENV=production
FRONTEND_URL=https://[YOUR-FRONTEND-NAME].onrender.com
```

### After Deployment

- [ ] Copy backend URL
- [ ] Test health endpoint: `https://[backend-url]/health`
- [ ] Check logs for errors

---

## 🎨 Frontend Deployment (Static Site)

### Configuration

- **Type**: Static Site
- **Name**: `devtinder-frontend`
- **Root Directory**: `frontend`
- **Build Command**: `npm install && npm run build`
- **Publish Directory**: `dist`

### Environment Variables

```
VITE_API_URL=https://[YOUR-BACKEND-NAME].onrender.com
```

### After Deployment

- [ ] Copy frontend URL
- [ ] Update backend `FRONTEND_URL` variable
- [ ] Wait for backend to redeploy

---

## ✅ Post-Deployment Testing

- [ ] Visit frontend URL
- [ ] Sign up with test account
- [ ] Login works
- [ ] Profile page loads
- [ ] Feed shows users
- [ ] Send connection request
- [ ] Accept request
- [ ] Chat works (real-time)
- [ ] No console errors

---

## 🔧 If Something Doesn't Work

### Backend Issues

1. Check Render logs
2. Verify MongoDB connection
3. Test `/health` endpoint
4. Check environment variables

### Frontend Issues

1. Check browser console (F12)
2. Verify `VITE_API_URL` is correct
3. Check Network tab for failed requests
4. Rebuild if env vars changed

### CORS Issues

1. Verify `FRONTEND_URL` in backend
2. Check both URLs match exactly
3. Include `https://` in URLs
4. Redeploy backend after changes

### Socket.io Issues

1. Check browser console for WebSocket errors
2. Verify backend URL is correct
3. Make sure backend is awake
4. Test with simple message

---

## 📝 Your Deployment URLs

Fill these in after deployment:

```
Backend URL: https://_________________________.onrender.com
Frontend URL: https://_________________________.onrender.com
Health Check: https://_________________________.onrender.com/health
```

---

## 🎯 Quick Commands

### Test Backend Health

```bash
curl https://[your-backend-url].onrender.com/health
```

### Push Changes

```bash
git add .
git commit -m "Your message"
git push origin main
```

### View Logs

Go to Render Dashboard → Service → Logs tab

---

## ⚠️ Remember

- Free tier backends sleep after 15 minutes
- First request takes 30-50 seconds
- Static sites don't sleep
- Environment variables need exact URLs
- Always include `https://` in URLs
- Redeploy after changing env vars
