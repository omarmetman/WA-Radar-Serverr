<div align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&height=200&section=header&text=WA-Radar-Server&fontSize=70&animation=fadeIn&fontAlignY=35&desc=Instantly%20catch%20deleted%20WhatsApp%20messages!&descAlignY=60&descAlign=50" alt="WA-Radar-Server Header" />

  <h4>Backend server that monitors deleted WhatsApp messages and forwards them to a Telegram bot instantly.</h4>

  <p>
    <a href="https://nodejs.org/"><img src="https://img.shields.io/badge/Node.js-18+-43853D?style=for-the-badge&logo=node.js&logoColor=white" alt="Node.js"></a>
    <a href="https://www.docker.com/"><img src="https://img.shields.io/badge/Docker-Supported-2496ED?style=for-the-badge&logo=docker&logoColor=white" alt="Docker"></a>
    <a href="https://opensource.org/licenses/MIT"><img src="https://img.shields.io/badge/License-MIT-F7DF1E?style=for-the-badge&logo=opensourceinitiative&logoColor=black" alt="License: MIT"></a>
  </p>

  <p>
    <a href="#-about"><b>About</b></a> •
    <a href="#-how-it-works"><b>How It Works</b></a> •
    <a href="#-tech-stack"><b>Tech Stack</b></a> •
    <a href="#-installation--setup"><b>Installation</b></a> •
    <a href="#-docker-setup"><b>Docker</b></a> •
    <a href="#-license"><b>License</b></a>
  </p>
</div>

---

## 📖 About

**WA-Radar-Server** acts as a silent watcher for your WhatsApp account. It listens for incoming text messages and temporarily stores them in memory. The moment someone hits "Delete for everyone", the server intercepts it and immediately dispatches the deleted content straight to your Telegram bot. Never wonder *"what did they delete?"* again!

## ⚙️ How It Works

1. **Start Server:** The server initializes and requests a WhatsApp Web QR code.
2. **QR Code Delivery:** The QR code is forwarded directly to your Telegram bot.
3. **Scan & Link:** Scan it using your WhatsApp mobile app (**Settings > Linked Devices**).
4. **Background Monitoring:** The server continuously monitors and logs all incoming text messages.
5. **Instant Alert:** When a deletion is detected, you receive a perfectly formatted alert on Telegram.

<br>

<div align="center">

> **🔔 Alert Format Example:**
>
> ```text
> 🚨 Deleted Message Detected!
> 👤 Sender: John Doe
> 💬 Message: The deleted text content
> 🕒 Time: 10:30:45 PM
> ```

</div>

## 💻 Tech Stack

<p align="center">
  <img src="https://skillicons.dev/icons?i=nodejs,docker,github&theme=light" alt="Tech Stack" />
</p>

- **Node.js** - Runtime environment
- **whatsapp-web.js** - WhatsApp Web integration
- **Puppeteer** - Headless browser automation
- **Telegram Bot API** - Sending alerts
- **QRCode** - QR code generation
- **Docker** - Containerization (optional)

## 📋 Requirements

- **Node.js** (v18 or higher)
- **Telegram Bot Token** (Create one via [@BotFather](https://t.me/BotFather))
- **Telegram Chat ID** (Get yours from [@userinfobot](https://t.me/userinfobot))

## 🚀 Installation & Setup

### 1. Clone the repository
```bash
git clone https://github.com/omarmetman/WA-Radar-Serverr.git
cd WA-Radar-Serverr
```

### 2. Install dependencies
```bash
npm install
```

### 3. Environment Configuration
Create a `.env` file in the root directory and add your Telegram credentials:
```env
TG_TOKEN=your_bot_token_here
TG_CHAT_ID=your_chat_id_here
```
*(Note: It is recommended to use `.env.example` as a template for public repositories).*

### 4. Start the server
```bash
npm start
```

## 🐳 Docker Setup

Running the app via Docker is highly recommended for background execution and isolation.

**Start with Docker Compose:**
```bash
docker-compose up -d
```

**View Logs:**
```bash
docker logs -f wa-radar
```

<details>
<summary><b>View docker-compose.yml</b></summary>

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
</details>

<details>
<summary><b>View Dockerfile</b></summary>

```dockerfile
FROM node:18-slim
RUN apt-get update && apt-get install -y libgbm1 libgtk-3-0 libnss3 libx11-xcb1
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY index.js ./
CMD ["node", "index.js"]
```
</details>

## 🏁 First Time Setup Guide

1. Start the server using `npm start` or Docker.
2. Check your Telegram bot - you will receive a QR code image.
3. Open WhatsApp on your phone.
4. Navigate to **Settings > Linked Devices > Link a Device**.
5. Scan the QR code displayed in your Telegram chat.
6. Wait for the success message: `✅ WhatsApp Radar is active!`

## ⌨️ Commands Reference

| Command | Description |
|:---|:---|
| `npm start` | Start the server locally |
| `npm install` | Install all required dependencies |
| `Ctrl + C` | Stop the local server |
| `docker-compose up -d` | Start containerized server in background |
| `docker-compose down` | Stop and remove Docker container |
| `docker logs -f wa-radar` | View real-time container logs |

## 📂 Project Structure

```text
WA-Radar-Server/
├── index.js           # Main application code
├── package.json       # Dependencies list
├── package-lock.json  # Locked dependencies versions
├── .env.example       # Environment variables template (Use this!)
├── .gitignore         # Excluded files
└── session/           # WhatsApp session data (auto-generated)
```

## ⚠️ Limitations

- **Text Only:** Media files (images, videos, voice notes) are currently ignored.
- **Memory Limits:** Only stores the last 500 messages in RAM to prevent memory leaks.
- **Volatile Storage:** Restarting the server clears all temporarily stored messages.
- **No Retroactive Recovery:** Cannot recover messages deleted *before* the server was started.

## 🛑 Disclaimer

> **For Educational Purposes Only.**
> Using this project may violate WhatsApp's Terms of Service. By using this software, you assume full responsibility for any consequences, including potential account bans. The developer is not liable for any misuse.

## 📄 License

This project is licensed under the **MIT License**.

Copyright (c) 2024 **Omar M. Etman**

<br>

<div align="center">
  <h3>👨‍💻 Developed by Omar M. Etman</h3>
  <a href="https://github.com/omarmetman">
    <img src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white" alt="GitHub" />
  </a>
  <a href="https://omarmetman.vercel.app/">
    <img src="https://img.shields.io/badge/Website-000000?style=for-the-badge&logo=Vercel&logoColor=white" alt="Website" />
  </a>
  <a href="https://github.com/omarmetman/WA-Radar-Serverr.git">
    <img src="https://img.shields.io/badge/Repository-2b3137?style=for-the-badge&logo=git&logoColor=white" alt="Repo" />
  </a>
</div>
