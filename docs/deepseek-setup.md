# 🤖 DeepSeek API Setup Guide

This guide will help you set up DeepSeek AI API for natural language task extraction.

---

## 📋 What You'll Get

- ✅ DeepSeek API account
- ✅ API key for authentication
- ✅ Credits for API usage

---

## What is DeepSeek?

DeepSeek is an AI language model that powers the natural language understanding in this project. It extracts task details from casual messages like:

- "Add gym tomorrow at 7 AM" → Extracts: task name, date, time, category
- "Meeting with John next Friday 2 PM" → Understands: future dates, time format
- "Dentist appointment on Monday morning" → Infers: approximate time, category

---

## Step 1: Create DeepSeek Account

### 1.1 Visit DeepSeek Website

Go to: [https://platform.deepseek.com/](https://platform.deepseek.com/)

### 1.2 Sign Up

1. Click **"Sign Up"** or **"Get Started"**
2. Choose signup method:
   - Email address (recommended)
   - Google account
   - GitHub account
3. Complete registration:
   - Enter email
   - Create password
   - Verify email (check inbox)

### 1.3 Verify Account

1. Check your email for verification link
2. Click the verification link
3. You'll be redirected to the platform

---

## Step 2: Get API Key

### 2.1 Access API Keys Page

1. Log in to [https://platform.deepseek.com/](https://platform.deepseek.com/)
2. Navigate to **"API Keys"** section (usually in left sidebar or top menu)

### 2.2 Create New API Key

1. Click **"+ Create API Key"** or **"New Key"**
2. (Optional) Give it a name: `n8n Task Assistant`
3. Click **"Create"**

### 2.3 Copy API Key

1. Your API key will be displayed (only once!)
2. Copy the key immediately
3. Format: `sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxx`

**⚠️ CRITICAL:** Save this key securely! You won't be able to see it again.

```
DeepSeek API Key: sk-401fbd42cf00493b8c28db07f3027460
```

---

## Step 3: Add Credits (Optional)

DeepSeek may provide free credits for new accounts. If you need more:

### 3.1 Navigate to Billing

1. Go to **"Usage"** or **"Billing"** section
2. Check your current balance

### 3.2 Add Credits

1. Click **"Add Credits"** or **"Top Up"**
2. Choose amount:
   - $5 (recommended for testing)
   - $10 (enough for months of personal use)
   - $20+ (heavy usage)
3. Select payment method
4. Complete payment

### 3.3 Verify Credits

1. Check balance is updated
2. View usage statistics

---

## Step 4: Understand Pricing

### DeepSeek Pricing (approximate)

| Model | Input (per 1M tokens) | Output (per 1M tokens) |
|-------|----------------------|------------------------|
| DeepSeek Chat | ~$0.14 | ~$0.28 |
| DeepSeek Coder | ~$0.14 | ~$0.28 |

### What Does This Mean?

For this project, each task extraction costs approximately:
- **Input:** ~200 tokens = $0.000028 (~$0.00003)
- **Output:** ~150 tokens = $0.000042 (~$0.00004)
- **Total per task:** ~$0.00007 (less than 0.01 cents!)

**Monthly estimate:**
- 100 tasks = $0.007 (~1 cent)
- 1,000 tasks = $0.07 (7 cents)
- 10,000 tasks = $0.70 (70 cents)

💰 **Bottom line:** $5 credit = thousands of tasks!

---

## Step 5: Configure in n8n

### 5.1 Open Workflow A

1. In n8n, open **"Workflow A: Message Processor"**

### 5.2 Find DeepSeek HTTP Request Node

1. Locate the node named **"DeepSeek - Extract Tasks"**
2. Click to open it

### 5.3 Update Authorization Header

1. Find the **"Authentication"** section
2. Look for **"Header Auth"** or **"Authorization"** header
3. Update the value:
   ```
   Bearer sk-YOUR_ACTUAL_API_KEY_HERE
   ```
   Replace `sk-YOUR_ACTUAL_API_KEY_HERE` with your real API key

### 5.4 Test the Node

1. Click **"Execute Node"**
2. Should return a successful response
3. Check response contains task extraction

---

## Step 6: Verify API Connection

### 6.1 Test with Sample Message

Send a test message to your Telegram bot:
```
Add gym tomorrow at 7 AM
```

### 6.2 Check n8n Execution

1. Go to **"Executions"** in n8n
2. Find the latest execution
3. Check the DeepSeek node output
4. Verify it extracted:
   - Task name: "Gym"
   - Date: Tomorrow's date
   - Time: 07:00
   - Category: Health

### 6.3 Expected Output Format

DeepSeek should return JSON like:
```json
{
  "tasks": [
    {
      "name": "Gym",
      "date": "2024-12-11",
      "time": "07:00",
      "duration": 60,
      "priority": "Medium",
      "category": "Health"
    }
  ]
}
```

---

## Common Issues

### Invalid API Key

- **Problem:** "401 Unauthorized" error
- **Solution:**
  - Double-check API key is correct
  - Ensure format is: `Bearer sk-...`
  - No extra spaces or characters
  - Key is active (not deleted)

### Insufficient Credits

- **Problem:** "Insufficient balance" error
- **Solution:**
  - Check credit balance on platform
  - Add more credits
  - Wait for free tier reset (if applicable)

### Rate Limit Exceeded

- **Problem:** "429 Too Many Requests"
- **Solution:**
  - Wait a few seconds
  - Reduce message frequency
  - Check rate limits on your plan

### Connection Timeout

- **Problem:** Request takes too long
- **Solution:**
  - Check internet connection
  - Verify DeepSeek API status
  - Increase timeout in n8n node settings

---

## API Best Practices

### Optimize Token Usage

1. **Keep prompts concise:**
   - Remove unnecessary instructions
   - Use clear, brief examples

2. **Limit context:**
   - Don't send full conversation history
   - Only include relevant message

3. **Batch when possible:**
   - Extract multiple tasks in one call
   - Instead of: 3 calls for 3 tasks
   - Do: 1 call for 3 tasks

### Monitor Usage

1. **Track costs:**
   - Check usage dashboard weekly
   - Set up usage alerts (if available)

2. **Review logs:**
   - Check n8n execution logs
   - Identify failed requests
   - Optimize prompts

### Security

1. **Protect your API key:**
   - Never commit to GitHub
   - Use environment variables
   - Rotate keys periodically

2. **Limit exposure:**
   - Don't share keys in chat/email
   - Don't include in screenshots
   - Store securely (password manager)

---

## Alternative Models (Future)

If you want to switch from DeepSeek:

### Option 1: OpenAI GPT
- **Pros:** Very accurate, well-documented
- **Cons:** More expensive (~$0.002 per task)
- **Setup:** Similar to DeepSeek

### Option 2: Anthropic Claude
- **Pros:** Great for structured extraction
- **Cons:** Higher cost, API access required
- **Setup:** Similar authentication

### Option 3: Local Models (Ollama)
- **Pros:** Free, private, no API limits
- **Cons:** Requires powerful computer, setup complexity
- **Setup:** Run locally with Ollama

---

## Prompt Engineering

### Current Prompt Structure

The DeepSeek prompt in Workflow A:
```json
{
  "model": "deepseek-chat",
  "messages": [
    {
      "role": "system",
      "content": "Extract task details from natural language..."
    },
    {
      "role": "user",
      "content": "{{$json.message.text}}"
    }
  ]
}
```

### Customize the Prompt

To improve accuracy:

1. **Add examples:**
   ```
   Example 1: "Add gym tomorrow 7 AM" → {"name": "Gym", "date": "2024-12-11", "time": "07:00"}
   Example 2: "Meeting with Sarah on Friday afternoon" → {"name": "Meeting with Sarah", "date": "2024-12-13", "time": "14:00"}
   ```

2. **Specify formats:**
   ```
   Always return time in 24-hour format (HH:MM)
   Always return date in ISO format (YYYY-MM-DD)
   ```

3. **Add constraints:**
   ```
   Default duration: 60 minutes
   Default priority: Medium
   Working hours: 9 AM - 6 PM
   ```

---

## Monitoring & Analytics

### Track API Usage

1. **DeepSeek Dashboard:**
   - View daily/monthly usage
   - See cost breakdown
   - Monitor rate limits

2. **n8n Execution Logs:**
   - Count successful extractions
   - Identify failed requests
   - Measure response times

### Performance Metrics

Track these metrics:
- ✅ Success rate (% of successful extractions)
- ⏱️ Average response time
- 💰 Cost per task
- 🔄 Retry rate

---

## Resources

- 📚 [DeepSeek API Documentation](https://platform.deepseek.com/docs)
- 💡 [DeepSeek Model Comparison](https://platform.deepseek.com/models)
- 🔧 [API Rate Limits](https://platform.deepseek.com/docs/rate-limits)

---

## Next Steps

✅ You now have:
- DeepSeek API account
- API key configured
- Credits added (if needed)

📚 Complete the setup:
- [Back to Main Setup Guide →](setup-guide.md)
- [Troubleshooting Guide →](troubleshooting.md)

---

**Previous:** [← Notion Setup](notion-setup.md) | **Next:** [Troubleshooting →](troubleshooting.md)
