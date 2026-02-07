# 🤖 AI Customer Service Chatbot v2.0

**A free, ready-to-use AI chatbot for ANY business** — built with n8n and Groq (100% free AI).

Works for restaurants, auto shops, salons, clinics, agencies, stores, and any service business!

![n8n](https://img.shields.io/badge/n8n-workflow-orange) ![Groq](https://img.shields.io/badge/AI-Groq%20Llama%203.3-blue) ![License](https://img.shields.io/badge/license-MIT-green)

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| 🤖 **AI-Powered Chat** | Groq Llama 3.3 70B — smart, fast, and **completely free** |
| 📋 **Lead Capture** | Automatically saves customer info to Google Sheets |
| 🎯 **Priority Scoring** | AI classifies leads as Hot / Warm / Normal |
| 💡 **Customer Intent** | AI writes exactly what the customer wants in one line |
| 🚨 **Urgent Alerts** | Instant email for complaints, emergencies, cancellations |
| 📧 **Chat Summaries** | Email recap when conversation ends |
| 🧠 **Memory** | Remembers conversation context per session |
| 🌍 **Bilingual** | Auto-responds in the customer's language |
| 💬 **Natural Collection** | Asks for info one field at a time (not a boring form) |

---

## 🏗️ Architecture

```
Customer → [Chat Widget] → [Chat Trigger] → [AI Agent] → [Respond to Webhook]
                                                  │
                                    ┌──────────────┼──────────────┐
                                    ▼              ▼              ▼
                            Save Lead Tool   Urgent Alert   Chat Summary
                                    │              │              │
                           ┌────────┴────────┐     │              │
                           ▼                 ▼     ▼              ▼
                    [Sub-Workflow]     [Sub-Workflow]     [Sub-Workflow]
                           │                 │              │
                    ┌──────┴──────┐          │              │
                    ▼             ▼          ▼              ▼
              Google Sheets   Response    Gmail Alert   Gmail Summary
```

**Main Workflow** — AI brain + chat interface + 3 tool connections
**Sub-Workflow** — Handles all actions: save leads, send emails, route by action type

---

## 📋 Prerequisites

| Tool | Purpose | Cost |
|------|---------|------|
| [n8n](https://n8n.io) | Workflow automation | Free (self-hosted) or $20/mo (cloud) |
| [Groq](https://console.groq.com) | AI model (Llama 3.3 70B) | **Free** |
| [Google Sheets](https://sheets.google.com) | Lead storage | Free |
| [Gmail](https://gmail.com) | Alert & summary emails | Free |

---

## 🚀 Installation (4 Steps)

### Step 1: Import Sub-Workflow

1. Go to your n8n instance
2. Click **"Add Workflow"** → **"Import from file"**
3. Select `AI_Chatbot_Sub_Actions.json`
4. **Copy the workflow ID** from the URL (e.g., `pnO4Wv0JHltaV0r8`)

### Step 2: Import Main Workflow

1. Import `AI_Chatbot_Main.json`
2. Open each of the **3 Tool nodes** (Save Lead Tool, Urgent Alert Tool, Conversation Summary Tool)
3. Replace `YOUR_SUB_WORKFLOW_ID` with the ID you copied in Step 1

### Step 3: Configure Credentials & Settings

**In the Sub-Workflow:**

| Node | What to Change |
|------|----------------|
| **Google Sheets node** | Replace `YOUR_GOOGLE_SHEET_ID` with your sheet ID |
| **Gmail nodes** (×2) | Replace `YOUR_EMAIL@gmail.com` with your email |
| **Extract Query Data** | Change timezone if not in Toronto (line 42) |

**Add credentials:**
- **Groq API** → Get free key at [console.groq.com](https://console.groq.com) → add to Main workflow's Groq node
- **Google Sheets OAuth** → Add to Sub-workflow's Google Sheets node
- **Gmail OAuth** → Add to Sub-workflow's Gmail nodes

### Step 4: Create Google Sheet

Create a new Google Sheet with a tab named **"Leads"** and these column headers in Row 1:

```
Timestamp | Name | Phone | Email | Service Interest | Customer Intent | Priority | Notes | Session ID | Source
```

> 💡 **Tip:** Copy the Sheet ID from the URL: `docs.google.com/spreadsheets/d/`**`THIS_IS_YOUR_SHEET_ID`**`/edit`

---

## 🎨 Customize for YOUR Business

Open the **AI Agent** node in the Main Workflow and edit the System Message:

```
## BUSINESS INFO (CUSTOMIZE THIS)
Business Name: [Your Business Name]       ← Change this
Location: [Your City, Country]             ← Change this
Phone: [Your Phone]                        ← Change this
Email: [Your Email]                        ← Change this
Website: [Your Website]                    ← Change this

## SERVICES (CUSTOMIZE THIS)
- Service 1: [Description]                 ← List your services
- Service 2: [Description]
- Service 3: [Description]
```

### Business Examples

**Restaurant:**
```
Business Name: Bella Italia Restaurant
Services:
- Dine-in: Italian fine dining, reservation for 2-50 guests
- Takeout: Full menu available for pickup
- Catering: Corporate events and private parties
```

**Auto Shop:**
```
Business Name: QuickFix Auto Center
Services:
- Oil Change & Maintenance: All vehicle types
- Brake Service: Inspection, repair, replacement
- Tire Shop: New tires, rotation, alignment
```

**Hair Salon:**
```
Business Name: Style Studio
Services:
- Haircuts: Men, women, children
- Color & Highlights: Full color, balayage, ombre
- Treatments: Keratin, deep conditioning, scalp therapy
```

**Web Agency:**
```
Business Name: Digital Pro Agency
Services:
- Web Design: Custom WordPress & e-commerce sites
- SEO: Technical SEO, content optimization, local SEO
- Digital Marketing: Social media, Google Ads, email campaigns
```

---

## 📊 Google Sheet Columns Explained

| Column | Example | Description |
|--------|---------|-------------|
| **Timestamp** | `2026-02-07 14:30:25` | Auto-generated (Toronto timezone by default) |
| **Name** | `John Smith` | Customer's name |
| **Phone** | `905-555-1234` | Customer's phone number |
| **Email** | `john@example.com` | Customer's email |
| **Service Interest** | `Web Design` | General service category |
| **Customer Intent** | `Needs e-commerce site for bakery, wants online ordering and delivery integration` | AI-written detailed description of what they want |
| **Priority** | `Hot` / `Warm` / `Normal` | AI-determined urgency level |
| **Notes** | `Currently using Wix, unhappy with speed` | Additional context from conversation |
| **Session ID** | `exec-abc123` | Unique conversation identifier |
| **Source** | `Chatbot` | Always "Chatbot" (useful if you add other lead sources later) |

### Priority Levels

| Priority | Meaning | Example |
|----------|---------|---------|
| 🔴 **Hot** | Wants service NOW / this week | "I need this done ASAP" |
| 🟡 **Warm** | Interested, comparing options | "What are your prices?" |
| 🟢 **Normal** | Just asking questions | "Do you offer X service?" |

---

## ⏰ Change Timezone

In the **Sub-Workflow → Extract Query Data** node, find this line:

```javascript
const local = new Date(now.toLocaleString('en-US', { timeZone: 'America/Toronto' }));
```

Replace `America/Toronto` with your timezone:

| City | Timezone |
|------|----------|
| New York | `America/New_York` |
| Chicago | `America/Chicago` |
| Denver | `America/Denver` |
| Los Angeles | `America/Los_Angeles` |
| London | `Europe/London` |
| Paris | `Europe/Paris` |
| Dubai | `Asia/Dubai` |
| Riyadh | `Asia/Riyadh` |
| Tokyo | `Asia/Tokyo` |
| Sydney | `Australia/Sydney` |

---

## 🤖 Change AI Model

In the Main Workflow → **Groq Chat Model** node, change the model:

| Model | Speed | Quality | Best For |
|-------|-------|---------|----------|
| `llama-3.3-70b-versatile` | ⚡⚡ | ⭐⭐⭐⭐⭐ | Best quality (default) |
| `llama-3.1-8b-instant` | ⚡⚡⚡⚡ | ⭐⭐⭐ | Fastest response |
| `mixtral-8x7b-32768` | ⚡⚡⚡ | ⭐⭐⭐⭐ | Good balance |

> All models are **free** on Groq with generous rate limits.

---

## 🧪 Testing

1. **Activate BOTH workflows** (Main + Sub)
2. Open the Main Workflow's **Chat Trigger** node
3. Copy the chat URL (or click the chat icon in n8n)
4. Start a conversation!

**Test conversation:**
```
You: Hi, I'm interested in your services
Bot: Welcome! I'd be happy to help. What service are you interested in?
You: I need [your service]
Bot: Great choice! Could I get your name please?
You: John
Bot: Thanks John! What's the best phone number to reach you?
You: 555-123-4567
Bot: And your email address so we can send you details?
You: john@test.com
Bot: [Saves lead to Google Sheets with Priority + Intent]
```

---

## 🔧 Troubleshooting

| Problem | Solution |
|---------|----------|
| Chatbot not responding | Make sure BOTH workflows are **active** (green toggle) |
| Leads not saving | Check Google Sheets credential + Sheet ID in sub-workflow |
| No emails received | Check Gmail credential + email address in sub-workflow |
| Wrong timestamp | Change timezone in Extract Query Data node (see above) |
| Session ID shows `$json.sessionId` | Already fixed in v2.0! Uses `$execution.id` instead |
| Timestamp shows `YYYY-` prefix | Already fixed in v2.0! Uses JavaScript Date instead of Luxon |
| Sub-workflow not found | Make sure Tool nodes have the correct Sub-Workflow ID |
| Rate limit errors | Groq free tier has limits — switch to `llama-3.1-8b-instant` for lower usage |

---

## 📁 Files Included

| File | Description |
|------|-------------|
| `AI_Chatbot_Main.json` | Main workflow — AI Agent, Chat Trigger, Tool connections |
| `AI_Chatbot_Sub_Actions.json` | Sub-workflow — Google Sheets, Gmail alerts, action routing |
| `README.md` | This setup guide |

---

## 🔒 Security Notes

- ✅ All credentials are **removed** from the export files — you add your own
- ✅ Session data is isolated per conversation
- ✅ No customer data is stored in the workflow itself
- ✅ The `allowedOrigins: "*"` setting allows any website to embed the chat — restrict this in production to your domain only

---

## 🌟 What's New in v2.0

| Feature | v1.0 | v2.0 |
|---------|------|------|
| Timestamp | ❌ Showed `YYYY-` prefix | ✅ Correct `2026-02-07 14:30:25` |
| Session ID | ❌ Showed `$json.sessionId` literal | ✅ Uses unique execution ID |
| Priority | ❌ Not available | ✅ Hot / Warm / Normal |
| Customer Intent | ❌ Not available | ✅ AI writes what customer wants |
| Lead Collection | ❌ Asked all at once | ✅ One field at a time (natural) |
| AI Model | ❌ Llama 3.1 8B | ✅ Llama 3.3 70B (much smarter) |
| Switch Node | ❌ Case-sensitive | ✅ Case-insensitive matching |

---

## 📄 License

MIT License — use it, modify it, share it, sell it. No restrictions.

---

**Created by [Adobely](https://adobely.com)** 🇨🇦

If this helped you, star ⭐ the repo!
