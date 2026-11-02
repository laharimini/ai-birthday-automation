# Setup Guide

## Required API Keys

| Service | Where to Get It | Used For |
|---|---|---|
| Gemini AI | [Google AI Studio](https://aistudio.google.com/) | Persona building, gift ideas, video generation |
| OpenAI | [platform.openai.com](https://platform.openai.com/) | Song lyrics generation |
| Suno API | [suno.com](https://suno.com/) | AI music generation |
| KIE API | [kie.ai](https://kie.ai/) | AI image card generation |
| Serp API | [serpapi.com](https://serpapi.com/) | Real product search for gift ideas |
| Telegram Bot | [@BotFather on Telegram](https://t.me/botfather) | Birthday morning notification |

## n8n Credential Setup

For each API, go to **n8n → Settings → Credentials → Add Credential** and create:

- `Google OAuth2` — covers Gmail, Google Sheets, Google Calendar
- `HTTP Header Auth` — for Gemini, Suno, KIE, Serp API (add `Authorization: Bearer YOUR_KEY`)
- `Telegram API` — paste your bot token

## Workflow Configuration

After importing the workflow JSON, update the following nodes:

1. **Birthday Form (Webhook)** — copy the webhook URL; this is what your form POSTs to
2. **Append row in sheet** — update with your Google Sheets document ID
3. **Create an event** — update with your Google Calendar ID
4. **Send a text message (Telegram)** — update with your chat/channel ID
5. **Email nodes** — verify sender address matches your Gmail credential

## Test Run

Submit a test payload to your webhook URL:

```json
{
  "name": "Test Person",
  "email": "test@example.com",
  "birthday": "2025-12-25",
  "hobbies": "cooking, reading, hiking",
  "personality": "fun, energetic",
  "budget_min": 25,
  "budget_max": 50
}
```

Check n8n's **Executions** tab to trace each node's output.

## Common Issues

**Suno polling times out** — Increase the Wait node delay. Generation can take up to 2 minutes on busy servers.

**Gmail attachment too large** — KIE videos can be large. Add a file size check node before attaching if you hit Gmail's 25MB limit.

**Serp API returns no results** — Check that the gift category prompt is specific enough. Overly broad queries return mixed results.
