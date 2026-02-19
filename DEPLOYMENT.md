# Deploy Bus Management System (Frontend + Backend + Database)

This guide covers hosting the **web app (Next.js)**, **backend API (Express)**, and **database** using free-tier friendly platforms.

## Architecture (Recommended)

| Component | Platform   | URL / Notes                    |
|----------|------------|---------------------------------|
| **Frontend** | Vercel     | `https://your-app.vercel.app`   |
| **Backend**  | Railway    | `https://your-api.railway.app`  |
| **Database** | Railway PostgreSQL | Auto-created with backend service |

---

## Option 1: Railway (All-in-one: Backend + Database)

Railway can host your **backend** and provide a **PostgreSQL** database in one project.

### 1. Create Railway project

1. Go to [railway.app](https://railway.app) and sign in (e.g. GitHub).
2. **New Project** → **Deploy from GitHub repo** (connect your repo).
3. If you don’t use GitHub: **New Project** → **Empty Project**, then we’ll use Railway CLI or manual deploy.

### 2. Add PostgreSQL

1. In the project: **+ New** → **Database** → **PostgreSQL**.
2. After it’s created, open the PostgreSQL service → **Variables** (or **Connect**).
3. Copy **`DATABASE_URL`** (e.g. `postgresql://postgres:xxx@xxx.railway.app:5432/railway`).

### 3. Deploy backend service

1. **+ New** → **GitHub Repo** (or **Empty Service** and connect later).
2. Select this repo.
3. **Settings** for this service:
   - **Root Directory:** leave empty (repo root).
   - **Build Command:** `npm install && npx prisma generate`
   - **Start Command:** `npx prisma db push --accept-data-loss 2>/dev/null || true && npx tsx server/index.ts`
   - **Watch Paths:** `server/`, `prisma/`, `package.json`
4. **Variables** for this service:
   - `DATABASE_URL` = (paste from PostgreSQL service; Railway often links it automatically).
   - `JWT_SECRET` = (generate a long random string).
   - `FRONTEND_URL` = `https://your-frontend.vercel.app` (update after deploying frontend).
   - `PORT` = `5000` (or leave default if Railway sets `PORT`).
5. Deploy. After deploy, open **Settings** → **Networking** → **Generate Domain** and note the URL (e.g. `https://xxx.up.railway.app`). This is your **backend URL**.

### 4. Use PostgreSQL in production

- In production, Prisma must use the **PostgreSQL** schema.
- Either:
  - Temporarily swap: copy `prisma/schema.postgresql.prisma` over `prisma/schema.prisma` and commit, then run `npx prisma db push` (and `prisma generate`) in the build/start commands above, or
  - Add a build step that runs:
    - `cp prisma/schema.postgresql.prisma prisma/schema.prisma` (or equivalent)
    - then `npx prisma generate` and `npx prisma db push`.
- So in **Build Command** you can do:
  - `npm install && cp prisma/schema.postgresql.prisma prisma/schema.prisma && npx prisma generate`
- **Start Command** stays:
  - `npx prisma db push --accept-data-loss 2>/dev/null || true && npx tsx server/index.ts`

### 5. Seed production DB (optional)

- In Railway, open your backend service → **Variables** → add the same `DATABASE_URL` if not already there.
- Locally set `DATABASE_URL` to the **production** PostgreSQL URL, then run:
  - `cp prisma/schema.postgresql.prisma prisma/schema.prisma && npx prisma db push && npm run seed`
- Or use Railway’s **Run Command** / one-off run with the same env and commands.

---

## Option 2: Vercel (Frontend only)

The **Next.js** app is deployed on Vercel and talks to the backend via `NEXT_PUBLIC_API_URL`.

### 1. Connect repo

1. Go to [vercel.com](https://vercel.com) and sign in (e.g. GitHub).
2. **Add New** → **Project** → import this repo.

### 2. Project settings

- **Framework Preset:** Next.js.
- **Root Directory:** leave default (repo root).
- **Build Command:** `npm run build` (default).
- **Install Command:** `npm install` (default).

### 3. Environment variables

In **Settings** → **Environment Variables** add:

| Name                    | Value                          | Environments   |
|-------------------------|--------------------------------|----------------|
| `NEXT_PUBLIC_API_URL`   | `https://your-backend.railway.app` | Production, Preview |

Use the **exact** backend URL from Railway (no trailing slash).  
Redeploy after adding variables.

### 4. Deploy

- Push to your main branch (or the branch you connected) to trigger a deploy.
- Your site will be at `https://your-project.vercel.app` (or your custom domain).

---

## Option 3: Single platform (e.g. Railway for frontend + backend + DB)

You can also run the **Next.js** app on Railway as a second service:

1. **Backend service:** Same as Option 1 (Express + Prisma + PostgreSQL).
2. **Frontend service:**
   - **+ New** → same repo.
   - **Root Directory:** leave empty.
   - **Build Command:** `npm run build`
   - **Start Command:** `npm run start`
   - **Variables:**
     - `NEXT_PUBLIC_API_URL` = `https://your-backend.railway.app` (backend service URL).
   - **Generate Domain** for this service → frontend URL.
3. In backend **Variables**, set `FRONTEND_URL` to this frontend URL (for CORS).

---

## Environment variables summary

### Backend (Railway or any host)

| Variable         | Description                    | Example                          |
|------------------|--------------------------------|----------------------------------|
| `DATABASE_URL`   | PostgreSQL connection string   | `postgresql://...` (from Railway DB) |
| `JWT_SECRET`     | Secret for JWT signing         | Long random string               |
| `FRONTEND_URL`   | Allowed CORS origin (frontend)| `https://your-app.vercel.app`    |
| `PORT`           | Server port                    | `5000` (or host’s `PORT`)        |

### Frontend (Vercel or any host)

| Variable               | Description              | Example                          |
|------------------------|--------------------------|----------------------------------|
| `NEXT_PUBLIC_API_URL`  | Backend API base URL     | `https://your-api.railway.app`   |

---

## Post-deploy checklist

- [ ] Backend health: `curl https://your-backend.railway.app/health`
- [ ] Frontend loads and shows no CORS errors in browser console.
- [ ] Login/Register use backend (check Network tab: requests go to `NEXT_PUBLIC_API_URL`).
- [ ] Book a ticket and check dashboard (data in PostgreSQL).
- [ ] Optional: run seed once against production `DATABASE_URL` for initial data.

---

## Quick commands (local, for production DB)

```bash
# Use production PostgreSQL and push schema + seed
export DATABASE_URL="postgresql://..."
cp prisma/schema.postgresql.prisma prisma/schema.prisma
npx prisma db push
npm run seed
```

---

## Troubleshooting

- **CORS errors:** Ensure backend `FRONTEND_URL` exactly matches the frontend origin (protocol + domain, no trailing slash).
- **API 404:** Confirm `NEXT_PUBLIC_API_URL` has no trailing slash and points to the backend root (e.g. `https://xxx.up.railway.app`).
- **Database connection:** Ensure backend service has `DATABASE_URL` from the same Railway project (PostgreSQL service).
- **Build fails (Prisma):** Use PostgreSQL schema in build: e.g. `cp prisma/schema.postgresql.prisma prisma/schema.prisma && npx prisma generate`.

Once this is done, your **web page (frontend)**, **backend**, and **database** are all hosted and working together.
