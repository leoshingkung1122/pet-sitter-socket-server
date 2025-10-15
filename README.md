# Pet Sitter Socket.IO Server

Standalone Socket.IO server for Pet Sitter App chat functionality.

## 🚀 Features

- ✅ Real-time messaging
- ✅ User online/offline status
- ✅ Unread message count
- ✅ Chat list updates
- ✅ CORS support for Vercel deployment
- ✅ Automatic reconnection
- ✅ Database integration with Prisma
- ✅ Health check endpoints

## 📋 Prerequisites

- Node.js 18+
- npm or yarn
- PostgreSQL database
- Railway account (for deployment)

## 🛠️ Installation

```bash
# Install dependencies
npm install

# Generate Prisma client
npm run prisma:generate
```

## ⚙️ Environment Variables

Create a `.env` file based on `env.example`:

```bash
cp env.example .env
```

Required environment variables:

```bash
# Server port (Railway sets this automatically)
PORT=4000

# CORS origins (comma-separated)
SOCKET_IO_CORS_ORIGIN=http://localhost:3000,https://your-app.vercel.app

# Database connection
DATABASE_URL=postgresql://user:password@host:5432/database?schema=public
```

## 💻 Development

```bash
# Development mode with hot reload
npm run dev

# Production build
npm run build

# Start production server
npm start
```

### Testing Locally

1. Visit `http://localhost:4000/health` - Health check
2. Visit `http://localhost:4000/socket-status` - Socket.IO status

Expected responses:
```json
// /health
{
  "status": "ok",
  "server": "Socket.IO Standalone",
  "timestamp": "2024-01-01T00:00:00.000Z",
  "connectedClients": 0
}

// /socket-status
{
  "success": true,
  "isReady": true,
  "message": "Socket.IO server is running",
  "connectedClients": 0
}
```

## 🚢 Deployment

### Railway Deployment

See [DEPLOYMENT.md](./DEPLOYMENT.md) for detailed deployment instructions.

Quick steps:
1. Push to GitHub
2. Connect repository to Railway
3. Set environment variables in Railway dashboard
4. Deploy automatically

### Required Environment Variables in Railway

```bash
PORT=${{PORT}}  # Auto-set by Railway
SOCKET_IO_CORS_ORIGIN=https://your-app.vercel.app
DATABASE_URL=postgresql://...
```

## 🔗 Integration with Pet-Sitter-AppTest

See [SETUP_CLIENT.md](./SETUP_CLIENT.md) for client setup instructions.

In your Next.js app, set:
```bash
NEXT_PUBLIC_SOCKET_SERVER_URL=https://your-socket-server.railway.app
```

## 📡 API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/health` | GET | Server health check |
| `/socket-status` | GET | Socket.IO server status |

## 🔌 Socket.IO Events

### Client → Server

| Event | Data | Description |
|-------|------|-------------|
| `join_app` | `userId: string` | User joins the app |
| `send_message` | `SendMessageData` | Send a chat message |
| `set_current_chat` | `{userId, chatId}` | Set active chat |
| `disconnect` | - | User disconnects |

### Server → Client

| Event | Data | Description |
|-------|------|-------------|
| `user_online` | `userId: string` | User came online |
| `user_offline` | `userId: string` | User went offline |
| `receive_message` | `MessagePayload` | New message received |
| `unread_update` | `{chatId, count}` | Unread count updated |
| `online_users_list` | `string[]` | List of online users |
| `chat_list_update` | `{chatId, action}` | Chat visibility update |
| `error` | `{message, details}` | Error occurred |

## 📁 Project Structure

```
pet-sitter-socket-server/
├── index.ts              # Main server file
├── prisma/
│   └── schema.prisma     # Database schema
├── package.json          # Dependencies
├── tsconfig.json         # TypeScript config
├── railway.json          # Railway deployment config
├── nixpacks.toml         # Build config
├── Procfile              # Start command
├── .gitignore           # Git ignore rules
├── env.example          # Environment template
├── README.md            # This file
├── DEPLOYMENT.md        # Deployment guide
└── SETUP_CLIENT.md      # Client setup guide
```

## 🔧 Scripts

```bash
npm run dev             # Development with hot reload
npm run build           # Build TypeScript
npm start               # Start production server
npm run prisma:generate # Generate Prisma client
```

## 🐛 Troubleshooting

### Build Issues
- Ensure `DATABASE_URL` is set
- Run `npm run prisma:generate`
- Check Prisma schema syntax

### Connection Issues
- Verify CORS settings
- Check client `NEXT_PUBLIC_SOCKET_SERVER_URL`
- Review Railway logs

### Database Issues
- Confirm DATABASE_URL is accessible
- Verify Prisma schema matches database
- Check database connection

## 📊 Monitoring

Railway provides:
- Real-time logs
- Performance metrics
- Deployment history
- Automatic health checks

## 🔒 Security Notes

1. Always use environment variables for sensitive data
2. Keep `DATABASE_URL` secure
3. Limit CORS origins to trusted domains
4. Use HTTPS in production
5. Monitor for suspicious connection patterns

## 📝 License

ISC

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request
