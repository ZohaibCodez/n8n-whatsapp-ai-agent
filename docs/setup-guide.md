# Setup Guide - WhatsApp AI Agent

This guide will walk you through setting up your WhatsApp AI Agent from scratch.

---

## 📋 Prerequisites Checklist

* [ ] n8n instance (self-hosted or cloud)
* [ ] Meta Developer Account
* [ ] Google Cloud Account (for Gemini API)
* [ ] WhatsApp Business Account
* [ ] Domain with valid SSL certificate (required for WhatsApp webhooks)

---

## 🏗️ Step-by-Step Setup

### 1. WhatsApp Business API Setup

#### A. Create Meta Developer Account

1. Go to [developers.facebook.com](https://developers.facebook.com)
2. Click **Get Started**
3. Complete account verification

#### B. Create WhatsApp Business App

1. Create a new **Business** app
2. Add the **WhatsApp** product
3. Note your **App ID** and **App Secret**

#### C. Get Phone Number ID & Access Token

1. Go to **WhatsApp → Getting Started**
2. Select or add a phone number
3. Copy the **Phone Number ID**
4. Generate an **access token**

   * ⚠️ Temporary tokens expire in **24 hours**
   * For production, generate a **permanent access token** in Meta Business Settings

#### D. Configure Webhook

1. In WhatsApp settings, add webhook URL:

   ```
   https://your-n8n-domain.com/webhook/{workflow-id}/whatsapp
   ```

   (replace `{workflow-id}` with your workflow’s actual ID or custom path)
2. Set a **secure random verification token**
3. Subscribe to the **messages** field

---

### 2. Google Gemini API Setup

#### A. Create Google Cloud Project

1. Go to [Google AI Studio](https://makersuite.google.com)
2. Create a new project or select an existing one
3. Enable **Gemini API**

#### B. Generate API Key

1. Go to **API Keys** section
2. Create a new API key
3. Restrict it to **Gemini API** (recommended)
4. Store the key securely

---

### 3. n8n Configuration

#### A. Import Workflow

1. Download `whatsapp-agent.json` from this repository
2. In n8n: **Workflows → Import from File**
3. Select the JSON file

#### B. Configure WhatsApp Credentials

1. In n8n, go to **Credentials → Add Credential**
2. Select **WhatsApp**
3. Enter:

   * **Phone Number ID** (from Meta setup)
   * **Access Token** (temporary or permanent)
   * **Webhook Verification Token** (your chosen string)

#### C. Configure Google Gemini Credentials

1. Add a new credential: **Google PaLM API** *(used in n8n for Gemini)*
2. Enter your **Gemini API key** from Google AI Studio

#### D. Configure HTTP Header Auth (for media downloads)

1. Add a credential: **Header Auth**
2. Header Name: `Authorization`
3. Header Value:

   ```
   Bearer YOUR_WHATSAPP_ACCESS_TOKEN
   ```

---

### 4. Test Your Setup

#### A. Activate Workflow

1. Ensure the workflow is **active** in n8n
2. Verify the webhook endpoint is **publicly accessible over HTTPS**

#### B. Test Webhook

* Use **Meta Developer Console → Test Webhook** to verify connection
* Ensure the verification token matches

#### C. Send Test Messages

1. Send **“Hello”** to your WhatsApp Business number
2. Check n8n execution logs for errors
3. Test multi-modal features:

   * **Voice** → send a voice note
   * **Image** → send image with or without caption
   * **Text** → send a normal message

---

## 🔧 Configuration Options

### AI Personality

Edit the system message in the **AI Agent node**:

```text
You are a helpful assistant called [YOUR_BOT_NAME].
Respond in a natural and friendly tone.
[Add specific instructions for your use case]
```

### Memory Settings

* **Context Window Length**: Number of messages to remember (default: 20)
* **Session Key**: Uses WhatsApp ID for user-specific conversations

### Media Processing

* **Audio**: WhatsApp voice messages → Gemini transcription
* **Images**: Analyzed and described by Gemini Vision
* **Text**: Direct AI processing with conversation memory

---

## 🚨 Troubleshooting

### Webhook Issues

**Problem**: Webhook not receiving messages
**Solutions**:

* Verify your n8n webhook URL is public and **HTTPS-enabled**
* Ensure SSL certificate is valid
* Check that verification token matches
* Confirm webhook subscription is active in Meta Console

### Authentication Errors

**Problem**: API authentication failing
**Solutions**:

* Confirm WhatsApp token hasn’t expired (temporary = 24h)
* Verify Gemini API key permissions
* Check that credentials are correctly configured in n8n

### Media Download Issues

**Problem**: Audio/image processing fails
**Solutions**:

* Check media URL accessibility
* Confirm Authorization header is correct
* Ensure media file size is within WhatsApp limits
* Verify n8n server can reach Meta endpoints

### Memory Issues

**Problem**: Bot not remembering context
**Solutions**:

* Verify **Session Key** uses WhatsApp ID
* Ensure **Memory node** is connected properly
* Adjust **context window length** if needed

---

## 🔐 Security Best Practices

### API Security

* Store all secrets in **n8n credentials** (never hardcode)
* Rotate tokens regularly
* Monitor API usage and costs

### Webhook Security

* Always use a **verification token**
* HTTPS is mandatory for WhatsApp webhooks
* Optionally validate request signatures
* Whitelist Meta’s IP ranges if possible

### Data Privacy

* Define message retention policies
* Implement user data deletion
* Ensure compliance with GDPR/privacy laws
* Log access and usage securely

---

## 📈 Production Considerations

### Scaling

* Monitor CPU/RAM usage of n8n
* Use horizontal scaling for high traffic
* Implement queue management for large media files

### Reliability

* Add retry logic for API failures
* Use monitoring and alerts for webhook health
* Create backup/restore procedures for workflows

### Cost Optimization

* Track Gemini & WhatsApp API usage
* Cache responses when possible
* Optimize workflows for media-heavy cases
* Set quotas/alerts for API consumption

---

## 📞 Support

If you encounter issues:

1. Check this guide’s troubleshooting section
2. Review **n8n execution logs**
3. Check Meta & Google API status pages
4. Open a GitHub Issue with:

   * Error messages
   * Execution log snippets
   * Relevant configuration (without secrets)

---

## 🎯 Next Steps

Once your basic setup is working:

1. Customize your AI personality
2. Add more media type support (e.g., documents, video)
3. Build user management features
4. Add analytics and monitoring dashboards
5. Extend integration to other platforms (Telegram, Slack, etc.)