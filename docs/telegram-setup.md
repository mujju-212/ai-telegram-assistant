# 📱 Telegram Bot Setup Guide

This guide will help you create and configure your Telegram bot.

---

## 📋 What You'll Get

- ✅ Telegram bot token
- ✅ Your chat ID
- ✅ Bot configured and ready to use

---

## Step 1: Create a Telegram Bot

### 1.1 Find BotFather

1. Open Telegram (mobile or desktop)
2. In the search bar, type: `@BotFather`
3. Click on the official **BotFather** (verified with a blue checkmark)

### 1.2 Start Conversation

1. Click **"Start"** or send `/start`
2. You'll see a list of available commands

### 1.3 Create New Bot

1. Send the command: `/newbot`
2. BotFather will ask for a **name** for your bot
   - Example: `My Task Assistant`
   - This is the display name users will see
3. BotFather will ask for a **username**
   - Must end with `bot`
   - Must be unique
   - Examples: `mytask_bot`, `ai_assistant_bot`, `personal_planner_bot`

### 1.4 Get Your Bot Token

After creating the bot, BotFather will send you a message like:

```
Done! Congratulations on your new bot. You will find it at t.me/mytask_bot. 
You can now add a description, about section and profile picture for your bot.

Use this token to access the HTTP API:
1234567890:ABCdefGHIjklMNOpqrsTUVwxyz

Keep your token secure and store it safely, it can be used by anyone to control your bot.
```

**⚠️ Important:** Copy and save this token! You'll need it for n8n configuration.

---

## Step 2: Get Your Chat ID

You need your Chat ID so the bot knows where to send messages.

### Method 1: Using @userinfobot (Easiest)

1. Open Telegram
2. Search for: `@userinfobot`
3. Click **"Start"** or send `/start`
4. The bot will reply with your user information:
   ```
   Id: 987654321
   First name: Your Name
   Username: @yourusername
   ```
5. Copy your **Id** (e.g., `987654321`)

### Method 2: Using @RawDataBot

1. Open Telegram
2. Search for: `@RawDataBot`
3. Send any message to it
4. It will reply with JSON data containing your chat ID:
   ```json
   {
     "message": {
       "from": {
         "id": 987654321
       }
     }
   }
   ```
5. Copy the `id` value

### Method 3: Using Telegram API (Advanced)

1. Send a message to your bot (e.g., "Hello")
2. Open in browser:
   ```
   https://api.telegram.org/bot<YOUR_BOT_TOKEN>/getUpdates
   ```
   Replace `<YOUR_BOT_TOKEN>` with your actual bot token

3. Look for `"chat":{"id":987654321}` in the response
4. Copy the chat ID

---

## Step 3: Configure Bot Settings (Optional)

### Set Bot Description

1. Go back to BotFather
2. Send: `/setdescription`
3. Select your bot
4. Send a description:
   ```
   Your personal AI assistant for task management. 
   Send me tasks in natural language, and I'll add them to your calendar and Notion!
   ```

### Set Bot About Text

1. Send to BotFather: `/setabouttext`
2. Select your bot
3. Send:
   ```
   AI-powered task manager that syncs with Google Calendar and Notion.
   ```

### Set Bot Profile Picture

1. Send to BotFather: `/setuserpic`
2. Select your bot
3. Upload an image (512x512 pixels recommended)

### Set Bot Commands (for Easy Access)

1. Send to BotFather: `/setcommands`
2. Select your bot
3. Send:
   ```
   start - Start the bot
   help - Show help message
   ```

---

## Step 4: Test Your Bot

1. Search for your bot in Telegram using the username (e.g., `@mytask_bot`)
2. Click **"Start"** or send `/start`
3. The bot should appear, but won't respond yet (we'll configure that in n8n)

---

## Step 5: Save Your Credentials

Create a note or file with:

```
Telegram Bot Token: 1234567890:ABCdefGHIjklMNOpqrsTUVwxyz
Telegram Chat ID: 987654321
Bot Username: @mytask_bot
```

**🔐 Security Note:** Never share your bot token publicly!

---

## Step 6: Configure in n8n

1. Open n8n
2. Go to **Settings** → **Credentials**
3. Click **"+ Add Credential"**
4. Search for **"Telegram API"**
5. Enter your bot token
6. Click **"Save"**

---

## Common Issues

### Bot Not Found

- **Problem:** Can't find bot in Telegram search
- **Solution:** Make sure username ends with `bot` and is unique

### Invalid Token

- **Problem:** n8n says "Invalid token"
- **Solution:** 
  - Copy token exactly as provided by BotFather
  - No extra spaces or characters
  - Token format: `NUMBERS:LETTERS_AND_NUMBERS`

### Bot Doesn't Respond

- **Problem:** Bot doesn't reply to messages
- **Solution:** 
  - This is normal! The bot will only respond after configuring Workflow A in n8n
  - Make sure Workflow A is activated (green toggle)

### Can't Get Chat ID

- **Problem:** @userinfobot not working
- **Solution:** 
  - Try @RawDataBot instead
  - Or use the API method described above

---

## Next Steps

✅ You now have:
- Telegram bot token
- Your chat ID

📚 Next, set up:
- [Google Calendar](google-calendar-setup.md)
- [Notion Database](notion-setup.md)
- [DeepSeek API](deepseek-setup.md)

---

## Telegram Bot Features

### What Your Bot Can Do

- ✅ Receive messages 24/7
- ✅ Send instant replies
- ✅ Send scheduled reminders
- ✅ Support rich formatting (bold, italic, links)
- ✅ Work on mobile and desktop

### Telegram Bot Limits

- **Messages:** Up to 30 messages per second
- **Message Length:** Up to 4096 characters
- **File Size:** Up to 50MB per file
- **Rate Limits:** Usually no issues for personal use

---

## Tips & Best Practices

1. **Keep Token Secret**
   - Don't share on GitHub/public repos
   - Don't post in forums or chats
   - Use environment variables

2. **Customize Your Bot**
   - Add a profile picture
   - Write a clear description
   - Set up command menu

3. **Test Regularly**
   - Send test messages
   - Check bot responsiveness
   - Monitor for errors in n8n

4. **Backup**
   - Save bot token securely
   - Document chat ID
   - Keep BotFather conversation

---

## Resources

- 📚 [Telegram Bot API Documentation](https://core.telegram.org/bots/api)
- 🤖 [BotFather Commands](https://core.telegram.org/bots#botfather)
- 💬 [Telegram Bot Features](https://core.telegram.org/bots/features)

---

**Next:** [Google Calendar Setup →](google-calendar-setup.md)
