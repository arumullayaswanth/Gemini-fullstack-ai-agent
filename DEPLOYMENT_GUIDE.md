

# 🚀 Full Deployment Guide for Gemini Fullstack AI Agent on AWS


This guide will walk you through **step-by-step** to deploy the [Gemini Fullstack AI Agent](https://github.com/arumullayaswanth/Gemini-fullstack-ai-agent) on an **AWS EC2 instance** using **Docker**.

---

## 🧰 Prerequisites

| Requirement | Description |
|-------------|-------------|
| ✅ AWS Account | [Sign up here](https://aws.amazon.com) |
| ✅ GitHub Repo | Clone: `https://github.com/arumullayaswanth/Gemini-fullstack-ai-agent` |
| ✅ Gemini API Key | Get from [Google AI Studio](https://aistudio.google.com/app/apikey) |
| ✅ Terminal or CMD | To run SSH and commands |

## 🧰 Tools Required
- AWS EC2 (Ubuntu 22.04)
- Docker & Docker Compose
- GitHub
- Gemini API Key (from Google AI Studio)
- (Optional) LangSmith API Key

---

## 🧱 Step 1: Create EC2 Instance (Cloud Server)

1. **Log in** to AWS Console → Search for `EC2`
2. Click **“Launch Instance”**
3. Fill in:
   - **Name**: `gemini-ai-server`
   - **OS**: Ubuntu 22.04
   - **Instance Type**: `t2.micro` (Free tier)
   - **Key Pair**: Create new → name: `gemini-key` → download `.pem` file
   - **Firewall (Security Group)**: Add rules for ports: `22`, `8123`, `5432`, `6379`
4. Click **Launch Instance**

---


## 1️⃣ Set Up AWS EC2 Instance

### 1.1 Create an EC2 Instance
- Go to AWS Console → EC2 → Launch Instance
- Choose **Ubuntu 22.04**
- Name it e.g. `gemini-ai-server`
- Instance Type: `t2.micro` (free tier)
- Create or use an existing key pair (`.pem` file)
- Allow inbound ports:
  - `22` for SSH
  - `8123` for Backend API
  - `80` (optional for frontend)
  - `5433` (optional for PostgreSQL)
- Launch the instance

### 1.2 Connect via SSH
```bash
chmod 400 gemini-ai-server.pem
ssh -i "gemini-ai-server.pem" ubuntu@<YOUR_EC2_PUBLIC_IP>
```

---

## 2️⃣ Install Required Tools

```bash

sudo apt update && sudo apt upgrade -y
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
docker --version
sudo apt install docker-compose-plugin -y
docker compose version
sudo apt install git -y
git --version

```

---

## 3️⃣ Clone Your GitHub Repo

```bash
git clone https://github.com/arumullayaswanth/Gemini-fullstack-ai-agent.git
cd Gemini-fullstack-ai-agent
```

---

## 4️⃣ Create Environment Variables File

Create a `.env` file in the root directory:

```bash
cd backend
nano .env
```

Paste your keys:

```env
GEMINI_API_KEY=your_gemini_api_key
LANGSMITH_API_KEY=your_langsmith_api_key  # optional
```

Save and exit (`CTRL+O`, `Enter`, `CTRL+X`).

---

## 5️⃣ Run Docker Compose

Build and start containers:

```bash
cd ..
sudo docker-compose up --build
```
- Wait until it finishes. It may take a few minutes.
This will:
- Start Redis
- Start PostgreSQL
- Start your backend app on port 8123

---

## 6️⃣ Check the App

In your browser, go to:

```
http://<YOUR_EC2_PUBLIC_IP>:8123
```

You should see the backend running.

---

## 7️⃣ (Optional) Run Frontend

If you want to serve the React frontend too:

### Add this to `docker-compose.yml`:
```yaml
  frontend:
    build: ./frontend
    ports:
      - "80:3000"
    depends_on:
      - langgraph-api
```

Then rebuild and run:
```bash
sudo docker-compose up --build
```

Access it at:
- Now your app will be running at:
```
http://<YOUR_EC2_PUBLIC_IP>

http://<your-ec2-ip>:8123  ✅ if using 8123
http://<your-ec2-ip>       ✅ if using port 80

```



---

# Docker Container Verification Guide for Gemini Fullstack AI Agent

This guide helps you verify that your Docker containers are running correctly after deployment.

---

## 🚀 Step 1: Run Your Docker Compose

```bash
docker compose up --build
```

This command builds and starts all containers defined in your `docker-compose.yml`.

---

## ✅ Step 2: Verify Running Containers

### Show All Running Containers:

```bash
docker ps
```

**Expected Output Example:**

```
CONTAINER ID   IMAGE                           NAMES                  STATUS          PORTS
abc123456789   gemini-fullstack-langgraph      langgraph-api          Up 20 seconds   0.0.0.0:8123->8000/tcp
def234567890   redis:6                         langgraph-redis        Up 20 seconds   6379/tcp
ghi345678901   postgres:16                     langgraph-postgres     Up 20 seconds   5433->5432/tcp
jkl456789012   langgraph-frontend              langgraph-frontend     Up 20 seconds   0.0.0.0:80->3000/tcp
```

### Show All Containers (Running + Stopped):

```bash
docker ps -a
```

---

## 🧪 Step 3: Check Service Health and Status

```bash
docker compose ps
```

**Output Example:**

```
NAME                SERVICE             STATUS              PORTS
langgraph-api       langgraph-api       running (healthy)   8123/tcp
langgraph-frontend  frontend            running (healthy)   80/tcp
langgraph-redis     langgraph-redis     running             6379/tcp
langgraph-postgres  langgraph-postgres  running             5433->5432/tcp
```

---

## 📜 Step 4: View Logs for Each Container

### Backend (API):

```bash
docker logs langgraph-api
```

### Frontend:

```bash
docker logs langgraph-frontend
```

### Redis:

```bash
docker logs langgraph-redis
```

### Postgres:

```bash
docker logs langgraph-postgres
```

---

## 🐚 Step 5: Enter Container Shell (for Debugging)

### Example: Enter backend container:

```bash
docker exec -it langgraph-api /bin/sh
```

### Or with bash (if available):

```bash
docker exec -it langgraph-api bash
```

---

## 🌐 Step 6: Test in Browser

Assuming your EC2 public IP is `http://your-ec2-ip`:

* **Frontend:** [http://your-ec2-ip](http://your-ec2-ip)
* **Backend API:** [http://your-ec2-ip:8123](http://your-ec2-ip:8123)

---

## 📌 Notes

* Make sure ports 80 and 8123 are **open in EC2 security group**.
* Replace `your-ec2-ip` with the actual public IP of your AWS EC2 instance.
* Ensure environment variables like `GEMINI_API_KEY` and `LANGSMITH_API_KEY` are defined in a `.env` file or passed before running.

---

You're all set to monitor and debug your fullstack AI app using Docker Compose! ✅


---

## 🎯 Step 8 (Optional): Keep It Running After Exit

Install screen if not already:

```bash
sudo apt install screen
screen
```

Run app inside screen:

```bash
docker-compose up --build
```

To leave screen:  
`CTRL + A`, then press `D`

To come back later:

```bash
screen -r
```

---

## ✅ Summary

| Step | Task |
|------|------|
| 1 | Launch EC2 & open ports |
| 2 | SSH & install Docker |
| 3 | Clone repo |
| 4 | Create `.env` |
| 5 | Run app using Docker Compose |
| 6 | Visit app in browser |
| 7 | Add frontend (optional) |

---

## 🔑 Tips
- Your PostgreSQL DB is automatically created by Docker.
- You **don’t need** to manually create tables unless your code requires it.
- LangSmith API Key is optional (for tracing/debugging AI flows).

---

Made with ❤️ to help beginners succeed.
