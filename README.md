# 📧 JobHunt Mailer

> An automated n8n workflow for sending personalized job application emails at scale.

[![n8n](https://img.shields.io/badge/n8n-workflow-orange)](https://n8n.io)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

## 🎯 Overview

JobHunt Mailer is a powerful automation workflow built with [n8n](https://n8n.io) that streamlines your job search process by automatically sending personalized application emails to multiple companies using data from Google Sheets.

### ✨ Features

- 📊 **Google Sheets Integration** - Import candidate/company data from spreadsheets
- ✉️ **Personalized Emails** - Dynamic content insertion (name, company, etc.)
- 🔄 **Batch Processing** - Send emails one by one with controlled rate limiting
- 🔐 **Secure OAuth** - Uses Gmail OAuth2 for secure email sending
- 🎛️ **Manual Control** - Trigger workflow only when you're ready

## 🏗️ Workflow Architecture

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│  Manual Trigger │────▶│  Google Sheets  │────▶│ Split In Batches│
│   (Start Here)  │     │  (Read Data)    │     │ (Process 1 by 1)│
└─────────────────┘     └─────────────────┘     └─────────────────┘
                                                          │
                              ┌─────────────────┐     ┌─────────────────┐
                              │   Send Email    │◄────│ Set Email Content│
                              │  (Gmail OAuth)  │     │  (Personalize)   │
                              └─────────────────┘     └─────────────────┘
```

## 📋 Prerequisites

- [n8n](https://n8n.io) instance (cloud or self-hosted)
- Google Cloud Project with Sheets API enabled
- Gmail account with OAuth2 credentials
- Google Sheet with candidate data

## 🚀 Setup Instructions

### 1. Google Sheets Setup

Create a Google Sheet with the following columns:

| email | name | company |
|-------|------|---------|
| hr@company1.com | John Doe | Tech Corp |
| hiring@startup.io | Jane Smith | StartupXYZ |

**Note:** Replace `PASTE_YOUR_SHEET_ID_HERE` in the workflow with your actual Sheet ID.

### 2. Google Sheets Credentials

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create OAuth2 credentials for Google Sheets API
3. In n8n, create new credentials:
   - **Name:** `Google Sheets OAuth`
   - **ID:** Update `YOUR_CREDENTIAL_ID` in the workflow

### 3. Gmail Credentials

1. Enable Gmail API in Google Cloud Console
2. Create OAuth2 credentials for Gmail
3. In n8n, create new credentials:
   - **Name:** `Gmail OAuth`
   - **ID:** Update `YOUR_GMAIL_CREDENTIAL_ID` in the workflow

### 4. Customize Email Template

Edit the **"Set Email Content"** node to personalize your message:

```javascript
{
  "to": "={{$json[\"email\"]}}",
  "subject": "Application for Software Developer Role",
  "body": "Hi {{$json[\"name\"] || \"Hiring Team\"}},
  
  I'm writing to apply for a Software Developer role at {{$json[\"company\"] || \"your organization\"}}.
  
  [Your custom message here]
  
  Resume: YOUR_RESUME_LINK
  
  Best regards,
  Your Name"
}
```

## ⚙️ Configuration Variables

| Variable | Description | Location |
|----------|-------------|----------|
| `sheetId` | Google Sheet ID | Google Sheets node |
| `range` | Sheet range to read | Google Sheets node (default: `Sheet1!A:C`) |
| `batchSize` | Emails per batch | Split In Batches node (default: 1) |
| `credentials` | OAuth credentials | Both Google Sheets & Gmail nodes |

## 🎮 Usage

1. **Import Workflow:** Copy the JSON into n8n workflow editor
2. **Configure Credentials:** Set up OAuth connections
3. **Prepare Data:** Fill your Google Sheet with target companies
4. **Customize Message:** Edit the email template in "Set Email Content" node
5. **Execute:** Click "Execute Workflow" to start sending

## ⚠️ Important Notes

- **Rate Limiting:** Gmail has sending limits (500 emails/day for personal accounts)
- **Spam Prevention:** Ensure your emails are personalized and comply with CAN-SPAM laws
- **Testing:** Always test with your own email first before bulk sending
- **Privacy:** Handle recipient data responsibly and securely

## 🔧 Customization Ideas

- ⏰ **Schedule Trigger:** Replace Manual Trigger with Schedule Trigger for timed sends
- 📎 **Attachments:** Add resume attachment handling
- 📊 **Tracking:** Integrate with Airtable/Notion to track application status
- 🤖 **AI Enhancement:** Add OpenAI node to generate custom cover letters

## 📝 License

MIT License - feel free to use and modify for your job search!

## 🤝 Contributing

Found a bug or have a feature idea? Open an issue or submit a PR!

---

**Happy Job Hunting! 🚀**
