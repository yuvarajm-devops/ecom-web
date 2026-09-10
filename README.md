# 🛒 E-Commerce Microservices App

Microservices-based architecture for an e-commerce app with CI/CD using Jenkins, Docker, and Git.
---

## 📌 Services
- 🔐 **Auth Service** → Port `3001`
- 📦 **Product Service** → Port `3002`
- 🛍️ **Order Service** → Port `3003`

---

## 🛠️ Technologies
- Node.js + Express
- Docker
- Jenkins
- GitHub

---

## 🚀 Run Locally

### 1. Build Docker Images
```bash
# Auth Service
docker build -t auth-service ./auth

# Product Service
docker build -t product-service ./product

# Order Service
docker build -t order-service ./order

```
# 2. Run Containers
```bash
# Auth Service (Port 3001)
docker run -p 3001:3000 --env SERVICE_NAME=auth-service auth-service

# Product Service (Port 3002)
docker run -p 3002:3000 --env SERVICE_NAME=product-service product-service

# Order Service (Port 3003)
docker run -p 3003:3000 --env SERVICE_NAME=order-service order-service
```
# 🌍 Access Services on AWS

### Replace <EC2_PUBLIC_IP> with your actual EC2 instance Public IP.

### Auth Service → http://<EC2_PUBLIC_IP>:3001

### Product Service → http://<EC2_PUBLIC_IP>:3002

### Order Service → http://<EC2_PUBLIC_IP>:3003
---

# 👉 Pro tip:
### make sure the **security group** for your EC2 instance allows **inbound traffic on ports 3001, 3002, 3003 (TCP)**. Otherwise, you won’t be able to access the services externally.  
# CI/CD test
# docker runner test
