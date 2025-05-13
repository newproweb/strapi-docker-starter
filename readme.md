# 🚀 Strapi Docker Starter

A production- and development-ready Docker starter for Strapi 4+.

This starter helps you quickly containerize your Strapi backend with optional PostgreSQL and frontend services.

---

## 🧪 Features

- ✅ Multi-stage Dockerfile (dev / prod)
- ✅ Optional PostgreSQL database
- ✅ Optional frontend service integration
- ✅ Volume-based development with hot-reloading
- ✅ Remote Container support (VS Code)
- ✅ No need to install Node.js locally

---

## 📦 Quick Start

1. Click [Use this template](https://github.com/newproweb/strapi-docker-starter.git) to fork the repository.
2. Then clone your copy:

```bash
git clone https://github.com/newproweb/strapi-docker-starter.git
cd strapi-docker-starter
docker compose build
docker compose up
```

---

## 📂 Project Structure

```
strapi-docker-starter/
├── frontend/               # (Optional) Your frontend app
│   └── Dockerfile
├── server/                 # Strapi backend
│   └── Dockerfile
├── docker-compose.yml
└── README.md
```

---

## 🐳 Docker Compose

### 🔹 Option 1: Strapi Only (uses SQLite, no DB or frontend)

```yaml
version: "3.9"
services:
  strapi:
    build:
      context: ./server
      dockerfile: Dockerfile
    ports:
      - "1337:1337"
    volumes:
      - ./server:/app
      - /app/node_modules
    environment:
      - NODE_ENV=development
```

### 🔸 Option 2: Strapi + PostgreSQL + Frontend

```yaml
version: "3.9"
services:
  strapi:
    build:
      context: ./server
      dockerfile: Dockerfile
    ports:
      - "1337:1337"
    volumes:
      - ./server:/app
      - /app/node_modules
    environment:
      - DATABASE_CLIENT=postgres
      - DATABASE_NAME=strapi
      - DATABASE_HOST=db
      - DATABASE_PORT=5432
      - DATABASE_USERNAME=strapi
      - DATABASE_PASSWORD=strapi

  db:
    image: postgres:15
    restart: always
    environment:
      POSTGRES_USER: strapi
      POSTGRES_PASSWORD: strapi
      POSTGRES_DB: strapi
    volumes:
      - pgdata:/var/lib/postgresql/data

  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile
    ports:
      - "3000:3000"
    volumes:
      - ./frontend:/app

volumes:
  pgdata:
```

---

## ⚙️ Dockerfile (`server/Dockerfile`)

```dockerfile
# dev | prod
ARG RUN_MODE=dev

# === Dev Mode ===
FROM node:18 as dev
WORKDIR /app
ADD ./ /app
RUN npm install
ENTRYPOINT [ "npm", "run", "develop" ]

# === Prod Mode ===
FROM node:18 as prod
WORKDIR /app
ADD ./ /app
RUN npm install
ENTRYPOINT [ "npm", "start" ]

# Final stage
FROM ${RUN_MODE}
```

---

## 🚀 Running Your Application

Instead of using `npm run develop` or `yarn develop` commands directly, use Docker:

1. First build your containers:
```bash
docker compose build
```

2. Then start your application:
```bash
docker compose up
```

3. To run in detached mode (in background):
```bash
docker compose up -d
```

4. To view logs when running in detached mode:
```bash
docker compose logs -f
```

5. To stop your application:
```bash
docker compose down
```

---

## 🖥 Recommended VS Code Setup

Install the [Remote - Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack) extension to develop directly inside the container.

### Enable Auto Save

Open `settings.json` in VS Code and add:

```json
{
  "files.autoSave": "afterDelay",
  "files.autoSaveDelay": 1000
}
```

---

## 🧼 Clean Build Commands

```bash
docker compose down -v
docker compose build --no-cache
docker compose up
```

---

## 🧳 Environment Configuration

If you're using PostgreSQL, add a `.env` file in `server/` with:

```
DATABASE_CLIENT=postgres
DATABASE_HOST=db
DATABASE_PORT=5432
DATABASE_NAME=strapi
DATABASE_USERNAME=strapi
DATABASE_PASSWORD=strapi
```

---

## 🧾 License

MIT © [Aliev Davlatbek](https://github.com/newproweb)