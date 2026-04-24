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
   git clone https://github.com/tekleab/webstack-portfolio-project-todo-app.git
   cd webstack-portfolio-project-todo-app
   ```
2. **Configure Environment Variables**:
   Check the `.env` file in the root directory. For a quick start, the defaults are set to `root`.
3. **Start the Stack**:
   ```bash
   docker compose up -d --build
   ```

## 2. CI/CD Pipeline

The project uses GitHub Actions for continuous integration and delivery.

### How it Works
- **Trigger**: Every push or pull request to the `main` branches.
- **Build Stage**: 
    - Installs dependencies for both Frontend and Backend using `npm install` (to ensure version consistency).
    - Compiles the TypeScript code to verify there are no syntax or type errors.
- **Docker Stage**:
    - Builds the Docker images for both services to ensure they are container-ready.
    - it builds the images and push them to a registry and registry credentials is set in GitHub Secrets.

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

### Accessing the Services

Once the containers are running, you can access the services at:

- **Application (Frontend)**: `http://196.188.187.153:8080/`
- **Application (Backend)**: `http://196.188.187.153:8080/api/`
- **Grafana (Metrics Dashboard)**: `http://196.188.187.153:8080/grafana/` (Login: `admin` / `root`)`
- **Dozzle (Real-time Logs)**: `http://196.188.187.153:8080/dozzle/`

---
