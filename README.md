# Student Registration Application – Dockerized Deployment

A full-stack Student Registration Application built using **React, Spring Boot, and MariaDB**, deployed using Docker and Docker Compose.

The application uses three separate Docker containers for the frontend, backend, and database. Docker Compose manages the containers and their communication through a shared Docker network.

## Architecture

```text
              USER / WEB BROWSER
                      |
                      |
                HTTP Request
                      |
                      v
        +---------------------------+
        |   FRONTEND CONTAINER      |
        |                           |
        |   React + Vite + Nginx    |
        |                           |
        +---------------------------+
                      |
                      | REST API
                      v
        +---------------------------+
        |    BACKEND CONTAINER      |
        |                           |
        |   Java + Spring Boot      |
        |                           |
        +---------------------------+
                      |
                      | JDBC Connection
                      v
        +---------------------------+
        |   DATABASE CONTAINER      |
        |                           |
        |       MariaDB             |
        |                           |
        +---------------------------+
```

### Docker Containers

| Container | Technology | Responsibility |
|---|---|---|
| Frontend | React, Vite, Nginx | User interface |
| Backend | Java, Spring Boot | REST API and business logic |
| Database | MariaDB | Stores registration data |

## Technologies Used

### Frontend
- React.js
- Vite
- JavaScript
- HTML5
- CSS3
- Nginx

### Backend
- Java
- Spring Boot
- Spring Data JPA
- Hibernate
- Maven
- REST API

### Database
- MariaDB
- SQL

### DevOps and Deployment
- Docker
- Docker Compose
- Dockerfiles
- Docker Networking
- Environment Variables

## Project Structure

```text
student-registration-docker/
│
├── aws/
│   ├── backend-user-data.sh
│   └── frontend-user-data.sh
│
├── backend/
│   ├── src/
│   ├── pom.xml
│   ├── Dockerfile
│   └── .dockerignore
│
├── database/
│   └── Dockerfile
│
├── frontend/
│   ├── src/
│   ├── public/
│   ├── package.json
│   ├── Dockerfile
│   ├── nginx.conf
│   └── .dockerignore
│
├── .dockerignore
├── .env.example
├── .gitignore
├── docker-compose.yml
├── DOCKER_COMMANDS.md
└── README.md
```

## Application Workflow

1. A user opens the frontend application in a web browser.
2. The React frontend displays the student registration form.
3. The user enters registration details and submits the form.
4. The frontend sends an HTTP request to the Spring Boot backend.
5. The backend processes the request using REST APIs.
6. Spring Data JPA communicates with MariaDB.
7. The registration data is stored in the database.
8. The backend returns a response to the frontend.

## Prerequisites

Install the following tools before running the application:

- Docker
- Docker Compose
- Git

Verify the installations:

```bash
docker --version
docker compose version
git --version
```

## Clone the Repository

```bash
git clone https://github.com/anjalilambat/student-registration-docker.git

cd student-registration-docker
```

## Environment Configuration

Create your local environment file:

```bash
cp .env.example .env
```

Open `.env` and configure the database credentials and other variables required by your `docker-compose.yml`.

The backend uses these environment variables:

```env
DB_URL=jdbc:mariadb://database:3306/student_db
DB_USERNAME=studentapp
DB_PASSWORD=your_secure_password
```

Use the same database credentials in your Docker Compose database configuration.

**Important:** Never commit your actual `.env` file or database passwords to GitHub.

## Build and Run the Application

Build and start all three containers:

```bash
docker compose up -d --build
```

Check running containers:

```bash
docker compose ps
```

View container logs:

```bash
docker compose logs
```

View logs for individual services:

```bash
docker compose logs frontend
docker compose logs backend
docker compose logs database
```

## Access the Application

Open the frontend using the host port configured in `docker-compose.yml`.

Typical local development URLs, if configured with the standard port mappings:

| Service | URL |
|---|---|
| Frontend | http://localhost |
| Backend | http://localhost:8080 |
| MariaDB | localhost:3306 |

For remote EC2 deployment, replace `localhost` with your EC2 public IP or configured domain. Ensure the relevant security group rules are configured.

## Useful Docker Commands

**Start containers**

```bash
docker compose up -d
```

**Rebuild containers**

```bash
docker compose up -d --build
```

**Stop containers**

```bash
docker compose down
```

**List running containers**

```bash
docker ps
```

**View logs**

```bash
docker compose logs -f
```

**Restart a service**

```bash
docker compose restart backend
```

**Access the backend container**

```bash
docker compose exec backend sh
```

**Access the database container**

```bash
docker compose exec database sh
```

**Stop containers and remove volumes**

```bash
docker compose down -v
```

Warning: Removing volumes can permanently delete stored database data.

## Docker Networking

Docker Compose creates a network for communication between the containers.

- The frontend communicates with the backend through its configured API endpoint.
- The backend connects to MariaDB using the Docker service hostname `database`.
- The database does not need to be exposed publicly for backend communication.
- Services communicate internally using their Docker Compose service names and container ports.

## Security Best Practices

- Store credentials in environment variables.
- Keep `.env` out of Git.
- Use strong database passwords.
- Avoid exposing the database port publicly.
- Configure AWS Security Groups carefully.
- Use HTTPS when deploying publicly.
- Do not store passwords or secret keys inside Dockerfiles.

## Troubleshooting

**1. Check container status**

```bash
docker compose ps
```

**2. Check backend logs**

```bash
docker compose logs backend
```

**3. Check frontend logs**

```bash
docker compose logs frontend
```

**4. Check database logs**

```bash
docker compose logs database
```

**5. Rebuild the application**

```bash
docker compose down
docker compose up -d --build
```

**6. Check Docker networks**

```bash
docker network ls
```

## Future Improvements

- Deploy the application on AWS EC2.
- Configure HTTPS using an SSL certificate.
- Add CI/CD using GitHub Actions.
- Push Docker images to Amazon ECR.
- Implement monitoring and centralized logging.
- Use AWS RDS for managed database hosting.

## Author

**Anjali Lambat**

GitHub Repository:

[Student Registration Docker Project](https://github.com/anjalilambat/student-registration-docker)

---

⭐ If you find this project useful, consider giving the repository a star.
