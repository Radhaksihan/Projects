# Blue-Green Deployment Mini Project

## Docker + Nginx (KodeKloud Playground)

---

## 📘 Project Overview

This project demonstrates a simplified **Blue-Green Deployment strategy** using Docker containers and Nginx as a traffic router.

Two identical environments run simultaneously:

* **Blue Environment** — Current production version
* **Green Environment** — New version ready for release

Traffic switching between versions is handled by Nginx, enabling:

* Zero downtime deployment
* Safe validation before release
* Instant rollback capability

This simulation reflects how production CI/CD pipelines manage risk during deployments.

---

## 🧭 Architecture Concept

```
User Request
     │
     ▼
 Nginx Reverse Proxy
     │
 ┌───────────────┐
 │               │
 ▼               ▼
Blue Container   Green Container
(port 8081)      (port 8082)
```

---

## 🛠 Technologies Used

* Docker
* Nginx
* Linux (Ubuntu)
* Bash scripting

---

# 🚀 Implementation Steps

---

## Step 1 — Install Docker

Docker is used to build and run isolated application versions.

```bash
apt update
apt install -y docker.io
systemctl start docker
systemctl enable docker
docker run hello-world
```

---

## Step 2 — Create Project Workspace

A dedicated working directory keeps project artifacts organized.

```bash
mkdir ~/bluegreen-mini
cd ~/bluegreen-mini
```

---

## Step 3 — Build BLUE Version

This version represents the current production deployment.

### Create Web Page

```bash
echo "<h1>BLUE VERSION — Deployed via Docker</h1>" > index.html
```

### Dockerfile

```bash
cat <<EOF > Dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EOF
```

### Build Image

```bash
docker build -t blue-app .
```

### Run Container

```bash
docker run -d -p 8081:80 --name app-blue blue-app
```

### Verify Deployment

```bash
curl localhost:8081
```

---

## Step 4 — Build GREEN Version

This simulates a new application release deployed alongside Blue.

```bash
echo "<h1>GREEN VERSION — New Release</h1>" > index.html
docker build -t green-app .
docker run -d -p 8082:80 --name app-green green-app
curl localhost:8082
```

---

## Step 5 — Configure Nginx Traffic Routing

Nginx acts as a reverse proxy directing user requests.

### Install Nginx

```bash
apt install -y nginx
```

### Configure Routing to BLUE

```bash
cat <<EOF > /etc/nginx/sites-enabled/default
server {
    listen 80;

    location / {
        proxy_pass http://localhost:8081;
    }
}
EOF
```

### Apply Configuration

```bash
systemctl restart nginx
curl localhost
```

At this stage, users are routed to the Blue environment.

---

## Step 6 — Switch Traffic to GREEN

Modify routing target and reload Nginx.

```bash
sed -i 's/8081/8082/' /etc/nginx/sites-enabled/default
systemctl reload nginx
curl localhost
```

Traffic now flows to the Green environment without stopping containers.

---

## Step 7 — Rollback Simulation

Restore routing to Blue if issues are detected.

```bash
sed -i 's/8082/8081/' /etc/nginx/sites-enabled/default
systemctl reload nginx
curl localhost
```

Rollback completes instantly.

---

## Step 8 — Cleanup (Optional)

```bash
docker rm -f app-blue app-green
docker rmi blue-app green-app
systemctl stop nginx
```

---

# 🎯 Learning Outcomes

* Building container images
* Running parallel deployment environments
* Reverse proxy routing
* Zero-downtime release strategy
* Instant rollback execution
* Practical DevOps deployment thinking

-
---
