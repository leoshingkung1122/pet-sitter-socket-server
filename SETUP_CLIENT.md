# Setting Up Pet-Sitter-AppTest to Use Deployed Socket Server

## Overview
This guide will help you configure your Pet-Sitter-AppTest to connect to the deployed socket server on Railway.

## Step 1: Create Environment File

In your `Pet-Sitter-AppTest` directory, create or update `.env.local`:

```bash
# Socket Server URL - Replace with your Railway deployment URL
NEXT_PUBLIC_SOCKET_SERVER_URL=https://your-socket-server.railway.app

# Your other environment variables...
DATABASE_URL=your_database_url
NEXTAUTH_SECRET=your_nextauth_secret
NEXTAUTH_URL=your_nextauth_url
```

## Step 2: Verify Socket Configuration

The socket client is already configured in:
- `src/hooks/useSocket.ts`
- `src/lib/utils/socket.ts`

These files already use `process.env.NEXT_PUBLIC_SOCKET_SERVER_URL` with a fallback to `http://localhost:4000`.

## Step 3: Update CORS on Railway

Make sure your Railway socket server has the correct CORS configuration:

In Railway dashboard, set:
```
SOCKET_IO_CORS_ORIGIN=https://your-app.vercel.app,http://localhost:3000
```

Include:
- Your production URL (e.g., Vercel deployment)
- `http://localhost:3000` for local development

## Step 4: Test Connection

### Local Testing
1. Start your socket server locally: `cd pet-sitter-socket-server && npm run dev`
2. Start Pet-Sitter-AppTest: `cd Pet-Sitter-AppTest && npm run dev`
3. Login and test chat functionality

### Production Testing
1. Deploy socket server to Railway
2. Update `NEXT_PUBLIC_SOCKET_SERVER_URL` in Vercel environment variables
3. Redeploy your Next.js app
4. Test chat functionality in production

## Step 5: Verify Connection

The app will:
1. Check socket server status at `/socket-status`
2. Show loading state while connecting
3. Automatically retry on connection failure

You can monitor connection in browser console:
- Look for "🔌 Connecting to Socket.IO server"
- Check for "✅ New socket connection established"

## Troubleshooting

### Connection Fails
- Verify `NEXT_PUBLIC_SOCKET_SERVER_URL` is correct
- Check Railway logs for errors
- Ensure CORS origins include your frontend URL

### Messages Not Sending
- Check browser console for errors
- Verify user is authenticated
- Check Railway logs for database errors

### Slow Connection
- Socket.IO uses polling first, then upgrades to WebSocket
- This is normal and usually completes within 2-3 seconds

## Environment Variables Checklist

### Pet-Sitter-AppTest (.env.local)
- ✅ `NEXT_PUBLIC_SOCKET_SERVER_URL` - Your Railway socket server URL
- ✅ `DATABASE_URL` - Your database connection string
- ✅ `NEXTAUTH_SECRET` - Your NextAuth secret
- ✅ `NEXTAUTH_URL` - Your app URL

### Railway Socket Server
- ✅ `PORT` - Auto-set by Railway
- ✅ `SOCKET_IO_CORS_ORIGIN` - Your frontend URLs (comma-separated)
- ✅ `DATABASE_URL` - Your database connection string

## Architecture

```
Frontend (Vercel)                Socket Server (Railway)
Pet-Sitter-AppTest        <-->   pet-sitter-socket-server
     |                                    |
     |                                    |
     +----------> PostgreSQL <------------+
                (Shared Database)
```

Both use the same database for:
- User data
- Chat messages
- Online status
- Unread counts

## Important Notes

1. **Same Database**: Use the same `DATABASE_URL` for both frontend and socket server
2. **Environment Variables**: Remember to set in both local (.env.local) and production (Vercel)
3. **CORS**: Must include all frontend URLs in `SOCKET_IO_CORS_ORIGIN`
4. **Reconnection**: Client automatically reconnects on connection loss
5. **Fallback**: If socket fails, app should still function (without real-time features)

