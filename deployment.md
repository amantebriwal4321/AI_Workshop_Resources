# 🚀 Deployment Guide: Taking Your Chatbot Live

This guide will help you deploy your chatbot to the internet **for free**. We have two methods:

| Method | Platform | Payment Required? | Difficulty | Best For |
|--------|----------|:-:|:-:|----------|
| **Option 1** ⭐ | **Render** | ❌ No card/UPI | Easy | Everyone |
| **Option 2** | **Google Cloud Run** | ✅ Card or UPI | Medium | Advanced users |

> **💰 Both options are 100% free.** Render requires no payment info at all. Google Cloud gives $300 free credits but needs payment verification.

---

## Option 1: Deploy with Render (Recommended) ⭐

**Why Render?** It's free, requires **zero payment info**, and works perfectly with your chatbot. Just connect your GitHub and click deploy.

> ⚠️ **One catch**: The free tier sleeps after 15 min of inactivity. The first visit after sleeping takes ~30 seconds to load. After that, it's instant.

---

### ⚙️ Before You Start (One-Time Setup)

#### Step 1: Push Your Project to GitHub
If your chatbot is **not** already on GitHub, do this:
1. Go to [github.com/new](https://github.com/new) and create a **new repository** (e.g., `my-car-chatbot`).
2. Set it to **Private** (so your code isn't public).
3. Open a terminal in your chatbot project folder and run:
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
   git branch -M main
   git push -u origin main
   ```

#### Step 2: Make Sure These Files Exist
Your project needs these two files to deploy:

1. **`requirements.txt`** — Lists your Python packages. If you don't have one, run:
   ```bash
   pip freeze > requirements.txt
   ```

2. **`Procfile`** (Capital P, no extension) — Tells Render how to start your app. Create it with this exact line:
   ```
   web: uvicorn main:app --host 0.0.0.0 --port $PORT
   ```

---

### 🚀 Method A: Auto-Deploy with AI (Paste this Prompt)

Open your chatbot project in your AI assistant and paste:

```text
I want to deploy this chatbot to the internet using Render.com (free tier). Please help me step-by-step:

1. PREP CHECK: Make sure I have a `requirements.txt` and a `Procfile` in this project. If not, create them. The Procfile should contain: `web: uvicorn main:app --host 0.0.0.0 --port $PORT`
2. SAFETY CHECK: Make sure `.gitignore` includes `.env` so my API key doesn't get pushed to GitHub.
3. GIT PUSH: Help me push this project to GitHub if it's not already there. Ask me for my GitHub repo URL.
4. RENDER SETUP: Walk me through creating a free account on render.com, connecting my GitHub repo, and deploying. Tell me exactly what to click.
5. ENVIRONMENT VARIABLE: Once deployed, tell me how to add my GEMINI_API_KEY as an environment variable in Render's dashboard (Settings → Environment → Add Environment Variable).
6. FINAL TEST: Give me the live URL and tell me to test it!
```

---

### 🛠️ Method B: Manual Step-by-Step

#### Phase 1: Create a Render Account
1. Go to [render.com](https://render.com) and click **"Get Started for Free"**.
2. Click **"Sign in with GitHub"** (easiest — links your repos automatically).
3. Authorize Render to access your GitHub. **Done!** No payment info needed.

#### Phase 2: Create a New Web Service
1. In the Render dashboard, click **"New +"** → **"Web Service"**.
2. Connect your **GitHub repository** (the one with your chatbot code).
   - If you don't see your repo, click **"Configure account"** to grant Render access.
3. Fill in the settings:
   - **Name**: `autoexpert-bot` (or whatever you want)
   - **Region**: `Singapore` (closest to India)
   - **Branch**: `main`
   - **Runtime**: `Python 3`
   - **Build Command**: `pip install -r requirements.txt`
   - **Start Command**: `uvicorn main:app --host 0.0.0.0 --port $PORT`
   - **Instance Type**: Select **Free** 
4. Click **"Deploy Web Service"**. Wait 2-3 minutes for the build to finish.

#### Phase 3: Add Your API Key
Your chatbot is live but broken — it needs the Gemini API key!
1. In your Render service dashboard, go to **"Environment"** (left sidebar).
2. Click **"Add Environment Variable"**.
3. Set:
   - **Key**: `GEMINI_API_KEY`
   - **Value**: *(paste your actual API key from your `.env` file)*
4. Click **"Save Changes"**. Render will automatically redeploy.

#### Phase 4: You're Live! 🎉
Your chatbot URL will be: `https://autoexpert-bot.onrender.com` (or whatever name you chose).

Click it, test it, share it! You just deployed your AI chatbot to the internet! 🚀

---
---

## Option 2: Deploy with Google Cloud Run (Alternative)

> ⚠️ **Requires payment verification** (UPI or debit/credit card). You will NOT be charged — Google uses it only for identity verification. You get **$300 in free credits for 90 days**.

Use this option if you already have a Google Cloud account with billing set up, or if you want a more "production-grade" deployment that doesn't sleep.

### ⚙️ Before You Start: Setting Up Google Cloud

#### Step 1: Create a Google Cloud Account
1. Go to [console.cloud.google.com](https://console.cloud.google.com).
2. Sign in with your **Google account** (Gmail works fine).
3. Click **"Try it free"** or **"Activate"** banner.
4. Fill in your details:
   - **Country**: India
   - **Account type**: Individual
   - **Payment method**: Select **UPI: QR code** (easiest!) or debit/credit card

   > ⚠️ **"Wait, I have to pay?!"** — NO! Google says it right on the page: *"Don't worry, this trial is still free."* They collect payment info **only to verify you're a real person**. There are **no automatic charges**.

5. Click **"Start free"**. A popup will appear:
   - **On your laptop**: You'll see a **QR code** with a 5-minute timer. Scan it with any UPI app (GPay, PhonePe, Paytm, etc.).
   - **On your phone**: Your UPI app will show an **"Autopay details"** screen:
     - **Payment Limit**: Up to ₹15,000
     - **Note**: *"This isn't a charge"*
     - **You can pause or cancel this Autopay anytime**
   - Tap **"Continue"** → Approve with your UPI PIN.

   > 💡 **This is NOT a payment.** It's a UPI mandate (like saving a card). Google will **never charge you** unless you manually upgrade. You can cancel the autopay from your UPI app anytime.

6. You now have **$300 in free credits** for 90 days! 🎉

#### Step 2: Create a New Project
1. Click the **project dropdown** at the top-left → **"New Project"**.
2. Name it (e.g., `my-chatbot-2026`) → Click **"Create"**.
3. Make sure it's **selected** in the dropdown.

#### Step 3: Copy Your Project ID
1. Go to the [Dashboard](https://console.cloud.google.com/home/dashboard).
2. Copy your **Project ID** (under the project name). You'll need this later.

---

### 🚀 Auto-Deploy Prompt (for AI Assistant)

Open your chatbot project in your AI assistant and paste:

```text
I am ready to deploy this chatbot to the internet using Google Cloud Run. Please execute this entire process for me step-by-step. Do NOT skip any step.

1. DEPENDENCIES FILE: Check if a `requirements.txt` file exists in this project. If it does NOT exist, generate one by running `pip freeze > requirements.txt`. This file is critical — Cloud Run uses it to install all Python packages.
2. THE BLUEPRINT: Create a `Procfile` in this root directory containing exactly this line: `web: uvicorn main:app --host 0.0.0.0 --port $PORT`. Save the file.
3. SAFETY CHECK: Make sure a `.gitignore` file exists and that it includes `.env` so my API keys never get uploaded to GitHub.
4. CLI CHECK: Check if the `gcloud` CLI is installed on my system by running `gcloud --version`. If it is not installed, give me the exact terminal command to install it for Windows, and PAUSE. Tell me to restart my editor after installing.
5. AUTHENTICATION: Once `gcloud` is installed, check if I am logged in by running `gcloud auth list`. If not logged in, run `gcloud auth login`.
6. PROJECT SETUP: Ask me for my Google Cloud Project ID. Once I provide it, run `gcloud config set project [MY_PROJECT_ID]`.
7. ENABLE APIS: Run this command to enable the required services:
   `gcloud services enable run.googleapis.com cloudbuild.googleapis.com artifactregistry.googleapis.com`
8. THE LAUNCH: Run the deployment command. Use these exact flags so it doesn't get stuck asking me interactive questions: 
   `gcloud run deploy autoexpert-bot --source . --region asia-south1 --allow-unauthenticated`
   This will take 2-5 minutes. Wait for it to complete.
9. INJECTING SECRETS (CRITICAL): Once the deployment is successful, the site will be live but broken (403 error) because it's missing the API key. Automatically read my `GEMINI_API_KEY` from my local `.env` file and securely inject it into the live server using this command:
   `gcloud run services update autoexpert-bot --update-env-vars GEMINI_API_KEY=[THE_KEY_YOU_FOUND] --region asia-south1`
10. THE RESULT: Provide me with the final, live URL and open it in my browser so I can test my chatbot!
```

---

### 🛠️ Manual Google Cloud Run Steps

#### Phase 1: Prepare Your Files
1. Make sure **`requirements.txt`** exists. If not: `pip freeze > requirements.txt`
2. Make sure **`Procfile`** exists with: `web: uvicorn main:app --host 0.0.0.0 --port $PORT`

#### Phase 2: Install & Connect Google Cloud CLI
1. Open PowerShell as Admin and run:
   ```powershell
   (New-Object Net.WebClient).DownloadFile("https://dl.google.com/dl/cloudsdk/channels/rapid/GoogleCloudSDKInstaller.exe", "$env:Temp\GoogleCloudSDKInstaller.exe"); & $env:Temp\GoogleCloudSDKInstaller.exe
   ```
2. **Close VS Code completely and reopen it** after installing.

#### Phase 3: Deploy
1. Log in:
   ```bash
   gcloud auth login
   ```
2. Set your project:
   ```bash
   gcloud config set project YOUR-PROJECT-ID-HERE
   ```
3. Enable services:
   ```bash
   gcloud services enable run.googleapis.com cloudbuild.googleapis.com artifactregistry.googleapis.com
   ```
4. Deploy:
   ```bash
   gcloud run deploy autoexpert-bot --source . --region asia-south1 --allow-unauthenticated
   ```
   *(2-5 minutes. Grab some chai ☕)*

#### Phase 4: Add Your API Key
```bash
gcloud run services update autoexpert-bot --update-env-vars GEMINI_API_KEY=YOUR_ACTUAL_API_KEY_HERE --region asia-south1
```

Refresh your live URL, and you're done! 🎉
