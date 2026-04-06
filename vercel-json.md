# 🚨 My Deployment Debugging Journey (Vercel + Render + Vite + Express)

## 📌 Context: What I Was Trying to Do

I was deploying my full‑stack project:  
- Frontend: Vite + React  
- Backend: Node.js + Express + MongoDB  
- Hosting: Frontend on Vercel, Backend on Render  

At one point everything was working smoothly. Then I touched `vercel.json`, redeployed, and suddenly I was staring at 404s and dead APIs instead of my app.

***

## ❌ Where I Broke Things: vercel.json

To “optimize” the setup, I changed my `vercel.json`:

- I added a custom `distDir`
- I changed routes to point to:
  - `/task-management-frontends/dist/index.html`
  - `/index.html`

In my head it made sense: Vite builds to `dist`, so I tried to wire that directly. In reality, this is not how Vercel’s `@vercel/static-build` works. It builds the Vite app, handles `dist/` internally, and mounts the built app under the project path, not under `/dist`. [vite](https://vite.dev/guide/static-deploy)

So the paths I configured basically pointed to files that **don’t exist** in Vercel’s runtime filesystem. That’s why the app suddenly started returning 404s everywhere.

The last working deployment (commit `b6c8b13`) had this config:

```json
{
  "version": 2,
  "builds": [
    { "src": "backend/server.js", "use": "@vercel/node" },
    { "src": "task-management-frontends/package.json", "use": "@vercel/static-build" }
  ],
  "routes": [
    { "src": "/api/.*", "dest": "/backend/server.js" },
    { "src": "^/[^.]+$", "dest": "/task-management-frontends/index.html" },
    { "src": "/(.*)", "dest": "/task-management-frontends/$1" }
  ]
}
```

This worked because Vercel mounts the built frontend under `/task-management-frontends/`, and my routes were aligned with that structure. [vercel](https://vercel.com/docs/frameworks/frontend/vite)

The fix here was humbling but simple: stop being clever and **restore the exact previous config**.

***

## ❌ The “App Is Broken” Illusion: Render Cold Starts

My frontend was using this env:

```bash
VITE_API_URL=https://ias-management-a6k0.onrender.com/api
```

On Render’s free tier, the backend goes to sleep after roughly 15 minutes of inactivity, and a cold start can take tens of seconds to spin back up.  To me, this looked like: [community.render](https://community.render.com/t/free-tier-queries/6555)

- First request: super slow or failing
- Frontend: “APIs are not working”
- My brain: “I broke everything again”

What was actually happening:  
- The older deployment “felt” fine because the server was already awake.  
- After a while, the free tier put it to sleep again, and I kept misdiagnosing it as a code or config problem.

To keep the server awake, I wired a cron/uptime monitor to hit:

`https://ias-management-a6k0.onrender.com/api/auth/profile`

My rule of thumb became:

- 401 → good (server is alive, auth doing its job)
- 404 → wrong endpoint
- no response / timeout → server asleep or crashed

This was a big mindset shift for me: not every “bug” is my code; sometimes it’s just platform behavior.

***

## ❌ Architecture Confusion: Two Backends, One Actually Used

Another thing I realized about my own setup:

- I had a backend deployed on Vercel.
- I had a backend deployed on Render.
- But my frontend was calling only the Render API.

So I was effectively maintaining two backends, but **only one was actually in use**. That meant:

- The Vercel backend was just noise and confusion.
- All the real issues were tied to Render’s free tier and cold starts.

This taught me to be more disciplined about **one clear source of truth** for my backend in a given environment.

***

## ✅ What Finally Stabilized Everything

### 1. I Reverted vercel.json Completely

I stopped experimenting and went back to the last known‑good configuration:

- Same `builds`
- Same `routes`
- No manual pointing to `dist/`

Then I redeployed with:

```bash
vercel --prod
```

This instantly fixed the 404 mess on the frontend.

### 2. I Accepted Render’s Free Tier Behavior and Worked Around It

Instead of blaming my code every time, I:

- Set up a cron/uptime monitor to ping the backend every 10 minutes.
- Used a real API route (`/api/auth/profile`) to verify if the server was awake.
- Treated 401 as a “healthy but protected” signal.

This reduced the random “my app is down” feeling and helped me see patterns.

### 3. I Respected How Vite Handles Env Variables

I reminded myself that Vite env variables are baked at **build time**. [dev](https://dev.to/dutchskull/setting-up-dynamic-environment-variables-with-vite-and-docker-5cmj)

So if I change:

```bash
VITE_API_URL=...
```

I must:

- Update `.env` (or Vercel project env)
- Rebuild the frontend
- Redeploy

No more assuming that changing an env on the server instantly updates the client bundle.

***

## 💡 Key Learnings About Myself and My Setup

1. I tend to over‑tweak config when something breaks.  
   - Reality: reverting to a known‑good version is often the fastest path.

2. I learned how opinionated platforms are.  
   - Vercel has strong defaults (especially for Vite static builds), and fighting them with manual paths usually backfires. [vite](https://vite.dev/guide/static-deploy)

3. I confused platform behavior with bugs.  
   - Render cold starts made me think my APIs were broken when they were just slow to spin up. [linkedin](https://www.linkedin.com/posts/rishabh-jain-69789622b_coldstart-freehosting-optimization-activity-7238996036684275712-OU61)

4. I saw the cost of mixed deployments.  
   - Two backends (Vercel + Render) for one app created mental overhead without any real benefit.

5. I started treating env management as part of “code”, not an afterthought.  
   - Vite’s build‑time envs forced me to be intentional about builds and deploys. [vueschool](https://vueschool.io/articles/vuejs-tutorials/how-to-use-environment-variables-in-vite-js/)

***

## 🚀 My Current Stable Setup

Right now, my **stable** setup looks like this:

- Frontend on Vercel  
  - Vite build, served via `@vercel/static-build`
  - Routes correctly pointing to `/task-management-frontends/`
- Backend on Render (free tier)  
  - MongoDB Atlas connected
  - Kept warm with periodic pings
- Frontend calls the Render API using `VITE_API_URL` set at build time

It’s not perfect, but it’s predictable, and I understand the trade‑offs.

***

## 🔥 How I’m Thinking About Next Steps

From this journey, I see two clear future options for myself:

- If I want **zero cold starts** and a more serverless‑native setup:
  - Move the backend fully to Vercel.
  - Replace multer/local uploads with something like Cloudinary or S3.

- If I want to keep things simple for now:
  - Stick with Render for backend.
  - Possibly move to a paid tier to avoid cold starts.
  - Keep Vercel purely for frontend.

Either way, this debugging journey forced me to understand my tooling better instead of just copy‑pasting configs and hoping they work.

If you want, I can turn this into a short “story style” LinkedIn post or a Twitter/X thread template based on my journey.
