# Deployment Guide

This document explains how to set up, deploy, and maintain the Todo Application using Docker and GitHub Actions.

## 1. Server Setup

To get the application running on your server, follow these steps:

### Prerequisites
- Docker and Docker Compose installed.
- Git installed.

### Steps
1. **Clone the repository**:
   ```bash
   git clone <your-repo-url>
   cd webstack-portfolio-project-todo-app
   ```
2. **Configure Environment Variables**:
   Check the `.env` file in the root directory. For a quick start, the defaults are set to `root` as per project requirements.
3. **Start the Stack**:
   ```bash
   docker compose up -d --build
   ```
   This will start the Database, Backend, Frontend, Nginx Reverse Proxy, and the Monitoring stack.

## 2. CI/CD Pipeline

The project uses GitHub Actions for continuous integration and delivery.

### How it Works
- **Trigger**: Every push or pull request to the `main` or `master` branches.
- **Build Stage**: 
    - Installs dependencies for both Frontend and Backend using `npm install` (to ensure version consistency).
    - Compiles the TypeScript code to verify there are no syntax or type errors.
- **Docker Stage**:
    - Builds the Docker images for both services to ensure they are container-ready.
    - Note: Currently, it only builds the images. To push them to a registry, you would need to add `push: true` and provide registry credentials in GitHub Secrets.

## 3. Nginx & Domain Setup

The system uses Nginx as an **API Gateway** and **Reverse Proxy**.

### Configuration Details
- **Port 80**: Nginx listens on the standard HTTP port.
- **Routing**:
    - `/api/` requests are forwarded to the Node.js backend (internal port 3000).
    - All other requests (`/`) are served by the React frontend.
- **Security**: Nginx is pre-configured with production-grade security headers:
    - Content Security Policy (CSP)
    - HSTS (Strict-Transport-Security)
    - X-Frame-Options (Clickjacking protection)
- **Domain Mapping**: To run this on a custom domain, simply point your domain's A record to the server's IP and update the `server_name` in `nginx/nginx.conf`.

### Accessing the Services

Once the containers are running, you can access the services at:

- **Application (Frontend + Backend)**: `http://localhost/`
- **Grafana (Metrics Dashboard)**: `http://localhost:3001/` (Login: `admin` / `root`)
- **Prometheus (Metrics Store)**: `http://localhost:9090/`
- **Dozzle (Real-time Logs)**: `http://localhost:8888/`

---

## 4. Troubleshooting

### Common Issues

**1. Port Conflict (Bind for 0.0.0.0:3306 failed)**
- **Cause**: Another MySQL instance is running on the host.
- **Solution**: The `docker-compose.yml` is already updated to use port `3307` on the host to avoid this. If you still see conflicts, change the host port in `docker-compose.yml`.

**2. Backend "Server Error" (500)**
- **Check Database**: Ensure the `db` container is healthy (`docker ps`).
- **Check Credentials**: Verify `DB_USER`, `DB_PASSWORD`, and `DB_NAME` in `.env` match the `MYSQL_...` variables.
- **Check Logs**: Use **Dozzle** at `http://localhost:8888` to see real-time logs for the `backend` container.

**3. Frontend cannot find Backend**
- **Symptom**: Requests to `/api/signup` fail or time out.
- **Solution**: Ensure Nginx is running. The frontend is configured to use `window.location.origin` as its base URL, so it *must* go through Nginx (port 80) to reach the backend.

**4. Monitoring not showing data**
- **Cause**: Containers still initializing.
- **Solution**: Give cAdvisor and Prometheus a few seconds to start scraping metrics. Check Grafana status at `http://localhost:3001`.
