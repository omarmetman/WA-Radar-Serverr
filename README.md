<div align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=00a884&height=250&section=header&text=WA-Radar-Server&fontSize=70&animation=fadeIn&fontAlignY=35&desc=The%20Ultimate%20Deleted%20Message%20Interceptor&descAlignY=60&descAlign=50" alt="Header" width="100%" />

  <a href="https://readme-typing-svg.herokuapp.com">
    <img src="https://readme-typing-svg.herokuapp.com?font=Fira+Code&weight=600&size=22&pause=1000&color=00a884&center=true&vCenter=true&width=600&lines=Catch+Deleted+Messages+Instantly;Forward+Directly+to+Telegram;100%25+Automated+Node.js+Server;Secure,+Fast,+and+Dockerized" alt="Typing SVG" />
  </a>

  <br>

  <!-- Repo Stats Badges -->
  <a href="https://github.com/omarmetman/WA-Radar-Serverr/stargazers"><img src="https://img.shields.io/github/stars/omarmetman/WA-Radar-Serverr?style=for-the-badge&color=FFD700&logo=github&logoColor=white" alt="Stars" /></a>
  <a href="https://github.com/omarmetman/WA-Radar-Serverr/network/members"><img src="https://img.shields.io/github/forks/omarmetman/WA-Radar-Serverr?style=for-the-badge&color=007EC6&logo=github&logoColor=white" alt="Forks" /></a>
  <a href="https://github.com/omarmetman/WA-Radar-Serverr/issues"><img src="https://img.shields.io/github/issues/omarmetman/WA-Radar-Serverr?style=for-the-badge&color=E83E8C&logo=github&logoColor=white" alt="Issues" /></a>
  <a href="https://opensource.org/licenses/MIT"><img src="https://img.shields.io/badge/License-MIT-28A745?style=for-the-badge&logo=opensourceinitiative&logoColor=white" alt="License: MIT"></a>

  <br><br>
  <b><a href="#-features">Features</a></b> •
  <b><a href="#-telegram-setup-guide">Telegram Setup</a></b> •
  <b><a href="#-quick-start">Quick Start</a></b> •
  <b><a href="#-docker-deployment">Docker</a></b> •
  <b><a href="#-disclaimer">Disclaimer</a></b>
</div>

<br>

---

## 📖 Introduction

**WA-Radar-Server** is a lightweight, headless Node.js backend that acts as a silent guardian for your WhatsApp account. It intercepts incoming text messages and temporarily buffers them. If a sender uses the *"Delete for everyone"* feature, the server instantly detects the revocation and forwards the original message directly to your private Telegram bot. 

Never lose a piece of information, and never wonder *"what did they delete?"* again.

---

## ✨ Core Features

| 🚀 Feature | 📝 Description |
| :--- | :--- |
| **Instant Alerts** | Zero-delay forwarding of deleted text messages directly to your Telegram. |
| **Stealth Operation** | Runs entirely in the background (`headless: true`) using Puppeteer. |
| **Smart Memory Management** | Auto-cleans RAM by keeping only the last 500 messages to ensure 24/7 uptime without memory leaks. |
| **Remote Authentication** | Sends the WhatsApp Web QR code to your Telegram bot for easy, remote linking. |
| **Docker Ready** | Pre-configured `Dockerfile` and `docker-compose.yml` for instant, containerized deployment. |

---

## 🤖 Telegram Setup Guide

To run this server, you need a Telegram Bot and your personal Chat ID. Follow these exact steps to get them:

<details>
<summary><b>1️⃣ How to create a bot and get the <code>TG_TOKEN</code></b></summary>
<br>

1. Open the Telegram app and search for **[@BotFather](https://t.me/BotFather)**.
2. Send the command `/newbot` to start creating your bot.
3. Choose a display name for your bot (e.g., `WA Radar Bot`).
4. Choose a unique username for your bot (must end in `bot`, e.g., `wa_radar_omar_bot`).
5. **BotFather** will reply with a congratulatory message containing your **API Token**. 
   * It looks something like this: `123456789:ABCdefGhIJKlmNoPQRsTUVwxyZ`.
   * **Copy this token!** This is your `TG_TOKEN`.
</details>

<details>
<summary><b>2️⃣ How to get your personal <code>TG_CHAT_ID</code></b></summary>
<br>

1. Open the Telegram app and search for **[@userinfobot](https://t.me/userinfobot)**.
2. Send the command `/start` to the bot.
3. The bot will instantly reply with your account details.
4. Look for the number next to **Id** (e.g., `Id: 987654321`).
5. **Copy this number!** This is your `TG_CHAT_ID`.
</details>

---

## 🚀 Quick Start (Local Development)

### Prerequisites
* **Node.js** v18+

### 1. Clone & Install
```bash
git clone https://github.com/omarmetman/WA-Radar-Serverr.git
cd WA-Radar-Serverr
npm install
```

### 2. Configure Environment
Create a `.env` file in the root directory (you can copy `.env.example`) and add your credentials:
```env
TG_TOKEN=your_telegram_bot_token_here
TG_CHAT_ID=your_chat_id_here
```

### 3. Launch
```bash
npm start
```
*Check your Telegram bot for the login QR code!*

---

## 🐳 Docker Deployment (Recommended)

Deploying via Docker ensures the server runs flawlessly in the background, fully isolated from your host system.

```bash
# Start the server in detached mode
docker-compose up -d

# Monitor the real-time logs
docker logs -f wa-radar
```

<details>
<summary><b>🛠️ Click to expand: View Docker Configuration Files</b></summary>

**`docker-compose.yml`**
```yaml
version: '3.8'
services:
  wa-radar:
    build: .
    container_name: wa-radar
    restart: unless-stopped
    environment:
      - TG_TOKEN=${TG_TOKEN}
      - TG_CHAT_ID=${TG_CHAT_ID}
    volumes:
      - ./session:/app/.wwebjs_auth
    stdin_open: true
    tty: true
```

**`Dockerfile`**
```dockerfile
FROM ghcr.io/puppeteer/puppeteer:latest

ENV PUPPETEER_SKIP_CHROMIUM_DOWNLOAD=true
ENV PUPPETEER_EXECUTABLE_PATH=/usr/bin/google-chrome-stable

USER root
WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY . .
CMD ["node", "index.js"]
```
</details>

---

## ⚙️ How It Works (The Flow)

<div align="center">
  <code>WhatsApp Web Initialization</code> ➔ <code>QR sent to Telegram</code> ➔ <code>User Scans QR</code> ➔ <code>Server Listens</code> <br><br>
  ⬇️ <br><br>
  <code>Message Received</code> ➔ <code>Stored in RAM Map (Max 500)</code> <br><br>
  ⬇️ <br><br>
  <code>Message Revoked</code> ➔ <code>Look up in RAM</code> ➔ <code>Forward to Telegram API</code>
</div>

<br>

> **🔔 Telegram Alert Example:**
> ```text
> 🚨 *Deleted Message Detected!*
> 👤 *Sender:* John Doe
> 📩 *Message:* The content they tried to hide.
> 🕒 *Time:* 10:30:45 PM
> ```

---

## 🛑 Disclaimer

> [!WARNING]
> **For Educational and Research Purposes Only.**
> 
> This project demonstrates the capabilities of browser automation and API integration. **Using automated tools, scripts, or unofficial clients may violate WhatsApp's Terms of Service.** > 
> By utilizing this software, you assume full responsibility for your actions, including any potential consequences such as account bans or restrictions. The developers and contributors of this repository accept absolutely no liability for any misuse, damage, or violation of third-party terms.

---

## 👨‍💻 Developer & Credits

<div align="center">
  Distributed under the <b>MIT License</b>. See <code>LICENSE</code> for more information.
  <br><br>
  <b>Developed by Omar M. Etman</b>
  <br><br>
  <a href="https://github.com/omarmetman">
    <img src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white" alt="GitHub" />
  </a>
  <a href="https://omarmetman.vercel.app/">
    <img src="https://img.shields.io/badge/Website-000000?style=for-the-badge&logo=Vercel&logoColor=white" alt="Website" />
  </a>
</div>

<br>

### ✨ Credits & Acknowledgements
> **Note:** This project is fully copied and inspired by the original work of **Ebtesam Ahmed**. 
> You can find the original creator on TikTok: **[Ebtesam Elganady (@saaamahmed)](https://www.tiktok.com/@saaamahmed?_r=1&_t=ZS-96vjX0c4J7M)**

