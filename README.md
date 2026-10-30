# My Accounts: setup (about 15 minutes, all free)

The app is these files: `index.html`, `firebase-config.js`, `sw.js`, `manifest.webmanifest`, two icons.
It works as is, on one device, with no setup. Steps 1 to 3 add the cloud database so all your devices share the same data.

## 1. Create your database (Firebase)
1. Go to https://console.firebase.google.com and sign in with a Google account. Click **Add project**, give it a name, and skip Google Analytics.
2. **Build > Authentication > Get started > Sign-in method > Email/Password > Enable > Save.**
3. **Build > Firestore Database > Create database.** Pick a location near you (for example `asia-southeast1`), choose **Production mode**.
4. In Firestore open the **Rules** tab, replace everything with the rules below, and click **Publish**:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{uid}/{document=**} {
      allow read, write: if request.auth != null && request.auth.uid == uid;
    }
  }
}
```
These rules mean each signed-in user can only reach their own data.

## 2. Connect the app to it
1. In the Firebase console click the gear icon > **Project settings > General**. Under **Your apps** click the web icon `</>`, register an app (no hosting needed), and copy the `firebaseConfig` values.
2. Open `firebase-config.js` in Notepad and paste each value over its `PASTE_...` placeholder. Save.

## 3. Put the app online (so every device opens the same link)
Pick one. All are free.
- **GitHub Pages (easiest):** create a free GitHub account, make a new public repository, click **Add file > Upload files**, drop in all the files, then **Settings > Pages > Deploy from branch > main > Save**. Your link will be `https://YOURNAME.github.io/REPO/`.
- **Cloudflare Pages or Netlify:** drag the whole folder onto their "deploy" page.

Your Firebase config is not a secret; the rules in step 1 are what protect your data. Your money data is never in the files you upload.

## 4. Use it on each device
1. Open the link in Chrome on the phone (Android 10 or higher) and on your computer.
2. Phone: Chrome menu **⋮ > Add to Home screen** (or **Install app**). It opens like a normal app and works offline.
3. Open **Settings > Cloud sync**, tap **Create account** with your email and a password (first device), and **Sign in** on every other device.
4. If you already entered data on a device before signing in, tap **Upload this device's data** once.

Optional: after you have created your account, in Firebase **Authentication > Settings > User actions**, turn off **Enable create (sign-up)** so nobody else can register.

## Daily use
- Works offline. Changes you make without internet are saved on the device and upload automatically when you are back online.
- The badge at the top shows **Synced**, **Syncing…**, or **Offline**.
- Balances are calculated from your transactions, so devices always agree on them.
- If you edit the very same transaction on two devices, the most recent edit wins.
- Download a backup now and then (Settings > Download backup).

## Updating the app later
Replace the files on your host. Installed devices pick up the new version the next time they are online.
