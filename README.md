# Automated Email Marketing Pipeline
### n8n + ChatGPT + Gmail + Google Sheets

An automated email marketing pipeline that sends personalized emails to segmented contact lists using AI-generated greetings, a weekly scheduler, and Gmail delivery — with automatic tracking to avoid duplicates.

---

## How it works

```
Weekly trigger (every Monday 9am)
  → Read contacts from Google Sheets (skip already sent)
  → Limit to 1,000 contacts per run
  → Filter valid emails
  → ChatGPT generates a personalized greeting per contact
  → Gmail sends the email with full HTML template
  → Mark contact as "Sent" in Google Sheets
```

---

## Features

- **AI-personalized greetings** — ChatGPT detects gender from first name and generates the appropriate salutation (e.g. "Dear Dra. García," or "Dear Juan Pérez,")
- **Segmentation-aware** — adapts tone and content based on contact type (doctor, client, distributor, event attendee)
- **Duplicate prevention** — tracks sent status in Google Sheets; contacts are never emailed twice
- **Rate limiting** — capped at 1,000 emails per run to stay within free tier limits
- **Professional HTML email** — responsive design with logo, structured body, and branded footer
- **2-second delay** between sends to avoid spam filters

---

## Stack

| Tool | Purpose |
|---|---|
| **n8n** | Workflow automation |
| **OpenAI GPT-4o-mini** | AI greeting generation |
| **Gmail API** | Email delivery |
| **Google Sheets** | Contact list + sent tracking |

---

## Setup

### 1. Google Sheets
Create a spreadsheet with these columns:
```
Segment | Company | Last Name | First Name | Contact | Email | Phone | Address | City | State | Country | Zip Code | Sent
```

### 2. n8n
- Import `automated-email-pipeline.json` into your n8n instance
- Connect credentials: Google Sheets OAuth2, Gmail OAuth2, OpenAI API
- Replace all `YOUR_*` placeholders with your actual values

### 3. Placeholders to replace
| Placeholder | Replace with |
|---|---|
| `YOUR_GOOGLE_SHEET_ID` | Your Google Sheets document ID |
| `YOUR_COMPANY` | Your company name |
| `YOUR_NAME` | Sender full name |
| `YOUR_TITLE` | Sender job title |
| `YOUR_ADDRESS` | Company address |
| `YOUR_PHONE_1` / `YOUR_PHONE_2` | Phone numbers |
| `your@email.com` | Sender email |
| `www.yourcompany.com` | Company website |
| `YOUR_GITHUB_USER/YOUR_REPO/main/your-logo.png` | Logo URL |

---

## Customization

**Change send frequency** — edit the Schedule Trigger node (daily, weekly, monthly)

**Change email limit** — edit the Limit node (default: 1,000)

**Adapt the email body** — edit the HTML in the Gmail node to match your company's content and branding

**Adapt the AI prompt** — edit the ChatGPT node's System prompt to match your company's tone and industry

---

## Notes

- Contacts without a valid email (no `@`) are automatically skipped
- The `Sent` column is set to `"Yes"` after each successful send
- Logo must be hosted on a publicly accessible URL (GitHub raw, Imgur, etc.) for Gmail to render it
