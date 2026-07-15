# Technical Architecture Documentation

This document describes the architectural decisions, database models, and AI engine design for the **BusPay Portal**.

---

## 1. Database Layer (`backend/config/db.js`)

To enable instant out-of-the-box local setup on any OS without PostgreSQL or MongoDB requirements, we created a lightweight, transaction-safe JSON database service.

### Atomic Write Protocol
To prevent data corruption during concurrent write calls:
1. The manager locks state read-writes.
2. Data updates are serialized and written first to a temporary file (`db.json.tmp`).
3. Node's atomic `fs.renameSync()` replaces `db.json` with the temporary file. This ensures that a crash during writing never leaves the primary JSON file corrupted.

---

## 2. API Security Architecture
Authentication is implemented via JSON Web Tokens (JWT) signed using SHA-256. Role-based routing is restricted by Express middleware checking matching user roles:
- `/api/backups/*` & `/api/students/meta/teachers` ➡ **Admin Only**
- `/api/payments/collect` & `/api/ai/ocr-scan` ➡ **Teacher & Admin**
- `/api/payments/dues/parent` ➡ **Parent Only**
- `/api/drivers/profile/dashboard` ➡ **Driver Only**

---

## 3. AI Features Design

### A. AI Payment Predictor (`getPaymentPredictions`)
Predicts payment failures by calculating a Risk Score (0-100%) using the following statistics:
```
Risk = Base Risk (10%) 
     + Consecutively Unpaid Months (35% for 1 month, 65% for 2+ months)
     + Average Delay Days past Due Date (Delay Days * 2.5, max 20%)
```

### B. Smart Reminder Scheduler (`getSmartReminders`)
Calculates parent behavioral patterns:
- Checks past payment timestamps.
- Finds the statistical mode for day of the week (e.g. Saturday) and time of day (e.g., Evening 6:30 PM).
- Returns recommendations to the admin panel with confidence scores.

### C. Chatbot Parser (`handleChatbotMessage`)
Natural language keyword parsing matching query intents:
- Inquiries about outstanding totals ➡ Queries `paymentService.parentDues()` and summarizes outstanding amounts.
- Inquiries about bus schedule details ➡ Fetches the student's pickup point, bus plate number, and driver's name.

### D. OCR Scanner Heuristics (`scanReceiptSlip`)
Decodes slips by scanning:
- Filename substring matches for student names.
- Digit extraction matching standard fee values (₹2000, ₹2500, ₹3000).
- Text matching calendar months.
- Populates collection forms automatically.
