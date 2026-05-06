# ⚔ ROTK X Thai Translation Auto-Patcher

> **Romance of the Three Kingdoms X — โปรเจกต์แปลภาษาไทย โดย ThawidejC**

[(https://user-images.githubusercontent.com/Screenshot%202026-05-05%20221611.png](https://github.com/ThawidejC/ROTK-X-Thai-Translation-Tools/blob/main/Screenshot%202026-05-05%20221611.png))]

---

## 📖 เกี่ยวกับโปรเจกต์

**ROTK X Thai Translation Auto-Patcher** คือเครื่องมือสำหรับผู้เล่นเกม *Romance of the Three Kingdoms X* (三國志X) บน Steam ที่ต้องการเล่นเกมในภาษาไทย โดยดึงคำแปลล่าสุดจากแพลตฟอร์ม [ParaTranz](https://paratranz.cn/projects/18980) มาแพทช์ลงในไฟล์เกมโดยอัตโนมัติ ด้วยการคลิกเพียงครั้งเดียว ไม่ต้องมีความรู้ด้านเทคนิคใดๆ

คำแปลครอบคลุม ชื่อนายพล, เหตุการณ์, เนื้อเรื่อง, เมนู, และข้อความในเกมทั้งหมด

---

## ✨ ฟีเจอร์

| ฟีเจอร์ | รายละเอียด |
|---|---|
| 🚀 **Apply Patch** | โหลดคำแปลล่าสุดจาก ParaTranz แล้วแพทช์ไฟล์เกมครบทุกไฟล์อัตโนมัติ |
| 💾 **Backup** | สำรองไฟล์เกมต้นฉบับก่อนแพทช์ พร้อม timestamp |
| ♻ **Restore Backup** | กู้คืนไฟล์จาก backup ที่เคยสร้างไว้ผ่าน UI แบบ list |
| 🌐 **Restore Eng V.** | คืนค่าไฟล์ English version (msg/, FontB.s10, San10WPK.exe) |
| 📊 **ดู Progress** | ตรวจสอบความคืบหน้าการแปลจาก ParaTranz แบบ real-time |
| 📂 **เปิดโฟลเดอร์เกม** | เปิด File Explorer ไปที่โฟลเดอร์เกมโดยตรง |
| 🔍 **Auto-Detect** | สแกนหาเกม ROTK X ใน Steam library ทุก Drive อัตโนมัติ |
| 🔤 **Thai Encoding** | รองรับ ThMaping.txt สำหรับ encode ตัวอักษรไทยลงในไฟล์เกม |

---

## ⚙ การทำงานภายใน

### Apply Patch — 4 ขั้นตอนอัตโนมัติ

```
Step 0 — Auto-Restore
  ThaiName/San10WPK.exe  ──copy_into──▶  [game dir EXE]
  msg/*.s10  ──────────────copy──────────▶  [game]/msg/
  FontB.s10  ──────────────copy──────────▶  [game]/msg/
  (รับประกันว่าแพทช์บนไฟล์ English ที่สะอาดเสมอ)

Step 1 — โหลดไฟล์เกม
  อ่านไฟล์ .s10 ทั้งหมดจาก msg/ + EXE ไฟล์

Step 2 — ดาวน์โหลดคำแปลจาก ParaTranz
  ดึง strings ที่แปลแล้วทั้งหมดผ่าน API

Step 3 — Apply
  • S10 files  →  Pointer Rebuild Mode (ปรับ pointer table อัตโนมัติ)
  • EXE / อื่นๆ →  In-place Patch Mode
```

### รูปแบบ Key ใน ParaTranz

```
{filename}|0x{offset:08X}|{max_bytes}|{encoding}
เช่น: KOEI0001.s10|0x00001234|64|gbk
```

---

## 🚀 วิธีติดตั้งและใช้งาน

### ความต้องการของระบบ

- **OS:** Windows 10 / 11
- **เกม:** Romance of the Three Kingdoms X (Steam)


### วิธีใช้งานครั้งแรก

```
1. เปิด ROTK X Thai Translation Auto-Patcher  v1.0.exe
   → โปรแกรมจะสแกนหาเกม ROTK X อัตโนมัติ

2. กด  💾 Backup ไฟล์ต้นฉบับ
   → สำรองไฟล์เกมเดิมไว้ก่อน (แนะนำทำทุกครั้ง)

3. กด  🚀 Apply Patch (โหลด + แพทช์)
   → ระบบจะ Auto-Restore English files ก่อน
   → จากนั้นดาวน์โหลดคำแปลล่าสุดจาก ParaTranz
   → แพทช์ไฟล์เกมอัตโนมัติ

4. เปิดเกมแล้วเล่นได้เลย! 🎮
```

---

## 🔄 Flow การทำงานโดยละเอียด

```
User กด Apply Patch
        │
        ▼
┌─────────────────────────────────────────────┐
│ Step 0: Auto-Restore                        │
│  • copy msg/*.s10 จาก patcher dir → game   │
│  • copy FontB.s10 → game/msg/               │
│  • copy ThaiName/San10WPK.exe → game dir    │
│    (Thai EXE — มีชื่อนายพลภาษาไทย)          │
└─────────────────────────────────────────────┘
        │
        ▼
┌─────────────────────────────────────────────┐
│ Step 1: Load Game Files                     │
│  • อ่านไฟล์ .s10 ทั้งหมดใน msg/            │
│  • อ่าน EXE ไฟล์                            │
│  • ตรวจสอบ S10 Pointer Table                │
└─────────────────────────────────────────────┘
        │
        ▼
┌─────────────────────────────────────────────┐
│ Step 2: Fetch from ParaTranz                │
│  • GET /projects/{id}/strings               │
│  • รองรับ pagination (1000/page)             │
│  • กรองเฉพาะ strings ที่แปลแล้ว             │
└─────────────────────────────────────────────┘
        │
        ▼
┌─────────────────────────────────────────────┐
│ Step 3: Apply Patch                         │
│  • S10 → rebuild pointer table              │
│  • EXE → in-place patch                    │
│  • Thai encoding via ThMaping.txt           │
└─────────────────────────────────────────────┘
        │
        ▼
     🎮 เล่นเกมได้เลย!
```

---

## 🖥 ปุ่มและฟังก์ชันทั้งหมด

### Row 1 — ฟังก์ชันหลัก

| ปุ่ม | การทำงาน |
|---|---|
| 💾 **Backup ไฟล์ต้นฉบับ** | คัดลอก `.s10` ทั้งหมด + EXE ไปเก็บใน `backup_YYYYMMDD_HHMMSS/` |
| 🚀 **Apply Patch** | Auto-Restore → Fetch ParaTranz → Patch (ครบทุกขั้นตอนในคลิกเดียว) |
| ♻ **Restore Backup** | เปิด dialog เลือก backup แล้วกู้คืนไฟล์ |

### Row 2 — ฟังก์ชันเสริม

| ปุ่ม | การทำงาน |
|---|---|
| 📊 **ดู Progress คำแปล** | ดึงสถิติจาก ParaTranz API แสดง % ความคืบหน้า |
| 📂 **เปิดโฟลเดอร์เกม** | `explorer.exe` → game directory |
| 🌐 **Restore Eng V.** | คืนค่าไฟล์ English (msg/ + FontB.s10 + San10WPK.exe [Eng EXE]) |
| 🔗 **หน้าเว็บโปรเจกต์** | เปิด paratranz.cn/projects/18980 |

---


## 📊 ความคืบหน้าการแปล

ติดตามความคืบหน้าได้ที่ → **[paratranz.cn/projects/18980](https://paratranz.cn/projects/18980)**

หรือกดปุ่ม 📊 **ดู Progress คำแปล** ในตัวโปรแกรม

---

## 🤝 ร่วมแปล

หากต้องการร่วมแปลหรือแก้ไขคำแปล:

1. สมัครบัญชีที่ [paratranz.cn](https://paratranz.cn)
2. เข้าร่วมโปรเจกต์ → [ROTK X Thai Translation](https://paratranz.cn/projects/18980)
3. เลือก string ที่ต้องการแปลหรือแก้ไข
4. Submit คำแปล รอ review

---

## ⚠ ข้อควรระวัง

- **Backup แค่ครั้งแรกครั้งเดียว** — กด 💾 Backup ก่อน Apply Patch
- Tool นี้แก้ไขไฟล์ใน Steam directory โดยตรง Steam จะไม่ detect การเปลี่ยนแปลง
- หากต้องการคืนค่าเป็น English ให้กด 🌐 **Restore Eng V.** หรือ **Verify Integrity** ผ่าน Steam
- ไฟล์ใน `msg/` และ `ThaiName/` ในโฟลเดอร์ patcher ต้องเป็นต้นฉบับ **ไม่ได้แพทช์**

---

## 📜 License

โปรเจกต์นี้เป็น **Fan Translation Project** ไม่ใช่งานเชิงพาณิชย์  
ROTK X / 三國志X เป็นทรัพย์สินทางปัญญาของ © KOEI TECMO GAMES CO., LTD.

---

*ทำด้วยใจ ❤ เพื่อผู้เล่นชาวไทย — ThawidejC*
-----------------
# ⚔ ROTK X Thai Translation Auto-Patcher

> **Romance of the Three Kingdoms X — Thai Fan Translation Project by ThawidejC**

---

## 📖 About

**ROTK X Thai Translation Auto-Patcher** is a one-click tool for Steam players of *Romance of the Three Kingdoms X* (三國志X) who want to play the game in Thai. It automatically fetches the latest translations from [ParaTranz](https://paratranz.cn/projects/18980) and patches them into the game files — no technical knowledge required.

The translation covers general names, in-game events, story dialogue, menus, and all on-screen text.

---

## ✨ Features

| Feature | Description |
|---|---|
| 🚀 **Apply Patch** | Fetches the latest translations from ParaTranz and patches all game files in one click |
| 💾 **Backup** | Creates a timestamped backup of original game files before patching |
| ♻ **Restore Backup** | Restores game files from any previously created backup via a selection UI |
| 🌐 **Restore Eng V.** | Restores the English version files (msg/, FontB.s10, San10WPK.exe) |
| 📊 **Translation Progress** | Checks real-time translation progress from the ParaTranz API |
| 📂 **Open Game Folder** | Opens File Explorer directly at the game directory |
| 🔍 **Auto-Detect** | Automatically scans all drives to locate ROTK X in the Steam library |
| 🔤 **Thai Encoding** | Supports ThMaping.txt for encoding Thai Unicode characters into game-compatible bytes |

---

## ⚙ How It Works

### Apply Patch — 4 Automatic Steps

```
Step 0 — Auto-Restore (ensures patch is always applied on clean English files)
  ThaiName/San10WPK.exe  ──copy_into──▶  [game dir EXE]
  msg/*.s10  ─────────────────copy──────▶  [game]/msg/
  FontB.s10  ─────────────────copy──────▶  [game]/msg/

Step 1 — Load Game Files
  Read all .s10 files from msg/ + EXE file

Step 2 — Fetch Translations from ParaTranz
  Pull all translated strings via API

Step 3 — Apply
  • S10 files  →  Pointer Rebuild Mode (pointer table is rebuilt automatically)
  • EXE / others  →  In-place Patch Mode
```

## 🚀 Installation & Usage

### Requirements

- **OS:** Windows 10 / 11
- **Game:** Romance of the Three Kingdoms X (Steam)

### First-Time Usage

```
1. ROTK X Thai Translation Auto-Patcher  v1.0.exe
   → The program will auto-scan for ROTK X on all drives

2. Click  💾 Backup Original Files
   → Always back up before patching

3. Click  🚀 Apply Patch (Load + Patch)
   → Automatically restores English files first
   → Downloads the latest translations from ParaTranz
   → Patches all game files

4. Launch the game and enjoy! 🎮
```

---

## 🔄 Detailed Workflow

```
User clicks Apply Patch
        │
        ▼
┌─────────────────────────────────────────────────┐
│ Step 0: Auto-Restore                            │
│  • copy msg/*.s10 from patcher dir → game dir  │
│  • copy FontB.s10 → game/msg/                  │
│  • copy ThaiName/San10WPK.exe → game dir       │
│    (Thai EXE — generals have Thai names)        │
└─────────────────────────────────────────────────┘
        │
        ▼
┌─────────────────────────────────────────────────┐
│ Step 1: Load Game Files                         │
│  • Read all .s10 files from msg/               │
│  • Read EXE file                               │
│  • Detect S10 Pointer Tables                   │
└─────────────────────────────────────────────────┘
        │
        ▼
┌─────────────────────────────────────────────────┐
│ Step 2: Fetch from ParaTranz                    │
│  • GET /projects/{id}/strings                  │
│  • Supports pagination (1000 per page)          │
│  • Filters only translated strings             │
└─────────────────────────────────────────────────┘
        │
        ▼
┌─────────────────────────────────────────────────┐
│ Step 3: Apply Patch                             │
│  • S10 → rebuild pointer table                 │
│  • EXE → in-place patch                        │
│  • Thai encoding via ThMaping.txt              │
└─────────────────────────────────────────────────┘
        │
        ▼
     🎮 Ready to play!
```

---

## 🖥 All Buttons & Functions

### Row 1 — Core Functions

| Button | Function |
|---|---|
| 💾 **Backup Original Files** | Copies all `.s10` files + EXE into `backup_YYYYMMDD_HHMMSS/` |
| 🚀 **Apply Patch** | Auto-Restore → Fetch ParaTranz → Patch (full pipeline in one click) |
| ♻ **Restore Backup** | Opens a dialog to select and restore a previous backup |

### Row 2 — Utility Functions

| Button | Function |
|---|---|
| 📊 **Translation Progress** | Fetches stats from ParaTranz API and displays % completion |
| 📂 **Open Game Folder** | Runs `explorer.exe` → game directory |
| 🌐 **Restore Eng V.** | Restores English files (msg/ + FontB.s10 + San10WPK.exe [Eng EXE]) |
| 🔗 **Translation Project** | Opens paratranz.cn/projects/18980 |

---

## 🔧 Technical Details

### Thai Encoding

ROTK X originally uses GBK/Shift-JIS encoding, which does not support Thai characters. The tool uses **ThMaping.txt** to map Thai Unicode → custom Big5 code points that the game engine can render with the custom Thai font.


### Two EXE Types

| File | Location | Purpose |
|---|---|---|
| `San10WPK.exe` | Root patcher dir | English EXE → used by **Restore Eng V.** button |
| `San10WPK.exe` | `ThaiName/` subfolder | Thai EXE (Thai general names) → used by **Auto-Restore** before Apply Patch |

---

## 📊 Translation Progress

Track the project at → **[paratranz.cn/projects/18980](https://paratranz.cn/projects/18980)**

Or click the 📊 **Translation Progress** button inside the app.

---

## 🤝 Contributing to the Translation

Want to help translate or fix existing translations?

1. Create an account at [paratranz.cn](https://paratranz.cn)
2. Join the project → [ROTK X Thai Translation](https://paratranz.cn/projects/18980)
3. Select a string to translate or improve
4. Submit your translation and wait for review

---

## ⚠ Important Notes

- **Backup first one time** — Click 💾 Backup before Apply Patch One time
- This tool modifies files directly inside the Steam directory; Steam will not detect changes
- To revert to English, click 🌐 **Restore Eng V.** or use **Verify Integrity** via Steam
- Files inside `msg/` and `ThaiName/` in the patcher folder must be **original, un-patched** versions

---

## 📜 License

This is a **Fan Translation Project** — not a commercial release.  
ROTK X / 三國志X is the intellectual property of © KOEI TECMO GAMES CO., LTD.

---

*Made with ❤ for Thai players — ThawidejC*
