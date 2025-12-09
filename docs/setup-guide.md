# 📋 Complete Setup Guide

This guide will walk you through setting up the entire AI Personal Assistant system from scratch.

---

## 📖 Table of Contents

1. [Prerequisites](#prerequisites)
2. [System Requirements](#system-requirements)
3. [Installation Steps](#installation-steps)
4. [Configuration](#configuration)
5. [Testing](#testing)
6. [Going Live](#going-live)

---

## Prerequisites

Before starting, ensure you have:

- [ ] **Telegram account** (iOS/Android/Desktop)
- [ ] **Google Account** with Calendar access
- [ ] **Notion account** (free tier works)
- [ ] **DeepSeek API account** with credits
- [ ] **n8n instance** (self-hosted or cloud)
- [ ] Basic understanding of:
  - JSON format
  - API credentials
  - OAuth authentication

---

## System Requirements

### For Self-Hosted n8n

- **OS:** Windows, macOS, Linux, or Docker
- **Node.js:** v18+ (if installing via npm)
- **RAM:** 2GB minimum, 4GB recommended
- **Storage:** 1GB free space
- **Network:** Internet connection with open port 5678

### For n8n Cloud

- Just a web browser! ✅

---

## Installation Steps

### Step 1: Install n8n

#### Option A: Docker (Recommended)

```bash
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n
```

#### Option B: npm

```bash
npm install n8n -g
n8n start
```

#### Option C: n8n Cloud

1. Go to [https://n8n.io/cloud](https://n8n.io/cloud)
2. Sign up for an account
3. Create a new workspace

### Step 2: Access n8n

Open your browser and navigate to:
- **Self-hosted:** `http://localhost:5678`
- **Cloud:** Your n8n cloud URL

### Step 3: Create Account (First-time Setup)

1. Enter your email
2. Create a password
3. Set up your workspace name

---

## Configuration

### Step 4: Set Up External Services

Follow these detailed guides in order:

1. **[Telegram Bot Setup](telegram-setup.md)**
   - Create bot with BotFather
   - Get bot token
   - Get your chat ID

2. **[Google Calendar Setup](google-calendar-setup.md)**
   - Enable Calendar API
   - Create OAuth credentials
   - Connect to n8n

3. **[Notion Setup](notion-setup.md)**
   - Create database
   - Set up integration
   - Get database ID

4. **[DeepSeek API Setup](deepseek-setup.md)**
   - Create account
   - Get API key
   - Add credits

### Step 5: Configure n8n Credentials

1. In n8n, go to **Settings** → **Credentials**

2. Add **Telegram API** credential:
   - Click "+ Add Credential"
   - Search "Telegram"
   - Enter your bot token
   - Click "Save"

3. Add **Google Calendar OAuth2 API** credential:
   - Click "+ Add Credential"
   - Search "Google Calendar OAuth2"
   - Upload credentials JSON
   - Authorize with Google
   - Click "Save"

4. Add **Notion API** credential:
   - Click "+ Add Credential"
   - Search "Notion API"
   - Enter integration token
   - Click "Save"

### Step 6: Import Workflows

1. **Download workflow files** from the `workflows/` folder

2. **Import Workflow A (Message Processor)**:
   - In n8n, click "Add workflow" → "Import from File"
   - Select `workflow-a-message-processor.json`
   - Click "Import"

3. **Import Workflow B (Evening Reminder)**:
   - Click "Add workflow" → "Import from File"
   - Select `workflow-b-evening-reminder.json`
   - Click "Import"

4. **Import Workflow C (Morning Summary)**:
   - Click "Add workflow" → "Import from File"
   - Select `workflow-c-morning-summary.json`
   - Click "Import"

### Step 7: Configure Workflow A (Message Processor)

1. Open Workflow A in n8n

2. **Update Telegram Trigger node**:
   - Click the "Telegram Trigger" node
   - Select your Telegram credential
   - Click "Save"

3. **Update DeepSeek HTTP Request node**:
   - Click the "DeepSeek - Extract Tasks" node
   - Find the Authorization header
   - Replace `sk-401fbd42cf00493b8c28db07f3027460` with your DeepSeek API key
   - Click "Save"

4. **Update Google Calendar nodes**:
   - Click "Get Calendar Events" node
   - Select your Google Calendar credential
   - Click "Save"
   - Repeat for "Create Calendar Event" node

5. **Update Notion nodes**:
   - Click "Create Notion Task" node
   - Select your Notion credential
   - Replace database ID `2c40d73ab97b8018ae73f28e20099d86` with yours
   - Click "Save"

6. **Update Telegram Reply node**:
   - Click "Send Confirmation" node
   - Select your Telegram credential
   - Click "Save"

7. **Save and Activate** the workflow (toggle switch in top-right)

### Step 8: Configure Workflow B (Evening Reminder)

1. Open Workflow B in n8n

2. **Update Schedule Trigger**:
   - Click "Schedule - 11 PM" node
   - Verify time is set to 23:00 (11 PM)
   - Adjust if needed
   - Click "Save"

3. **Update Telegram node**:
   - Click "Send Planning Prompt" node
   - Select your Telegram credential
   - Replace `YOUR_CHAT_ID` with your actual chat ID
   - Click "Save"

4. **Save and Activate** the workflow

### Step 9: Configure Workflow C (Morning Summary)

1. Open Workflow C in n8n

2. **Update Schedule Trigger**:
   - Click "Schedule - 7 AM" node
   - Verify time is set to 07:00 (7 AM)
   - Adjust if needed
   - Click "Save"

3. **Update Google Calendar node**:
   - Click "Get Today's Events" node
   - Select your Google Calendar credential
   - Click "Save"

4. **Update Telegram node**:
   - Click "Send Morning Summary" node
   - Select your Telegram credential
   - Replace `YOUR_CHAT_ID` with your actual chat ID
   - Click "Save"

5. **Save and Activate** the workflow

---

## Testing

### Test Workflow A (Message Processing)

1. Open Telegram and find your bot
2. Send: `/start`
3. Send: `Add gym tomorrow at 7 AM`
4. You should receive a confirmation message
5. Check:
   - ✅ Google Calendar has the event
   - ✅ Notion database has the task
   - ✅ Telegram shows confirmation

### Test Workflow B (Evening Reminder)

**Option 1: Wait until 11 PM**
- The bot will automatically send the planning prompt

**Option 2: Manual Test**
1. Open Workflow B in n8n
2. Click "Execute Workflow" button
3. Check Telegram for the reminder message

### Test Workflow C (Morning Summary)

**Option 1: Wait until 7 AM**
- The bot will automatically send the morning summary

**Option 2: Manual Test**
1. Open Workflow C in n8n
2. Click "Execute Workflow" button
3. Check Telegram for the summary message

---

## Going Live

### Final Checklist

- [ ] All three workflows are active (green toggle)
- [ ] All credentials are configured correctly
- [ ] Test message successfully creates task
- [ ] Google Calendar event appears
- [ ] Notion task is created
- [ ] Telegram confirmations work
- [ ] Evening reminder tested
- [ ] Morning summary tested

### Production Tips

1. **For Self-Hosted n8n**:
   - Set up as a system service (systemd/PM2)
   - Configure a reverse proxy (nginx)
   - Use HTTPS with SSL certificate
   - Set up automatic backups

2. **For n8n Cloud**:
   - Already production-ready! ✅
   - Monitor execution history regularly

3. **General**:
   - Keep API keys secure
   - Monitor DeepSeek API usage
   - Regularly check workflow execution logs
   - Back up workflow JSON files

---

## Environment Variables Reference

Create a `.env` file with these values:

```env
# Telegram
TELEGRAM_BOT_TOKEN=1234567890:ABCdefGHIjklMNOpqrsTUVwxyz
TELEGRAM_CHAT_ID=987654321

# DeepSeek
DEEPSEEK_API_KEY=sk-your_api_key_here

# Notion
NOTION_API_TOKEN=secret_your_token_here
NOTION_DATABASE_ID=2c40d73ab97b8018ae73f28e20099d86

# Google Calendar
# (OAuth handled by n8n, no env vars needed)
```

---

## Next Steps

- 📚 Read the [Troubleshooting Guide](troubleshooting.md)
- 🎨 Customize reminder times and messages
- 🚀 Explore advanced features
- 📊 Monitor your task completion rates

---

## Need Help?

- 📖 Check the [Troubleshooting Guide](troubleshooting.md)
- 💬 Join the n8n community forum
- 🐛 Report issues on GitHub

---

**Congratulations! Your AI Personal Assistant is now live! 🎉**
