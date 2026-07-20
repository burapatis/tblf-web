# TBLF Knowledge Hub — เว็บไซต์ฐานองค์ความรู้ (Custom Design)

เว็บไซต์ Static Site สำหรับโครงการการวิจัยและพัฒนากรอบแนวคิดภาวะผู้นำแบบไทยพอดี (TBLF)
สร้างด้วย **Hugo + Layout ที่ออกแบบเองทั้งหมด** (ไม่ใช้ธีมสำเร็จรูป ไม่มี dependency ภายนอก)

รูปแบบ: เว็บไซต์ศูนย์ความรู้สมัยใหม่ — Landing Page เต็มรูปแบบ, เมนูด้านบน, การ์ด, ไทม์ไลน์,
FAQ แบบ accordion, การ์ดดาวน์โหลด — ไม่ใช่รูปแบบหนังสือ/ตำราออนไลน์

---

## 1. การติดตั้งและรันบนเครื่อง

### ติดตั้ง Hugo (Extended edition, v0.146 ขึ้นไป — ทดสอบแล้วกับ v0.158.0)

- **Windows:** `winget install Hugo.Hugo.Extended`
- **macOS:** `brew install hugo`
- **Linux:** ดาวน์โหลดจาก https://github.com/gohugoio/hugo/releases

ตรวจสอบ: `hugo version`

### รันเซิร์ฟเวอร์ทดสอบ (ไม่ต้องติดตั้งธีมใด ๆ)

```bash
cd tblf-site
hugo server -D
```

เปิดเบราว์เซอร์ที่ http://localhost:1313

### Build เว็บไซต์จริง

```bash
hugo --minify
```

ไฟล์เว็บไซต์ทั้งหมดจะอยู่ในโฟลเดอร์ `public/` พร้อมนำไปวางบนเซิร์ฟเวอร์ใดก็ได้

---

## 2. การ Deploy บนโดเมนจริง

### ทางเลือก A — Cloudflare Pages (แนะนำ: ฟรี, เร็ว, CDN ทั่วโลก)

1. Push โค้ดขึ้น GitHub
2. Cloudflare Pages → Create Project → เชื่อม repo
3. Build command: `hugo --minify` / Output: `public`
4. Environment variable: `HUGO_VERSION = 0.158.0`
5. Custom domains → เพิ่มโดเมนจริง เช่น `tblf.ac.th` แล้วตั้งค่า DNS ตามที่ระบบแนะนำ

### ทางเลือก B — Netlify

Build command: `hugo --minify` / Publish directory: `public` → Add custom domain → ตั้งค่า DNS

### ทางเลือก C — GitHub Pages

ใช้ GitHub Actions workflow มาตรฐานของ Hugo แล้วตั้ง custom domain ใน Settings → Pages

### ก่อน deploy ทุกครั้ง

- `baseURL` ตั้งเป็นโดเมนจริงแล้ว: `https://tblf.thamdee.com/` (แก้ใน `hugo.toml` หากเปลี่ยนโดเมน)
- HTTPS: ผู้ให้บริการข้างต้นออกใบรับรอง SSL ให้อัตโนมัติ

---

## 3. โครงสร้างโปรเจกต์

```
tblf-site/
├── hugo.toml                  # ค่าตั้งค่า + เมนูหลัก (แก้ baseURL ที่นี่)
├── assets/css/main.css        # ดีไซน์ทั้งหมด (สี ฟอนต์ layout — แก้ธีมสีที่ :root)
├── layouts/
│   ├── index.html             # หน้าแรก (Landing Page) — แก้ข้อความหน้าแรกที่นี่
│   ├── _default/baseof.html   # โครง HTML หลัก
│   ├── _default/single.html   # หน้าเนื้อหาทั่วไป (page hero + prose)
│   ├── _default/list.html     # หน้ารายการ (ข่าว/หมวดหมู่)
│   ├── news/single.html       # หน้าข่าวรายชิ้น
│   ├── partials/              # head / header (เมนูบน) / footer
│   └── shortcodes/            # hint, faq, downloads, dl
├── content/
│   ├── about.md               # เกี่ยวกับโครงการ
│   ├── framework.md           # กรอบแนวคิด TBLF/TBLT
│   ├── research.md            # การวิจัย 10 ระยะ
│   ├── outputs.md             # ผลผลิต + ศูนย์ดาวน์โหลด
│   ├── resources.md           # สื่อการเรียนรู้และเครื่องมือ (หน้ารวม)
│   ├── learn.md               # เข้าใจ TBLF ฉบับเข้าใจง่าย (/resources/learn/)
│   ├── decision-card.md       # TBLF Decision Card v0.2 (/resources/decision-card/)
│   ├── casebook.md            # TBLF Casebook v0.2 — 14 กรณี (/resources/casebook/)
│   ├── toolkit.md             # TBLF Learning Toolkit v1.1 (/resources/toolkit/)
│   ├── checklist.md           # TBLF Operational Checklist v1.0 (/resources/checklist/)
│   ├── quick-checklist.md     # TBLF Quick Checklist 42 กิจกรรม (/resources/quick-checklist/)
│   ├── quick-start.md         # TBLF Quick Start Guide 12 หน้า (/resources/quick-start/)
│   ├── positioning.md         # ตำแหน่งทางวิชาการ เทียบ 11 แนวคิด (/framework/positioning/)
│   ├── decision-logs-hub.md   # หน้ารวมตัวอย่าง Decision Log (/resources/decision-logs/)
│   ├── dl-01.md … dl-08.md    # ตัวอย่าง Decision Log รายกรณี 8 หน้า
│   ├── faq.md / glossary.md / contact.md
│   └── news/                  # ข่าว ตั้งชื่อ YYYY-MM-slug.md
└── static/
    ├── downloads/             # วางไฟล์ PDF 4 ฉบับหลัก → URL: /downloads/ชื่อไฟล์.pdf
    │                          # (รวม tblf-brs01-v0-3.pdf — Master Manuscript v0.3)
    └── images/
```

## 4. วิธีแก้ไข/เพิ่มเนื้อหา

### แก้ข้อความ

- **หน้าแรก:** แก้ใน `layouts/index.html` (ข้อความ hero, ชิป 6 ฐาน, ระยะวิจัย, ผลผลิต, CTA)
- **หน้าอื่น:** แก้ไฟล์ Markdown ใน `content/`

### เพิ่มข่าว

สร้างไฟล์ใน `content/news/` เช่น `2026-08-workshop.md`:

```yaml
---
title: "ชื่อข่าว"
date: 2026-08-01
categories: ["กิจกรรม"]
tags: ["อบรม"]
---
เนื้อหาข่าว...
```

ข่าว 3 รายการล่าสุดจะแสดงบนหน้าแรกอัตโนมัติ

### Shortcodes ที่ใช้ได้ในทุกหน้า

```
{{</* hint info */>}} ข้อความกล่องสีฟ้า {{</* /hint */>}}
{{</* hint warning */>}} ข้อความกล่องสีเหลือง {{</* /hint */>}}

{{</* faq q="คำถาม?" */>}} คำตอบ (markdown ได้) {{</* /faq */>}}

{{</* downloads */>}}
{{</* dl href="/downloads/file.pdf" title="ชื่อเอกสาร" note="คำอธิบายสั้น" */>}}
{{</* dl href="/images/pic.png" title="ชื่อภาพ" note="คำอธิบาย" ext="PNG" download="true" */>}}
{{</* /downloads */>}}

{{</* figure-info src="/images/pic.png" alt="คำอธิบายภาพ" caption="คำบรรยายใต้ภาพ" */>}}
```

### เพิ่มเอกสารดาวน์โหลด

1. วางไฟล์ PDF ใน `static/downloads/`
2. เพิ่มบรรทัด `{{</* dl ... */>}}` ในส่วนดาวน์โหลดของ `content/outputs.md`

### ปรับธีมสี/ฟอนต์

แก้ตัวแปรที่ `:root` ใน `assets/css/main.css` — สีหลัก (`--indigo-*`), สีเน้น (`--gold`), ฟอนต์ (`--font-display`, `--font-body`)

## 5. แนวทางดูแลระยะยาว

- **Versioning:** ใช้ git — ทุกการแก้ไขมีประวัติ ตรงกับหลักการบริหารเอกสารของ Foundation Charter
- **วงจรอัปเดต:** ผลผลิตพร้อมเผยแพร่ → อัปเดตสถานะใน `outputs.md`/`resources.md` → วาง PDF ใน `downloads/` → โพสต์ข่าวใน `news/`
- **ทบทวนความสอดคล้อง:** เมื่อ TBLF/TBLT ปรับรุ่น ให้ทบทวน framework.md, outputs.md, resources.md, glossary.md พร้อมกัน
- **ไม่มีธีมภายนอกให้ต้องอัปเดต** — โค้ดทั้งหมดอยู่ในโปรเจกต์ ควบคุมได้ 100%
