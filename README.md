# VEO Studio — Landing Page

หน้า Landing แบบ **static** (HTML/CSS/JS ไฟล์เดียว ไม่มี build step) สำหรับ VEO Studio —
Thai-first AI Reel Studio โชว์ตัวอย่างคลิปที่สร้างจากตัวโปรดักต์จริง แล้วส่งผู้ใช้ต่อไปที่แอป

> นี่เป็นเรโปแยกเฉพาะหน้า Landing เท่านั้น — **ตัวแอปหลัก** (Next.js) อยู่คนละเรโป/คนละดีพลอย
> ปุ่ม CTA ทั้งหมดลิงก์ออกไปที่โดเมนของแอป (`/create`, `/pricing`, `/playbook`)

## โครงสร้าง

```
index.html          หน้าเดียวจบ (inline CSS + JS, ไม่มี dependency)
og-cover.jpg        ภาพ social share (1200×630)
vercel.json         cleanUrls + cache headers + security headers
assets/
  hero-reel.mp4/.jpg      คลิป hero (พรีเซนเตอร์ — Commerce)
  showcase-1..3.mp4/.jpg  การ์ดตัวอย่าง (Viral / อนิเมะ / มาสคอต 3D)
  strip-1..6.jpg          รูป tile ของแถบรีลเลื่อน
```

ทุกคลิปเป็น output จริงจาก VEO Studio (transcode เป็น 540px, muted, faststart)

## Deploy บน Vercel

**วิธี A — Dashboard (แนะนำ)**
1. push เรโปนี้ขึ้น GitHub
2. Vercel → **Add New… → Project → Import** เรโปนี้
3. Framework Preset: **Other** · Build Command: *(เว้นว่าง)* · Output Directory: `./`
4. **Deploy** — ได้ URL `*.vercel.app` ทันที

**วิธี B — CLI**
```bash
npm i -g vercel
vercel          # preview
vercel --prod   # production
```

## ⚠️ ตั้งค่าโดเมนก่อนขึ้นจริง

`index.html` ฝัง URL โปรดักชันไว้ที่ **3 จุด** (ตอนนี้ = `https://xn--42cfc0k1a8b.net/` / จองคิว.net)
ถ้าดีพลอยที่โดเมนอื่น ต้องแก้ให้ตรงโดเมนที่หน้านี้จะอยู่จริง เพื่อ SEO/แชร์ให้ถูก:

- `<link rel="canonical" href="…">`
- `<meta property="og:url" content="…">`
- `<meta property="og:image" content="…/og-cover.jpg">` และ `<meta name="twitter:image" …>`

(ปุ่ม CTA ที่ชี้ไปแอป **ไม่ต้องแก้** — มันชี้ไปโดเมนแอปอยู่แล้ว)

## อัปเดตคลิป

แทนไฟล์ใน `assets/` โดยใช้ **ชื่อไฟล์เดิม** — cache ตั้งไว้ 1 วัน (stale-while-revalidate 7 วัน)
ของใหม่จะขึ้นภายใน ~1 วัน หรือกด **Redeploy** ใน Vercel ให้ขึ้นทันที
