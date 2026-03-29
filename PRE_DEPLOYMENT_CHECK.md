# Pre-Deployment Verification Checklist

Run through this checklist before deploying to Render to ensure everything is configured correctly.

---

## ✅ Code Changes Verification

### 1. Check render.yaml

```bash
cat render.yaml
```

Should show:

- ✅ Backend: `type: web`, `healthCheckPath: /health`, `PORT=10000`
- ✅ Frontend: `type: static`, `staticPublishPath: ./dist`

### 2. Check backend health endpoint

```bash
cat backend/src/app.js | grep -A 3 "health"
```

Should show:

```javascript
app.get("/health", (req, res) => {
  res.status(200).json({ status: "ok", message: "Server is running" });
});
```

### 3. Check server listen configuration

```bash
cat backend/src/app.js | grep -A 2 "listen"
```

Should show:

```javascript
server.listen(PORT, "0.0.0.0", () => {
```

### 4. Check frontend BASE_URL

```bash
cat frontend/src/utils/constants.js
```

Should show:

```javascript
export const BASE_URL = import.meta.env.VITE_API_URL || "http://localhost:7777";
```

---

## ✅ Local Testing

### 1. Test Backend Locally

```bash
cd backend
npm install
npm start
```

Expected output:

```
Database connection established...
Server is successfully listening on port 7777...
```

### 2. Test Health Endpoint

Open new terminal:

```bash
curl http://localhost:7777/health
```

Expected response:

```json
{ "status": "ok", "message": "Server is running" }
```

### 3. Test Frontend Locally

Open new terminal:

```bash
cd frontend
npm install
npm run dev
```

Expected output:

```
VITE v5.4.1  ready in XXX ms
➜  Local:   http://localhost:5173/
```

### 4. Test Full Application

1. Open browser: http://localhost:5173
2. Try to sign up
3. Try to login
4. Check browser console for errors

---

## ✅ MongoDB Atlas Verification

### 1. Check Network Access

1. Go to https://cloud.mongodb.com/
2. Click your cluster
3. Click "Network Access" in left sidebar
4. Verify you have: `0.0.0.0/0` (Allow access from anywhere)

If not:

- Click "Add IP Address"
- Click "Allow Access from Anywhere"
- Click "Confirm"

### 2. Test Connection String

```bash
# In backend directory
node -e "require('dotenv').config(); const mongoose = require('mongoose'); mongoose.connect(process.env.DB_CONNECTION_SECRET).then(() => { console.log('✅ MongoDB Connected'); process.exit(0); }).catch(err => { console.error('❌ MongoDB Error:', err.message); process.exit(1); });"
```

Expected output:

```
✅ MongoDB Connected
```

---

## ✅ GitHub Repository

### 1. Check Git Status

```bash
git status
```

Should show:

```
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

If you have uncommitted changes:

```bash
git add .
git commit -m "Fix Render deployment configuration"
git push origin main
```

### 2. Verify Files Are Pushed

```bash
git log --oneline -1
```

Should show your latest commit.

### 3. Check Remote Repository

```bash
git remote -v
```

Should show your GitHub repository URL.

---

## ✅ Environment Variables Preparation

### Backend Environment Variables

Copy this template and fill in your values:

```
PORT=10000
DB_CONNECTION_SECRET=mongodb+srv://manikanta31914_db_user:wzGaJx4oQBZnZslc@cluster0.iescdku.mongodb.net/devtinder?retryWrites=true&w=majority&appName=Cluster0
JWT_SECRET=DevTinder2024_SecureJWT_Key_Manikanta_Random_String_9x7z3k5m
NODE_ENV=production
FRONTEND_URL=https://[WILL-ADD-AFTER-FRONTEND-DEPLOYMENT].onrender.com
```

Optional (if using):

```
RAZORPAY_KEY_ID=your_key
RAZORPAY_KEY_SECRET=your_secret
RAZORPAY_WEBHOOK_SECRET=your_webhook_secret
AWS_ACCESS_KEY=your_aws_key
AWS_SECRET_KEY=your_aws_secret
```

### Frontend Environment Variables

```
VITE_API_URL=https://[WILL-ADD-AFTER-BACKEND-DEPLOYMENT].onrender.com
```

---

## ✅ File Structure Verification

### Check Required Files Exist

```bash
# Backend files
ls -la backend/src/app.js
ls -la backend/package.json
ls -la backend/.env

# Frontend files
ls -la frontend/package.json
ls -la frontend/vite.config.js
ls -la frontend/public/_redirects

# Root files
ls -la render.yaml
ls -la package.json
```

All should exist without errors.

### Check Routes Exist

```bash
ls -la backend/src/routes/
```

Should show:

- auth.js
- chat.js
- payment.js
- profile.js
- request.js
- user.js

---

## ✅ Dependencies Check

### Backend Dependencies

```bash
cd backend
npm list --depth=0
```

Should show all dependencies without errors.

### Frontend Dependencies

```bash
cd frontend
npm list --depth=0
```

Should show all dependencies without errors.

---

## ✅ Build Test

### Test Frontend Build

```bash
cd frontend
npm run build
```

Expected output:

```
vite v5.4.1 building for production...
✓ XXX modules transformed.
dist/index.html                   X.XX kB
dist/assets/index-XXXXX.css      XX.XX kB
dist/assets/index-XXXXX.js      XXX.XX kB
✓ built in XXXms
```

Check dist folder was created:

```bash
ls -la dist/
```

Should show:

- index.html
- assets/ folder

---

## ✅ Configuration Files Check

### 1. Check package.json scripts

```bash
cat backend/package.json | grep -A 3 "scripts"
```

Should have:

```json
"scripts": {
  "start": "node src/app.js",
  "dev": "nodemon src/app.js"
}
```

```bash
cat frontend/package.json | grep -A 4 "scripts"
```

Should have:

```json
"scripts": {
  "dev": "vite",
  "build": "vite build",
  "start": "node server.js",
}
```

### 2. Check .gitignore

```bash
cat backend/.gitignore
```

Should include:

- node_modules
- .env (important!)

```bash
cat frontend/.gitignore
```

Should include:

- node_modules
- dist
- .env.local

---

## ✅ Security Check

### 1. Verify .env is not in Git

```bash
git ls-files | grep "\.env$"
```

Should return nothing (empty). If it shows .env files:

```bash
git rm --cached backend/.env
git commit -m "Remove .env from git"
git push
```

### 2. Check for Hardcoded Secrets

```bash
grep -r "mongodb+srv://" --include="*.js" --include="*.jsx" backend/src/ frontend/src/
```

Should return nothing (secrets should only be in .env).

---

## ✅ Final Checklist

Before deploying, confirm:

- [ ] All code changes committed and pushed to GitHub
- [ ] MongoDB Atlas allows 0.0.0.0/0 connections
- [ ] Health endpoint works locally
- [ ] Frontend builds successfully
- [ ] Backend starts without errors
- [ ] .env file is NOT in git
- [ ] All dependencies installed
- [ ] render.yaml is correct
- [ ] Environment variables template ready

---

## 🚀 Ready to Deploy!

If all checks pass, you're ready to deploy:

1. Follow **RENDER_DEPLOYMENT_FIX.md** for step-by-step deployment
2. Use **DEPLOYMENT_CHECKLIST.md** as you deploy
3. Refer to **DEPLOYMENT_ARCHITECTURE.md** if you need to understand the flow

---

## ❌ If Any Check Fails

### MongoDB Connection Failed

- Check connection string in .env
- Verify MongoDB Atlas is running
- Check network access settings

### Build Failed

- Run `npm install` in frontend
- Check for syntax errors
- Verify all dependencies are installed

### Git Push Failed

- Check you have push access to repository
- Verify remote URL is correct
- Try: `git pull origin main` then push again

### Health Endpoint Not Working

- Verify changes in app.js
- Check if server is running
- Try restarting the server

---

## 📞 Need Help?

If you encounter issues:

1. Check the error message carefully
2. Look at the relevant guide:
   - RENDER_DEPLOYMENT_FIX.md
   - DEPLOYMENT_ARCHITECTURE.md
   - CHANGES_MADE.md
3. Verify environment variables
4. Check Render logs after deployment

Good luck! 🎉
