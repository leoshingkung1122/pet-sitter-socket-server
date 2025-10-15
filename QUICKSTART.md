# Quick Start Guide

## สำหรับการ Deploy Socket Server ไป Railway

### ขั้นตอนที่ 1: เตรียม Environment Variables

คุณต้องเตรียม environment variables เหล่านี้สำหรับ Railway:

```bash
PORT=${{PORT}}  # Railway จะตั้งให้อัตโนมัติ
SOCKET_IO_CORS_ORIGIN=https://your-app.vercel.app,http://localhost:3000
DATABASE_URL=postgresql://user:password@host:5432/database
```

### ขั้นตอนที่ 2: Deploy บน Railway

1. **สร้าง Project ใหม่บน Railway**
   - ไปที่ [Railway.app](https://railway.app)
   - คลิก "New Project"
   - เลือก "Deploy from GitHub repo"
   - เลือก repository `pet-sitter-socket-server`

2. **ตั้งค่า Environment Variables**
   - ไปที่ Variables tab
   - เพิ่มตัวแปรทั้ง 3 ตัวข้างต้น
   - คลิก "Add Variable"

3. **Deploy**
   - Railway จะ build และ deploy อัตโนมัติ
   - รอจนกว่าจะเห็น "Deployed" status
   - คัดลอก URL ของ deployment (เช่น `https://your-app.railway.app`)

### ขั้นตอนที่ 3: ตรวจสอบการ Deploy

เปิดเบราว์เซอร์และทดสอบ endpoints เหล่านี้:

```
https://your-app.railway.app/health
https://your-app.railway.app/socket-status
```

ถ้าทำงานได้ จะเห็น JSON response พร้อม status "ok"

### ขั้นตอนที่ 4: Update Pet-Sitter-AppTest

1. **สร้างไฟล์ `.env.local`** ใน Pet-Sitter-AppTest:

```bash
NEXT_PUBLIC_SOCKET_SERVER_URL=https://your-app.railway.app
DATABASE_URL=your_database_url
NEXTAUTH_SECRET=your_secret
NEXTAUTH_URL=http://localhost:3000
```

2. **ถ้า Deploy บน Vercel**:
   - ไปที่ Vercel Dashboard
   - เลือก project Pet-Sitter-AppTest
   - Settings → Environment Variables
   - เพิ่ม `NEXT_PUBLIC_SOCKET_SERVER_URL=https://your-app.railway.app`
   - Redeploy

### ขั้นตอนที่ 5: ทดสอบ

1. เปิด Pet-Sitter-AppTest
2. Login เข้าระบบ
3. ทดสอบ chat functionality
4. เช็ค browser console ว่ามี connection errors หรือไม่

## Checklist ก่อน Deploy

- [ ] มี PostgreSQL database พร้อมใช้งาน
- [ ] มี Railway account
- [ ] เตรียม DATABASE_URL แล้ว
- [ ] รู้ URL ของ frontend app (Vercel)
- [ ] push code ขึ้น GitHub แล้ว

## Checklist หลัง Deploy

- [ ] เช็ค `/health` endpoint ทำงานได้
- [ ] เช็ค `/socket-status` endpoint ทำงานได้
- [ ] Update `NEXT_PUBLIC_SOCKET_SERVER_URL` ใน Pet-Sitter-AppTest
- [ ] ทดสอบ chat functionality
- [ ] เช็ค Railway logs ว่าไม่มี errors

## ปัญหาที่อาจเจอ

### 1. Build Failed
**สาเหตุ**: ไม่มี DATABASE_URL
**แก้ไข**: ตั้ง DATABASE_URL ใน Railway environment variables

### 2. Connection Timeout
**สาเหตุ**: CORS ไม่ตรงกับ frontend URL
**แก้ไข**: เช็ค SOCKET_IO_CORS_ORIGIN ใน Railway

### 3. Database Error
**สาเหตุ**: DATABASE_URL ไม่ถูกต้อง
**แก้ไข**: ตรวจสอบ connection string และให้แน่ใจว่า database accessible

## สรุปขั้นตอน

```
1. Deploy Socket Server ไป Railway
   └─> ตั้ง environment variables
   
2. ได้ Railway URL
   └─> https://your-app.railway.app
   
3. Update Pet-Sitter-AppTest
   └─> .env.local: NEXT_PUBLIC_SOCKET_SERVER_URL
   └─> Vercel: Environment Variables
   
4. ทดสอบ
   └─> Login
   └─> Chat
   └─> เช็ค console
```

## Next Steps

หลังจาก deploy สำเร็จแล้ว:

1. **Monitor** - ดู Railway logs เป็นประจำ
2. **Optimize** - ปรับแต่ง performance ตามการใช้งาน
3. **Scale** - เพิ่ม resources ถ้าจำเป็น
4. **Backup** - สำรอง configuration และ environment variables

## ข้อมูลเพิ่มเติม

- [DEPLOYMENT.md](./DEPLOYMENT.md) - คู่มือ deployment แบบละเอียด
- [SETUP_CLIENT.md](./SETUP_CLIENT.md) - คู่มือตั้งค่า client
- [README.md](./README.md) - เอกสารหลัก

