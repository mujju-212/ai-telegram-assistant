# 🐛 Troubleshooting Guide

Common issues and solutions for the AI Personal Assistant.

---

## 📋 Quick Diagnostics

Before diving into specific issues, run through this checklist:

- [ ] All three workflows are **active** (green toggle in n8n)
- [ ] All credentials are **configured** and **saved**
- [ ] Bot has been started with `/start` command in Telegram
- [ ] Internet connection is stable
- [ ] n8n instance is running (if self-hosted)
- [ ] All API services are active (check status pages)

---

## 🤖 Telegram Issues

### Bot Doesn't Respond to Messages

**Symptoms:** Send message to bot, no response

**Solutions:**

1. **Check Workflow A is Active:**
   ```
   n8n → Workflows → Workflow A → Toggle should be GREEN
   ```

2. **Verify Telegram Webhook:**
   - Open Workflow A
   - Click "Telegram Trigger" node
   - Click "Execute Node" or "Save" to re-register webhook

3. **Check Bot Token:**
   - Settings → Credentials → Telegram API
   - Verify token format: `1234567890:ABCdefGHI...`
   - Test with BotFather: send `/mybots` → select bot → should show "Active"

4. **Send /start Command:**
   - In Telegram, send `/start` to your bot
   - This initializes the conversation

5. **Check n8n Execution Log:**
   - n8n → Executions
   - Look for errors in recent executions
   - Check if webhook is receiving messages

---

### Bot Returns Error Messages

**Symptoms:** Bot responds but says "Error" or gives wrong information

**Solutions:**

1. **Check DeepSeek API Key:**
   - Open Workflow A
   - Find "DeepSeek - Extract Tasks" node
   - Verify API key in Authorization header: `Bearer sk-...`

2. **Check API Credits:**
   - Go to [DeepSeek Platform](https://platform.deepseek.com/)
   - Check balance in "Usage" section
   - Add credits if needed

3. **Review Execution Logs:**
   - n8n → Executions → Click latest execution
   - Check each node for errors
   - Look at error messages for clues

---

### Can't Find Bot in Telegram

**Symptoms:** Searching for bot returns no results

**Solutions:**

1. **Verify Bot Exists:**
   - Open Telegram, search for `@BotFather`
   - Send `/mybots`
   - Your bot should be listed

2. **Check Username:**
   - Bot username must end with `bot`
   - Try exact username (e.g., `@mytask_bot`)
   - Check for typos

3. **Bot Deleted:**
   - If bot doesn't appear in BotFather, it may have been deleted
   - Create a new bot following [Telegram Setup Guide](telegram-setup.md)

---

## 📅 Google Calendar Issues

### Calendar Events Not Created

**Symptoms:** Bot confirms task added, but nothing in Google Calendar

**Solutions:**

1. **Check Calendar Credential:**
   - n8n → Settings → Credentials → Google Calendar OAuth2 API
   - Click "Edit" → "Reconnect Account"
   - Re-authorize if prompted

2. **Verify Calendar ID:**
   - Open [Google Calendar](https://calendar.google.com/)
   - Settings → Your calendar → Integrate calendar
   - Copy Calendar ID
   - Update in n8n workflow nodes

3. **Check API Quota:**
   - Go to [Google Cloud Console](https://console.cloud.google.com/)
   - APIs & Services → Dashboard
   - Check Calendar API usage
   - Should be well below limits

4. **Test Node Execution:**
   - Open Workflow A
   - Click "Create Calendar Event" node
   - Click "Execute Node"
   - Check output for errors

---

### "Calendar not found" Error

**Symptoms:** n8n shows "Calendar not found" error

**Solutions:**

1. **Double-Check Calendar ID:**
   - Remove any spaces or special characters
   - Should be email format: `yourname@gmail.com`
   - Or: `random-string@group.calendar.google.com`

2. **Verify Calendar Exists:**
   - Open Google Calendar
   - Check calendar is not deleted
   - Try accessing from web browser

3. **Check OAuth Scopes:**
   - Google Cloud Console → APIs & Services → OAuth consent screen
   - Verify scopes include:
     - `https://www.googleapis.com/auth/calendar`
     - `https://www.googleapis.com/auth/calendar.events`

---

### Events Created in Wrong Time Zone

**Symptoms:** Events show up at wrong times

**Solutions:**

1. **Check Google Calendar Time Zone:**
   - Google Calendar → Settings → General
   - Verify "Time zone" setting
   - Update if incorrect

2. **Check n8n Time Zone:**
   - n8n → Settings → General
   - Set timezone to match your location

3. **Update Workflow:**
   - In "Find Optimal Time" node
   - Ensure date calculations use correct timezone

---

## 📊 Notion Issues

### Tasks Not Added to Notion

**Symptoms:** Calendar event created, but no Notion task

**Solutions:**

1. **Check Notion Credential:**
   - n8n → Settings → Credentials → Notion API
   - Verify token is correct
   - Format: `secret_abc123...`

2. **Verify Database Connection:**
   - Open Notion database
   - Click "..." (three dots) → "Connections"
   - Your integration should be listed
   - If not, reconnect following [Notion Setup Guide](notion-setup.md)

3. **Check Database ID:**
   - Verify 32-character ID (no spaces, no hyphens)
   - Should be exactly 32 characters
   - All lowercase letters and numbers

4. **Test Node:**
   - Open Workflow A
   - Click "Create Notion Task" node
   - Click "Execute Node"
   - Check output for errors

---

### "Database not found" Error

**Symptoms:** n8n returns 404 "database not found"

**Solutions:**

1. **Reconnect Integration:**
   - Notion database → "..." → "Add connections"
   - Select your integration
   - Click "Confirm"

2. **Verify Token:**
   - Go to [Notion Integrations](https://www.notion.so/my-integrations)
   - Check your integration exists
   - Copy token again if needed

3. **Check Database ID Format:**
   - Must be 32 characters
   - No hyphens, no spaces
   - Example: `2c40d73ab97b8018ae73f28e20099d86`

---

### Property Type Errors

**Symptoms:** "Property type mismatch" or "Invalid property" errors

**Solutions:**

1. **Verify Property Names:**
   - Open Notion database
   - Check column names match exactly (case-sensitive):
     - `Status` (Select)
     - `Priority` (Select)
     - `Category` (Select)
     - `Due Date` (Date)
     - `Duration (min)` (Number)
     - `Calendar Link` (URL)
     - `Created By` (Text)

2. **Check Property Types:**
   - Each property must be the correct type
   - Refer to [Notion Setup Guide](notion-setup.md) for proper types

3. **Update Workflow Mapping:**
   - Open "Create Notion Task" node
   - Verify field mappings match database properties

---

## 🤖 DeepSeek API Issues

### "Invalid API Key" Error

**Symptoms:** 401 Unauthorized error in n8n logs

**Solutions:**

1. **Verify API Key Format:**
   ```
   Bearer sk-401fbd42cf00493b8c28db07f3027460
   ```
   - Must start with `Bearer ` (with space)
   - Followed by `sk-...`

2. **Check Key Status:**
   - Go to [DeepSeek Platform](https://platform.deepseek.com/)
   - API Keys section
   - Verify key is active (not deleted)

3. **Regenerate Key:**
   - Delete old key
   - Create new key
   - Update in n8n workflow

---

### "Insufficient Balance" Error

**Symptoms:** 402 Payment Required error

**Solutions:**

1. **Check Credit Balance:**
   - DeepSeek Platform → Usage
   - View current balance

2. **Add Credits:**
   - Click "Add Credits" or "Top Up"
   - Add $5-$10 (enough for thousands of tasks)

3. **Monitor Usage:**
   - Check daily usage
   - Set up alerts if available

---

### Slow Response Times

**Symptoms:** Bot takes 10+ seconds to respond

**Solutions:**

1. **Check DeepSeek API Status:**
   - Visit DeepSeek status page
   - Look for service outages

2. **Optimize Prompt:**
   - Reduce system prompt length
   - Remove unnecessary examples
   - Keep instructions concise

3. **Increase Timeout:**
   - Open "DeepSeek - Extract Tasks" node
   - Increase timeout setting to 30-60 seconds

---

### Task Extraction Inaccurate

**Symptoms:** Wrong dates, times, or categories

**Solutions:**

1. **Improve Prompt:**
   - Add more examples
   - Specify date/time formats
   - Add context about working hours

2. **Be More Specific:**
   - Instead of: "Add meeting tomorrow"
   - Use: "Add meeting tomorrow at 2 PM"

3. **Update System Prompt:**
   - Edit "DeepSeek - Extract Tasks" node
   - Add clarifications:
     ```
     - Always use 24-hour time format
     - Default duration is 60 minutes
     - Morning = 9 AM, Afternoon = 2 PM, Evening = 6 PM
     ```

---

## ⏰ Scheduled Workflows Issues

### 11 PM Reminder Not Sent

**Symptoms:** No planning prompt at 11 PM

**Solutions:**

1. **Check Workflow B is Active:**
   - n8n → Workflows → Workflow B
   - Toggle should be GREEN

2. **Verify Schedule Time:**
   - Open Workflow B
   - Click "Schedule - 11 PM" node
   - Check time: `23:00` (11 PM)
   - Verify timezone is correct

3. **Check Chat ID:**
   - In "Send Planning Prompt" node
   - Verify `chatId` matches your Telegram chat ID

4. **Manual Test:**
   - Click "Execute Workflow" button
   - Should immediately send message

---

### 7 AM Summary Not Sent

**Symptoms:** No morning summary at 7 AM

**Solutions:**

1. **Check Workflow C is Active:**
   - n8n → Workflows → Workflow C
   - Toggle should be GREEN

2. **Verify Schedule Time:**
   - Open Workflow C
   - Click "Schedule - 7 AM" node
   - Check time: `07:00` (7 AM)

3. **Check Google Calendar Connection:**
   - "Get Today's Events" node should have valid credential

4. **Test with Manual Execution:**
   - Click "Execute Workflow"
   - Check Telegram for summary
   - Review node outputs for errors

---

### Wrong Timezone for Reminders

**Symptoms:** Reminders come at wrong times

**Solutions:**

1. **Set n8n Timezone:**
   - n8n → Settings → General
   - Set timezone to your location
   - Restart n8n (if self-hosted)

2. **Adjust Schedule Times:**
   - Update trigger hour to match your timezone
   - Example: If UTC and you're UTC+5, use 18:00 instead of 23:00

---

## 🔧 n8n Platform Issues

### Workflow Not Executing

**Symptoms:** No executions showing in logs

**Solutions:**

1. **Check Workflow is Active:**
   - Green toggle in top-right
   - If red, click to activate

2. **Check Workflow Errors:**
   - n8n → Executions
   - Look for failed executions
   - Click to view error details

3. **Restart n8n (Self-Hosted):**
   ```bash
   # Docker
   docker restart n8n
   
   # npm
   n8n stop
   n8n start
   ```

---

### "Workflow could not be started" Error

**Symptoms:** Can't activate workflow

**Solutions:**

1. **Check for Invalid Nodes:**
   - Red exclamation marks on nodes
   - Click node to see error
   - Fix configuration

2. **Verify All Credentials:**
   - Each node with credential should be valid
   - Re-save credentials if needed

3. **Check Node Connections:**
   - All nodes should be connected
   - No orphaned nodes

---

### Execution Timeout

**Symptoms:** Workflow stops after 2-3 minutes

**Solutions:**

1. **Increase Execution Timeout:**
   - n8n Settings → General
   - Increase "Execution timeout" (default: 120s)

2. **Optimize Workflow:**
   - Remove unnecessary nodes
   - Reduce API wait times
   - Parallelize when possible

---

## 🌐 Network Issues

### "Connection Refused" Errors

**Symptoms:** Can't connect to external APIs

**Solutions:**

1. **Check Internet Connection:**
   - Verify n8n can access internet
   - Test with: `curl https://api.telegram.org`

2. **Check Firewall:**
   - Allow n8n to make outbound connections
   - Ports: 443 (HTTPS), 80 (HTTP)

3. **Verify API Status:**
   - Telegram: [https://telegram.org/](https://telegram.org/)
   - Google: [https://status.cloud.google.com/](https://status.cloud.google.com/)
   - DeepSeek: Check platform announcements

---

### SSL/TLS Certificate Errors

**Symptoms:** "Certificate verification failed"

**Solutions:**

1. **Update System Certificates:**
   ```bash
   # Ubuntu/Debian
   sudo apt-get update
   sudo apt-get install ca-certificates
   
   # Windows
   # Update Windows (certificates included)
   ```

2. **Disable SSL Verification (Not Recommended):**
   - Only for testing!
   - In HTTP Request nodes, disable "SSL Certificate Validation"

---

## 📱 Mobile-Specific Issues

### Messages Not Syncing

**Symptoms:** Bot works on desktop, not mobile

**Solutions:**

1. **Update Telegram App:**
   - Check for app updates in App Store/Play Store

2. **Check Notifications:**
   - Allow Telegram notifications
   - Disable battery optimization for Telegram

3. **Sync Issue:**
   - Log out and log back in
   - Force sync: pull down to refresh

---

## 🔍 Debugging Tips

### Enable Verbose Logging

1. **In n8n (Self-Hosted):**
   ```bash
   export N8N_LOG_LEVEL=debug
   n8n start
   ```

2. **Check Logs:**
   - Watch terminal output
   - Look for detailed error messages

### Test Nodes Individually

1. Open workflow in n8n
2. Click on each node
3. Click "Execute Node" button
4. Review output for each step

### Use Sticky Notes

1. Add sticky notes to workflow
2. Document expected values
3. Compare actual vs expected

---

## 📞 Getting Help

### Before Asking for Help

Gather this information:

- [ ] n8n version
- [ ] Error messages (exact text)
- [ ] Workflow JSON (if possible)
- [ ] Steps to reproduce
- [ ] What you've already tried

### Support Channels

1. **n8n Community Forum:**
   - [https://community.n8n.io/](https://community.n8n.io/)
   - Search before posting
   - Include error details

2. **GitHub Issues:**
   - For bugs in this project
   - Include full error logs
   - Describe expected behavior

3. **Documentation:**
   - [n8n Docs](https://docs.n8n.io/)
   - [Telegram Bot API](https://core.telegram.org/bots/api)
   - [Google Calendar API](https://developers.google.com/calendar/api)
   - [Notion API](https://developers.notion.com/)

---

## 🎯 Preventive Maintenance

### Regular Checks

**Weekly:**
- [ ] Review execution logs for errors
- [ ] Check API credit balances
- [ ] Verify all workflows are active

**Monthly:**
- [ ] Update n8n to latest version
- [ ] Rotate API keys (security)
- [ ] Archive old Notion tasks
- [ ] Review and optimize workflows

### Backup Strategy

1. **Export Workflows:**
   - n8n → Workflows → Export
   - Save JSON files to GitHub/backup

2. **Document Changes:**
   - Keep notes on customizations
   - Track credential IDs

3. **Test Regularly:**
   - Send test messages weekly
   - Verify all integrations work

---

## ✅ Health Check Checklist

Run this checklist monthly:

```
[ ] Telegram bot responds to messages
[ ] Tasks are extracted correctly
[ ] Google Calendar events are created
[ ] Notion tasks are added
[ ] 11 PM reminder is sent
[ ] 7 AM summary is sent
[ ] All credentials are valid
[ ] API credits are sufficient
[ ] n8n is updated to latest version
[ ] Backups are current
```

---

## 🚨 Emergency Recovery

### Complete System Failure

If nothing works:

1. **Restart n8n**
2. **Deactivate all workflows**
3. **Test credentials individually**
4. **Re-import workflows from backup**
5. **Reconfigure one workflow at a time**
6. **Test after each step**

### Lost Credentials

1. **Telegram:** Create new bot with BotFather
2. **Google:** Regenerate OAuth credentials
3. **Notion:** Create new integration
4. **DeepSeek:** Generate new API key

---

**Need more help?** Check the [Setup Guide](setup-guide.md) or open a [GitHub Issue](https://github.com/yourusername/ai-telegram-assistant/issues).
