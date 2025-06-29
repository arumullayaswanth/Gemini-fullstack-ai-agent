
# 🚀 Fullstack Gemini AI Agent Deployment Guide on AWS (Beginner-Friendly)

This guide will walk you through **step-by-step** to deploy the [Gemini Fullstack AI Agent](https://github.com/arumullayaswanth/Gemini-fullstack-ai-agent) on an **AWS EC2 instance** using **Docker**.

---

## 🧰 Prerequisites

| Requirement | Description |
|-------------|-------------|
| ✅ AWS Account | [Sign up here](https://aws.amazon.com) |
| ✅ GitHub Repo | Clone: `https://github.com/arumullayaswanth/Gemini-fullstack-ai-agent` |
| ✅ Gemini API Key | Get from [Google AI Studio](https://aistudio.google.com/app/apikey) |
| ✅ Terminal or CMD | To run SSH and commands |

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

## 🖥️ Step 2: Connect to EC2

1. Find **IPv4 Public IP** from EC2 dashboard
2. In terminal:

```bash
chmod 400 gemini-key.pem
ssh -i "gemini-key.pem" ubuntu@<YOUR_EC2_PUBLIC_IP>
```

✅ You are now connected to your cloud server.

---

## ⚙️ Step 3: Install Software (Docker + Git)

```bash
sudo apt update
sudo apt install -y docker.io docker-compose git screen
```

---

## 📁 Step 4: Clone Your Project

```bash
git clone https://github.com/arumullayaswanth/Gemini-fullstack-ai-agent.git
cd Gemini-fullstack-ai-agent
```

---

## 🔐 Step 5: Add Gemini API Key

```bash
cd backend
cp .env.example .env
nano .env
```

In `.env`, add:

```env
GEMINI_API_KEY=your_gemini_api_key_here
```

Save and exit (CTRL + X → Y → ENTER)

---

## 🐳 Step 6: Build & Run App in Docker

Go back to root folder:

```bash
cd ..
docker-compose up --build
```

Wait until it finishes. It may take a few minutes.

---

## 🌍 Step 7: Access Your Deployed App

Open your browser and go to:

```
http://<YOUR_EC2_PUBLIC_IP>:8123/app
```

You should see your working app.

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

## ✅ Summary of Commands

```bash
ssh -i "gemini-key.pem" ubuntu@<EC2_IP>
sudo apt update
sudo apt install -y docker.io docker-compose git screen

git clone https://github.com/arumullayaswanth/Gemini-fullstack-ai-agent.git
cd Gemini-fullstack-ai-agent

cd backend
cp .env.example .env
nano .env  # paste GEMINI_API_KEY
cd ..

docker-compose up --build
```

---

## ✅ Final Notes

- Your app runs at: `http://<EC2_IP>:8123/app`
- Make sure your firewall allows port `8123`
- To stop app: `docker-compose down`
- To restart: `docker-compose up --build`

---

## 🧠 Want More?

- Custom domain name? Use Route 53 or Cloudflare
- HTTPS (SSL)? Install NGINX + Certbot
- CI/CD Pipeline? Use GitHub Actions + ECR + ECS
- Monitoring? Use AWS CloudWatch

---

**Created with ❤️ for beginners**
