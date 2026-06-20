# 🐳 Inventory Management System — Docker Deployment

One-command deployment for the **Inventory Management System**. This repository
contains the Docker Compose orchestration that brings up the full stack:
**PostgreSQL + Backend API + Frontend (nginx)**.

The backend and frontend images are **built directly from their source repositories**,
so you only need this file to run everything.

| Component | Source repository |
| --------- | ----------------- |
| Backend (Express + TS + Prisma) | https://github.com/devilthecoder1123/inventory-management-backend |
| Frontend (React + Vite + TS)    | https://github.com/devilthecoder1123/inventory-management-frontend |

---

## 🚀 Run it

Requires **Docker** + **Docker Compose**.

```bash
git clone https://github.com/devilthecoder1123/inventory-management-docker.git
cd inventory-management-docker
docker compose up --build
```

Then open:

| Service     | URL                              |
| ----------- | -------------------------------- |
| 🖥️ Web app  | http://localhost:8080            |
| 🔌 API      | http://localhost:4000/api/health |
| 🐘 Postgres | localhost:5432                   |

Migrations are applied and demo data is seeded automatically on first boot.

### Demo accounts

| Role  | Email          | Password |
| ----- | -------------- | -------- |
| Admin | admin@ims.dev  | admin123 |
| Staff | staff@ims.dev  | staff123 |

Stop with `docker compose down` (add `-v` to also remove the database volume).

---

## 🧱 What it runs

```
┌──────────────┐   /api proxy   ┌──────────────┐         ┌──────────────┐
│  frontend    │ ─────────────▶ │   backend    │ ──────▶ │  PostgreSQL  │
│ nginx :8080  │ ◀───────────── │  API :4000   │  Prisma │    :5432     │
└──────────────┘     JSON       └──────────────┘         └──────────────┘
```

- **frontend** serves the built React SPA via nginx and proxies `/api` to the backend.
- **backend** runs `prisma migrate deploy` on startup, then seeds when `RUN_SEED=true`.
- **db** persists to the `db_data` named volume.

## ⚙️ Configuration

Override defaults via environment variables on the `backend` service in
[`docker-compose.yml`](./docker-compose.yml) — notably `JWT_SECRET`, `DATABASE_URL`,
and `CORS_ORIGIN` for production use.
