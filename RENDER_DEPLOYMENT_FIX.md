# Render Deployment Fix Guide

## Issues Fixed

1. ✅ Changed frontend from web service to static site
2. ✅ Added health check endpoint for backend
3. ✅ Fixed BASE_URL logic in frontend
4. ✅ Updated server to listen on 0.0.0.0 (required for Render)
5. ✅ Changed PORT to 10000 (Render's default)
6. ✅ Fixed render.yaml configuration

---

## Step-by-Step Deployment Instructions

### 1. Push Updated Code to GitHub

```bash
git add .
git commit -m "Fix Render deployment configuration"
git push origin main
```

### 2. Deploy Backend on Render

1. Go to [Render Dashboard](https://dashboard.render.com/)
2. Click **"New +"** → **"Web Service"**
3. Connect your GitHub repository
4. Configure:
   - **Name**: `devtinder-backend`
   - **Region**: Oregon (US West)
   - **Branch**: `main`
   - **Root Directory**: `backend`
   - **Runtime**: Node
   - **Build Command**: `npm install`
   - **Start Command**: `npm start`
   - **Plan**: Free

5. **Add Environment Variables** (click "Advanced"):

   ```
   PORT=10000
   DB_CONNECTION_SECRET=mongodb+srv://manikanta31914_db_user:wzGaJx4oQBZnZslc@cluster0.iescdku.mongodb.net/devtinder?retryWrites=true&w=majority&appName=Cluster0
   JWT_SECRET=DevTinder2024_SecureJWT_Key_Manikanta_Random_String_9x7z3k5m
   NODE_ENV=production
   FRONTEND_URL=https://devtinder-frontend.onrender.com
   ```

   **IMPORTANT**: Replace `devtinder-frontend.onrender.com` with your actual frontend URL after you create it in step 3.

6. Click **"Create Web Service"**
7. Wait for deployment (5-10 minutes)
8. **Copy your backend URL** (e.g., `https://devtinder-backend.onrender.com`)
9. Test health check: Visit `https://your-backend-url.onrender.com/health`

### 3. Deploy Frontend on Render

1. Click **"New +"** → **"Static Site"**
2. Connect the same GitHub repository
3. Configure:
   - **Name**: `devtinder-frontend`
   - **Region**: Oregon (US West)
   - **Branch**: `main`
   - **Root Directory**: `frontend`
   - **Build Command**: `npm install && npm run build`
   - **Publish Directory**: `dist`
   - **Plan**: Free

4. **Add Environment Variable**:

   ```
   VITE_API_URL=https://devtinder-backend.onrender.com
   ```

   **IMPORTANT**: Replace with your actual backend URL from step 2.

5. Click **"Create Static Site"**
6. Wait for deployment (3-5 minutes)

### 4. Update Backend CORS Settings

1. Go back to your backend service on Render
2. Go to **"Environment"** tab
3. Update `FRONTEND_URL` with your actual frontend URL:
   ```
   FRONTEND_URL=https://devtinder-frontend.onrender.com
   ```
4. Click **"Save Changes"** (backend will auto-redeploy)

### 5. Verify MongoDB Atlas Configuration

1. Go to [MongoDB Atlas](https://cloud.mongodb.com/)
2. Click on your cluster → **"Network Access"**
3. Make sure you have **0.0.0.0/0** in the IP whitelist (allows connections from anywhere)
4. If not, click **"Add IP Address"** → **"Allow Access from Anywhere"** → **"Confirm"**

---

## Testing Your Deployment

1. Visit your frontend URL: `https://devtinder-frontend.onrender.com`
2. Try signing up with a new account
3. Test login
4. Browse the feed
5. Send connection requests
6. Test chat functionality

### Check for Errors

- **Browser Console**: Press F12 → Console tab
- **Network Tab**: Check for failed API requests
- **Render Logs**: Go to your service → "Logs" tab

---

## Common Issues & Solutions

### Issue 1: Backend shows "Service Unavailable"

**Solution:**

- Check Render logs for errors
- Verify MongoDB connection string is correct
- Make sure MongoDB Atlas allows connections from 0.0.0.0/0

### Issue 2: Frontend can't connect to backend

**Solution:**

- Verify `VITE_API_URL` environment variable is set correctly
- Check browser console for CORS errors
- Make sure `FRONTEND_URL` is set in backend environment variables
- Rebuild frontend after changing environment variables

### Issue 3: Socket.io not working

**Solution:**

- Verify backend URL in frontend environment variables
- Check browser console for WebSocket errors
- Make sure backend is running (not sleeping)
- Verify CORS settings include your frontend URL

### Issue 4: "Application failed to respond"

**Solution:**

- Check if health check endpoint is working: `/health`
- Verify server is listening on `0.0.0.0` and correct PORT
- Check Render logs for startup errors

### Issue 5: Backend sleeps after 15 minutes

**Solution (Free Tier Limitation):**

- First request takes 30-50 seconds to wake up
- Use [UptimeRobot](https://uptimerobot.com/) to ping every 10 minutes
- Or upgrade to paid tier ($7/month) for always-on

---

## Environment Variables Checklist

### Backend Environment Variables

- ✅ `PORT=10000`
- ✅ `DB_CONNECTION_SECRET=mongodb+srv://...`
- ✅ `JWT_SECRET=your_jwt_secret`
- ✅ `NODE_ENV=production`
- ✅ `FRONTEND_URL=https://your-frontend-url.onrender.com`
- ⚠️ `RAZORPAY_KEY_ID` (optional, for payments)
- ⚠️ `RAZORPAY_KEY_SECRET` (optional)
- ⚠️ `RAZORPAY_WEBHOOK_SECRET` (optional)
- ⚠️ `AWS_ACCESS_KEY` (optional, for emails)
- ⚠️ `AWS_SECRET_KEY` (optional)

### Frontend Environment Variables

- ✅ `VITE_API_URL=https://your-backend-url.onrender.com`

---

## Alternative: Deploy Using Blueprint (render.yaml)

The `render.yaml` file has been updated. To use it:

1. Push code to GitHub
2. Go to Render Dashboard → **"New +"** → **"Blueprint"**
3. Connect your repository
4. Render will detect `render.yaml` and create both services
5. **Manually add environment variables** after creation:
   - Backend: Add all environment variables listed above
   - Frontend: Add `VITE_API_URL`
6. Update `FRONTEND_URL` in backend after frontend is deployed

---

## Important Notes

### Free Tier Limitations

- Backend sleeps after 15 minutes of inactivity
- First request takes 30-50 seconds to wake up
- 750 hours/month free (enough for one service 24/7)
- Static sites don't sleep

### Security

- Never commit `.env` files to GitHub
- Use Render's environment variables for secrets
- MongoDB credentials are already in the connection string

### Performance

- First load might be slow (backend waking up)
- Subsequent requests are fast
- Consider upgrading for production use

---

## Troubleshooting Commands

### Check Backend Health

```bash
curl https://your-backend-url.onrender.com/health
```

Expected response:

```json
{ "status": "ok", "message": "Server is running" }
```

### Check Backend Logs

1. Go to Render Dashboard
2. Click on your backend service
3. Click "Logs" tab
4. Look for errors or connection issues

### Check Frontend Build

1. Go to Render Dashboard
2. Click on your frontend static site
3. Click "Logs" tab
4. Verify build completed successfully

---

## Next Steps After Successful Deployment

1. ✅ Test all features (signup, login, feed, connections, chat)
2. ✅ Set up custom domain (optional)
3. ✅ Enable automatic deployments on git push
4. ✅ Monitor logs and performance
5. ✅ Set up UptimeRobot to keep backend awake
6. ✅ Consider upgrading for production use

---

## Support

If you still face issues:

1. Check Render logs for both services
2. Verify all environment variables are set correctly
3. Test MongoDB connection separately
4. Check browser console for frontend errors
5. Verify CORS settings match your URLs

Good luck with your deployment! 🚀
