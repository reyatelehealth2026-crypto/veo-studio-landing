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

## โดเมน / SEO

`index.html` ตั้ง canonical + OG ไว้ที่ **`https://veo.studio/`** แล้ว รวม **5 จุด**:

- `<link rel="canonical">`
- `<meta property="og:url">`
- `<meta property="og:image">` + `<meta name="twitter:image">` (`…/og-cover.jpg`)
- JSON-LD `"url"`

ถ้าย้ายโดเมนอีก ให้แก้ 5 จุดนี้ให้ตรงโดเมนใหม่
(ปุ่ม CTA ชี้ไปตัวแอปที่ **จองคิว.net** `/create` `/pricing` `/playbook` — **ไม่ต้องแก้**)

## Analytics / Insights

เปิด **Vercel Web Analytics** ที่ Vercel → project → **Analytics → Enable** (cookieless ไม่ต้องมี cookie banner)
หน้านี้ฝัง script ไว้แล้ว รวม **Speed Insights** (Web Vitals จริงจากผู้ใช้)

**Event ที่เก็บ** (ดูใน Vercel → Analytics → Custom Events):
- `cta_click` — คนกดปุ่ม/ลิงก์ · props: `label`, `dest` (create/pricing/playbook/anchor), `loc`
- `video_play` — คนดูคลิป (เล่นครั้งแรกของแต่ละคลิป) · props: `clip`
- `scroll_depth` — เลื่อนถึง 25 / 50 / 75 / 90%
- `section_view` — เห็น section: showcase / features / how / pricing / faq

**เพิ่ม Google Analytics 4 (ถ้าต้องการ funnel ลึก):** วาง gtag snippet มาตรฐาน (ที่มี `G-XXXXXXX`) ใน `<head>`
— event ทั้งหมดข้างบนจะ **ส่งเข้า GA4 อัตโนมัติ** (`track()` sink เข้า `gtag` ให้แล้ว) ไม่ต้องแก้โค้ดเพิ่ม

## อัปเดตคลิป

แทนไฟล์ใน `assets/` โดยใช้ **ชื่อไฟล์เดิม** — cache ตั้งไว้ 1 วัน (stale-while-revalidate 7 วัน)
ของใหม่จะขึ้นภายใน ~1 วัน หรือกด **Redeploy** ใน Vercel ให้ขึ้นทันที
