# Dashboard ติดตามความคืบหน้าแผน GRC — การไฟฟ้านครหลวง (กฟน.)

Dashboard แสดงความคืบหน้าแผนปรับปรุงการบูรณาการด้าน GRC ประจำปี 2569 (15 กิจกรรม)
ออกแบบให้อัปเดตง่าย: **กรอก Excel → แปลงเป็น data.json → push ขึ้น GitHub** แล้ว Dashboard จะอัปเดตเอง

---

## ไฟล์ในชุดนี้

| ไฟล์ | หน้าที่ |
|------|---------|
| `index.html` | หน้า Dashboard (โหลดข้อมูลจาก `data.json`) — ไฟล์ที่ GitHub Pages แสดง |
| `data.json` | ข้อมูลความคืบหน้า (สร้างจาก Excel ผ่านตัวแปลง) |
| `GRC_Progress_Input.xlsx` | ไฟล์ Excel สำหรับกรอกข้อมูลรายเดือน (ต้นทาง) |
| `convert.html` | ตัวแปลง Excel → data.json ในเบราว์เซอร์ (ไม่ต้องติดตั้งอะไร) |
| `convert.py` | ตัวแปลง Excel → data.json ด้วย Python (สำหรับใช้ command line) |
| `README.md` | คู่มือนี้ |

---

## วิธีอัปเดตข้อมูลประจำเดือน (ขั้นตอนหลัก)

### 1. กรอกข้อมูลใน Excel
เปิด `GRC_Progress_Input.xlsx`
- ชีต **"กิจกรรม"** — กรอกช่อง **พื้นสีเหลือง**:
  - **ผลจริงสะสมรายเดือน (%)** ของแต่ละกิจกรรม (ช่อง ม.ค.–ธ.ค.)
  - **สิ่งที่ดำเนินการ (รายไตรมาส)** Q1–Q4
- ชีต **"ตั้งค่า"** — เลือก **"เดือนล่าสุดที่รายงาน"** ให้ตรงกับเดือนข้อมูล (เช่น กรกฎาคม)
- บันทึกไฟล์ (Save)

> ช่อง "เป้าหมายสะสมรายเดือน" (พื้นเทา) คือค่าตามแผน ปกติไม่ต้องแก้

### 2. แปลงเป็น data.json — เลือกวิธีใดวิธีหนึ่ง

**วิธี A — ในเบราว์เซอร์ (ง่ายสุด ไม่ต้องติดตั้ง)**
1. ดับเบิลคลิกเปิด `convert.html`
2. ลาก/เลือกไฟล์ `GRC_Progress_Input.xlsx`
3. กด **"แปลงและดาวน์โหลด data.json"** → ได้ไฟล์ `data.json`
4. นำ `data.json` ที่ดาวน์โหลดมา วางทับไฟล์เดิมในโฟลเดอร์โปรเจกต์

**วิธี B — ด้วย Python**
```bash
pip install openpyxl
python convert.py GRC_Progress_Input.xlsx data.json
```

### 3. push ขึ้น GitHub
```bash
git add .
git commit -m "อัปเดตข้อมูลเดือน กรกฎาคม 2569"
git push
```
รอประมาณ 1 นาที GitHub Pages จะอัปเดต Dashboard ให้อัตโนมัติ

---

## การตั้งค่าครั้งแรก (สร้าง GitHub Pages)

1. สร้าง repository ใหม่บน GitHub (เช่นชื่อ `grc-dashboard-2569`)
2. อัปโหลดไฟล์ทั้งหมดในโฟลเดอร์นี้เข้า repo
   - ผ่านเว็บ: เปิด repo → **Add file → Upload files** → ลากไฟล์ทั้งหมด → Commit
   - หรือผ่าน git:
     ```bash
     git init
     git add .
     git commit -m "first commit"
     git branch -M main
     git remote add origin https://github.com/<ชื่อผู้ใช้>/grc-dashboard-2569.git
     git push -u origin main
     ```
3. เปิดใช้ Pages: ไปที่ **Settings → Pages**
   - Source: **Deploy from a branch**
   - Branch: **main** / **/ (root)** → Save
4. รอสักครู่ Dashboard จะอยู่ที่
   `https://<ชื่อผู้ใช้>.github.io/grc-dashboard-2569/`

---

## หมายเหตุ
- ถ้าเปิด `index.html` จากเครื่องโดยดับเบิลคลิกตรงๆ จะขึ้น error เรื่อง CORS (เบราว์เซอร์บล็อกการอ่าน `data.json`)
  ให้เปิดผ่าน GitHub Pages หรือรันเซิร์ฟเวอร์ในเครื่อง: `python -m http.server` แล้วเปิด `http://localhost:8000`
- รูปพื้นหลังและโลโก้ฝังอยู่ในตัว `index.html` แล้ว ไม่ต้องมีไฟล์รูปแยก
- ต้องการเปิด Dashboard แบบดับเบิลคลิกได้ทันที (ไม่ผ่านเซิร์ฟเวอร์) ให้ใช้ไฟล์ `GRC_Dashboard_2569.html` (เวอร์ชันฝังข้อมูลในตัว) แทน
