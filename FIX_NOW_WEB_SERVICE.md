# 🚨 URGENT FIX - Frontend Web Service

## Your Problem

Frontend is a **Web Service** (not Static Site) and showing blank page.

## Root Cause

Build command probably doesn't include `npm run build`, so the `dist` folder is empty or missing.

---

## Fix in 5 Steps (5 minutes)

### Step 1: Go to Render Dashboard

https://dashboard.render.com/

### Step 2: Click Your Frontend Service

(The one showing blank page)

### Step 3: Update Build Command

1. Click **"Settings"** tab
2. Find **"Build Command"**
3. Change to:
   ```
   npm install && npm run build
   ```
4. Verify **"Start Command"** is:
   ```
   npm start
   ```
5. Click **"Save Changes"**

### Step 4: Add Environment Variable

1. Click **"Environment"** tab
2. Click **"Add Environment Variable"**
3. Add:
   ```
   Key: VITE_API_URL
   Value: https://your-backend-url.onrender.com
   ```
   ⚠️ **Replace with your ACTUAL backend URL!**
4. Click **"Save Changes"**

### Step 5: Redeploy

1. Click **"Manual Deploy"** button (top right)
2. Select **"Clear build cache & deploy"**
3. Wait 5-7 minutes
4. Watch the logs

---

## What to Look For in Logs

You MUST see this in the logs:

```
==> Building
npm run build

> devtinder-web@0.0.0 build
> vite build

vite v5.4.1 building for production...
✓ 150 modules transformed.
dist/index.html                   0.46 kB
dist/assets/index-abc.css        50.23 kB
dist/assets/index-xyz.js        250.45 kB
✓ built in 5.23s

==> Starting server
npm start
Frontend server running on port 10000
```

If you DON'T see "vite build", the build command is still wrong!

---

## Quick Checklist

Before redeploying, verify:

- [ ] Build Command: `npm install && npm run build`
- [ ] Start Command: `npm start`
- [ ] Root Directory: `frontend`
- [ ] Environment Variable: `VITE_API_URL` = backend URL

---

## After Deployment

1. **Open your frontend URL**
2. **Should see login page** (not blank)
3. **Press F12** → Check console
4. **Should have no errors**

---

## If Still Blank

### Check 1: Did build run?

Look in logs for "vite build" - if missing, build command is wrong

### Check 2: Is VITE_API_URL set?

Environment tab should show it

### Check 3: Is backend URL correct?

Must be exact URL from Render, with https://

### Check 4: Did you redeploy after adding env var?

Environment variables are baked into build - must redeploy!

---

## Alternative: Convert to Static Site

If Web Service keeps failing, convert to Static Site (easier):

1. **Delete current service**
   - Settings → Delete Service

2. **Create Static Site**
   - New + → Static Site
   - Root Directory: `frontend`
   - Build Command: `npm install && npm run build`
   - Publish Directory: `dist`
   - Add `VITE_API_URL` environment variable

Static Sites are simpler and more reliable for React apps!

---

## Most Important

**The build command MUST include `npm run build`!**

Without it, the `dist` folder won't be created, and you'll get a blank page.

Current (wrong):

```
npm install
```

Correct:

```
npm install && npm run build
```

---

## Do This Now

1. ✅ Update build command
2. ✅ Add VITE_API_URL
3. ✅ Redeploy
4. ✅ Watch logs for "vite build"
5. ✅ Test frontend URL

**Time: 5-10 minutes**

Go! 🚀
