# 📊 Notion Database Setup Guide

This guide will help you create and configure a Notion database for task management.

---

## 📋 What You'll Get

- ✅ Notion database with proper structure
- ✅ Notion integration token
- ✅ Database connected to n8n

---

## Prerequisites

- Notion account (free tier works!)
- Access to Notion workspace

---

## Step 1: Create Notion Database

### Option A: Use Notion AI (Fastest) ⚡

1. Open [Notion](https://www.notion.so/)
2. Click **"+ New page"**
3. Press `/` to open the command menu
4. Type `AI` and select **"Ask AI to write"**
5. Paste this prompt:

```
Create a task management database with the following properties:

1. Name (Title) - the default title field
2. Status (Select) - with options: Not Started, In Progress, Completed
3. Priority (Select) - with options: High, Medium, Low
4. Category (Select) - with options: Work, Personal, Health, Learning, Home, Family
5. Due Date (Date) - date property
6. Duration (min) (Number) - number property for task duration in minutes
7. Calendar Link (URL) - URL property for Google Calendar link
8. Created By (Text) - text property for creator name

Make it a table view with all columns visible.
```

6. Wait for AI to create the database
7. Review and adjust if needed

### Option B: Manual Setup 🔧

#### 1.1 Create New Database Page

1. Open [Notion](https://www.notion.so/)
2. In your workspace, click **"+ New page"**
3. Give it a name: `AI Tasks` or `Task Manager`
4. Type `/table` and select **"Table - Inline"**

#### 1.2 Add Properties

Click the **"+" button** next to the last column to add each property:

| Property Name | Type | Configuration |
|--------------|------|---------------|
| **Name** | Title | (Default - already exists) |
| **Status** | Select | Options: `Not Started`, `In Progress`, `Completed` |
| **Priority** | Select | Options: `High`, `Medium`, `Low` |
| **Category** | Select | Options: `Work`, `Personal`, `Health`, `Learning`, `Home`, `Family` |
| **Due Date** | Date | Leave default settings |
| **Duration (min)** | Number | Format: Number |
| **Calendar Link** | URL | Leave default |
| **Created By** | Text | Leave default |

#### 1.3 Configure Select Properties

**For Status:**
1. Click the property header
2. Add options:
   - `Not Started` (color: Gray)
   - `In Progress` (color: Blue)
   - `Completed` (color: Green)

**For Priority:**
1. Click the property header
2. Add options:
   - `High` (color: Red)
   - `Medium` (color: Yellow)
   - `Low` (color: Green)

**For Category:**
1. Click the property header
2. Add options:
   - `Work` (color: Blue)
   - `Personal` (color: Purple)
   - `Health` (color: Red)
   - `Learning` (color: Yellow)
   - `Home` (color: Green)
   - `Family` (color: Pink)

---

## Step 2: Get Database ID

### 2.1 Share Database

1. Click the **"Share"** button (top-right of your database)
2. Click **"Copy link"**

### 2.2 Extract Database ID

The copied link will look like:
```
https://www.notion.so/workspace-name/DATABASE_ID?v=VIEW_ID
```

**Extract the Database ID:**
- It's the 32-character string between the last `/` and `?`
- Example: `2c40d73ab97b8018ae73f28e20099d86`

**Format for n8n:**
- Remove any hyphens (-)
- Should be exactly 32 characters
- All lowercase letters and numbers

### 2.3 Save Your Database ID

```
Notion Database ID: 2c40d73ab97b8018ae73f28e20099d86
```

---

## Step 3: Create Notion Integration

### 3.1 Access Integration Settings

1. Go to [https://www.notion.so/my-integrations](https://www.notion.so/my-integrations)
2. Click **"+ New integration"**

### 3.2 Configure Integration

**Basic Information:**
- **Name:** `n8n Task Bot` (or your preferred name)
- **Logo:** (Optional - upload an icon)
- **Associated workspace:** Select your workspace

**Capabilities:**
- ✅ Read content
- ✅ Update content
- ✅ Insert content

**Comment Capabilities:**
- (Leave unchecked unless needed)

**User Information:**
- (Leave unchecked unless needed)

3. Click **"Submit"**

### 3.3 Copy Integration Token

1. After creation, you'll see your **"Internal Integration Token"**
2. Click **"Show"** → **"Copy"**
3. Save it securely:
   ```
   Notion Integration Token: secret_abc123...
   ```

**⚠️ Important:** Keep this token secret! Anyone with it can access your Notion.

---

## Step 4: Connect Integration to Database

### 4.1 Open Your Database

1. Go back to your `AI Tasks` database in Notion

### 4.2 Add Connection

1. Click the **"⋯"** (three dots) at the top-right
2. Scroll down and click **"+ Add connections"**
3. Search for your integration name (e.g., `n8n Task Bot`)
4. Click to select it
5. Click **"Confirm"**

✅ Your database is now connected to the integration!

---

## Step 5: Configure in n8n

### 5.1 Add Notion API Credential

1. Open n8n
2. Go to **Settings** → **Credentials**
3. Click **"+ Add Credential"**
4. Search for **"Notion API"**
5. Select **"Notion API"**

### 5.2 Enter Token

1. Paste your **Internal Integration Token**
2. Click **"Save"**

### 5.3 Test Connection

1. Go to Workflow A in n8n
2. Find the **"Create Notion Task"** node
3. Select your Notion credential
4. Enter your database ID
5. Click **"Execute Node"** to test

---

## Step 6: Verify Database Structure

Your database should have these columns:

```
┌─────────────┬──────────┬──────────┬──────────┬──────────┬──────────────┬───────────────┬────────────┐
│ Name        │ Status   │ Priority │ Category │ Due Date │ Duration(min)│ Calendar Link │ Created By │
├─────────────┼──────────┼──────────┼──────────┼──────────┼──────────────┼───────────────┼────────────┤
│ (Title)     │ (Select) │ (Select) │ (Select) │ (Date)   │ (Number)     │ (URL)         │ (Text)     │
└─────────────┴──────────┴──────────┴──────────┴──────────┴──────────────┴───────────────┴────────────┘
```

---

## Step 7: Test Adding a Task

### 7.1 Manual Test

Add a test task manually to verify structure:
- **Name:** Test Task
- **Status:** Not Started
- **Priority:** Medium
- **Category:** Personal
- **Due Date:** Tomorrow
- **Duration (min):** 60
- **Calendar Link:** (Leave blank)
- **Created By:** Manual Test

### 7.2 Test from n8n

1. Send a message to your Telegram bot: `Add test task tomorrow at 2 PM`
2. Check your Notion database
3. A new task should appear with all details filled in

---

## Common Issues

### Can't Find Database ID

- **Problem:** Link doesn't contain database ID
- **Solution:**
  1. Open database as a full page (not inline)
  2. Copy link from browser address bar
  3. ID should be visible in URL

### Integration Not Showing

- **Problem:** Can't find integration when adding connection
- **Solution:**
  - Make sure you created the integration
  - Check you're in the correct workspace
  - Try refreshing the page

### "Database not found" Error

- **Problem:** n8n can't access database
- **Solution:**
  - Verify database ID is correct (32 characters, no hyphens)
  - Ensure integration is connected to database
  - Check token is valid

### Property Type Mismatch

- **Problem:** Task creation fails due to property type
- **Solution:**
  - Verify property names match exactly (case-sensitive)
  - Check property types are correct
  - Review Notion API error message for details

---

## Database Best Practices

### Organization Tips

1. **Use Views:**
   - Create filtered views (by Status, Priority, Category)
   - Add calendar view for Due Date
   - Create board view for Status

2. **Add Formulas:**
   - Calculate days until due date
   - Track completion percentage
   - Auto-categorize by date

3. **Use Templates:**
   - Create task templates
   - Save common task structures
   - Speed up manual entry

### Maintenance

1. **Regular Cleanup:**
   - Archive completed tasks monthly
   - Remove old/cancelled tasks
   - Update categories as needed

2. **Monitor Usage:**
   - Check for duplicate tasks
   - Verify all properties are used
   - Optimize for your workflow

---

## Advanced Features

### Notion Relations

Link tasks to other databases:
1. Create a "Projects" database
2. Add a Relation property to Tasks
3. Link tasks to projects

### Rollups

Calculate aggregates:
1. Add Rollup property
2. Summarize related data
3. Track project totals

### Formulas

Create calculated fields:
```
// Days until due
dateBetween(prop("Due Date"), now(), "days")

// Status emoji
if(prop("Status") == "Completed", "✅", 
   if(prop("Status") == "In Progress", "🔄", "⏸️"))
```

---

## API Limits

Notion API limits (rarely an issue):
- **Rate limit:** 3 requests per second
- **Payload size:** 1000 blocks per request
- **Database properties:** 50 properties max

---

## Resources

- 📚 [Notion API Documentation](https://developers.notion.com/)
- 🔧 [Notion Integration Guide](https://www.notion.so/help/create-integrations-with-the-notion-api)
- 💡 [Notion Templates](https://www.notion.so/templates)

---

## Next Steps

✅ You now have:
- Notion database created
- Integration token configured
- Database connected to n8n

📚 Next, set up:
- [DeepSeek API →](deepseek-setup.md)

---

**Previous:** [← Google Calendar Setup](google-calendar-setup.md) | **Next:** [DeepSeek Setup →](deepseek-setup.md)
