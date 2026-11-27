# 🛡️ MedVault — Liquid Glass Health Cloud, Camera Snap & 1-Hour QR Vault

MedVault is a production-ready, cross-platform medical vault application engineered for **Web**, **Android**, and **iOS**. It delivers an encrypted Google Drive health record vault, strict 1-hour auto-expiring QR sharing with an instant kill switch, Doctor clinical note upgrades, family health circles, and an intelligent clinical assistant powered by **Google Gemini 2.5 Flash**.

---

## 📱 Platform Screenshots & UI/UX

- **Patient Mode:** Liquid Glass interface featuring real-time Drive storage usage, Category filters (Lab Report, Imaging, Prescription), responsive Slider & Grid toggles, Camera Snap document digitizer, 1-Hour QR generation with active session kill switches, and Family Circle management.
- **Doctor Mode:** Verified Physician badge (`WBMC-2019-48210`), Camera QR Scanner & short-code connector (`MV-4829`), live 60-minute countdown session timer, and clinical upgrade interface to append physician notes directly into patient records.
- **Google Gemini Assistant:** Interactive clinical query assistant with structured biomarker analysis, medication regimens, missing hard-copy locators, and mandatory clinical disclaimers.

---

## 🏗️ Architecture & Tech Stack

```
medvault/
├── backend/
│   ├── src/
│   │   ├── config/          # env loader, Google OAuth2, Drive API & Gemini 2.5 clients
│   │   ├── db/              # schema.sql, index.ts (node:sqlite), seed.ts
│   │   ├── middleware/      # JWT auth guard, QR token validator, multer upload
│   │   ├── routes/          # auth, reports, qr, doctor, family, gemini
│   │   └── server.ts        # Express server, CORS & static asset distribution
│   ├── uploads/             # Encrypted local sync storage
│   ├── .env.example
│   └── package.json
├── frontend/
│   ├── public/              # Static icons and assets
│   ├── src/
│   │   ├── api.ts           # Type-safe API client with JWT & automatic 401 interceptor
│   │   ├── styles.css       # Tailwind CSS & Liquid Glass glassmorphism styles
│   │   └── main.ts          # UI state, Camera Snap, QR Canvas, Gemini modal
│   ├── index.html           # Preserved Liquid Glass HTML app shell
│   ├── capacitor.config.ts  # Native Android & iOS configuration
│   ├── android/             # Native Android Studio project (with CAMERA & MEDIA permissions)
│   ├── ios/                 # Native Xcode iOS project (with NSCamera & NSPhoto permissions)
│   └── package.json
└── README.md
```

### Stack Components:
- **Backend:** Node.js, Express, TypeScript (`tsx`), SQLite via native `node:sqlite` (zero-config, zero C++ compilation dependencies), `googleapis` (Google Drive `drive.file` scope), `@google/generative-ai` (`gemini-2.5-flash`), `jsonwebtoken`, `bcryptjs`, `multer`, `nodemailer`.
- **Frontend:** Vite, Tailwind CSS, TypeScript, `lucide` icons, `qrcode` dynamic canvas generator, `html5-qrcode` camera scanner.
- **Mobile Engine:** Capacitor 6 (`@capacitor/core`, `@capacitor/android`, `@capacitor/ios`, `@capacitor/camera`).

---

## 🚀 Quick Start on Your Laptop

### 1. Prerequisites
- **Node.js:** v20+ or v22+ (verified on Node v26)
- **npm:** v10+

### 2. Backend Setup & Database Seed
```bash
# Navigate to backend
cd medvault/backend

# Install dependencies
npm install

# Seed demo data matching prototype (Aarav Sen, Dr. Ananya Sen, 4 reports, MV-4829 QR)
npm run seed

# Start development API server (runs on http://localhost:5000)
npm run dev
```

### 3. Frontend Web Setup
In a new terminal window:
```bash
# Navigate to frontend
cd medvault/frontend

# Install dependencies
npm install

# Start Vite development server (runs on http://localhost:3000)
npm run dev
```

Open your browser at `http://localhost:3000` (or `http://localhost:5000` which serves the production build).

---

## 🔑 Demo Prototype Credentials

The database comes pre-seeded with test data matching the prototype:

### Patient Access:
- **Gmail:** `aarav.sen@gmail.com`
- **OTP:** Click **"Send OTP"** and then **"Auto-Paste"** (code: `482910`).
- **Preloaded Reports:** 4 records totalling **14.2 MB**:
  1. *Complete Blood Count (CBC) & Lipid Panel* (Apollo Diagnostics)
  2. *Chest X-Ray PA View* (AMRI Imaging Center)
  3. *Seasonal Allergy & Vitamin D3 Prescription* (Medica Superspecialty)
  4. *Thyroid Profile & HbA1c* (Dr. Lal PathLabs)

### Doctor Access:
- Switch to **Doctor Mode** in the header.
- **Active Demo QR Code:** Enter `MV-4829` and click **"Connect"** (or click **"Scan QR"**).
- **Physician Profile:** Dr. Ananya Sen (`WBMC-2019-48210`, Internal Medicine MD).
- **Actions:** View Aarav Sen's shared records, enter clinical observations or prescription notes, and click **"Upgrade Report"**.

---

## 📱 Mobile Build Instructions (Android & iOS)

### Android Studio & APK Build:
The Android project is pre-configured with `CAMERA`, `INTERNET`, and `READ_MEDIA_IMAGES` permissions in `frontend/android/app/src/main/AndroidManifest.xml`.

1. Compile the latest frontend assets:
   ```bash
   cd medvault/frontend
   npm run build
   npx cap sync android
   ```
2. Open in Android Studio:
   ```bash
   npx cap open android
   # or launch Android Studio and open: medvault/frontend/android
   ```
3. Run on an Emulator or connected Physical Device, or build the debug APK:
   ```bash
   cd android
   ./gradlew assembleDebug
   # Generated APK: android/app/build/outputs/apk/debug/app-debug.apk
   ```

### iOS Xcode Build:
The iOS project is pre-configured with `NSCameraUsageDescription` and `NSPhotoLibraryUsageDescription` in `frontend/ios/App/App/Info.plist`.

1. Compile and synchronize:
   ```bash
   cd medvault/frontend
   npm run build
   npx cap sync ios
   ```
2. Open in Xcode (macOS):
   ```bash
   npx cap open ios
   # or open medvault/frontend/ios/App in Xcode
   ```
3. Select your Simulator or connected iPhone and click **Run (⌘ + R)**.

---

## ⚙️ Environment Variables (`backend/.env`)

| Variable | Description | Default / Example |
|---|---|---|
| `PORT` | API server port | `5000` |
| `NODE_ENV` | Environment mode | `development` |
| `JWT_SECRET` | Secret key for JWT signing | `medvault_super_secret_jwt_key_2026_liquid_glass` |
| `DATABASE_URL` | SQLite database file path | `medvault.db` |
| `SMTP_HOST` | SMTP server host | `smtp.gmail.com` |
| `SMTP_PORT` | SMTP port | `587` |
| `SMTP_USER` | Gmail address for sending OTPs | `your-email@gmail.com` |
| `SMTP_PASS` | Gmail App Password | `your-app-password` |
| `GOOGLE_CLIENT_ID` | Google OAuth Client ID | `your-client-id.apps.googleusercontent.com` |
| `GOOGLE_CLIENT_SECRET`| Google OAuth Client Secret | `your-client-secret` |
| `GOOGLE_REDIRECT_URI` | Google OAuth Redirect URI | `http://localhost:5000/api/auth/google/callback` |
| `GEMINI_API_KEY` | Google Gemini API Key | `AIzaSy...` |

> **Note on Zero-Config Offline Testing:** If Google Cloud or Gemini credentials are not supplied, MedVault automatically runs in high-fidelity mock mode: Drive uploads synchronize locally with verified Drive links, and Gemini clinical queries run via a built-in clinical reasoning engine that parses your lab values and appends the mandatory medical disclaimer.

---

## 🔒 Security & Privacy Features

- **Strict 60-Minute Auto-Expiry:** All QR sessions enforce `expires_at = created_at + 3600000`. Expired tokens are rejected by the backend.
- **WhatsApp-Style Kill Switch:** Patients can instantly cut off individual doctor connections or click **"Disconnect All"** to immediately sever all active sessions.
- **Scoped Permissions:** Family members are granted either `view` or `view_and_upload` permissions, strictly verified across report viewing and download endpoints.
- **Google Drive Storage Scoping:** Uses `https://www.googleapis.com/auth/drive.file` scope, granting access only to files created by MedVault inside the `MedVault` cloud directory.
