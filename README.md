# Chuchu's Pure Oat Bite – Business Records App

A phone-friendly app for recording sales, expenses, loans and customers in Ghana cedis (GH₵).
Partners sign in and see the same records instantly.



## Part A – Firebase (about 10 minutes, no coding)

1. Go to **https://console.firebase.google.com** and sign in with a Google account.
2. Click **Create a project** → name it `chuchus-oat-bite` → you can turn **Google Analytics off** → Create.
3. **Turn on email login**
   - Left menu: **Build → Authentication → Get started**
   - **Sign-in method** tab → **Email/Password** → Enable → Save.
4. **Create the two accounts** (nobody else can sign up through the app)
   - **Users** tab → **Add user** → your email + a strong password.
   - **Add user** again → your partner's email + a password for her.
   - If Authentication **Settings** shows an option to stop new users signing up, switch it off. The app has no sign-up page anyway, and the rules in step 6 block everyone except your two emails.
5. **Create the database**
   - Left menu: **Build → Firestore Database → Create database**
   - Choose a location. **The location can't be changed later.** A European location such as `europe-west1` is usually a good choice for Ghana.
   - Choose **Start in production mode** → Create.
6. **Lock the database to just the two of you**
   - Firestore → **Rules** tab → delete everything there and paste this, using **your two real emails** (lowercase):

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /{document=**} {
         allow read, write: if request.auth != null
           && request.auth.token.email in ['you@gmail.com', 'partner@gmail.com'];
       }
     }
   }
   ```
   - Click **Publish**.
7. **Get the app's settings**
   - Click the **gear icon → Project settings** → scroll to **Your apps** → click the **`</>`** (Web) icon.
   - Nickname `oat-bite` → **Register app** (leave "Firebase Hosting" unticked).
   - You'll see a block that starts with `const firebaseConfig = {`. Keep this page open.
8. **Paste the settings into the app**
   - Open `index.html` in any text editor (Notepad, TextEdit, or on GitHub in Part B).
   - Near the top of the `<script>` section, find `const FIREBASE_CONFIG = {` and replace the `PASTE…` values with the values from step 7.
   - Optional: just below it, fill in `PEOPLE` so records show names instead of emails, e.g.
     `const PEOPLE = { "you@gmail.com": "Chuckles", "partner@gmail.com": "Chuchu" };`

> **Is it safe to have the Firebase settings in a public file?** Yes. The `apiKey` only identifies your project; it is not a password. The rules from step 6 are the real lock: only the two signed-in emails can read or write anything. For extra hardening you can restrict the key to your GitHub Pages address in Google Cloud Console → APIs & Services → Credentials.

---

## Part B – Put the app online with GitHub Pages

1. Sign in at **https://github.com** (create a free account if needed).
2. Click **+ → New repository** → name it `oat-bite` → **Public** → Create.
   (Free GitHub Pages needs a public repository. Only the app's code is public. Your business records live in Firebase behind the login, not on GitHub.)
3. On the new repository page click **uploading an existing file** → drag in `index.html` (with your Firebase settings pasted in) and this `README.md` → **Commit changes**.
4. **Settings → Pages** → under "Build and deployment" choose **Deploy from a branch** → Branch **main**, folder **/ (root)** → **Save**.
5. Wait a minute or two and refresh. The page will show your link, something like
   `https://YOUR-USERNAME.github.io/oat-bite/`
6. **Allow that address in Firebase:** Firebase → Authentication → **Settings → Authorized domains → Add domain** → `YOUR-USERNAME.github.io`.

---

## Part C – On both phones

1. Open the link and sign in.
2. Add it to the home screen so it opens like an app:
   - **Android (Chrome):** ⋮ menu → **Add to Home screen**
   - **iPhone (Safari):** Share button → **Add to Home Screen**
3. Her July–August 2026 notebook records, the price list and the expense items list are **loaded automatically** the first time either of you signs in. This happens only once.

---

## Using the app day to day

| Tab | What it's for |
|---|---|
| **Home** | This month's profit, today's totals, who still owes money, deliveries to do, loan balance |
| **Sales** | One entry per order: several products, payment (paid / part paid / not paid), delivery date. Grouped by day with a "Total for the day", like the notebook |
| **Expenses** | One entry per shopping trip: several items, each with a category, plus where the money came from (sales cash, bank, MoMo, loan…) |
| **Money** | Loans taken, loan repayments, money sent to the bank. These are kept out of profit |
| **More** | Monthly summary, customers, price list, expense items list, CSV backup |

- **Prices** fill in from the price list but can be changed on any sale (discounts, special orders).
- **Changing prices later:** More → Price list → **Update prices**. Change the prices, then tap Save. New sales use the new prices, past sales keep their old price, and each product shows what it was before ("was ₵110 · changed 10 Nov").
- **Expenses are picked from a dropdown** (oats, eggs, margarine, labels, transport…). Each item already has its category, and the form shows what was paid last time. If she buys something new, she chooses *Something new (type it)* once and it joins the list. Manage the list under More → Expense items.
- **Updating an old order:** tap it, change "Part paid" → "Paid in full", or tick "Delivered", then Save.
- **Backups:** More → Backup → download Sales / Expenses / Loans as spreadsheet files once a month.

## Updating the app later
On GitHub, open `index.html` → pencil icon (or upload the new file) → Commit. Your records are not affected, because they live in Firebase.
