# เว็บไซต์เข้ารหัสไฟล์ (FilesLock)
FilesLock หรือ เว็บไซต์เข้ารหัสไฟล์ คือระบบเว็บไซต์ที่ออกแบบมาเพื่อเพิ่มความปลอดภัยในการส่งไฟล์ที่มีความลับหรือข้อมูลสำคัญ เช่น ข้อมูลทางธุรกิจ ข้อมูลส่วนบุคคล หรือเอกสารลับของหน่วยงานภาครัฐ โดยมีจุดเด่นในการเข้ารหัสไฟล์ก่อนส่งให้แก่ผู้รับ เพื่อป้องกันการเข้าถึงจากบุคคลที่ไม่ได้รับอนุญาต

เมื่อผู้ใช้ทำการอัปโหลดไฟล์ผ่านเว็บไซต์ FilesLock ระบบจะให้ผู้ใช้กำหนดรหัสผ่านสำหรับการเปิดไฟล์ จากนั้นระบบจะสร้างไฟล์ที่ถูกเข้ารหัสไว้ พร้อมลิงก์ดาวน์โหลดสำหรับผู้รับ โดยผู้รับจะสามารถเปิดไฟล์ได้ก็ต่อเมื่อใส่รหัสผ่านที่ถูกต้องเท่านั้น ซึ่งรหัสผ่านนี้จะถูกส่งให้เฉพาะผู้ที่ผู้ส่งต้องการให้เข้าถึงเท่านั้น

# เทคโนโลยีและเครื่องมือที่ใช้ใน FilesLock
- ภาษาและไลบรารี
  - HTML: สร้างหน้าเว็บหลัก, หน้าเข้ารหัส, และหน้าถอดรหัส
    
  - JavaScript: จัดการเข้ารหัส/ถอดรหัสไฟล์ด้วย AES-256 และควบคุมการทำงานในเบราว์เซอร์
    
  - CSS: ออกแบบ UI ให้เรียบง่ายและรองรับทุกอุปกรณ์
    
  - CryptoJS (v4.1.1): ใช้เข้ารหัส/ถอดรหัสฝั่ง Client ด้วย  อัลกอริทึม AES-256
    
  - Netlify: แพลตฟอร์มโฮสต์เว็บไซต์แบบ static

---

- เครื่องมือพัฒนา
  - VS Code: เขียนและแก้ไขโค้ด
    
  - เว็บเบราว์เซอร์ (Chrome, Firefox): ทดสอบและ debug
    
  - Git/GitHub: จัดการเวอร์ชันและเก็บโค้ด
    
  - Netlify: โฮสต์เว็บไซต์แบบ static พร้อม HTTPS

---
    
- Web APIs
  - FileReader API: อ่านไฟล์ก่อนเข้ารหัส/ถอดรหัส
    
  - Blob API: สร้างไฟล์ .locked และไฟล์ที่ถอดรหัสเพื่อดาวน์โหลด

# ฟังก์ชันการเข้ารหัส/ถอดรหัส
- เข้ารหัสไฟล์ทุกประเภท (≤10MB) ด้วยรหัสผ่าน โดยใช้ AES-256 และบันทึกเป็นไฟล์ .locked
  
- ถอดรหัสไฟล์ .locked ด้วยรหัสผ่านที่ถูกต้อง เพื่อคืนค่าไฟล์ต้นฉบับ
  
- ประมวลผลทั้งหมดบนฝั่งผู้ใช้ (Client-Side Encryption) โดยไม่ส่งข้อมูลขึ้นเซิร์ฟเวอร์
  
- ผู้ใช้คัดลอกรหัสผ่านเองเพื่อส่งให้ผู้รับผ่านช่องทางปลอดภัย

# หลักการทำงานของ FilesLock
1. ผู้ใช้อัปโหลดไฟล์ (≤10MB) และตั้งรหัสผ่าน
   
2. ระบบเข้ารหัสไฟล์ด้วย AES-256 และสร้างไฟล์ .locked
   
3. ไฟล์ .locked เปิดไม่ได้หากไม่มีรหัสผ่าน
   
4. ผู้ใช้ส่งไฟล์และรหัสผ่านให้ผู้รับผ่านช่องทางปลอดภัย
   
5. ผู้รับอัปโหลดไฟล์ .locked และกรอกรหัสผ่านเพื่อถอดรหัสไฟล์ต้นฉบับ

## วิธีใช้งาน

### 1. เข้าเว็บไซต์
เปิดเบราว์เซอร์แล้วเข้า:  
[https://your-fileslock.netlify.app](https://fileslock-encrypt.netlify.app/)

---

### 2. เข้ารหัสไฟล์ (ผู้ส่ง)

1. คลิก **"เข้ารหัสไฟล์"**
2. เลือกไฟล์ (ขนาด **≤ 10 MB**)
3. กรอกรหัสผ่าน (≥ 8 ตัวอักษร)
4. คลิก **"เข้ารหัส"**

ระบบจะ:
- เข้ารหัสไฟล์เป็น `.locked`
- ดาวน์โหลดอัตโนมัติ
- แสดงรหัสผ่านให้คัดลอก

> **คัดลอกรหัสผ่าน: @Example1234**

---

### 3. ส่งไฟล์ให้ผู้รับ

| ส่งอะไร | วิธีส่ง |
|--------|--------|
| ไฟล์ `.locked` | อีเมล, Line, Google Drive, USB |
| รหัสผ่าน | **Signal, Telegram, หรือโทรศัพท์** |

**ห้ามส่งรหัสผ่านในช่องทางเดียวกับไฟล์**

---

### 4. ถอดรหัสไฟล์ (ผู้รับ)

1. คลิก **"ถอดรหัสไฟล์"**
2. อัปโหลดไฟล์ `.locked`
3. กรอกรหัสผ่านที่ได้รับ
4. คลิก **"ถอดรหัส"**

ไฟล์ต้นฉบับจะดาวน์โหลดทันที

---

## ข้อควรระวัง

| ควรทำ | ห้ามทำ |
|------|--------|
| ใช้รหัสผ่านยาว ≥8 ตัว | ใช้รหัสผ่านง่าย เช่น `12345678` |
| ส่งรหัสผ่านผ่านแอปเข้ารหัส | ส่งรหัสผ่านในอีเมลเดียวกับไฟล์ |
| ลบไฟล์ `.locked` หลังส่ง | เก็บไฟล์ไว้ในที่สาธารณะ |

---

# แหล่งอ้างอิง
1. **Brix.** (2021). *CryptoJS: A JavaScript library for cryptography.* GitHub.  
   สืบค้นจาก [https://github.com/brix/crypto-js](https://github.com/brix/crypto-js)

2. **Mozilla Developer Network (MDN).** (2025). *FileReader - Web APIs.*  
   สืบค้นจาก [https://developer.mozilla.org/en-US/docs/Web/API/FileReader](https://developer.mozilla.org/en-US/docs/Web/API/FileReader)

3. **Mozilla Developer Network (MDN).** (2025). *Blob - Web APIs.*  
   สืบค้นจาก [https://developer.mozilla.org/en-US/docs/Web/API/Blob](https://developer.mozilla.org/en-US/docs/Web/API/Blob)

4. **Mozilla Developer Network (MDN).** (2025). *HTML: HyperText Markup Language.*  
   สืบค้นจาก [https://developer.mozilla.org/en-US/docs/Web/HTML](https://developer.mozilla.org/en-US/docs/Web/HTML)

5. **Netlify.** (2025). *Netlify: Deploy modern static websites.*  
   สืบค้นจาก [https://www.netlify.com](https://www.netlify.com)

6. **GitHub.** (2025). *GitHub Pages: Websites for you and your projects.*  
   สืบค้นจาก [https://pages.github.com](https://pages.github.com)

7. **National Institute of Standards and Technology (NIST).** (2001). *Advanced Encryption Standard (AES).*  
   สืบค้นจาก [https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.197.pdf](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.197.pdf)

---

# Project

        https://fileslock-encrypt.netlify.app/

---

ผู้จัดทำ : นาย ณัฐภัทร บุญภิรมย์  
Github : nattaken  
Language: HTML, CSS, JAVASCRIPT
