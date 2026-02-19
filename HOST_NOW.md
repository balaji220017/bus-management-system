# Host Your App Now (Step-by-Step)

Follow these steps to get your **Bus Management System** live on the internet (frontend + backend + database).

---

## Part 1: Backend + Database (Railway)

### Step 1 – Create account and project

1. Open **https://railway.app** and sign in (GitHub recommended).
2. Click **“New Project”**.
3. Choose **“Deploy from GitHub repo”**.
   - If your code is not on GitHub yet:
     - Create a new repo on GitHub, push this folder (`bus-management-system`), then connect that repo to Railway.
   - Or choose **“Empty Project”** and we’ll use Railway CLI later.

### Step 2 – Add PostgreSQL

1. In the project, click **“+ New”**.
2. Select **“Database”** → **“PostgreSQL”**.
3. Wait for it to deploy. Click the PostgreSQL service.
4. Open the **“Variables”** or **“Connect”** tab.
5. Copy the **`DATABASE_URL`** value (starts with `postgresql://`). You’ll use it in the next step.

### Step 3 – Add Backend service

1. Click **“+ New”** → **“GitHub Repo”** (or **“Empty Service”** if no GitHub).
2. Select the repo that contains this project (root = folder with `package.json`, `server/`, `app/`).
3. Click the new **service** (your backend).

### Step 4 – Configure Backend

1. Go to **“Settings”** for this service.
2. **Build:**
   - **Build Command:**  
     `npm install && node scripts/use-postgres-schema.js && npx prisma generate`
   - **Output Directory:** leave empty.
3. **Deploy:**
   - **Start Command:**  
     `npx prisma db push --accept-data-loss 2>nul & npx tsx server/index.ts`  
     (On Railway’s Linux env you can use:  
     `npx prisma db push --accept-data-loss 2>/dev/null; npx tsx server/index.ts`  
     or simply: `npm run start:railway` if that script exists.)
4. **Variables** (Settings → Variables, or “Variables” tab):
   - `DATABASE_URL` = paste the PostgreSQL URL from Step 2 (Railway often links it automatically if you added the DB in the same project).
   - `JWT_SECRET` = any long random string (e.g. use https://generate-secret.vercel.app/32).
   - `FRONTEND_URL` = leave empty for now; set after Part 2 (e.g. `https://your-app.vercel.app`).
   - `PORT` = `5000` (or leave unset if Railway sets `PORT`).

5. Save. Railway will build and deploy.

### Step 5 – Get backend URL

1. In the backend service, open **“Settings”** → **“Networking”** (or **“Deploy”**).
2. Click **“Generate Domain”**.
3. Copy the URL (e.g. `https://bus-management-production.up.railway.app`).  
   This is your **Backend URL**. Use it in Part 2 and in `FRONTEND_URL` in Part 1 Step 4.

### Step 6 – Point frontend to backend (CORS)

1. Go back to the **backend** service → **Variables**.
2. Set **`FRONTEND_URL`** = your Vercel URL from Part 2 (e.g. `https://bus-management.vercel.app`).
3. Redeploy the backend (e.g. “Redeploy” in Deployments) so CORS allows your frontend.

---

## Part 2: Frontend (Vercel)

### Step 1 – Create account and import project

1. Open **https://vercel.com** and sign in (e.g. GitHub).
2. Click **“Add New…”** → **“Project”**.
3. **Import** the same GitHub repo (this project).
4. **Root Directory:** leave default (repository root).

### Step 2 – Environment variable

1. Before deploying, open **“Environment Variables”**.
2. Add:
   - **Name:** `NEXT_PUBLIC_API_URL`
   - **Value:** your **Backend URL** from Part 1 Step 5 (e.g. `https://bus-management-production.up.railway.app`).
   - No trailing slash.
3. Apply to **Production** (and Preview if you want).

### Step 3 – Deploy

1. Click **“Deploy”**.
2. Wait for the build to finish. Vercel will show a URL like `https://bus-management-xxx.vercel.app`.

This URL is your **live website**. Remember to set `FRONTEND_URL` on Railway to this URL (Part 1 Step 6).

---

## Part 3: Seed the production database (optional)

To have sample buses, routes, and users (e.g. admin) in production:

1. **Temporarily** set `DATABASE_URL` on your **local** machine to the **production** PostgreSQL URL from Railway (same as in backend Variables).
2. In the project folder run:

```bash
cd bus-management-system
node scripts/use-postgres-schema.js
npx prisma db push
npm run seed
```

3. Remove or unset the production `DATABASE_URL` from your local environment so you don’t use it by mistake.

Default seeded users (change passwords in production):

- **Admin:** `admin@bussystem.com` / `admin123`
- **Passenger:** `passenger@bussystem.com` / `passenger123`

---

## Checklist

- [ ] Railway: PostgreSQL added and `DATABASE_URL` copied.
- [ ] Railway: Backend service added, build/start commands and variables set.
- [ ] Railway: Domain generated and Backend URL copied.
- [ ] Vercel: Repo imported, `NEXT_PUBLIC_API_URL` = Backend URL, deployed.
- [ ] Railway: `FRONTEND_URL` = Vercel URL, backend redeployed.
- [ ] Optional: Production DB seeded locally with `npm run seed`.

---

## Test

- Open your **Vercel URL** (e.g. `https://bus-management-xxx.vercel.app`).
- Try **Login** with `admin@bussystem.com` / `admin123` (if you ran the seed).
- Try **Book Ticket**, **Routes**, **Track Bus**. All should call the backend; no CORS errors in the browser console.

You’re done: **web page, backend, and database are hosted.**

For more detail (Docker, alternate platforms, troubleshooting), see **DEPLOYMENT.md**.
