<h1 align="center">
  <br>
  📡 WA-Radar-Server
  <br>
</h1>

<h4 align="center">Backend server that monitors deleted WhatsApp messages and forwards them to a Telegram bot instantly.</h4>

<p align="center">
  <a href="https://nodejs.org/"><img src="https://img.shields.io/badge/Node.js-18+-green.svg" alt="Node.js"></a>
  <a href="https://www.docker.com/"><img src="https://img.shields.io/badge/Docker-Supported-blue.svg" alt="Docker"></a>
  <a href="https://opensource.org/licenses/MIT"><img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License: MIT"></a>
</p>

<p align="center">
  <a href="#-about">About</a> •
  <a href="#-how-it-works">How It Works</a> •
  <a href="#-tech-stack">Tech Stack</a> •
  <a href="#-installation--setup">Installation</a> •
  <a href="#-docker-setup">Docker</a> •
  <a href="#-license">License</a>
</p>

---

## 📖 About

**WA-Radar-Server** connects to your WhatsApp account, listens for incoming text messages, and stores them temporarily in memory. When someone deletes a message ("Delete for everyone"), the server immediately sends the deleted message content to your Telegram bot. Never miss a deleted message again!

## ⚙️ How It Works

1. **Start Server:** The server starts and requests a WhatsApp Web QR code.
2. **QR Code Delivery:** The QR code is sent directly to your Telegram bot.
3. **Scan & Link:** Scan it from your WhatsApp mobile app (**Settings > Linked Devices**).
4. **Background Monitoring:** The server continuously monitors all incoming text messages.
5. **Instant Alert:** When a message is deleted, you receive a perfectly formatted alert on Telegram.

> **🔔 Alert Format Example:**
> ```text
> 🚨 Deleted Message Detected!
> 👤 Sender: John Doe
> 💬 Message: The deleted text content
> 🕒 Time: 10:30:45 PM
> ```

## 💻 Tech Stack

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

### 4. Start the server
```bash
npm start
```

## 🐳 Docker Setup

Running the app via Docker is highly recommended for background execution and containerization.

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
|---------|-------------|
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
├── .env               # Environment variables (ignored in Git)
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

## 👨‍💻 Developer

**Omar M. Etman**
- 🌐 Website: [omarmetman.vercel.app](https://omarmetman.vercel.app/)
- 🐙 GitHub: [@omarmetman](https://github.com/omarmetman)
- 📦 Repository: [WA-Radar-Serverr](https://github.com/omarmetman/WA-Radar-Serverr.git)
