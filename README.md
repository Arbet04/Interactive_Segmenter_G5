# Interactive_Segmenter_G5

# MediaPipe Interactive Segmenter

## 1. ภาพรวม

โปรเจกต์นี้เป็นระบบ **Interactive Image Segmentation** โดยใช้ **Google MediaPipe Tasks Vision – Interactive Segmenter**

ระบบสามารถเลือกและตัดแยกวัตถุออกจากพื้นหลังได้ 2 รูปแบบ

### 1.1 Interactive Segmenter — รูปภาพ

ผู้ใช้สามารถ:

* อัปโหลดรูปภาพ
* ลากเมาส์วาดเส้นบนวัตถุที่ต้องการ
* ให้ AI วิเคราะห์วัตถุ
* ปรับค่า Threshold
* ดูพื้นที่ที่ AI เลือก
* ส่งออกเป็นไฟล์ PNG แบบพื้นหลังโปร่งใส

### 1.2 Interactive Segmenter — Live Webcam

ผู้ใช้สามารถ:

* เปิดกล้อง Webcam
* คลิกบนวัตถุที่ต้องการเลือก
* AI วิเคราะห์วัตถุแบบ Real-time
* ปรับ Threshold
* ดู Mask ของวัตถุ
* ถ่ายภาพและส่งออกเป็น PNG แบบพื้นหลังโปร่งใส

---

# 2. โครงสร้างโปรเจกต์

แนะนำให้จัดไฟล์ประมาณนี้

```text
InteractiveSegmenter/
│
├── mediapipe-interactive-segmenter.html
├── webcam-segmenter.html
│
└── README.md
```

โดย

```text
mediapipe-interactive-segmenter.html
```

คือหน้าเลือกวัตถุจากรูปภาพ

และ

```text
webcam-segmenter.html
```

คือหน้า Interactive Segmenter จาก Webcam

---

# 3. เทคโนโลยีที่ใช้

โปรเจกต์ใช้เทคโนโลยีหลักดังนี้

* HTML5
* CSS3
* JavaScript ES Module
* Canvas API
* WebRTC / Webcam API
* MediaPipe Tasks Vision
* Interactive Segmenter
* TensorFlow Lite Model

ตัวระบบประมวลผลหลักเกิดขึ้นภายใน Browser

```text
รูปภาพ / Webcam
       ↓
   JavaScript
       ↓
MediaPipe Interactive Segmenter
       ↓
 Confidence Mask
       ↓
 Threshold
       ↓
 ตัดพื้นหลัง
       ↓
 PNG โปร่งใส
```

---

# 4. MediaPipe Interactive Segmenter คืออะไร

Interactive Segmenter เป็นโมเดล AI สำหรับแยกวัตถุออกจากภาพตามตำแหน่งที่ผู้ใช้ระบุ

ตัวอย่างเช่น

```text
ภาพคน
   ↓
ผู้ใช้ลากเส้นบนตัวคน
   ↓
MediaPipe วิเคราะห์
   ↓
สร้าง Mask
   ↓
แยกคนออกจากพื้นหลัง
```

ข้อดีคือผู้ใช้ไม่จำเป็นต้องเขียน Bounding Box หรือกำหนด Mask เองทั้งหมด

---

# 5. Model ที่ใช้

โค้ดใช้โมเดล

```text
magic_touch.tflite
```

จาก MediaPipe Model Repository

โดยกำหนด URL ใน JavaScript เช่น

```javascript
const MODEL_URL =
"https://storage.googleapis.com/mediapipe-models/interactive_segmenter/magic_touch/float32/1/magic_touch.tflite";
```

Browser จะดาวน์โหลดโมเดลมาใช้งาน

---

# 6. การติดตั้ง

ไม่จำเป็นต้องติดตั้ง MediaPipe ผ่าน Python

โค้ดเรียกใช้งานผ่าน CDN โดยตรง

```javascript
import {
    FilesetResolver,
    InteractiveSegmenterLegacy
} from "https://cdn.jsdelivr.net/npm/@mediapipe/tasks-vision@latest/vision_bundle.mjs";
```

และโหลด WebAssembly Runtime จาก

```text
https://cdn.jsdelivr.net/npm/@mediapipe/tasks-vision@latest/wasm
```

ดังนั้นเครื่องที่ใช้งานควรเชื่อมต่อ Internet

---

# 7. การเปิดโปรเจกต์

## วิธีที่ 1 — ใช้ Visual Studio Code

เปิดโฟลเดอร์โปรเจกต์ เช่น

```text
D:\InteractiveSegmenter
```

จากนั้นติดตั้ง Extension

```text
Live Server
```

แล้วเปิดไฟล์

```text
mediapipe-interactive-segmenter.html
```

คลิกขวา

```text
Open with Live Server
```

Browser จะเปิดหน้าเว็บขึ้นมา

---

# 8. ไม่ควรเปิดด้วย file:// โดยตรง

ไม่แนะนำให้เปิดแบบ

```text
file:///D:/InteractiveSegmenter/mediapipe-interactive-segmenter.html
```

เพราะ Browser อาจมีปัญหากับ Module, WebAssembly หรือการโหลด Resource

ควรใช้ Web Server เช่น

```text
Live Server
```

หรือ

```text
localhost
```

แทน

ตัวอย่าง

```text
http://127.0.0.1:5500/
```

---

# 9. วิธีใช้งาน — โหมดรูปภาพ

เปิดหน้า

```text
mediapipe-interactive-segmenter.html
```

## ขั้นตอนที่ 1

กด

```text
อัปโหลดภาพ
```

หรือสามารถลากรูปภาพไปวางใน Drop Zone ได้

รองรับไฟล์ เช่น

```text
JPG
PNG
WEBP
```

---

## ขั้นตอนที่ 2

เมื่อโหลดรูปสำเร็จ รูปจะปรากฏบน Canvas

ระบบจะแสดงพื้นที่ทำงาน

```text
Image
 +
Mask Overlay
 +
Stroke
```

---

## ขั้นตอนที่ 3

ใช้เมาส์ลากเส้นบนวัตถุที่ต้องการเลือก

ตัวอย่าง

```text
       คน
      ███
     █████
      ███

      ↑
   ลากเส้นตรงนี้
```

เส้นสีฟ้าเป็นตำแหน่งที่ส่งให้ MediaPipe วิเคราะห์

---

## ขั้นตอนที่ 4

ปล่อยเมาส์

ระบบจะส่งข้อมูลไปยัง

```javascript
segmenter.segment()
```

เพื่อสร้าง Confidence Mask

ตัวอย่าง

```javascript
const result = segmenter.segment(imgEl, roi);
```

จากนั้นอ่าน Mask

```javascript
const mask = result.confidenceMasks[0];
```

---

# 10. Confidence Mask

Mask จะเก็บค่าความมั่นใจของโมเดลในแต่ละ Pixel

โดยทั่วไปค่าจะอยู่ประมาณ

```text
0.0 → ไม่ใช่วัตถุ
1.0 → มีความมั่นใจสูงว่าเป็นวัตถุ
```

ตัวอย่าง

```text
Pixel       Confidence

พื้นหลัง      0.05
พื้นหลัง      0.12
ขอบวัตถุ      0.48
วัตถุ         0.82
วัตถุ         0.97
```

---

# 11. Threshold

ระบบมี Slider

```text
Threshold
```

ค่าเริ่มต้น

```text
0.50
```

ถ้ากำหนด

```text
Threshold = 0.50
```

ระบบจะเลือก Pixel ที่

```text
Confidence >= 0.50
```

ตัวอย่าง

```text
Confidence = 0.80
Threshold  = 0.50

→ เลือก
```

แต่

```text
Confidence = 0.30
Threshold  = 0.50

→ ไม่เลือก
```

---

# 12. การปรับ Threshold

ถ้าพื้นหลังติดมาด้วยมากเกินไป

ให้เพิ่ม Threshold

เช่น

```text
0.50 → 0.60 → 0.70
```

ถ้าวัตถุถูกตัดออกมากเกินไป

ให้ลด Threshold

เช่น

```text
0.50 → 0.40 → 0.30
```

แนะนำให้เริ่มจาก

```text
0.50
```

แล้วค่อยปรับ

---

# 13. การล้างการเลือก

กด

```text
ล้างเส้น (Clear Strokes)
```

ระบบจะล้าง

* Stroke
* Mask
* Overlay

จากนั้นสามารถลากเส้นใหม่ได้

---

# 14. การ Export PNG

เมื่อระบบสร้าง Mask สำเร็จแล้ว

กด

```text
ส่งออก PNG
```

ระบบจะสร้าง Canvas ใหม่

จากนั้นกำหนด Alpha Channel

```javascript
outData.data[idx+3] =
    selected ? 255 : 0;
```

ความหมายคือ

```text
วัตถุ
Alpha = 255
→ มองเห็น

พื้นหลัง
Alpha = 0
→ โปร่งใส
```

ผลลัพธ์จึงเป็น PNG แบบ Transparent Background

---

# 15. ตัวอย่างผลลัพธ์

```text
ภาพต้นฉบับ

┌────────────────────┐
│                    │
│       คน           │
│      ███           │
│     █████          │
│                    │
│      พื้นหลัง      │
└────────────────────┘


หลัง Segment

┌────────────────────┐
│                    │
│       ███          │
│      █████         │
│                    │
│   Transparent      │
└────────────────────┘
```

---

# 16. วิธีใช้งาน — Webcam

เปิดไฟล์

```text
webcam-segmenter.html
```

จากนั้นรอให้ระบบโหลด

```text
Wasm Runtime
```

และ

```text
Interactive Segmenter Model
```

จนขึ้น

```text
โมเดลพร้อมใช้งาน
```

---

# 17. เปิดกล้อง

กดปุ่ม

```text
เปิดใช้งานกล้อง (Start Camera)
```

Browser จะถาม Permission

เช่น

```text
Allow camera?
```

ให้เลือก

```text
Allow
```

---

# 18. เลือกวัตถุจาก Webcam

เมื่อกล้องทำงานแล้ว

ให้คลิกบนวัตถุที่ต้องการเลือก

เช่น

```text
กล้อง
       ↓

┌────────────────────┐
│                    │
│       👤           │
│                    │
│       ↑            │
│    คลิกตรงนี้      │
│                    │
└────────────────────┘
```

ตำแหน่งที่คลิกจะถูกส่งให้ Interactive Segmenter

```javascript
activePoint = {
    x: px / baseCanvas.width,
    y: py / baseCanvas.height
};
```

จากนั้น

```javascript
segmenter.segment(
    webcam,
    { keypoint: activePoint }
);
```

---

# 19. Real-time Processing

ระบบ Webcam จะทำงานวนซ้ำด้วย

```javascript
requestAnimationFrame(renderLoop);
```

ลำดับการทำงานคือ

```text
Webcam Frame
      ↓
Canvas
      ↓
MediaPipe
      ↓
Confidence Mask
      ↓
Threshold
      ↓
Overlay
```

---

# 20. ถ่ายภาพส่งออก

เมื่อเลือกวัตถุสำเร็จ

ปุ่ม

```text
📷 ถ่ายภาพส่งออก PNG
```

จะเปิดใช้งาน

เมื่อกด ระบบจะนำ Frame ปัจจุบันจาก Webcam มารวมกับ Mask

แล้วสร้าง PNG ที่พื้นหลังโปร่งใส

---

# 21. เปรียบเทียบ 2 โหมด

| คุณสมบัติ       | รูปภาพ | Webcam |
| --------------- | ------ | ------ |
| อัปโหลดรูป      | ✓      | -      |
| Webcam          | -      | ✓      |
| วาดเส้น         | ✓      | -      |
| คลิกเลือก       | -      | ✓      |
| Real-time       | -      | ✓      |
| Threshold       | ✓      | ✓      |
| Mask            | ✓      | ✓      |
| Export PNG      | ✓      | ✓      |
| พื้นหลังโปร่งใส | ✓      | ✓      |

---

# 22. ความแตกต่างของ Interaction

## รูปภาพ

ใช้

```javascript
{
    scribble: currentScribble
}
```

ผู้ใช้ลากเส้นบนวัตถุ

---

## Webcam

ใช้

```javascript
{
    keypoint: activePoint
}
```

ผู้ใช้คลิกตำแหน่งบนวัตถุ

---

# 23. ทำไมใช้ InteractiveSegmenterLegacy

ในโค้ดมีการใช้

```javascript
InteractiveSegmenterLegacy
```

แทน API รุ่นใหม่บางรูปแบบ

เหตุผลคือโค้ดนี้ถูกออกแบบให้ใช้กับ

```text
magic_touch.tflite
```

และ Legacy API

จึงช่วยให้รูปแบบการเรียกโมเดลเข้ากันได้กับโค้ดชุดนี้

---

# 24. ข้อจำกัดของระบบ

ระบบปัจจุบันมีข้อจำกัดบางอย่าง

### รูปภาพ

การวาดเส้นใหม่จะเป็นการแทนที่การเลือกเดิม

```text
Stroke 1
   ↓
Selection 1

Stroke 2
   ↓
Selection 2
```

ไม่ได้สะสมหลาย Stroke เพื่อเพิ่มหรือลบพื้นที่พร้อมกัน

### Webcam

การเลือกใช้ตำแหน่ง

```text
Keypoint
```

ดังนั้นควรคลิกบริเวณที่อยู่ภายในวัตถุที่ต้องการเลือก

---

# 25. ปัญหาที่พบบ่อย

## โมเดลโหลดไม่ได้

ตรวจสอบ Internet

และลองเปิดหน้าใหม่

ระบบต้องเข้าถึง

```text
jsdelivr.net
```

และ

```text
storage.googleapis.com
```

---

## กล้องเปิดไม่ได้

ตรวจสอบว่า Browser ได้รับ Permission หรือไม่

ไปที่

```text
Browser Settings
→ Site Settings
→ Camera
```

แล้วอนุญาตเว็บไซต์

---

## ภาพไม่แสดง

ตรวจสอบว่าไฟล์เป็น

```text
JPG
PNG
WEBP
```

และไฟล์ไม่เสียหาย

---

## Segment ไม่ตรงวัตถุ

ลองวาดเส้นตรงกลางวัตถุแทนการวาดที่ขอบ

ตัวอย่าง

```text
ไม่แนะนำ

    ┌───────┐
    │ ↑     │
    │ เส้น  │ ← ใกล้ขอบเกินไป
    └───────┘


แนะนำ

    ┌───────┐
    │       │
    │   ─── │ ← กลางวัตถุ
    │       │
    └───────┘
```

จากนั้นปรับ Threshold

---

# 26. การทดสอบระบบ

## Test 1 — โหลด Model

เปิดหน้าเว็บ

ตรวจสอบ Status

```text
โมเดลพร้อมใช้งาน
```

---

## Test 2 — Upload Image

อัปโหลด

```text
test.jpg
```

ตรวจสอบว่ารูปแสดง

---

## Test 3 — Interactive Segmentation

ลากเส้นบนวัตถุ

ตรวจสอบว่ามี Mask สีฟ้าปรากฏ

---

## Test 4 — Threshold

ปรับ

```text
0.30
0.50
0.70
```

ตรวจสอบการเปลี่ยนแปลงของ Mask

---

## Test 5 — Export

กด

```text
ส่งออก PNG
```

ตรวจสอบว่าไฟล์ถูกดาวน์โหลด

และพื้นหลังเป็น Transparent

---

## Test 6 — Webcam

เปิดกล้อง

คลิกวัตถุ

ตรวจสอบ Mask แบบ Real-time

จากนั้นกด

```text
ถ่ายภาพส่งออก PNG
```

---

# 27. สรุปการทำงาน

ระบบทั้งหมดสามารถสรุปได้ดังนี้

```text
                 MediaPipe
                     │
          Interactive Segmenter
                     │
          ┌──────────┴──────────┐
          │                     │
       รูปภาพ                  Webcam
          │                     │
     Upload Image          Start Camera
          │                     │
     วาด Stroke             Click Object
          │                     │
          └──────────┬──────────┘
                     ↓
              AI Segmentation
                     ↓
              Confidence Mask
                     ↓
                 Threshold
                     ↓
              Selected Object
                     ↓
               Transparent PNG
```

---

# 28. คำสั่งสำคัญในโปรเจกต์

### โหลด MediaPipe

```javascript
FilesetResolver.forVisionTasks()
```

### สร้าง Segmenter

```javascript
InteractiveSegmenterLegacy.createFromOptions()
```

### Segment รูปภาพ

```javascript
segmenter.segment(imgEl, roi)
```

### Segment Webcam

```javascript
segmenter.segment(webcam, {
    keypoint: activePoint
})
```

### อ่าน Confidence Mask

```javascript
const mask = result.confidenceMasks[0];

confidenceMask = mask.getAsFloat32Array();
```

### สร้าง Transparent PNG

```javascript
outData.data[idx+3] =
    selected ? 255 : 0;
```

---

# 29. ผลลัพธ์ที่ได้

เมื่อระบบทำงานสมบูรณ์ ผู้ใช้จะสามารถ

1. โหลด MediaPipe Model
2. อัปโหลดภาพ
3. วาดเส้นเลือกวัตถุ
4. ให้ AI สร้าง Segmentation Mask
5. ปรับ Threshold
6. ดูผลลัพธ์แบบ Overlay
7. Export PNG แบบพื้นหลังโปร่งใส
8. เปิด Webcam
9. คลิกเลือกวัตถุแบบ Real-time
10. Export ภาพจาก Webcam เป็น PNG

---

# 30. สรุปสั้น ๆ

**MediaPipe Interactive Segmenter** คือระบบ AI สำหรับแยกวัตถุออกจากภาพ โดยให้ผู้ใช้ช่วยระบุตำแหน่งของวัตถุ

มี 2 รูปแบบ:

```text
Image Mode
→ Upload → Draw → Segment → Export PNG

Webcam Mode
→ Camera → Click → Segment → Capture → Export PNG
```

ระบบใช้ MediaPipe Tasks Vision และประมวลผลภายใน Browser ทำให้ไม่จำเป็นต้องส่งรูปไปประมวลผลบน Server
