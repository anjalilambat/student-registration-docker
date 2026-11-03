# Dockerized Student Registration Application

A Docker deployment setup for a student registration application using a Spring Boot backend, a Vite frontend served by Nginx, and MariaDB.

> **Source-code note:** This repository currently contains Docker and deployment configuration. The original Spring Boot and Vite application source must be added to `backend/` and `frontend/` before building the images. Keep your existing application source; do not overwrite it with configuration-only folders.

## Architecture

```text
Browser --HTTP:80--> Nginx frontend --/api--> Spring Boot backend:8080
                                               |
                                               v
                                         MariaDB:3306
```

## Repository structure

```text
.
├── README.md
├── DOCKER_COMMANDS.md
├── docker-compose.yml
├── .env.example
├── .gitignore
├── .dockerignore
├── backend/
│   ├── Dockerfile
│   ├── .dockerignore
│   └── src/main/resources/application.properties
├── frontend/
│   ├── Dockerfile
│   ├── .dockerignore
│   └── nginx.conf
└── database/
    └── Dockerfile
```

## Requirements

- Ubuntu Linux / Ubuntu EC2 instance
- Docker Engine and Docker Compose plugin
- Backend source with Maven `pom.xml` and `src/`
- Frontend source with `package.json`, lock file, `src/`, and `index.html`

## Step 1 — Configure environment

Create your local environment file and replace the sample passwords:

```bash
cp .env.example .env
nano .env
```

Never commit `.env`, PEM keys, private keys, access tokens, or credentials. The root `.gitignore` excludes them.

## Step 2 — Build and start the application

Run these commands from the repository root after adding the original application source:

```bash
docker compose config
docker compose up --build -d
docker compose ps
```

Open `http://YOUR_EC2_PUBLIC_IP/` in a browser. Allow inbound TCP port 80 in the EC2 security group for intended clients.

## Services and ports

| Component | Container port | Host binding |
|---|---:|---:|
| Frontend / Nginx | 80 | 80 |
| Spring Boot backend | 8080 | 8080 |
| MariaDB | 3306 | 127.0.0.1:3306 |

MariaDB is bound to localhost on the EC2 host. Do not expose port 3306 publicly. Restrict port 8080 if direct backend access is not required.

## Step 3 — Useful Docker commands

```bash
# View containers
docker compose ps
docker ps -a

# Follow logs
docker compose logs -f
docker compose logs -f backend
docker compose logs -f frontend
docker compose logs -f database

# Rebuild after source/config changes
docker compose up --build -d

# Stop containers but keep the database volume
docker compose down

# WARNING: also deletes persistent database data
docker compose down -v
```

See [DOCKER_COMMANDS.md](DOCKER_COMMANDS.md) for the complete command reference.

## Step 4 — Access MariaDB

```bash
docker compose exec database mariadb -u studentapp -p student_db
```

Enter the application database password configured in your local `.env` file.

## Step 5 — Publish images to Docker Hub

Replace `YOUR_DOCKERHUB_USERNAME` with your Docker Hub username:

```bash
docker login
docker tag student-db:local YOUR_DOCKERHUB_USERNAME/student-db:v1
docker tag student-backend:local YOUR_DOCKERHUB_USERNAME/student-backend:v1
docker tag student-frontend:local YOUR_DOCKERHUB_USERNAME/student-frontend:v1
docker push YOUR_DOCKERHUB_USERNAME/student-db:v1
docker push YOUR_DOCKERHUB_USERNAME/student-backend:v1
docker push YOUR_DOCKERHUB_USERNAME/student-frontend:v1
```

## Security checklist

- Keep `.env`, PEM keys, SSH private keys, and tokens out of Git.
- Use strong, unique database passwords.
- Do not expose MariaDB port 3306 to the internet.
- Restrict SSH (22) to trusted IP addresses.
- Use HTTPS for production.
- Use a secrets manager for production credentials.

## Troubleshooting

```bash
docker compose ps
docker compose logs backend
docker compose logs database
docker compose logs frontend
docker inspect student-backend
```

If the backend cannot connect to MariaDB, verify that the database health check passes and that the backend uses the Compose hostname `database`, not `localhost`.

If frontend API requests fail, verify that the frontend uses the relative API path `/api` and Nginx proxies `/api/` to `backend:8080`.

## Deployment workflow

1. Add the original backend and frontend source folders.
2. Configure local environment variables in `.env`.
3. Validate configuration with `docker compose config`.
4. Build and start with `docker compose up --build -d`.
5. Check container health and logs.
6. Test registration through the browser.
7. Publish versioned images if required.
