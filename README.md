# MiLife Sales Performance: putting it live

This folder is the complete app: one page (`index.html`), the logos, and a few setup files.
It saves data in **Google Firebase** (free plan is enough for a 50-person team) and asks everyone to sign in.

| File | What it is |
| --- | --- |
| `index.html` | The app |
| `config.js` | Your Firebase settings (you fill this in) |
| `seed-staff.json` | Your 40-person sales team, loaded on first launch |
| `firestore.rules` | Who may read and change the data |
| `firebase.json` | Settings for Firebase Hosting |
| `logo*.png`, `emblem*.png` | Brand images |

Open `index.html` before setup and it runs in **preview mode** with sample data (nothing is saved).

---

## 1. Create the Firebase project (about 10 minutes)

1. Go to <https://console.firebase.google.com> and sign in with the company Google account.
2. **Add project** → name it e.g. `milife-crm` → you can turn Google Analytics off → **Create**.
3. **Build → Firestore Database → Create database** → choose location **asia-south1 (Mumbai)** → start in **production mode**.
4. **Build → Authentication → Get started → Sign-in method → Email/Password → Enable → Save**.
5. Still in Authentication, open **Settings → User actions** and **untick "Enable create (sign-up)"**, so only people you add can have accounts.
6. **Project settings (gear icon) → General → Your apps → Web (`</>`)** → nickname `milife-crm` → **Register app**.
   Copy the values in `firebaseConfig` into **`config.js`** in this folder.

## 2. Add your team's logins

1. **Authentication → Users → Add user**: enter each person's email and a starting password.
2. Open **`firestore.rules`** and put the same emails in the list (lower case), for example:
   ```
   'gokul@milifemedicity.com',
   'sales.manager@milifemedicity.com'
   ```
3. **Firestore Database → Rules** → paste the whole contents of `firestore.rules` → **Publish**.

Anyone not on that list can sign in but will see no data. To remove someone, delete them in Authentication and take their email out of the rules.

## 3. Put it online (pick one)

**Option A: Firebase Hosting (recommended, free, gives you https)**
1. Install Node.js from <https://nodejs.org>, then in a terminal:
   ```
   npm install -g firebase-tools
   firebase login
   ```
2. In this folder run:
   ```
   firebase use --add        (pick your project)
   firebase deploy
   ```
   You get a link like `https://milife-crm.web.app`. This also publishes `firestore.rules`.

**Option B: your own website hosting (cPanel, Hostinger, etc.) or Netlify**
1. Upload everything in this folder to a sub-folder or subdomain, e.g. `crm.milifemedicity.com`.
2. In Firebase: **Authentication → Settings → Authorized domains → Add domain** → enter that domain.
3. Publish the rules as in step 2 above.

**Option C: Vercel**
1. Install Node.js from <https://nodejs.org>, then in a terminal inside this folder run:
   ```
   npx vercel login
   npx vercel --prod
   ```
   Accept the defaults (no build command, output directory `.`). You get a link like `https://milife-crm.vercel.app`.
2. In Firebase: **Authentication → Settings → Authorized domains → Add domain** → enter that `*.vercel.app` domain (and any custom domain you attach).
3. Publish the rules as in step 2 above (Vercel does not publish Firestore rules).

## 4. First launch

1. Open the link and sign in with one of the accounts you created.
2. A banner says the database is empty: press **Load sales team**. Your 40 people are added.
3. Go to **Settings** to check the reviewer (Gokul Sir), platforms and scoring, then start adding leads and campaigns.

## Good to know

- **Backups:** Firestore keeps your data in Google's cloud. For extra safety, use the app's export buttons now and then,
  or set up scheduled exports in Google Cloud (paid plan).
- **Forgot password:** the sign-in page has a "Forgot password?" link that emails a reset link.
- **Costs:** the free Spark plan covers 50,000 reads and 20,000 writes a day, which is far more than this team needs.
- **Updating the app later:** replace `index.html` and deploy/upload again. Your data is not affected.
