# Medical Chatbot Deployment Flow

## Phase 1: Develop the Project

### Step 1: Create Environment

```bash
conda create -n medibot python=3.10 -y
conda activate medibot
```

### Step 2: Install Libraries

```bash
pip install -r requirements.txt
```

### Step 3: Add API Keys

Create `.env`

```env
PINECONE_API_KEY=xxxxxxxx
GEMINI_API_KEY=xxxxxxxx
```

### Step 4: Run Locally

```bash
python store_index.py
python app.py
```

Open:

```text
http://localhost:8080
```

If it works locally, move to deployment.

---

# Phase 2: Dockerize the Project

## Why Docker?

Problem:

```text
My laptop works
Client machine fails
AWS server fails
```

Solution:

```text
Docker creates the same environment everywhere.
```

### Dockerfile

Docker creates an image containing:

```text
Python
Libraries
Project Code
```

Build image:

```bash
docker build -t healthmatebot .
```

Run image:

```bash
docker run -p 8080:8080 healthmatebot
```

Flow:

```text
Project
   ↓
Dockerfile
   ↓
Docker Image
```

---

# Phase 3: Store Docker Image in AWS

## Why ECR?

ECR = Docker Hub of AWS

Instead of:

```text
Laptop
```

Store image in:

```text
AWS ECR
```

Flow:

```text
Docker Image
      ↓
AWS ECR Repository
```

Example:

```text
healthmatebot:latest
```

---

# Phase 4: Create AWS Server

## EC2

EC2 = Virtual Computer in AWS

Example:

```text
Windows Laptop
        ↓
AWS Ubuntu Server
```

Install Docker on EC2:

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

Flow:

```text
EC2
   ↓
Docker Installed
```

---

# Phase 5: CI/CD Automation

## Without CI/CD

Every change:

```text
Code Change
    ↓
Build Docker
    ↓
Push to ECR
    ↓
Login EC2
    ↓
Pull Image
    ↓
Run Container
```

Manual work every time.

---

## With CI/CD

Push code:

```bash
git push origin main
```

Everything happens automatically.

Flow:

```text
GitHub Push
      ↓
GitHub Actions
      ↓
Build Docker Image
      ↓
Push to ECR
      ↓
EC2 Pulls Latest Image
      ↓
Container Restarts
      ↓
Application Updated
```

---

# Phase 6: GitHub Secrets

Never store keys inside code.

Store in:

```text
GitHub
   ↓
Settings
   ↓
Secrets
```

Add:

```text
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
AWS_DEFAULT_REGION
ECR_REPO
PINECONE_API_KEY
GEMINI_API_KEY
```

---

# Complete Deployment Architecture

Developer
↓
GitHub
↓
GitHub Actions (CI/CD)
↓
Build Docker Image
↓
Push to AWS ECR
↓
EC2 Server
↓
Docker Container
↓
Flask Medical Chatbot
↓
Users Access Website

---

# Reusable Flow For Any Future Project

Resume Analyzer
Restaurant Chatbot
Manufacturing Chatbot
Medical Chatbot
Business Chatbot

All use the same deployment flow:

Project
↓
Docker
↓
ECR
↓
EC2
↓
GitHub Actions
↓
Live Website

