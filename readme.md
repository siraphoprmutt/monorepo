# Monorepo with pnpm

## 1. สร้าง root project

```bash

mkdir monorepo
cd monorepo
pnpm init

```

## 2. สร้าง workspace config (pnpm-workspace.yaml)

```bash
packages:
  - 'packages/*'

```

## 3. สร้าง folder packages

```bash

mkdir -p packages/webhook

```

## 4. ใช้ pnpm init และติดตั้ง express

```bash
cd packages/webhook
pnpm init
pnpm add express nodemon

```

## 5. กลับไป root แล้วรันติดตั้งทั้งหมด

```bash
pnpm install

```

## 6. สร้างไฟล์ index.js ใน folder webhook

```bash

import express from 'express';

const app = express();

app.use(express.json());
app.use(express.urlencoded({ extended: true }));

app.post('/webhook', (req, res) => {
    console.log('Received webhook:', req.body);
    res.sendStatus(200);
});

const port = 3000

app.listen(port, () => {
    console.log(`Server is running on port ${port}`);
});

export default app;

```

## 7. ทดสอบรันแต่ละระบบ

```bash
pnpm --filter webhook run dev

```

## 8. ติดตั้ง pm2 แบบ global

```bash

npm install -g pm2

```

## 9. สร้าง ecosystem.config.js ที่ root ของ monorepo

```bash

module.exports = {
    apps: [
        {
            name: 'webhook',
            cwd: './packages/webhook',
            script: 'index.js',
            interpreter: 'node',
        },
    ]
}

```

## 10. สั่งรันทั้งหมด

```bash
# สั่งรันทั้งหมด
pm2 start ecosystem.config.js

# สั่งบันทึกและให้รันต่อแม้เครื่อง restart
pm2 save
pm2 startup

# ดู status
pm2 list

```

### ✅ ทางเลือกสำหรับ Windows: ใช้ pm2-windows-startup หรือ Task Scheduler

```bash
# ทางเลือกสำหรับ Windows: ใช้ pm2-windows-startup หรือ Task Scheduler

## ติดตั้ง
npm install -g pm2-windows-startup

## สั่งรัน
pm2-startup install

## บันทึก process ที่ต้องการให้เริ่มอัตโนมัติ
pm2 save

```

### 📚 อ่านเพิ่มเติม

[https://pnpm.io/](https://pnpm.io/)
[https://pm2.keymetrics.io/](https://pm2.keymetrics.io/)
