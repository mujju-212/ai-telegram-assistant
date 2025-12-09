# 📱 AI Personal Assistant - Telegram Bot

An intelligent task management system that uses AI to extract tasks from natural language, automatically schedules them in Google Calendar, tracks them in Notion, and sends daily summaries via Telegram.

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![n8n](https://img.shields.io/badge/n8n-workflow-orange.svg)
![Status](https://img.shields.io/badge/status-active-success.svg)

---

## 🌟 Features

- ✅ **Natural Language Processing** - Just chat naturally: "Add gym tomorrow at 7 AM"
- 🤖 **AI-Powered Task Extraction** - DeepSeek AI understands context and extracts tasks automatically
- 📅 **Smart Calendar Integration** - Auto-detects conflicts and finds optimal time slots
- 📊 **Notion Database Sync** - All tasks automatically tracked in Notion
- ⏰ **Daily Reminders** - 11 PM planning prompt & 7 AM schedule summary
- 🔄 **Real-time Updates** - Instant confirmations via Telegram

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     USER (Telegram)                         │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                   N8N WORKFLOWS                             │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  Workflow A: Message Processor (Active 24/7)        │  │
│  │  ├─ Telegram Trigger                                │  │
│  │  ├─ DeepSeek AI (Task Extraction)                   │  │
│  │  ├─ Google Calendar (Conflict Detection)            │  │
│  │  ├─ Notion Database (Task Storage)                  │  │
│  │  └─ Telegram Reply (Confirmation)                   │  │
│  └──────────────────────────────────────────────────────┘  │
│                                                             │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  Workflow B: 11 PM Reminder (Daily Schedule)        │  │
│  │  └─ Sends planning prompt every night               │  │
│  └──────────────────────────────────────────────────────┘  │
│                                                             │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  Workflow C: 7 AM Summary (Daily Schedule)          │  │
│  │  └─ Sends daily schedule every morning              │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│              EXTERNAL SERVICES                              │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐    │
│  │  DeepSeek AI │  │Google Calendar│  │    Notion    │    │
│  │  (Task NLP)  │  │(Scheduling)   │  │  (Database)  │    │
│  └──────────────┘  └──────────────┘  └──────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

---

## 🚀 Quick Start Guide

### Prerequisites

- ✅ n8n installed (cloud or self-hosted)
- ✅ Telegram account
- ✅ Google Account (for Calendar)
- ✅ Notion Account (free tier works)
- ✅ DeepSeek API Key

### Installation

1. **Clone this repository**
   ```bash
   git clone https://github.com/mujju-212/ai-telegram-assistant.git
   cd ai-telegram-assistant
   ```

2. **Set up environment variables**
   ```bash
   cp .env.example .env
   # Edit .env with your credentials
   ```

3. **Follow the detailed setup guides**
   - [Complete Setup Guide](docs/setup-guide.md)
   - [Telegram Bot Setup](docs/telegram-setup.md)
   - [Google Calendar Setup](docs/google-calendar-setup.md)
   - [Notion Database Setup](docs/notion-setup.md)
   - [DeepSeek API Setup](docs/deepseek-setup.md)

4. **Import workflows into n8n**
   - Import `workflows/workflow-a-message-processor.json`
   - Import `workflows/workflow-b-evening-reminder.json`
   - Import `workflows/workflow-c-morning-summary.json`

5. **Configure credentials in n8n**
   - Update Telegram, Google Calendar, Notion, and DeepSeek credentials
   - Activate all workflows

---

## 🎯 Usage Examples

### Adding Tasks

**Single Task:**
```
You: Add gym tomorrow at 7 AM

Bot: ✅ Task Added Successfully!

     💪 Gym
     📅 Wed, Dec 11 at 07:00 AM
     ⏱️ 60 min | ⚡ Medium
     🏷️ Category: Health

     ━━━━━━━━━━━━━━━━
     ✅ Added to Google Calendar
     ✅ Added to Notion
```

![Adding Task](screenshots/adding%20task.png)

![Task Confirmation](screenshots/task%20adding%20confirmation%20.jpg)

**Multiple Tasks:**
```
You: Add these tasks:
     - Client meeting Friday 2 PM
     - Dentist next Monday 9 AM
     - Gym session tomorrow 6 PM

Bot: ✅ Task Added Successfully!

     🏢 Client meeting
     📅 Fri, Dec 13 at 02:00 PM
     ⏱️ 60 min | ⚡ Medium

     🏥 Dentist
     📅 Mon, Dec 16 at 09:00 AM
     ⏱️ 60 min | ⚡ Medium

     💪 Gym session
     📅 Wed, Dec 11 at 06:00 PM
     ⏱️ 60 min | ⚡ Medium

     ━━━━━━━━━━━━━━━━
     📊 Total: 3 tasks
     ✅ Added to Calendar & Notion
```

**Natural Language:**
```
You: I need to call John tomorrow morning and finish the report by Friday afternoon

Bot: ✅ 2 tasks extracted and added!
```

---

## 📊 How It Works

### Workflow A: Message Processing (24/7)

1. You send message → Telegram Trigger catches it
2. DeepSeek AI extracts:
   - Task name
   - Date (calculates from "today", "tomorrow", "Friday", etc.)
   - Time
   - Duration
   - Priority
   - Category
3. Checks Google Calendar for conflicts
4. Finds optimal time slot (or reschedules if conflict)
5. Creates event in Google Calendar
6. Creates task in Notion database
7. Sends confirmation to Telegram

### Workflow B: Evening Reminder (11 PM)

Every day at 11 PM:
- Sends planning prompt to help you prepare for tomorrow

![Evening Planning Reminder](screenshots/evening%20notification%20to%20add%20task%20or%20plan.jpg)

### Workflow C: Morning Summary (7 AM)

Every day at 7 AM:
- Fetches today's calendar events
- Formats them nicely
- Sends summary with event details

![Morning Summary](screenshots/morning%20notigication.jpg)

![Morning Notification Sending](screenshots/morning%20notification%20sending.png)

---

## 🛠️ Customization

### Change Reminder Times

Edit Workflow B or C:
```javascript
"triggerAtHour": 23,  // Change to any hour (0-23)
"triggerAtMinute": 0  // Change to any minute (0-59)
```

### Adjust Working Hours

Edit "Find Optimal Time" node in Workflow A:
```javascript
const workStart = 9 * 60;   // 9 AM
const workEnd = 18 * 60;    // 6 PM
```

### Modify Categories

Update DeepSeek prompt and Notion Select options to add/remove categories.

---

## 📁 Project Structure

```
ai-telegram-assistant/
├── README.md
├── .env.example
├── .gitignore
├── workflows/
│   ├── workflow-a-message-processor.json
│   ├── workflow-b-evening-reminder.json
│   └── workflow-c-morning-summary.json
├── docs/
│   ├── setup-guide.md
│   ├── telegram-setup.md
│   ├── google-calendar-setup.md
│   ├── notion-setup.md
│   ├── deepseek-setup.md
│   └── troubleshooting.md
└── screenshots/
    ├── adding-task.png
    ├── task-confirmation.jpg
    ├── notion-database.png
    ├── morning-notification.jpg
    └── evening-notification.jpg
```

### 📊 Notion Database

All tasks are automatically tracked in your Notion database:

![Notion Database](screenshots/notion%20db.png)

---

## 🐛 Troubleshooting

Having issues? Check our [Troubleshooting Guide](docs/troubleshooting.md) for common problems and solutions.

**Quick Fixes:**

- **Bot doesn't respond** → Check if Workflow A is active
- **DeepSeek errors** → Verify API key and account credits
- **Notion errors** → Check database ID and integration connection
- **Calendar conflicts not detected** → Verify timezone settings

---

## 📈 Future Enhancements

- [ ] Voice message support
- [ ] Task editing ("Move gym to Friday 8 AM")
- [ ] Task deletion ("Cancel dentist appointment")
- [ ] Weekly planning summaries
- [ ] Priority-based notifications
- [ ] Recurring task support
- [ ] Team collaboration features

---

## 🤝 Contributing

Contributions welcome! Please:

1. Fork the repository
2. Create a feature branch
3. Test your changes
4. Submit a pull request

---

## 📄 License

MIT License - feel free to use and modify!

---

## 👨‍💻 Author

Created with ❤️ using:

- **n8n** - Workflow Automation
- **DeepSeek AI** - Natural Language Processing
- **Google Calendar API**
- **Notion API**
- **Telegram Bot API**

---

## 🙏 Acknowledgments

- n8n community for amazing automation platform
- DeepSeek for powerful AI capabilities
- Open source community

---

## 📞 Support

- 📧 Email: your@email.com
- 💬 Telegram: @yourusername
- 🐛 Issues: [GitHub Issues](https://github.com/mujju-212/ai-telegram-assistant/issues)

---

⭐ **If this project helped you, please star it on GitHub!**
