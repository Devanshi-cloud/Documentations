# 📘 Deployment Documentation: IAS Management (Frontend + Backend)

## 🧠 Overview

This document explains:

- How I deployed the IAS Management project  
- Differences between static vs web services  
- Why some deployments failed  
- Why the final setup worked  
- Extra tips for debugging, logs, and env setup on Vercel + Render [vercel](https://vercel.com/docs/frameworks/backend)

***

## 🚀 1. Initial Deployment (Frontend on Vercel)

### ✅ What I did

- Logged into Vercel CLI  
- Linked my GitHub repo  
- Deployed using:

  ```bash
  vercel
  ```

- Then deployed to production:

  ```bash
  vercel --prod
  ```

- Build command configured (in Vercel dashboard or `vercel.json`):

  ```text
  npm install
  npm run build
  ```

  Output directory: `dist` or `build` (depending on the frontend setup). [vercel](https://vercel.com/docs/frameworks/backend)

### ✅ Why it worked

Vercel is optimized for:

- React / Next.js / SPA frontends  
- Static assets and serverless functions  
- Automatic CI/CD with GitHub integration [vercel](https://vercel.com/docs/limits)

My **frontend** (`task-management-frontends`) is a client-side app with API calls to an external backend URL, so:

- No need for custom Node.js server on Vercel  
- Vercel just serves built static files from its CDN  
- All backend logic is delegated to Render

### ⚠️ Important Note (Vercel limits)

Vercel:

- Is great for static or serverless frontends  
- Has strict limits on function duration, no persistent processes, no native WebSocket support [northflank](https://northflank.com/blog/vercel-backend-limitations)
- Is **not suitable** for:

  - Long-running Express servers  
  - Background workers / queues  
  - Persistent WebSocket connections  
  - Stateful backend services [reddit](https://www.reddit.com/r/nextjs/comments/1f997jm/how_to_handle_longrunning_tasks_on_vercel_over_15/)

So using Vercel for the IAS backend (full Express + Mongo + messaging) would be unreliable.

***

## ❌ 2. Issue: Trying to Deploy Backend on Vercel

### 🧩 Problem

Backend includes:

- Express server  
- MongoDB connection  
- Messaging / real-time style logic (needs persistent process)  

When attempted on Vercel:

- Deployment “succeeded” but  
- APIs did **not** behave reliably  
- Any long-running logic and DB connections were fragile

### 🧠 Reason

Vercel backend is serverless:

- Functions spin up per-request and then shut down  
- No guarantee of persistent DB connection  
- No long-running process, no proper WebSockets [northflank](https://northflank.com/blog/vercel-backend-limitations)

This doesn’t match a typical Express + Mongo backend that expects to:

- Keep a server process alive  
- Maintain DB pool connections  
- Handle continuous traffic and possibly real-time features

***

## 🚀 3. Switching to Render (Correct Approach)

### ✅ What I used

- **Render Web Service** (Node.js / Express backend)  
- **Not** a Static Site (that’s only for frontends) [render](https://render.com/docs/static-sites)

Render Web Service gives:

- Container-like environment for Node.js  
- Long-running server process  
- Configurable start command (`node server.js`)  
- Built-in `PORT` env variable for HTTP binding [render-web.onrender](https://render-web.onrender.com/docs/environment-variables)

***

## 🔥 4. Static Site vs Web Service (CRITICAL DIFFERENCE)

### ❌ Static Site (WRONG for backend)

Used for:

- HTML, CSS, JS frontend bundles  
- Static React/Next builds  
- Documentation sites [render](https://render.com/docs/static-sites)

Does **NOT** support:

- Node.js server or Express  
- Custom backend routes  
- Long-running processes

If you deploy the backend as a Static Site:

- Render will just serve built files (if any)  
- `/api/...` routes will 404 or be missing  
- No Express server actually runs [community.render](https://community.render.com/t/cant-connect-api-calls-to-web-service-express-from-my-static-site/10229)

👉 That’s why backend failed initially when treated like a static deployment.

### ✅ Web Service (CORRECT)

Used for:

- Node.js backend  
- Express apps and REST APIs  
- Anything that needs a persistent process

Supports:

- Long-running HTTP server  
- MongoDB / Postgres connections  
- Background work and timers (within container lifetime) [forum.freecodecamp](https://forum.freecodecamp.org/t/render-web-service-deploy/717518)

***

## ⚙️ 5. Backend Deployment Steps (Render)

### Step 1: Create Web Service

- Go to Render dashboard  
- “New” → “Web Service”  
- Connect GitHub repo  
- Select backend folder / root (where `package.json` and `server.js` live)

### Step 2: Configure settings

- **Environment**: Node  
- **Build Command** (if applicable):

  ```text
  npm install
  npm run build   # or just npm install if no build step
  ```

- **Start Command**:

  ```text
  node server.js
  ```

  or, if you have a script:

  ```text
  npm start
  ```

- **Environment Variables** (in Render dashboard → Environment):

  ```text
  MONGO_URI=your_mongodb_connection
  JWT_SECRET=your_secret
  NODE_ENV=production
  FRONTEND_URL=https://your-vercel-domain.vercel.app
  ```

  Render automatically sets `PORT`, which your app must respect. [forum.freecodecamp](https://forum.freecodecamp.org/t/render-web-service-deploy/717518)

***

## 🔥 Step 3: Fix Critical Bugs

### ❌ Issue 1: Port Error (`connection refused`)

**Cause**

- Hard-coded port like `5000` or `8000`  
- On Render, incoming traffic is routed to whichever port is in the `PORT` env var (often `10000`), not your own port. [render-web.onrender](https://render-web.onrender.com/docs/environment-variables)

**Fix**

```js
const PORT = process.env.PORT || 5000;

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

This allows:

- Local dev on `5000`  
- Render to inject correct port at runtime [render-web.onrender](https://render-web.onrender.com/docs/environment-variables)

***

### ❌ Issue 2: `Cannot GET /`

**Cause**

- No root route defined  
- Hitting base URL `/` returns 404  
- Hard to verify if the server is running

**Fix**

```js
app.get("/", (req, res) => {
  res.send("Backend is running 🚀");
});
```

This gives:

- Simple health-check endpoint  
- Easy test in browser or via `curl`:

  ```bash
  curl https://ias-management-a6k0.onrender.com
  ```

***

### ❌ Issue 3: Deployment Timeout / Crashes

**Possible causes**

- MongoDB not connecting (wrong `MONGO_URI`)  
- Missing or invalid env variables  
- App crashing on startup before listening on `PORT` [forum.freecodecamp](https://forum.freecodecamp.org/t/render-web-service-deploy/717518)

**Fixes**

- Ensure `MONGO_URI` is set correctly in Render env  
- Use proper error logging around DB connection, for example:

  ```js
  mongoose
    .connect(process.env.MONGO_URI)
    .then(() => console.log("MongoDB connected"))
    .catch((err) => {
      console.error("MongoDB connection error:", err);
      process.exit(1);
    });
  ```

- Check Render **Logs** (Deploy logs + Live logs) whenever you see:

  - “Build succeeded but health check failed”  
  - “Service failed to start within 90 seconds” [forum.freecodecamp](https://forum.freecodecamp.org/t/render-web-service-deploy/717518)

***

### 🌐 CORS / Frontend Integration

To allow the Vercel frontend to call the Render backend:

```js
import cors from "cors";

app.use(
  cors({
    origin: process.env.FRONTEND_URL, // Vercel URL
    credentials: true,
  })
);
```

- Set `FRONTEND_URL` in Render env to your deployed frontend URL  
- This avoids CORS issues when making `fetch` calls from the browser [community.render](https://community.render.com/t/cant-connect-api-calls-to-web-service-express-from-my-static-site/10229)

***

## 🔗 6. Connecting Frontend + Backend

### Backend URL

```text
https://ias-management-a6k0.onrender.com
```

### Frontend: API calls

Updated to:

```js
fetch("https://ias-management-a6k0.onrender.com/api/...")
```

Key points:

- Always use the **Render** URL, not `localhost`, in production  
- You can keep `localhost:5000` for dev using environment-based config in the frontend (e.g., `.env.development` / `.env.production`). [community.render](https://community.render.com/t/cant-connect-api-calls-to-web-service-express-from-my-static-site/10229)

***

## ⚡ 7. Final Working Architecture

### ✅ Frontend

- Hosted on **Vercel**  
- Handles UI and routing  
- Calls backend via HTTPS

### ✅ Backend

- Hosted on **Render Web Service**  
- Handles:

  - Auth  
  - APIs  
  - Business logic  
  - DB access

### ✅ Database

- **MongoDB Atlas**  
- Exposed via `MONGO_URI` in Render env [forum.freecodecamp](https://forum.freecodecamp.org/t/render-web-service-deploy/717518)

***

## 🧠 Why This Setup Works

| Component | Platform          | Why it fits                                                                 |
|----------|-------------------|-----------------------------------------------------------------------------|
| Frontend | Vercel            | Fast static hosting, global CDN, perfect for SPA/UI                        |
| Backend  | Render Web Service| Full Node.js server, long-running process, can handle DB + APIs            |
| Database | MongoDB Atlas     | Managed cloud DB, connects over internet from Render container             |  [vercel](https://vercel.com/docs/frameworks/backend)

This separation matches platform strengths:

- Vercel → **stateless** frontend & light serverless  
- Render → **stateful** backend  
- MongoDB Atlas → **external managed DB**

***

## 🔥 Key Learnings

- Frontend deployment and backend deployment have different requirements  
- Vercel is mainly for frontend + short-lived serverless functions  
- Render Web Service (or similar platforms) is better for full Express backends [northflank](https://northflank.com/blog/vercel-backend-limitations)

Always:

- Use `process.env.PORT` for server port  
- Store secrets (Mongo URI, JWT, etc.) in environment variables  
- Choose **Static Site** for pure frontend, **Web Service** for Node/Express

***

## ✅ Final Result

- ✔ Frontend deployed successfully (Vercel)  
- ✔ Backend running without crashes (Render Web Service)  
- ✔ APIs working end-to-end  
- ✔ MongoDB connected via Atlas  
- ✔ Full IAS Management system functional and ready for use 🚀
