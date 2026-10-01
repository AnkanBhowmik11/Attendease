# Firebase Setup Guide for AttendEase

## Why Firebase?
Firebase gives you cloud backup — your attendance data stays safe even if you clear the browser,
and you can access it from any phone/browser by logging in with the same anonymous UID.

---

## Step-by-Step: Create a FREE Firebase Project

### 1. Go to Firebase Console
- Open: https://console.firebase.google.com
- Sign in with your Google account

### 2. Create a New Project
- Click "Add project"
- Name it: AttendEase (or anything you like)
- Disable Google Analytics (not needed) → Click "Create project"

### 3. Register a Web App
- In your project dashboard, click the Web icon (</>)
- App nickname: AttendEase
- Do NOT enable Firebase Hosting
- Click "Register app"
- COPY the firebaseConfig object shown — you will need these values:
  - apiKey
  - authDomain
  - projectId
  - appId

### 4. Enable Authentication
- Left sidebar → Build → Authentication
- Click "Get started"
- Go to "Sign-in method" tab
- Enable "Anonymous" → Toggle ON → Save

### 5. Enable Firestore Database
- Left sidebar → Build → Firestore Database
- Click "Create database"
- Choose "Start in test mode" (good for 30 days, extend later)
- Select a region close to you (e.g., asia-south1 for India)
- Click "Done"

### 6. Set Firestore Security Rules (after testing)
Go to Firestore → Rules tab and paste:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /attendance/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

---

## Enter Config in the App

1. Open AttendEase → Profile tab (bottom nav, person icon)
2. Scroll to "Firebase Sync"
3. Paste your values:
   - API Key: paste apiKey value
   - Auth Domain: paste authDomain value
   - Project ID: paste projectId value
   - App ID: paste appId value
4. Tap "Connect Firebase"
5. You should see "Synced" status in green!

---

## Notes
- Your data is tied to an anonymous UID stored in your browser
- If you clear browser data, the UID changes → data in Firebase stays but you'd start fresh
- To avoid this: note your UID (shown after connecting) and contact Firebase to recover if needed
- Free Spark plan: 1 GB storage, 50K reads/day — more than enough for attendance tracking

---

## Troubleshooting
- "Error: ..." → Double-check apiKey and projectId
- "Permission denied" → Make sure Anonymous auth is enabled AND Firestore rules allow it
- Works offline: all data saved locally first, synced to Firebase when connected