[README.md](https://github.com/user-attachments/files/27584016/README.md)
# Vidhuvaru — Family Finance Setup

A mobile-first family finance tracker. Data syncs across all family devices via your personal Google Drive.

This guide takes ~10 minutes. No coding required.

---

## What you need

- A computer for the setup (afterwards everyone uses phones)
- A Google account (e.g. a Gmail)
- A free [Google Cloud Console](https://console.cloud.google.com) project (no payment needed)
- The app already deployed at `vidhuvaru.vercel.app` ✅

> **Don't want to set up Google Cloud right now?** Skip everything below. Open `vidhuvaru.vercel.app` on your phone and tap **"Use without cloud sync"** — the app works fully on that one phone. Connect Google later from inside the app.

---

## Step 1 — Create a Google Cloud project (1 min)

1. Go to **https://console.cloud.google.com**
2. Sign in with your Google account
3. At the very top, click the **project dropdown** (next to "Google Cloud" logo)
4. Click **"NEW PROJECT"** (top right of the popup)
5. Project name: `Vidhuvaru` → click **Create**
6. Wait ~10 seconds, then click the project dropdown again and **select Vidhuvaru**

---

## Step 2 — Enable Google Drive API (1 min)

1. In the search bar at the top of the Google Cloud console, type **"Google Drive API"** → click the result
2. Click the blue **"Enable"** button
3. Wait ~5 seconds. Done.

---

## Step 3 — Set up the OAuth consent screen (3 min)

This is the longest step. Just fill in what's asked.

1. In the left sidebar (or search bar) go to **"OAuth consent screen"**
2. Click **"Get Started"** (or "+ Create" if shown)
3. **App information:**
   - App name: `Vidhuvaru`
   - User support email: your Gmail
   - Click **Next**
4. **Audience:** Choose **"External"** → click Next
5. **Contact information:** your Gmail → Next
6. **Finish:** agree to the policy → Create

You'll land on the OAuth Overview page.

### Now add your family as test users

1. In the left sidebar click **"Audience"** (under "OAuth consent screen")
2. Scroll down to **"Test users"** → click **"+ Add users"**
3. Add the Gmail address of every family member who will use the app (up to 100)
   - If you'll all share one account (simplest), just add that one
4. Click **Save**

---

## Step 4 — Create the OAuth Client ID (1 min)

1. In the left sidebar click **"Clients"** (or **"Credentials" → "OAuth 2.0 Client IDs"**)
2. Click **"+ Create Client"** at the top
3. **Application type:** select **"Web application"**
4. **Name:** `Vidhuvaru Web`
5. **Authorized JavaScript origins:** click **+ Add URI** and paste:
   ```
   https://vidhuvaru.vercel.app
   ```
   (No trailing slash)
6. **Authorized redirect URIs:** click **+ Add URI** and paste the same:
   ```
   https://vidhuvaru.vercel.app
   ```
7. Click **Create**
8. A popup shows your **Client ID** — copy it. It looks like:
   ```
   123456789012-abcdefghijklmnop.apps.googleusercontent.com
   ```

---

## Step 5 — Paste Client ID into the app (1 min)

1. Open your GitHub repo's index.html for editing:
   **https://github.com/faathik-hue/Vidhuvaru/blob/main/vidhuvaru/index.html**
2. Click the **pencil icon (✏️)** at the top right of the file
3. Press **Cmd+F** (Mac) or Ctrl+F and search for: `YOUR_GOOGLE_CLIENT_ID_HERE`
4. Replace it with the Client ID you copied (keep the quotes!):
   ```js
   const GOOGLE_CLIENT_ID = '123456789012-abc...apps.googleusercontent.com';
   ```
5. Scroll to bottom → green **"Commit changes"** button → confirm

---

## Step 6 — Test it (1 min)

Vercel auto-redeploys when GitHub changes. Wait ~30 seconds.

1. Open **vidhuvaru.vercel.app** on your phone (or refresh if already open)
2. Tap **"Sign in with Google"**
3. Sign in with a Gmail you added as a test user
4. You'll see Google's warning **"Google hasn't verified this app"** → tap **"Advanced"** → **"Go to Vidhuvaru (unsafe)"** — this only shows for test apps; it's fine because you're a test user
5. Tap **"Allow"** to grant access to create files in your Drive
6. You're in! Set up your family members.

---

## Step 7 — Share with family

Each family member, on their phone:
1. Open `vidhuvaru.vercel.app`
2. Tap **"Sign in with Google"** — sign in with the **same Gmail** (or their own if you added them as test users in Step 3)
3. They land on the family picker → tap their name → enter their PIN
4. They see all family data, synced through Google Drive

**Add to home screen:**
- iPhone Safari: Share button → **Add to Home Screen**
- Android Chrome: ⋮ menu → **Install app**

---

## How it works

- Your data is one JSON file in Google Drive: `vidhuvaru-data.json`
- After every change, the app pushes a snapshot to Drive (3-second debounce)
- When someone opens the app, it pulls the latest from Drive
- **Last save wins** — if two people save simultaneously, one overwrites the other (rare in practice)
- You can open `vidhuvaru-data.json` directly at https://drive.google.com — it's plain JSON

---

## Troubleshooting

**"Error 400: redirect_uri_mismatch"**
Your app's URL doesn't match what's in Google Cloud. Open Google Cloud Console → APIs & Services → Credentials → your OAuth Client → make sure both "Authorized JavaScript origins" and "Authorized redirect URIs" include `https://vidhuvaru.vercel.app` exactly.

**"Access blocked: Vidhuvaru has not completed the Google verification process"**
The Gmail trying to sign in wasn't added as a test user. Go to Google Cloud Console → OAuth consent screen → Audience → Test users → add that Gmail.

**Avatar dot stays red after editing**
Open **More** tab → look at the **Cloud sync** card for the error message. Tap **Sync now** to retry.

**Token expired / "Sign in required"**
Google access tokens last 1 hour. Just tap Sign in with Google again. The app tries to refresh silently first.

**Want to start fresh**
- In the app: **More → Reset everything** (only Head can do this)
- Also delete `vidhuvaru-data.json` from Google Drive web

---

## Files in this folder

- `index.html` — the whole app
- `manifest.webmanifest` — PWA manifest (Add to Home Screen)
- `icon-192.png`, `icon-512.png`, `apple-touch-icon.png` — app icons
- `README.md` — this file

---

*Built for a 6-person family across Vidhuvaru (main house) and the Island house. Currency: MVR.*
