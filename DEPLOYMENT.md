# 🚀 Deployment Guide - Netlify + Backend Setup

## **Part 1: Deploy Backend (Render, Railway, or Heroku)**

Choose ONE of the following services to host your Node.js backend:

### **Option A: Using Render.com (Recommended - Free Tier)**

1. Go to [render.com](https://render.com) and sign up
2. Click **"New +"** → **"Web Service"**
3. Connect your GitHub repo (push your code first)
4. Configure:
   - **Name:** `online-counseling-api`
   - **Environment:** `Node`
   - **Build Command:** `npm install`
   - **Start Command:** `npm start` (in server folder)
   - **Root Directory:** `server`

5. Add Environment Variables:
   ```
   PORT=5000
   MONGODB_URI=your_mongodb_atlas_connection_string
   JWT_SECRET=your_jwt_secret_key
   AGORA_APP_ID=your_agora_app_id
   AGORA_APP_CERTIFICATE=your_agora_certificate
   STRIPE_SECRET_KEY=your_stripe_secret_key
   STRIPE_WEBHOOK_SECRET=your_webhook_secret
   EMAIL_USER=your_email@gmail.com
   EMAIL_PASSWORD=your_app_password
   CLIENT_URL=https://your-netlify-frontend.netlify.app
   ```

6. Deploy and copy the backend URL (e.g., `https://online-counseling-api.onrender.com`)

### **Option B: Using Railway.app**

1. Go to [railway.app](https://railway.app) and sign up
2. Click **"New Project"** → **"Deploy from GitHub"**
3. Select your repository
4. Add environment variables (same as above)
5. Deploy - copy the URL

---

## **Part 2: Deploy Frontend to Netlify**

### **Step 1: Prepare GitHub Repository**
```powershell
# Initialize git (if not done)
git init

# Add and commit all files
git add .
git commit -m "Initial commit"

# Create a repo on GitHub and push
git remote add origin https://github.com/YOUR_USERNAME/online-counseling.git
git branch -M main
git push -u origin main
```

### **Step 2: Deploy to Netlify**

1. Go to [netlify.com](https://netlify.com) and sign up/login
2. Click **"Add new site"** → **"Import an existing project"**
3. Select **GitHub** and authorize
4. Choose your repository
5. Configure Build Settings:
   - **Build command:** `cd client && npm run build`
   - **Publish directory:** `client/dist`
   - **Base directory:** (leave empty)

6. Add Environment Variables (Site settings → Build & deploy → Environment):
   ```
   VITE_API_URL=https://your-backend-url.onrender.com
   VITE_SOCKET_URL=https://your-backend-url.onrender.com
   ```

7. Click **"Deploy site"**

---

## **Part 3: Update Backend Configuration**

Update `server/index.js` to accept your Netlify frontend URL:

```javascript
const io = socketIo(server, {
  cors: { 
    origin: process.env.CLIENT_URL || "http://localhost:5173",
    methods: ["GET", "POST"],
    credentials: true
  },
});
```

---

## **Verification Checklist**

✅ Backend deployed and accessible (test with Postman)  
✅ MongoDB Atlas connection working  
✅ Environment variables set in Netlify  
✅ CORS configured for frontend URL  
✅ Stripe & Agora credentials set in backend  
✅ Email service configured  
✅ Frontend builds successfully  
✅ WebSocket connection working (test chat)  

---

## **Troubleshooting**

**Issue: "Cannot POST /api/..."**
- Check backend URL in Netlify environment variables
- Ensure backend is running and accessible

**Issue: "WebSocket connection failed"**
- Update `VITE_SOCKET_URL` in Netlify
- Check CORS settings in backend

**Issue: "Email not sending"**
- Verify EMAIL_USER and EMAIL_PASSWORD in backend environment

