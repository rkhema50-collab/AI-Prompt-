# 🤖 AI Learning Web App — Full Setup Guide

A complete web app with user registration, login, welcome onboarding, and a 30-day AI learning dashboard — powered by **Firebase Auth + Firestore**.

---

## 📁 File Structure

```
ai-learn-app/
├── register.html     ← New user registration page
├── login.html        ← Returning user sign in
├── welcome.html      ← Onboarding screen after registration
├── index.html        ← Main 30-day dashboard
└── README.md         ← This file
```

## 🔄 User Flow

```
register.html  →  welcome.html  →  index.html (dashboard)
     ↑                                    ↑
login.html  ────────────────────────────→ ┘
```

---

## 🔥 Step 1 — Create a Firebase Project (Free)

1. Go to **[firebase.google.com](https://firebase.google.com)**
2. Click **"Get started"** → **"Create a project"**
3. Name it: `ai-learning-app`
4. Disable Google Analytics (not needed) → **Create project**
5. Wait ~30 seconds for it to set up

---

## 🔑 Step 2 — Get Your Firebase Config

1. In Firebase Console, click **"</> Web"** (Web app icon)
2. Register app with nickname: `ai-learning-web`
3. Copy the `firebaseConfig` object that appears — looks like this:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "ai-learning-app.firebaseapp.com",
  projectId: "ai-learning-app",
  storageBucket: "ai-learning-app.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef"
};
```

---

## ✏️ Step 3 — Paste Config Into All 4 Files

Open each file and **replace the placeholder config** with your real one.

Search for this in each file:
```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  ...
```

Replace with your real config in these files:
- ✅ `register.html`
- ✅ `login.html`
- ✅ `welcome.html`
- ✅ `index.html`

---

## 🔐 Step 4 — Enable Email/Password Authentication

1. In Firebase Console → **Authentication** (left sidebar)
2. Click **"Get started"**
3. Click **"Email/Password"**
4. Toggle **Enable** → **Save**

---

## 🗄️ Step 5 — Set Up Firestore Database

1. In Firebase Console → **Firestore Database**
2. Click **"Create database"**
3. Select **"Start in test mode"** (allows read/write for 30 days)
4. Choose your region (pick closest to your users) → **Done**

### Update Security Rules (after testing)

Go to Firestore → **Rules** tab → replace with:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Users can only read/write their own data
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

Click **Publish**.

---

## 🗂️ Firestore Data Structure

Each registered user gets a document at `users/{uid}`:

```json
{
  "name": "Sofia K.",
  "email": "sofia@example.com",
  "createdAt": "2026-06-19T...",
  "currentDay": 1,
  "completedDays": [1, 2, 3],
  "completedTasks": {
    "d0": [0, 1, 2],
    "d1": [0]
  },
  "streak": 3,
  "lastActiveAt": "2026-06-19T..."
}
```

---

## 🚀 Step 6 — Deploy to GitHub Pages

### 6a. Create GitHub repository

1. Go to [github.com](https://github.com) → **New repository**
2. Name: `ai-learning-app`
3. Set to **Public** → **Create repository**

### 6b. Upload all files

1. Click **"uploading an existing file"**
2. Drag all files: `register.html`, `login.html`, `welcome.html`, `index.html`, `README.md`
3. Click **"Commit changes"**

### 6c. Enable GitHub Pages

1. Go to repo **Settings → Pages**
2. Source: **Deploy from a branch**
3. Branch: **main** → Folder: **/ (root)**
4. Click **Save**

Your app is live at:
```
https://YOUR-USERNAME.github.io/ai-learning-app
```

---

## 🔧 Step 7 — Fix Firebase Authorized Domains

Firebase blocks sign-ins from unknown domains by default.

1. Firebase Console → **Authentication → Settings → Authorized domains**
2. Click **"Add domain"**
3. Add: `YOUR-USERNAME.github.io`
4. Click **Add**

✅ Registration and login will now work on GitHub Pages.

---

## 🌐 Embed in Google Looker Studio

1. Go to [lookerstudio.google.com](https://lookerstudio.google.com)
2. New Report → **Insert → URL Embed**
3. Paste: `https://YOUR-USERNAME.github.io/ai-learning-app/index.html`
4. Resize to fill the canvas → Share the report

> **Note:** For Looker Studio, users should register directly at your GitHub Pages URL first, then the embedded dashboard will recognize their session.

---

## 🆓 Firebase Free Tier Limits

| Feature | Free limit |
|---------|-----------|
| Authentication | 50,000 sign-ins/month |
| Firestore reads | 50,000/day |
| Firestore writes | 20,000/day |
| Firestore storage | 1 GB |
| Hosting | 10 GB/month |

More than enough for a learning app with hundreds of users.

---

## 🔮 Future Features You Can Add

- **Google Sign-In** (one more line of Firebase config)
- **Email verification** after registration
- **Password reset** flow (`reset.html`)
- **Admin dashboard** to see all users' progress in Looker Studio
- **Daily reminder emails** via Firebase Cloud Functions
- **Course completion certificates** generated as PDFs

---

*Built with Claude AI + Firebase + GitHub Pages*
