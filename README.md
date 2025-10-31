# PostgreSQL + pgAdmin via Docker Compose

A minimal yet production-ready setup for running **PostgreSQL 18** and **pgAdmin 4** in Docker containers.  
Perfect for local development, testing, or small-scale deployments.

---

## 📦 Features
- PostgreSQL 18 (Alpine image, lightweight)
- pgAdmin 4 (v8+)
- Persistent storage via Docker volumes
- Auto health check for Postgres
- Isolated bridge network
- Optional auto-registration for pgAdmin servers
- Environment variable configuration via `.env`

---

## 🗂️ Project Structure
```
.
├── docker-compose.yml
├── .env.example
├── README.md
└── servers.json        # optional (auto-register server in pgAdmin)
```

---

## 🚀 Quick Start

### 1. Clone and prepare environment
```bash
git clone https://github.com/carry0987/Postgres-pgAdmin-Docker.git
cd Postgres-pgAdmin-Docker
cp .env.example .env
```

Then edit `.env` to suit your credentials.

---

### 2. Start services
```bash
docker compose up -d
```

This will spin up two containers:
- `pg-db` → PostgreSQL 18
- `pg-admin` → pgAdmin web interface (port `5050`)

---

### 3. Access pgAdmin
Open your browser at:

👉 http://localhost:5050  

Login using credentials defined in `.env`:
```
Email: admin@example.com
Password: changeme
```

---

### 4. Connect to PostgreSQL
When adding a new connection in pgAdmin:

| Field | Value |
|-------|--------|
| **Name** | Local DB |
| **Host name/address** | `db` |
| **Port** | `5432` |
| **Username** | value of `$POSTGRES_USER` |
| **Password** | value of `$POSTGRES_PASSWORD` |

---

## ⚙️ Configuration Notes

- **Persistent data** stored under Docker volumes:
  - `pgdata` → PostgreSQL database files  
  - `pgadmin_data` → pgAdmin configuration
- **Network isolation** via `pgnet` bridge.
- To **auto-register** PostgreSQL in pgAdmin, uncomment `servers.json` volume in `docker-compose.yml`.

---

## 🔒 Best Practices

- **Never commit `.env`** to version control. Only `.env.example` should be tracked.
- For production, use **Docker secrets** or external credential stores.
- Regularly back up your database:
  ```bash
  docker exec pg-db pg_dump -U $POSTGRES_USER -F c $POSTGRES_DB > backup.dump
  ```
- Use a strong password for both DB and pgAdmin.

---

## 🧹 Cleanup
To stop and remove containers and volumes:
```bash
docker compose down -v
```

---

## 🧠 Tips
- Change `ports` mapping if you have conflicts (e.g., `5433:5432`).
- Add a simple backup service or cron container for automated dumps.
- If you use this in CI/CD, you can pre-seed schemas or run migrations by adding init scripts to `/docker-entrypoint-initdb.d`.
