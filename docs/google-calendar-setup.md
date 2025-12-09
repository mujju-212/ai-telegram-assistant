# 📅 Google Calendar Setup Guide

This guide will help you set up Google Calendar API access for the task assistant.

---

## 📋 What You'll Get

- ✅ Google Calendar API enabled
- ✅ OAuth 2.0 credentials
- ✅ Calendar connected to n8n

---

## Prerequisites

- Google Account
- Access to Google Cloud Console
- n8n instance ready

---

## Step 1: Access Google Cloud Console

1. Go to [https://console.cloud.google.com/](https://console.cloud.google.com/)
2. Sign in with your Google Account
3. Accept Terms of Service if prompted

---

## Step 2: Create a New Project

### 2.1 Create Project

1. Click the **project dropdown** at the top (next to "Google Cloud")
2. Click **"New Project"**
3. Enter project details:
   - **Project name:** `AI Task Assistant` (or your preferred name)
   - **Organization:** Leave as default (No organization)
   - **Location:** Leave as default
4. Click **"Create"**
5. Wait for project creation (may take 30-60 seconds)

### 2.2 Select Your Project

1. Click the project dropdown again
2. Select your newly created project

---

## Step 3: Enable Google Calendar API

### 3.1 Navigate to APIs & Services

1. Click the **☰ hamburger menu** (top-left)
2. Go to **"APIs & Services"** → **"Library"**

### 3.2 Search and Enable

1. In the search bar, type: `Google Calendar API`
2. Click on **"Google Calendar API"** from results
3. Click **"Enable"**
4. Wait for activation (few seconds)

---

## Step 4: Create OAuth 2.0 Credentials

### 4.1 Configure OAuth Consent Screen

1. Go to **APIs & Services** → **"OAuth consent screen"**
2. Select **"External"** (unless you have a Google Workspace)
3. Click **"Create"**

### 4.2 Fill OAuth Consent Form

**App Information:**
- **App name:** `AI Task Assistant`
- **User support email:** Your email address
- **App logo:** (Optional - skip for now)

**App Domain:**
- Leave blank for now (optional)

**Authorized domains:**
- Leave blank for now

**Developer contact information:**
- **Email addresses:** Your email address

4. Click **"Save and Continue"**

### 4.3 Scopes

1. Click **"Add or Remove Scopes"**
2. Manually add these scopes:
   ```
   https://www.googleapis.com/auth/calendar
   https://www.googleapis.com/auth/calendar.events
   ```
3. Click **"Update"**
4. Click **"Save and Continue"**

### 4.4 Test Users (Important!)

1. Click **"+ Add Users"**
2. Enter your Google email address
3. Click **"Add"**
4. Click **"Save and Continue"**

### 4.5 Summary

1. Review the summary
2. Click **"Back to Dashboard"**

---

## Step 5: Create OAuth 2.0 Client ID

### 5.1 Go to Credentials

1. Click **"APIs & Services"** → **"Credentials"**
2. Click **"+ Create Credentials"**
3. Select **"OAuth client ID"**

### 5.2 Configure OAuth Client

1. **Application type:** Select **"Web application"**
2. **Name:** `n8n Task Assistant`

### 5.3 Set Authorized Redirect URIs

**For n8n Cloud:**
```
https://YOUR_N8N_INSTANCE.app.n8n.cloud/rest/oauth2-credential/callback
```
Replace `YOUR_N8N_INSTANCE` with your actual n8n cloud URL

**For Self-Hosted n8n:**
```
http://localhost:5678/rest/oauth2-credential/callback
```

Or if using a custom domain:
```
https://yourdomain.com/rest/oauth2-credential/callback
```

4. Click **"Create"**

### 5.4 Download Credentials

1. A dialog will appear with your credentials
2. Click **"Download JSON"**
3. Save the file as `google-credentials.json`

**⚠️ Important:** Keep this file secure! It contains sensitive data.

---

## Step 6: Configure in n8n

### 6.1 Add Google Calendar OAuth2 Credential

1. Open n8n
2. Go to **Settings** → **Credentials**
3. Click **"+ Add Credential"**
4. Search for **"Google Calendar OAuth2 API"**

### 6.2 Upload Credentials

**Method 1: Upload JSON File**
1. Click **"Upload JSON"** or similar option
2. Select your `google-credentials.json` file

**Method 2: Manual Entry**
1. Open `google-credentials.json` in a text editor
2. Copy the **Client ID** and **Client Secret**
3. Paste them into n8n

### 6.3 Authorize n8n

1. Click **"Connect my account"** or **"Authorize"**
2. You'll be redirected to Google sign-in
3. Sign in with your Google account
4. You may see "This app isn't verified" warning
   - Click **"Advanced"**
   - Click **"Go to [App Name] (unsafe)"** (it's safe - it's your app!)
5. Review permissions:
   - "See, edit, share, and permanently delete all calendars"
6. Click **"Allow"**
7. You'll be redirected back to n8n
8. Click **"Save"**

---

## Step 7: Select Your Calendar

### 7.1 Find Calendar ID

1. Open [Google Calendar](https://calendar.google.com/)
2. Click the **⚙️ gear icon** (top-right) → **"Settings"**
3. In the left sidebar, find your calendar under **"Settings for my calendars"**
4. Click your calendar name
5. Scroll down to **"Integrate calendar"**
6. Copy the **Calendar ID**
   - Usually looks like: `yourname@gmail.com`
   - Or: `random-string@group.calendar.google.com`

### 7.2 Configure in n8n Workflows

When setting up workflow nodes:
1. In Google Calendar nodes, select your credential
2. Set **Calendar ID** to your copied calendar ID

---

## Step 8: Test the Connection

### 8.1 Test in n8n

1. Open Workflow A in n8n
2. Find the **"Get Calendar Events"** node
3. Click **"Execute Node"**
4. You should see your calendar events (if any)

### 8.2 Troubleshooting Test

If test fails:
- ✅ Check credential is selected
- ✅ Check calendar ID is correct
- ✅ Check API is enabled in Google Cloud
- ✅ Check OAuth consent screen is configured
- ✅ Check your email is added as test user

---

## Common Issues

### "This app isn't verified"

- **Problem:** Google shows security warning
- **Solution:** 
  - Click "Advanced" → "Go to [App] (unsafe)"
  - This is normal for personal projects
  - Your app is safe (you created it!)

### "Access blocked: Authorization Error"

- **Problem:** Can't authorize in n8n
- **Solution:**
  - Ensure your email is added as a test user
  - Check OAuth consent screen is published
  - Verify redirect URI matches exactly

### "Calendar not found"

- **Problem:** Can't access calendar in workflow
- **Solution:**
  - Double-check calendar ID
  - Ensure calendar isn't deleted
  - Verify credential is authorized

### "Insufficient permissions"

- **Problem:** Can't create/edit events
- **Solution:**
  - Re-authorize credential
  - Check scopes include `.calendar` and `.calendar.events`
  - Revoke and re-grant access

---

## Calendar Permissions

### What n8n Can Do

With the configured permissions, n8n can:
- ✅ Read all calendar events
- ✅ Create new events
- ✅ Update existing events
- ✅ Delete events
- ✅ Check for conflicts

### Security Note

- 🔐 OAuth tokens are stored securely in n8n
- 🔐 Tokens expire and refresh automatically
- 🔐 You can revoke access anytime in [Google Account Settings](https://myaccount.google.com/permissions)

---

## Advanced Configuration

### Multiple Calendars

To use multiple calendars:
1. Get each calendar's ID
2. In workflows, specify which calendar ID to use
3. Can read from one, write to another

### Shared Calendars

To access shared calendars:
1. Ensure calendar is shared with your Google account
2. Get the shared calendar's ID
3. Use that ID in n8n nodes

### Calendar Colors

To set event colors:
1. In "Create Calendar Event" node
2. Add field "Color ID"
3. Values: 1-11 (different colors)

---

## API Quotas & Limits

Google Calendar API has these limits:
- **Queries per day:** 1,000,000 (more than enough!)
- **Queries per second:** 10
- **Rate limit:** Usually not an issue for personal use

---

## Resources

- 📚 [Google Calendar API Docs](https://developers.google.com/calendar/api/guides/overview)
- 🔐 [OAuth 2.0 Guide](https://developers.google.com/identity/protocols/oauth2)
- ⚙️ [Google Cloud Console](https://console.cloud.google.com/)

---

## Next Steps

✅ You now have:
- Google Calendar API enabled
- OAuth credentials configured
- n8n connected to your calendar

📚 Next, set up:
- [Notion Database →](notion-setup.md)

---

**Previous:** [← Telegram Setup](telegram-setup.md) | **Next:** [Notion Setup →](notion-setup.md)
