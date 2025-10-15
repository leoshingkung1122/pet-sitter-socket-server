# Socket Server Deployment Guide for Railway

## Prerequisites
- Railway account
- GitHub repository connected to Railway
- PostgreSQL database (can use the same database as your main app)

## Step 1: Setup Environment Variables in Railway

Add these environment variables in your Railway project:

```bash
# Railway will automatically set PORT
PORT=${{PORT}}

# Your production app URL (e.g., Vercel deployment)
SOCKET_IO_CORS_ORIGIN=https://your-app.vercel.app,https://your-custom-domain.com

# Database connection string (use your existing PostgreSQL database)
DATABASE_URL=postgresql://user:password@host:5432/database?schema=public
```

## Step 2: Configure Railway Service

Railway will automatically detect your configuration from:
- `railway.json` - Deployment settings
- `nixpacks.toml` - Build configuration
- `Procfile` - Start command

## Step 3: Deploy

1. Push your code to GitHub
2. Railway will automatically build and deploy
3. Note your Railway app URL (e.g., `https://your-app.railway.app`)

## Step 4: Update Your Next.js App

In your Pet-Sitter-AppTest project, create or update `.env.local`:

```bash
NEXT_PUBLIC_SOCKET_SERVER_URL=https://your-socket-server.railway.app
```

Replace with your actual Railway deployment URL.

## Step 5: Verify Deployment

1. Check health endpoint: `https://your-socket-server.railway.app/health`
2. Check socket status: `https://your-socket-server.railway.app/socket-status`

Both should return JSON with status "ok" or "isReady: true"

## Troubleshooting

### Build Fails
- Check Railway logs for errors
- Ensure DATABASE_URL is set correctly
- Verify Prisma schema matches your database

### Connection Issues
- Check CORS settings in SOCKET_IO_CORS_ORIGIN
- Ensure your Next.js app has correct NEXT_PUBLIC_SOCKET_SERVER_URL
- Check Railway logs for connection errors

### Database Issues
- Verify DATABASE_URL is accessible from Railway
- Ensure Prisma schema is up to date
- Run `prisma generate` during build (already configured)

## Important Notes

1. **Use the same DATABASE_URL** as your main app to ensure data consistency
2. **Update CORS origins** to include all your frontend URLs (localhost for dev, production URLs)
3. **Health checks** are configured at `/health` endpoint
4. **WebSocket support** - Railway supports WebSocket connections by default
5. **Automatic restarts** - Configured to restart on failure (max 10 retries)

## Monitoring

Railway provides:
- Real-time logs
- Metrics (CPU, Memory, Network)
- Deployment history
- Auto-scaling options

Check your Railway dashboard regularly to monitor your socket server's performance.

