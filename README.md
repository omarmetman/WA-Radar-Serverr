```markdown
# WA-Radar-Server

Backend server that monitors deleted WhatsApp messages and forwards them to a Telegram bot instantly.

---

## Table of Contents

- [About](#about)
- [How It Works](#how-it-works)
- [Tech Stack](#tech-stack)
- [Requirements](#requirements)
- [Installation](#installation)
- [Docker Setup](#docker-setup)
- [First Time Setup](#first-time-setup)
- [Commands](#commands)
- [Project Structure](#project-structure)
- [Limitations](#limitations)
- [Disclaimer](#disclaimer)
- [License](#license)
- [Developer](#developer)

---

## About

WA-Radar-Server connects to your WhatsApp account, listens for incoming text messages, and stores them temporarily in memory. When someone deletes a message ("delete for everyone"), the server immediately sends the deleted message content to your Telegram bot.

---

## How It Works

**Step 1:** Server starts and requests a WhatsApp Web QR code

**Step 2:** QR code is sent to your Telegram bot

**Step 3:** You scan it from WhatsApp mobile (Settings > Linked Devices)

**Step 4:** Server monitors all incoming text messages

**Step 5:** When a message is deleted, you receive an alert on Telegram

**Alert Format:**

```
Deleted Message Detected!
Sender: Name
Message: The deleted text
Time: 10:30:45 PM
```

---

## Tech Stack

- **Node.js** - Runtime environment
- **whatsapp-web.js** - WhatsApp Web integration
- **Puppeteer** - Headless browser automation
- **Telegram Bot API** - Sending alerts
- **QRCode** - QR code generation
- **Docker** - Containerization (optional)

---

## Requirements

- Node.js 18 or higher
- Telegram bot token (get from @BotFather)
- Telegram chat ID (get from @userinfobot)

---

## Installation

**Clone the repository**

```bash
git clone https://github.com/omarmetman/WA-Radar-Serverr.git
cd WA-Radar-Serverr
```

**Install dependencies**

```bash
npm install
```

**Create .env file**

```env
TG_TOKEN=your_bot_token_here
TG_CHAT_ID=your_chat_id_here
```

**Start the server**

```bash
npm start
```

---

## Docker Setup

**docker-compose.yml**

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

**Dockerfile**

```dockerfile
FROM node:18-slim
RUN apt-get update && apt-get install -y libgbm1 libgtk-3-0 libnss3 libx11-xcb1
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY index.js ./
CMD ["node", "index.js"]
```

**Run**

```bash
docker-compose up -d
```

---

## First Time Setup

1. Start the server using `npm start` or Docker

2. Check your Telegram - you will receive a QR code image

3. Open WhatsApp on your phone

4. Go to Settings > Linked Devices > Link a Device

5. Scan the QR code from your Telegram

6. The server will display: `WhatsApp Radar is active!`

---

## Commands

| Command | Description |
|---------|-------------|
| `npm start` | Start the server |
| `npm install` | Install dependencies |
| `Ctrl + C` | Stop the server |
| `docker-compose up -d` | Start with Docker (background) |
| `docker-compose down` | Stop Docker container |
| `docker logs -f wa-radar` | View container logs |

---

## Project Structure

```
WA-Radar-Server/
├── index.js           # Main application code
├── package.json       # Dependencies list
├── package-lock.json  # Locked dependencies versions
├── .env              # Environment variables (private)
├── .gitignore        # Excluded files from Git
└── session/          # WhatsApp session (auto-generated)
```

---

## Limitations

- Text messages only - media files are ignored
- Only stores last 500 messages in RAM
- Restarting the server clears stored messages
- Cannot recover messages deleted before server startup

---

## Disclaimer

This project is for educational and research purposes only. Using it may violate WhatsApp Terms of Service. You assume full responsibility for any consequences.

---

## License

MIT License

Copyright (c) 2024 Omar M. Etman

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files, to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

---

## Developer

**Omar M. Etman**

- Website: [omarmetman.vercel.app](https://omarmetman.vercel.app/)
- GitHub: [@omarmetman](https://github.com/omarmetman)

---

## Repository

[https://github.com/omarmetman/WA-Radar-Serverr.git](https://github.com/omarmetman/WA-Radar-Serverr.git)
```
