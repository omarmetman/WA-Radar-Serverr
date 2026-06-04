```markdown
# WA-Radar-Server

Backend server that monitors deleted WhatsApp messages and forwards them to a Telegram bot instantly.

---

## About

WA-Radar-Server connects to your WhatsApp account, listens for incoming text messages, and stores them temporarily in memory. When someone deletes a message ("delete for everyone"), the server immediately sends the deleted message content to your Telegram bot.

**Current limitation:** Only text messages are supported. Media files (images, videos, documents) are ignored.

---

## How It Works

1. Server starts and requests a WhatsApp Web QR code
2. QR code is sent to your Telegram bot
3. You scan it from WhatsApp mobile (Settings > Linked Devices)
4. Server monitors all incoming text messages
5. When a message is deleted, you receive an alert on Telegram

**Alert format:**
```
Deleted Message Detected!
Sender: Name
Message: The deleted text
Time: 10:30:45 PM
```

---

## Tech Stack

| Technology | Purpose |
|------------|---------|
| Node.js | Runtime environment |
| whatsapp-web.js | WhatsApp Web integration |
| Puppeteer | Headless browser automation |
| Telegram Bot API | Sending alerts |
| QRCode | QR code generation |

---

## Requirements

- Node.js 18 or higher
- Telegram bot token (get from @BotFather)
- Telegram chat ID (get from @userinfobot)

---

## Installation

### Option 1: Direct Run

```bash
git clone https://github.com/omarmetman/WA-Radar-Serverr.git
cd WA-Radar-Serverr
npm install
```

Create `.env` file:
```env
TG_TOKEN=your_bot_token_here
TG_CHAT_ID=your_chat_id_here
```

Run:
```bash
npm start
```

### Option 2: Docker

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

Run:
```bash
docker-compose up -d
```

---

## First Time Setup

1. Start the server
2. Check your Telegram - you'll receive a QR code image
3. Open WhatsApp on your phone
4. Go to Settings > Linked Devices > Link a Device
5. Scan the QR code from your Telegram
6. The server will show: `WhatsApp Radar is active!`

---

## Commands

| Command | Description |
|---------|-------------|
| `npm start` | Start the server |
| `npm install` | Install dependencies |
| `Ctrl + C` | Stop the server |
| `docker-compose up -d` | Start with Docker |
| `docker-compose down` | Stop Docker container |
| `docker logs -f wa-radar` | View logs |

---

## Project Structure

```
WA-Radar-Server/
├── index.js           # Main application code
├── package.json       # Dependencies
├── .env              # Environment variables (private)
├── .gitignore        # Excluded files
└── session/          # WhatsApp session (auto-generated)
```

---

## Important Notes

- Only stores last 500 messages in RAM
- Restarting the server clears stored messages
- Media deletion not detected
- Session persists across restarts (no need to re-scan QR)

---

## Disclaimer

This project is for educational and research purposes only. Using it may violate WhatsApp Terms of Service. You assume full responsibility for any consequences.

---

## License

MIT License

Copyright (c) 2024 Omar M. Etman

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files, to deal in the Software without restriction.

---

## Developer

Omar M. Etman

- Website: [omarmetman.vercel.app](https://omarmetman.vercel.app/)
- GitHub: [@omarmetman](https://github.com/omarmetman)

---

## Repository

[https://github.com/omarmetman/WA-Radar-Serverr.git](https://github.com/omarmetman/WA-Radar-Serverr.git)
```
