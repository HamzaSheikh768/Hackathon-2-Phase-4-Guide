
### CODEVERSE SOBAN
# 🚀 Next.js & FastAPI Applications → Docker Image → Docker Hub (Step by Step)

In this Guide:
1. Create a Next.js application
2. Dockerize a Next.js application
3. Create a FastAPI backend
4. Dockerize a FastAPI application
5. Push Docker images to Docker Hub
6. Run both applications using Docker

---

## ✅ Prerequisites
- Node.js installed
- Python 3.9+ installed
- Docker Desktop installed and running
- Docker Hub account

---

# 🌐 PART 1: Next.js Application with Docker

## 📁 Step 1: Create a Next.js Application

```bash
npx create-next-app@latest nextjs-app
cd nextjs-app
```

Run locally:

```bash
npm run dev
```

Open in browser:
```
http://localhost:3000
```

---

## 🐳 Step 2: Create Dockerfile for Next.js

```dockerfile
FROM node:20-alpine

# Set the working directory
WORKDIR /src

# Copy package.json and package-lock.json files
COPY package*.json ./

# Install only production dependencies
RUN npm install --only=production

# Install development dependencies for building the app
RUN npm install

# Copy the rest of the application code
COPY . .

# ensure environment variables are available
COPY .env .env

# Build the Next.js application
RUN npm run build

# Expose the port the app runs on
EXPOSE 3000

# Set environment variables for production
ENV NODE_ENV=production

# Start the Next.js application in production mode
CMD ["npm", "start"]
```

---

## 🔐 Step 3: Login to Docker Hub

```bash
docker login
```

---

## 🏗️ Step 4: Build Next.js Docker Image

```bash
docker build -t sheikhhamza02/todo_frontend .
```

---

## ▶️ Step 5: Run Next.js Container

```bash
docker run -p 3000:3000 sheikhhamza02/todo_frontend
```

---

## ⬆️ Step 6: Push Next.js Image to Docker Hub

```bash
docker push sheikhhamza02/todo_frontend
```

---

# ⚡ PART 2: FastAPI Application with Docker

## 📁 Step 1: Create FastAPI Project

```bash
mkdir fastapi-app
cd fastapi-app
```

Create `main.py`:

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"message": "Hello from FastAPI with Docker"}
```

Create `requirements.txt`:

```txt
fastapi
uvicorn
```

---

## ▶️ Step 2: Run FastAPI Locally (Optional)

```bash
pip install -r requirements.txt
uvicorn main:app --reload
```

Open in browser:
```
http://localhost:8000
```

---

## 🐳 Step 3: Create Dockerfile for FastAPI

```dockerfile
# Use Python 3.12 slim image as base
# Use the official Python runtime as a parent image
FROM python:3.12-slim

# Set the working directory inside the container
WORKDIR /src

# Install system dependencies
RUN apt-get update && apt-get install -y \
    gcc \
    && rm -rf /var/lib/apt/lists/*

# Copy requirements.txt first to leverage Docker cache
COPY requirements.txt .

# Copy environment variables file
COPY .env .env

# Install Python dependencies
RUN pip install --no-cache-dir --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt

# Copy the application code to the working directory
COPY . .

# Expose the port that the app runs on
EXPOSE 8000

# Define the command to run the application
CMD ["python", "-m", "uvicorn", "src.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

---

## 🏗️ Step 4: Build FastAPI Docker Image

```bash
docker build -t sheikhhamza02/todo_backend .
```

---

## ▶️ Step 5: Run FastAPI Container

```bash
docker run -p 8000:8000 sheikhhamza02/todo_backend
```

---

## ⬆️ Step 6: Push FastAPI Image to Docker Hub

```bash
docker push sheikhhamza021/todo_backend
```

---

# 🔗 OPTIONAL: Run Both Containers Together

```bash
docker run -d -p 3000:3000 sheikhhamza02/todo_website
docker run -d -p 8000:8000 sheikhhamza02/todo_backend
```

---

## 🎉 Done!
Your **Next.js Frontend & FastAPI Backend** are now running using Docker 🚀

---
