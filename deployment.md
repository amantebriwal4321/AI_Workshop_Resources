# 🚀 Deployment Guide: Taking Your Chatbot Live

This document contains everything you need to deploy your chatbot to the internet using **Google Cloud Run** for free. You have two options: the **1-Click Auto-Deploy** (using AI), or the **Manual Step-by-Step** method.

> **💰 Is this free?** Yes! New Google Cloud accounts get **$300 in free credits for 90 days**. Even after that, Cloud Run has an **always-free tier** (2M requests/month). A workshop chatbot costs literally $0.

---

## ⚙️ Before You Start: Setting Up Google Cloud (One-Time Setup)

You **must** complete these steps before deploying. This only takes ~5 minutes.

### Step 1: Create a Google Cloud Account
1. Go to [console.cloud.google.com](https://console.cloud.google.com).
2. Sign in with your **Google account** (your regular Gmail works fine).
3. If this is your first time, you'll see a **"Try it free"** or **"Activate"** banner — click it.
4. Fill in your details:
   - **Country**: India
   - **Account type**: Individual
   - **Payment method**: Select **UPI: QR code** (easiest!)
   
   > ⚠️ **"Wait, I have to pay?!"** — NO! Google says it right on the page: *"Don't worry, this trial is still free."* They collect payment info **only to verify you're a real person**. You will **NOT** be charged. There are **no automatic charges** — you only pay if you manually upgrade later (which you won't need to).
   
5. Click **"Start free"**. A popup will appear:
   - **On your laptop**: You'll see a **QR code** with a 5-minute timer. Open any UPI app (GPay, PhonePe, Paytm, etc.) and scan it.
   - **On your phone**: Your UPI app will show an **"Autopay details"** screen. Don't panic! Here's what it says:
     - **Payment Limit**: Up to ₹15,000
     - **Note**: *"This isn't a charge"*
     - **You can pause or cancel this Autopay anytime**
   - Tap **"Continue"** → Approve with your UPI PIN.
   
   > 💡 **This is NOT a payment.** It's a UPI mandate (like saving a card). Google will **never charge you** unless you manually upgrade to a paid account. You can cancel the autopay from your UPI app at any time after setup.

6. Once approved, you'll be redirected to the Google Cloud Console. You now have **$300 in free credits** for 90 days! 🎉

### Step 2: Create a New Project
1. At the top-left of the console, click the **project dropdown** (it may say "My First Project" or "Select a project").
2. Click **"New Project"**.
3. Enter a project name (e.g., `my-chatbot-2026`). Keep it short and lowercase with hyphens.
4. Click **"Create"** and wait a few seconds.
5. Make sure your new project is **selected** in the dropdown at the top.

### Step 3: Note Your Project ID
1. Go to the [Dashboard](https://console.cloud.google.com/home/dashboard).
2. Look for your **Project ID** (it's under the project name — looks like `my-chatbot-2026` or `my-chatbot-2026-a1b2c3`).
3. **Copy it somewhere** — you'll need this during deployment.

> **✅ Done!** That's all the setup you need. Now pick your deployment method below.

---

## Option 1: The "Auto-Deploy" Method (Recommended) ⭐

Since you are already using Antigravity (or Claude/Gemini) as your AI coding assistant, you can simply command it to do the entire deployment for you!

### What does the prompt do?
The prompt below tells your AI assistant to handle **everything** automatically:
- ✅ Creates the required deployment files (`Procfile`, `requirements.txt`)
- ✅ Checks and installs the Google Cloud CLI on your computer
- ✅ Logs you in and connects to your project
- ✅ Enables all required cloud services
- ✅ Deploys your chatbot to a live URL
- ✅ Securely injects your API key so the chatbot actually works

### What do YOU need to do first?
1. ✅ Complete the **"Before You Start"** section above (Google Cloud account + project)
2. ✅ Have your chatbot project **open** in VS Code / your editor
3. ✅ Have your **Project ID** ready (from Step 3 above)
4. That's it — paste the prompt and follow along!

### 📋 Copy and Paste this Prompt to your AI Assistant:

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

## Option 2: The Manual Step-by-Step Method

If you want to understand exactly how the magic works, follow these steps manually.

### 🛠️ Phase 1: Preparing the "Blueprint"
Google Cloud needs an instruction manual to turn your chatbot on.
1. Make sure you have a **`requirements.txt`** in your project root. If not, run `pip freeze > requirements.txt` in your terminal.
2. Create a New File named exactly **`Procfile`** (Capital 'P', no extension).
3. Paste this exact line inside: `web: uvicorn main:app --host 0.0.0.0 --port $PORT`
4. **Save the file!** (Ctrl+S)

### ☁️ Phase 2: Claiming Your Free Google Cloud Land
*(If you already completed the "Before You Start" section above, skip to Phase 3!)*
1. Go to [console.cloud.google.com](https://console.cloud.google.com) and sign in.
2. Activate the **Free Trial** banner at the top if you see it.
3. Create a **New Project** and name it something unique (e.g., `my-chatbot-2026`).

### 🔌 Phase 3: Connecting Your Computer
1. Open Windows PowerShell as an Administrator and run:
   ```powershell
   (New-Object Net.WebClient).DownloadFile("https://dl.google.com/dl/cloudsdk/channels/rapid/GoogleCloudSDKInstaller.exe", "$env:Temp\GoogleCloudSDKInstaller.exe"); & $env:Temp\GoogleCloudSDKInstaller.exe
   ```
2. Click through the installer. **When finished, you MUST close VS Code completely and reopen it.**

### 🚀 Phase 4: The Magic Launch
1. Open a terminal in VS Code and log in:
   ```bash
   gcloud auth login
   ```
2. Connect to your project:
   ```bash
   gcloud config set project YOUR-PROJECT-ID-HERE
   ```
3. Enable the required services:
   ```bash
   gcloud services enable run.googleapis.com cloudbuild.googleapis.com artifactregistry.googleapis.com
   ```
4. Deploy the code:
   ```bash
   gcloud run deploy autoexpert-bot --source . --region asia-south1 --allow-unauthenticated
   ```
   *(This will take 2-5 minutes. Grab some chai ☕)*

### 🔑 Phase 5: Fixing the 403 API Error
Your site is live, but it doesn't have your Gemini API key yet! Run this command to securely inject it:
```bash
gcloud run services update autoexpert-bot --update-env-vars GEMINI_API_KEY=YOUR_ACTUAL_API_KEY_HERE --region asia-south1
```

Refresh your live URL, and you are officially done! 🎉
