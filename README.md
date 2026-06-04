<div align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=00a884&height=220&section=header&text=WA-Radar-Server&fontSize=65&animation=fadeIn&fontAlignY=38&desc=The%20Ultimate%20Deleted%20Message%20Interceptor&descAlignY=65&descAlign=50" alt="Header" width="100%" />

  <a href="https://readme-typing-svg.herokuapp.com">
    <img src="https://readme-typing-svg.herokuapp.com?font=Fira+Code&weight=600&size=20&pause=1000&color=00a884&center=true&vCenter=true&width=600&lines=Catch+Deleted+Messages+Instantly...;Forward+Directly+to+Telegram...;100%25+Automated+Node.js+Server...;Secure,+Fast,+and+Dockerized..." alt="Typing SVG" />
  </a>

  <br>

  <!-- Repo Stats Badges -->
  <a href="https://github.com/omarmetman/WA-Radar-Serverr/stargazers"><img src="https://img.shields.io/github/stars/omarmetman/WA-Radar-Serverr?style=for-the-badge&color=FFD700&logo=github&logoColor=white" alt="Stars" /></a>
  <a href="https://github.com/omarmetman/WA-Radar-Serverr/network/members"><img src="https://img.shields.io/github/forks/omarmetman/WA-Radar-Serverr?style=for-the-badge&color=007EC6&logo=github&logoColor=white" alt="Forks" /></a>
  <a href="https://github.com/omarmetman/WA-Radar-Serverr/issues"><img src="https://img.shields.io/github/issues/omarmetman/WA-Radar-Serverr?style=for-the-badge&color=E83E8C&logo=github&logoColor=white" alt="Issues" /></a>
  <a href="https://opensource.org/licenses/MIT"><img src="https://img.shields.io/badge/License-MIT-28A745?style=for-the-badge&logo=opensourceinitiative&logoColor=white" alt="License: MIT"></a>

  <br><br>
  <b><a href="#-features">Features</a></b> •
  <b><a href="#-quick-start">Quick Start</a></b> •
  <b><a href="#-docker-deployment">Docker</a></b> •
  <b><a href="#-how-it-works">How it Works</a></b> •
  <b><a href="#-contributing">Contributing</a></b>
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

## 🛠️ Tech Stack & Tools

<div align="center">
  <img src="https://skillicons.dev/icons?i=nodejs,js,docker,bash,github,git&perline=6" alt="Tech Stack" />
</div>

---

## 🚀 Quick Start (Local Development)

### Prerequisites
* **Node.js** v18+
* A **Telegram Bot Token** (Get from [@BotFather](https://t.me/BotFather))
* Your **Telegram Chat ID** (Get from [@userinfobot](https://t.me/userinfobot))

### 1. Clone & Install
```bash
git clone https://github.com/omarmetman/WA-Radar-Serverr.git
cd WA-Radar-Serverr
npm install
```

### 2. Configure Environment
Create a `.env.example` file, rename it to `.env`, and add your credentials:
```env
# DO NOT commit your actual .env file to GitHub!
TG_TOKEN=your_telegram_bot_token_here
TG_CHAT_ID=your_chat_id_here
```

### 3. Launch
```bash
npm start
```
*Check your Telegram bot for the login QR code!*

---

## 🐳 Docker Deployment (Recommended for Production)

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

## 🤝 Contributing

Contributions make the open-source community such an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## ⚠️ Disclaimer & Limitations

* **Media Limitation:** Currently tracks **text messages only**. Media (images, videos, voice notes) are ignored to conserve server bandwidth and memory.
* **Volatile Memory:** Restarting the server clears the RAM. Messages deleted *before* the server starts cannot be recovered.
* **ToS Warning:** This project is for **educational and research purposes only**. Using automated tools may violate WhatsApp's Terms of Service. The developer is not responsible for any account suspensions.

---

## 👨‍💻 Developer & License

<div align="center">
  Distributed under the <b>MIT License</b>. See <code>LICENSE</code> for more information.
  <br><br>
  <b>Developed with ❤️ by Omar M. Etman</b>
  <br><br>
  <a href="https://github.com/omarmetman">
    <img src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white" alt="GitHub" />
  </a>
  <a href="https://omarmetman.vercel.app/">
    <img src="https://img.shields.io/badge/Website-000000?style=for-the-badge&logo=Vercel&logoColor=white" alt="Website" />
  </a>
</div>
