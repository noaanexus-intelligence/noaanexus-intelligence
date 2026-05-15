# สสอ.ปากช่อง — Smart Health Finance

ระบบบริหารอัตราค่าบริการสาธารณสุข อำเภอปากช่อง จังหวัดนครราชสีมา

---

## โครงสร้างโปรเจกต์

```
app/
  page.tsx              → redirect ไป /dashboard
  layout.tsx            → Root layout (SidebarLayout)
  dashboard/page.tsx    → KPI, Approval Queue, Facility Summary
  pricing/page.tsx      → ตารางราคา Admin (Export Excel/PDF)
  public/page.tsx       → Public Portal สำหรับประชาชน
components/
  SidebarLayout.tsx     → Sidebar + mobile menu
lib/
  supabase.ts           → Supabase client + API helpers
supabase/
  migrations/
    001_schema.sql      → SQL schema ทั้งหมด + ข้อมูลตัวอย่าง
netlify.toml            → Deploy config สำหรับ Netlify
```

---

## ขั้นตอนการติดตั้ง

### 1. ตั้งค่า Supabase

1. ไปที่ [supabase.com](https://supabase.com) → สร้าง project ใหม่
2. ไปที่ **SQL Editor** → New query
3. Copy ทั้งหมดจาก `supabase/migrations/001_schema.sql` แล้ว Run
4. ไปที่ **Settings → API** → คัดลอก:
   - `Project URL`
   - `anon public` key

### 2. ตั้งค่า Environment Variables

สร้างไฟล์ `.env.local` ที่ root:

```env
NEXT_PUBLIC_SUPABASE_URL=https://xxxxxxxxxxxx.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

### 3. รัน Development

```bash
npm install
npm run dev
```

เปิด [http://localhost:3000](http://localhost:3000)

---

## Deploy บน Netlify

### วิธีที่ 1: GitHub + Netlify (แนะนำ)

1. Push โปรเจกต์ขึ้น GitHub
2. ไปที่ [app.netlify.com](https://app.netlify.com) → **Add new site → Import an existing project**
3. เชื่อมต่อ GitHub → เลือก repo
4. Netlify จะตรวจจาก `netlify.toml` อัตโนมัติ
5. ไปที่ **Site configuration → Environment variables** → เพิ่ม:
   - `NEXT_PUBLIC_SUPABASE_URL`
   - `NEXT_PUBLIC_SUPABASE_ANON_KEY`
6. กด **Deploy site**

### วิธีที่ 2: Netlify CLI

```bash
npm install -g netlify-cli
netlify login
netlify init
netlify env:set NEXT_PUBLIC_SUPABASE_URL "https://xxxx.supabase.co"
netlify env:set NEXT_PUBLIC_SUPABASE_ANON_KEY "eyJ..."
netlify deploy --prod
```

---

## หน้าในระบบ

| URL | หน้า | ผู้ใช้ |
|-----|------|--------|
| `/dashboard` | Executive Dashboard | Admin |
| `/pricing` | ตารางอัตราค่าบริการ | Admin |
| `/public` | Public Portal | ประชาชนทั่วไป |

---

## Tech Stack

- **Frontend**: Next.js 14 + TypeScript + Tailwind CSS
- **Backend**: Supabase (PostgreSQL + RLS)
- **Icons**: Lucide React
- **Export**: xlsx + jsPDF + jspdf-autotable
- **Hosting**: Netlify
