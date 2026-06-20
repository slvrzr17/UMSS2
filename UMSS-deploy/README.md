# University Maintenance Services System (UMSS)

A campus maintenance portal for **Polytechnic University of the Philippines (PUP)** General Services Office. Students can report broken items, scan classroom QR codes, book rooms, and request equipment — while GSO admins and technicians manage the work-order pipeline.

---

## Tech stack

| Layer | Technology |
|---|---|
| Frontend | React 19 · TypeScript · Vite · Tailwind CSS v4 |
| Backend | Node.js · Express 5 · TypeScript |
| Validation | Zod |
| Logging | Pino |
| Database | PostgreSQL + Drizzle ORM *(wired up in next phase)* |
| Deployment | Vercel (frontend + serverless API) · Replit |

---

## Project structure

```
umss/
├── frontend/               # React SPA (Vite)
│   ├── src/
│   │   ├── components/
│   │   │   └── umss/
│   │   │       ├── UMSS.tsx       ← full UI (all screens)
│   │   │       └── mockData.ts    ← seed / mock data
│   │   ├── assets/
│   │   │   └── PUP_LOGO.png
│   │   ├── App.tsx
│   │   ├── main.tsx
│   │   └── index.css
│   ├── index.html
│   ├── vite.config.ts
│   └── package.json
│
├── backend/                # Express API
│   ├── src/
│   │   ├── routes/
│   │   │   ├── index.ts           ← register routes here
│   │   │   └── health.ts          ← GET /api/healthz
│   │   ├── lib/
│   │   │   └── logger.ts
│   │   ├── app.ts                 ← Express app + middleware
│   │   └── index.ts               ← server entry + graceful shutdown
│   ├── build.mjs                  ← esbuild production bundler
│   └── package.json
│
├── vercel.json             ← Vercel deployment config
├── .replit                 ← Replit Agent config
├── .env.example            ← required environment variables
└── README.md
```

---

## Local development

### Prerequisites
- Node.js 22+
- npm 10+

### 1 — Clone & install

```bash
git clone https://github.com/<your-username>/umss.git
cd umss

# Install root dev tools
npm install

# Install frontend deps
cd frontend && npm install && cd ..

# Install backend deps
cd backend && npm install && cd ..
```

### 2 — Configure environment

```bash
cp .env.example .env
# Edit .env with your DATABASE_URL if using Postgres
```

### 3 — Run both servers

```bash
npm run dev
# Frontend → http://localhost:5173
# Backend  → http://localhost:3000
```

Or run them separately:

```bash
npm run dev:frontend   # Vite dev server on :5173
npm run dev:backend    # Express with tsx watch on :3000
```

---

## Deployment

### Vercel (recommended for frontend + API)

1. Push this repo to GitHub.
2. Import the repo on [vercel.com](https://vercel.com).
3. Vercel auto-detects `vercel.json` and configures:
   - `/api/*` → serverless Express
   - everything else → Vite static SPA
4. Set environment variables in the Vercel dashboard (`DATABASE_URL`, `ALLOWED_ORIGINS`, etc.)

### Replit

1. On [replit.com](https://replit.com), click **Import from GitHub** and paste the repo URL.
2. Replit reads `.replit` automatically — click **Run**.
3. Set secrets (`PORT`, `DATABASE_URL`) in **Secrets** tab.

---

## Adding new API routes

```typescript
// backend/src/routes/workOrders.ts
import { Router } from "express";

const router = Router();

router.get("/", (_req, res) => {
  res.json({ workOrders: [] });
});

export default router;
```

Then register it in `backend/src/routes/index.ts`:

```typescript
import workOrdersRouter from "./workOrders.js";
router.use("/work-orders", workOrdersRouter);
```

---

## Screens (UMSS.tsx)

| Screen | Role | Description |
|---|---|---|
| `login` | All | Role-based login (student / admin / superadmin) |
| `student-dashboard` | Student | Quick actions, submissions, GSO access button |
| `qr-review` | Student | Classroom QR scan → rate & flag defects |
| `report-form` | Student | Report broken item by campus / location / category |
| `technician-queue` | GSO Tech | Work-order queue with status updates |
| `admin-dashboard` | Admin | Work-order overview, reassignment, audit log |
| `superadmin-dashboard` | SuperAdmin | System-wide stats, user management |
| `campus-assistant` | Student | FAQ chatbot |
| `room-reservation` | Student | Reserve study rooms with date chips |
| `equipment-request` | Student | Borrow / return equipment |
| `user-management` | Admin/SuperAdmin | Activate / deactivate accounts |

---

## License

MIT
