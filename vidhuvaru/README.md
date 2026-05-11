# Vidhuvaru — Family Finance Setup

A mobile-first family finance tracker. Data syncs across all family devices via your personal OneDrive.

This guide gets you from these files → a real URL like `https://vidhuvaru-fathik.vercel.app` that you can share with your family. **Total time: ~10 minutes.** No coding required.

---

## What you need

- A computer (just for setup — afterwards everyone uses phones)
- Your Microsoft account (the OneDrive that will hold the data, e.g. `fathik@outlook.com`)
- A free [Vercel](https://vercel.com) account (you can sign up with GitHub or email)
- A free [Azure](https://portal.azure.com) account (comes with your Microsoft account — nothing to pay)

> **Don't want to set up Azure right now?** You don't have to. After deploying to Vercel, open the URL on your phone and tap **"Use without cloud sync"** — the app works fully on that one phone. You can connect Microsoft later from **More → Cloud sync**. Skip to Step 1 below; ignore steps 2-3.

---

## Step 1 — Deploy to Vercel (2 min)

You need the URL first, because Azure asks for it in Step 2.

1. Go to **https://vercel.com/new**
2. Click **"Continue with email"** (or GitHub) and create the account
3. Drag the **whole `vidhuvaru` folder** onto the upload area (Vercel calls this "Deploy folder")
4. Project name: `vidhuvaru` (or whatever you want — this becomes part of the URL)
5. Click **Deploy**
6. After ~30 seconds you'll get a URL like `https://vidhuvaru-fathik-xxxx.vercel.app`
7. **Copy that URL** — you'll paste it in the next step

> Don't try to open the URL yet — it'll fail until you finish Step 3.

---

## Step 2 — Register the app in Azure (3 min)

This gives the app permission to read/write your OneDrive.

1. Go to **https://portal.azure.com** and sign in with `fathik@outlook.com`
2. In the search bar at top, type **"App registrations"** and click it
3. Click **"+ New registration"**
4. Fill in:
   - **Name**: `Vidhuvaru Family Finance`
   - **Supported account types**: select **"Personal Microsoft accounts only"**
   - **Redirect URI**:
     - Platform: **Single-page application (SPA)**
     - URL: paste the Vercel URL you copied (e.g. `https://vidhuvaru-fathik-xxxx.vercel.app`)
5. Click **Register**
6. On the page that loads, copy the **"Application (client) ID"** (long string with dashes — looks like `a1b2c3d4-...-...-...-xxxxxxxxxxxx`)

> Tip: also add `http://localhost:8080` as another redirect URI under **Authentication → Add a platform → Single-page application** if you ever want to test the app locally.

---

## Step 3 — Paste the Client ID into the app (1 min)

1. Open `index.html` in any text editor (Notepad, TextEdit, VS Code — anything)
2. Press Ctrl+F (or Cmd+F) and search for: **`YOUR_AZURE_CLIENT_ID_HERE`**
3. Replace it with the Application (client) ID you copied
   - Keep the quotes around it: `const AZURE_CLIENT_ID = 'a1b2c3d4-...';`
4. Save the file

---

## Step 4 — Redeploy (1 min)

You changed the file, so push the update:

1. Go back to **https://vercel.com** → your project
2. Click **"Deployments"** in the top nav
3. Click **"..." (three dots)** next to the most recent deployment → **"Redeploy"**
4. OR: re-drag the `vidhuvaru` folder onto the dashboard

Wait ~30 seconds. Done.

---

## Step 5 — Test it on your phone (1 min)

1. On your phone, open the Vercel URL in **Safari (iPhone)** or **Chrome (Android)**
2. Tap **"Sign in with Microsoft"**
3. Sign in with `fathik@outlook.com`
4. After redirect, you should see the **family setup screen** — add your 6 family members

**Add to home screen:**
- **iPhone (Safari)**: Tap the Share button (square with arrow up) → **"Add to Home Screen"**
- **Android (Chrome)**: Tap the **⋮ menu** → **"Add to Home screen"** or **"Install app"**

The app icon will appear on the home screen and launches like a real native app.

---

## Step 6 — Share with family

Each family member, on their phone:

1. Open the Vercel URL (you can text it to them or show them once)
2. Sign in with the **same Microsoft account** (`fathik@outlook.com`)
3. Add to home screen (as above)
4. On the family picker, they tap their own name and enter their 4-digit PIN

They'll see all the family data because everyone is reading from the same OneDrive file.

> The single Microsoft account is shared because it owns the OneDrive file. If you don't want family to know the Microsoft password, sign each phone in yourself once and they'll stay signed in.

---

## How it works

- Your data lives in **OneDrive › Apps/Vidhuvaru › vidhuvaru-data.json**
- After every change, the app waits 3 seconds then quietly pushes a snapshot to OneDrive
- When someone opens the app, it pulls the latest from OneDrive in the background
- **Last save wins** — if two people edit at the exact same second, one overwrites the other (rare in practice)
- You can open `vidhuvaru-data.json` directly in OneDrive web or any text editor — it's plain JSON

---

## Troubleshooting

**"AADSTS50011: The redirect URI specified in the request does not match..."**
The Vercel URL in Azure → App Registration → Authentication doesn't match exactly. Copy your Vercel URL again, exactly as it appears (no trailing slash, including `https://`), and add it to Azure.

**Sign in works but says "Sync error"**
You may have forgotten to grant `Files.ReadWrite` permission, or your Azure app's account type is wrong. Go to Azure → your app → Authentication → make sure "Personal Microsoft accounts only" or "Personal + work/school" is selected.

**Avatar dot is red after editing**
Open the **More** tab → look at the **Cloud sync** card for the error message. Usually it's a transient network hiccup — tap **Sync now** to retry.

**Want to reset everything**
- Head goes to **More → Reset everything** (this clears the device's local copy)
- Also delete `vidhuvaru-data.json` from OneDrive web at `Apps/Vidhuvaru/`

---

## Files in this folder

- `index.html` — the whole app (open in any browser)
- `manifest.webmanifest` — tells phones it's a PWA (Add to Home Screen)
- `icon-192.png`, `icon-512.png`, `apple-touch-icon.png` — app icons
- `README.md` — this file

---

*Built for a 6-person family across Vidhuvaru (main house) and the Island house. Currency: MVR.*
