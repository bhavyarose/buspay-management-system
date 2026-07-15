# BusPay - Smart School Bus Fee Management Portal

BusPay is a comprehensive web portal designed for school bus transportation fee collections, bus route monitoring, driver salary distributions, and parent-student invoice logging. Powered by specialized heuristics, it provides interactive AI predictions and chatbot experiences.

---

## 🚀 Key Features

### 1. Multi-Role Portals
- **Admin (School Management):** Register teachers/parents/drivers/students, assign students to routes, set monthly fee schedules, generate audit security logs, disburse driver salaries, and trigger backups.
- **Teacher (Fee Collector):** Quick student roster lookup, receive payments, print receipt slips with dynamic QR codes, and upload payment slips using the AI OCR parser.
- **Parent / Student:** Month-by-month status logs (January ✔, February ✔, March Pending), fee invoice histories, bus route and driver contact cards, and real-time chatbot assistants.
- **Bus Driver:** Daily bus route student listings, stop locations, monthly salary disbursement histories, and annual earnings trackers.

### 2. Specialized AI Operations
- **AI Payment Predictor:** Analyzes average delay days and unpaid month history to predict risk ratings (Low/Medium/High) of students defaulting on next month's payment.
- **Smart Reminders:** Customizes reminder notification timings (Best Day + Best Time) optimized per parent based on their historical payment timestamps.
- **AI Chatbot:** Responsive floating NLP widget answering parent queries about dues, billing histories, and route driver contact info.
- **AI OCR slip Scanner:** Instantly parses uploaded payment receipts, extracts amounts, months, dates, and names, and auto-fills payment records.

---

## 🛠 Tech Stack

- **Frontend:** React, Tailwind CSS, Lucide Icons, Recharts (Analytics).
- **Backend:** Node.js, Express, Multer (File uploads), JWT + Bcryptjs.
- **Database:** Transactional, atomic JSON-based file database (`database/db.json`) supporting backups, index lookups, and audit trails.
- **Mock Fallback (Zero Setup):** If the Node.js server is offline, the React frontend dynamically redirects all actions to a browser local-storage database duplicate, ensuring 100% operation immediately in the browser under any condition!

---

## 📂 Project Structure

```
bus-fee-management/
│
├── frontend/             # React + Vite + Tailwind Client
│   ├── src/
│   │   ├── components/   # AIChatbot, OCRScanner, etc.
│   │   ├── layouts/      # Role-based Dashboards (Admin, Teacher, Parent, Driver)
│   │   ├── services/     # Dual-mode API Client (localStorage proxy fallback)
│   │   ├── App.jsx       # App shell, routing, auth context
│   │   └── main.jsx
│   ├── tailwind.config.js
│   ├── vite.config.js
│   └── package.json
│
├── backend/              # Node.js + Express API Server
│   ├── config/           # custom JSON db manager (db.js) & seedData.js
│   ├── controllers/      # auth, student, payment, driver, ai, backup controllers
│   ├── middleware/       # JWT auth role blockers & Multer file filter
│   ├── routes/           # Express endpoint configurations
│   ├── server.js         # Express main entry file
│   └── package.json
│
├── database/             # JSON local store & auto-backups folder
│   ├── db.json           # Active seeded database
│   └── backups/          # JSON history backups
│
└── README.md             # Guide documentation
```

---

## ⚙️ Quick Start Installation

### Pre-requisites
- **Node.js** (v16.0.0 or higher) and **npm** installed on your system.

### Step 1: Run the Backend API Server
1. Open a terminal in the `backend` folder:
   ```bash
   cd backend
   ```
2. Install dependencies:
   ```bash
   npm install
   ```
3. Start the Express server:
   ```bash
   npm run dev
   ```
   *The server will run on `http://localhost:5000` and automatically populate `database/db.json` with pre-seeded data.*

### Step 2: Run the React Frontend Client
1. Open a new terminal in the `frontend` folder:
   ```bash
   cd frontend
   ```
2. Install dependencies:
   ```bash
   npm install
   ```
3. Launch the development server:
   ```bash
   npm run dev
   ```
   *The frontend client will run on `http://localhost:3000`.*

---

## 🔑 Demo Login Credentials (Password for all: `admin123`)

- **Admin Account:** Username `admin`
- **Teacher Account:** Username `teacher1`
- **Parent Account:** Username `parent1`
- **Driver Account:** Username `driver1`
