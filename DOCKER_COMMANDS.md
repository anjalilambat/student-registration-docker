# Docker Commands — Student Registration

## Verify Docker installation

```bash
docker --version
docker compose version
sudo systemctl status docker
```

## Build images individually

```bash
docker build -t student_db:latest ./database
docker build -t student-backend:latest ./backend
docker build --build-arg VITE_API_URL=/api -t student-frontend:latest ./frontend
docker images
```

## Start with Docker Compose

```bash
cp .env.example .env
nano .env
docker compose config
docker compose up --build -d
docker compose ps
```

## Inspect containers and images

```bash
docker ps
docker ps -a
docker images
docker inspect student-db
docker logs student-backend
docker logs student-frontend
docker compose logs -f
```

## Execute commands inside containers

```bash
docker exec -it student-db bash
mariadb -u studentapp -p student_db
```

Or use Compose:

```bash
docker compose exec database bash
docker compose exec backend sh
docker compose exec frontend sh
```

## Rebuild and stop

```bash
docker compose up --build -d
docker compose down
```

**Destructive command:** this removes the named database volume and its stored data:

```bash
docker compose down -v
```

## Service ports

| Service | Container port | Host port |
|---|---:|---:|
| Frontend / Nginx | 80 | 80 |
| Spring Boot | 8080 | 8080 |
| MariaDB | 3306 | 127.0.0.1:3306 |

Keep MariaDB private. Do not expose port 3306 to the public internet.
