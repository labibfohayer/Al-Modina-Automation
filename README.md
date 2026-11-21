# Al-Modina Automation - AI-Powered Meta and Telegram Operations Engine

[![Official Platform Page](https://img.shields.io/badge/Official_Page-Facebook_Institute-blue?style=for-the-badge&logo=facebook&logoColor=white)](https://www.facebook.com/AlMadinaModelMadrasaAndResearchInstitute)

An intelligent multi-platform automation system engineered for educational institutions. Powered by Node.js, Express, Google Gemini Large Language Models (`@google/generative-ai`), Meta Graph API Webhooks, and the Telegram Bot API.

---

## Project Overview

Al-Modina Automation provides round-the-clock conversational intelligence and social media workflow automation for Al-Madina Model Madrasah and Research Institute. 

The system handles two primary operational channels:
1. Meta Messenger & Page Integration: Automatically responds to prospective parent inquiries, admission requests, fee queries, and public post comments using context-grounded AI responses powered by Google Gemini.
2. Telegram Administrative Bot: Provides institutional administrators with remote control over social post drafting, caption generation, and broadcast scheduling.

---

## Key Features

- Context-Aware Conversational AI: Integrates Google Gemini (`gemini-1.5-flash` / `gemini-2.5-flash-lite`) configured with detailed institutional domain knowledge (`src/systemPrompt.js`) covering academic curricula (Play to Class 6), Islamic studies (Noorani, Nazera, Hifz), hostel boarding policies, and administrative contacts.
- Dual-Channel Webhook Gateway: Express 5 server featuring Meta Webhook verification handshake (`GET /webhook`) and real-time event ingestion (`POST /webhook`) for Facebook Messenger and Page comment interactions.
- Telegram Control Bot: Interactive Telegram bot interface (`src/telegram.js`) allowing administrators to generate educational post captions, audit conversational logs, and trigger manual content dispatches.
- Automated Content Publisher: Background cron runner (`node-cron`) that schedules and generates Islamic quotes, daily academic announcements, and curated media posts with pre-approved photo libraries.
- Fault-Tolerant Retry Logic: Robust error handling with exponential backoff and retry wrappers to guarantee response delivery during network or API rate-limit spikes.
- Cloudflare Tunnel Integration: Embedded configuration for Cloudflare Tunnel (`cloudflared`) enabling secure HTTPS webhook exposure from local development environments and private servers without static public IPs.

---

## Architecture & Communication Flow

```
                      +-----------------------------+
                      |   Parent / Student Inquiry  |
                      +--------------+--------------+
                                     |
                                     v
                       +-------------+-------------+
                       |    Meta Facebook Platform |
                       | (Messenger & Page Comments)|
                       +-------------+-------------+
                                     | Webhook (HTTPS)
                                     v
+-----------------------+     +------+---------------------+
| Telegram Bot Admin    | <-> | Express Server Gateway     |
| (Control & Dispatch)  |     | (`src/server.js`)          |
+-----------------------+     +------+---------------------+
                                     |
                                     v
                              +------+---------------------+
                              | AI Processing Engine       |
                              | (`src/ai.js`)              |
                              | - Context-grounded Prompt  |
                              | - Google Gemini LLM API    |
                              +------+---------------------+
                                     |
                                     v
                              +------+---------------------+
                              | Formatted Response Return  |
                              +----------------------------+
```

---

## Tech Stack

- Runtime: Node.js (v18.x or higher)
- Backend Server: Express 5.x
- Artificial Intelligence: Google Gemini API (`@google/generative-ai`)
- Telegram Integration: `node-telegram-bot-api`
- Task Scheduling: `node-cron`
- HTTP Client & Web Scraping: `axios`, `cheerio`, `form-data`
- Tunneling: Cloudflare Tunnel (`cloudflared`)
- Configuration: `dotenv`

---

## Directory Structure

```
Al-Modina-Automation/
├── src/
│   ├── server.js              # Express webhook listener and health endpoints
│   ├── ai.js                  # Google Gemini API integration and response logic
│   ├── systemPrompt.js        # Institutional knowledge base and prompt guidelines
│   └── telegram.js            # Telegram administrative bot commands and actions
├── generate_token.js          # Helper script for generating long-lived Meta tokens
├── index.js                   # Application master entry point
├── package.json               # Node.js dependencies and script definitions
├── test.js                    # Unit tests for prompt generation and AI responses
├── cloudflared.exe            # Cloudflare Tunnel executable (for local tunneling)
└── .gitignore                 # Sensitive file and dependency exclusion rules
```

---

## Installation and Setup Guide

### Prerequisites
- Node.js 18.x or higher
- Google Gemini API Key
- Meta Developer App (with Messenger and Pages permissions)
- Telegram Bot Token (from BotFather)

### 1. Clone the Repository
```bash
git clone https://github.com/labibfohayer/Al-Modina-Automation.git
cd Al-Modina-Automation
```

### 2. Install Dependencies
```bash
npm install
```

### 3. Configure Environment Variables
Create a `.env` file in the project root:
```env
# Application Server
PORT=3000

# Google Gemini AI
GEMINI_API_KEY=your_gemini_api_key_here

# Meta Graph API
META_VERIFY_TOKEN=your_custom_webhook_verify_token
META_PAGE_ACCESS_TOKEN=your_facebook_page_access_token

# Telegram Bot
TELEGRAM_BOT_TOKEN=your_telegram_bot_token_here
TELEGRAM_ADMIN_CHAT_ID=your_telegram_chat_id_here
```

### 4. Start the Application
```bash
npm start
```
The server will start on port 3000 (health check: `http://localhost:3000/`). Official institute integration is live at **[facebook.com/AlMadinaModelMadrasaAndResearchInstitute](https://www.facebook.com/AlMadinaModelMadrasaAndResearchInstitute)**.

---

## Webhook Tunneling Setup (Cloudflare Tunnel)

To expose your local development server to Meta Webhooks:

```bash
cloudflared tunnel --url http://localhost:3000
```
Copy the generated HTTPS URL (e.g., `https://your-tunnel.trycloudflare.com`) and configure it in the Meta App Developer Portal:
- Callback URL: `https://your-tunnel.trycloudflare.com/webhook`
- Verify Token: The value defined in `META_VERIFY_TOKEN`

---

## Author and Contact

Developed by **Labib Fohayer**.

- GitHub: [https://github.com/labibfohayer](https://github.com/labibfohayer)
- Portfolio: [https://labibfohayer.github.io](https://labibfohayer.github.io)

---

## License

This project is licensed under the ISC License.
