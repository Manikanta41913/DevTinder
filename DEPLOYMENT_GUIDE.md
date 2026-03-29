# DevTinder Deployment Guide - Render

## Prerequisites

✅ GitHub account connected to Render
✅ Code pushed to GitHub repository
✅ MongoDB Atlas database running

---

## Step-by-Step Deployment

### 1. Push Your Code to GitHub

```bash
git add .
git commit -m "Add Render deployment configuration"
git push origin main
```

### 2. Deploy Backend on Render

1. Go to [Render Dashboard](https://dashboard.render.com/)
2. Click **"New +"** → **"Web Service"**
3. Connect your GitHub repository: `Manikanta41913/DevTinder`
4. Configure the backend:
   - **Name**: `devtinder-backend`
   - **Region**: Oregon (US West) or closest to you
   - **Branch**: `main`
   - **Root Directory**: `backend`
   - **Runtime**: Node
   - **Build Command**: `npm install`
   - **Start Command**: `npm start`
   - **Plan**: Free

5. Add Environment Variables (click "Advanced" → "Add Environment Variable"):

   ```
   PORT=7777
   DB_CONNECTION_SECRET=mongodb+srv://manikanta31914_db_user:wzGaJx4oQBZnZslc@cluster0.iescdku.mongodb.net/devtinder?retryWrites=true&w=majority&appName=Cluster0
   JWT_SECRET=DevTinder2024_SecureJWT_Key_Manikanta_Random_String_9x7z3k5m
   NODE_ENV=production
   FRONTEND_URL=https://devtinder-frontend.onrender.com
   ```

   (Add Razorpay and AWS keys if you're using those features)

6. Click **"Create Web Service"**
7. Wait for deployment (5-10 minutes)
8. Copy your backend URL (e.g., `https://devtinder-backend.onrender.com`)

### 3. Deploy Frontend on Render

1. Click **"New +"** → **"Static Site"**
2. Connect the same GitHub repository
3. Configure the frontend:
   - **Name**: `devtinder-frontend`
   - **Region**: Oregon (US West)
   - **Branch**: `main`
   - **Root Directory**: `frontend`
   - **Build Command**: `npm install && npm run build`
   - **Publish Directory**: `dist`
   - **Plan**: Free

4. Add Environment Variable:

   ```
   VITE_API_URL=https://devtinder-backend.onrender.com
   ```

   (Replace with your actual backend URL from step 2)

5. Click **"Create Static Site"**
6. Wait for deployment (3-5 minutes)
7. Your app is live! 🎉

### 4. Update Backend CORS

1. Go back to your backend service on Render
2. Go to **"Environment"** tab
3. Update `FRONTEND_URL` with your actual frontend URL:
   ```
   FRONTEND_URL=https://devtinder-frontend.onrender.com
   ```
   (Replace with your actual frontend URL)
4. Save changes (backend will auto-redeploy)

---

## Important Notes

### Free Tier Limitations

- Backend sleeps after 15 minutes of inactivity
- First request takes 30-50 seconds to wake up
- 750 hours/month free (enough for one service 24/7)

### MongoDB Atlas

- Make sure your MongoDB Atlas allows connections from anywhere (0.0.0.0/0)
- Or add Render's IP addresses to whitelist

### Testing Your Deployment

1. Visit your frontend URL
2. Try logging in/signing up
3. Test all features (profile, feed, connections, chat)
4. Check browser console for errors

### Troubleshooting

**Backend won't start:**

- Check logs in Render dashboard
- Verify all environment variables are set
- Check MongoDB connection string

**Frontend can't connect to backend:**

- Verify `VITE_API_URL` is correct
- Check CORS settings in backend
- Look at browser network tab for failed requests

**Socket.io not working:**

- Verify backend URL in frontend env vars
- Check browser console for WebSocket errors
- Ensure backend is running (not sleeping)

---

## Alternative: Deploy Using render.yaml (Blueprint)

If you want to deploy both services at once:

1. Push the `render.yaml` file to your repo
2. Go to Render Dashboard → **"New +"** → **"Blueprint"**
3. Connect your repository
4. Render will detect `render.yaml` and create both services
5. Add environment variables manually after creation

---

## URLs After Deployment

- **Frontend**: `https://devtinder-frontend.onrender.com`
- **Backend**: `https://devtinder-backend.onrender.com`
- **API Health Check**: `https://devtinder-backend.onrender.com/health`

(Your actual URLs will be different based on availability)

---

## Keeping Backend Awake (Optional)

Free tier backends sleep after 15 minutes. To keep it awake:

1. Use a service like [UptimeRobot](https://uptimerobot.com/) (free)
2. Ping your backend URL every 10 minutes
3. Or upgrade to paid tier ($7/month) for always-on

---

## Next Steps

- Add custom domain (optional)
- Set up automatic deployments on git push
- Monitor logs and performance
- Consider upgrading for production use

Good luck! 🚀
