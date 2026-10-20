# Lunch Picks

A small web app for the boys to pick their GOe school meals on the family iPad. Parents review the picks, then paste a prompt into Claude in Chrome to fill the goe.sg cart before paying.

- **Hosting:** GitHub Pages (free)
- **Data:** Firebase Firestore (free Spark plan), locked to one family login
- **Files:** `index.html` (the app), `icon-180.png` / `icon-512.png` (home-screen icon), `firestore.rules` (database security rules)

---

## Part 1 · Set up Firebase (about 15 minutes)

### 1. Create the project
1. Go to https://console.firebase.google.com and sign in with a Google account.
2. Click **Create a project**. Name it something like `lunch-picks`.
3. Google Analytics isn't needed. Turn it off and click **Create project**.

### 2. Create the database
1. In the left menu, open **Build → Firestore Database** and click **Create database**.
2. Location: choose **asia-southeast1 (Singapore)**.
3. Start in **production mode**. You'll replace the rules in step 5.

### 3. Turn on email sign-in and create the family account
1. Open **Build → Authentication** and click **Get started**.
2. Under **Sign-in method**, enable **Email/Password** (leave "Email link" off) and save.
3. Go to the **Users** tab and click **Add user**. Enter a family email (it doesn't need to be a real inbox you check) and a strong password.
4. Copy the long **User UID** shown in the list. You need it in step 5.

### 4. Register the web app and copy its config
1. Click the gear icon → **Project settings**.
2. Under **Your apps**, click the web icon `</>`. Name it `Lunch Picks`. Don't tick Firebase Hosting.
3. Firebase shows a `firebaseConfig = { ... }` block. Copy the six values.
4. Open `index.html`, find `const FIREBASE_CONFIG`, and paste each value in place of the `PASTE_…` text.

These config values are safe to put in public code. They identify the project; the rules in step 5 are what protect the data.

### 5. Lock the database to your family account
1. Open **Firestore Database → Rules**.
2. Replace everything with the contents of `firestore.rules`.
3. Swap `PASTE_FAMILY_UID` for the User UID from step 3, then click **Publish**.

Now only someone signed in as the family account can read or change picks. Anyone else, even with the app's link, sees nothing.

---

## Part 2 · Put the app on GitHub Pages (about 10 minutes)

### 6. Create the repository
1. Sign in at https://github.com (create a free account if needed).
2. Click **+ → New repository**. Name it `lunch-picks`.
3. Set it to **Public**. GitHub Pages is free only for public repositories; private Pages needs a paid plan. Nothing private is in the code: no names, no passwords (the PIN is stored only as a scrambled hash), and the database is locked by step 5.
4. Click **Create repository**.

### 7. Upload the files
1. On the new repository page, click **uploading an existing file**.
2. Drag in `index.html`, `icon-180.png`, `icon-512.png`, `firestore.rules` and `README.md`.
3. Click **Commit changes**.

### 8. Turn on GitHub Pages
1. In the repository, open **Settings → Pages**.
2. Under **Build and deployment**, set Source to **Deploy from a branch**, branch **main**, folder **/ (root)**, and save.
3. After a minute or two the page shows your link, for example `https://YOUR-USERNAME.github.io/lunch-picks/`.

### 9. Allow sign-in from that address
1. Back in Firebase, open **Authentication → Settings → Authorized domains**.
2. Click **Add domain** and enter `YOUR-USERNAME.github.io` (no `https://`, no path).

---

## Part 3 · First use

### 10. Sign in and set the names
1. Open the GitHub Pages link and sign in with the family email and password.
2. Tap **🔒 Parents**, enter the PIN, then in **Review & order → Children** type the names **in this order**:
   - Child 1: **Max**
   - Child 2: **Liam**
   - Child 3: **Harvey**

   The order matters. The meals already paid for (20–26 Oct) are stored by position.

### 11. Add it to the iPad home screen
1. On the iPad, open the link in **Safari** and sign in once. It stays signed in.
2. Tap **Share → Add to Home Screen**. The boys get a Lunch Picks icon that opens full screen.

### 12. Use it on your laptop
Open the same link in Chrome and sign in once. When the boys have picked:
1. Tap **🔒 Parents → Review & order → Copy prompt**.
2. Open goe.sg in another tab, open the Claude side panel and paste the prompt.
3. Check the cart, pay, then tap **Mark week as ordered** in the app.

---

## Updating the app later

- **New menu or new paid orders:** ask Claude for an updated `index.html`. In GitHub, open the file, click the pencil icon (or **Add file → Upload files** to replace it), and commit. The site updates in about a minute. Your Firebase config lines stay the same, so copy them across if the new file still has `PASTE_…` placeholders.
- **Change the parent PIN:** on a Mac, run `echo -n NEWPIN | shasum -a 256` in Terminal and paste the result into `PARENT_PIN_HASH` in `index.html`.
- **The parent PIN keeps the boys out of the parent screens. It is not account security.** The family email and password are what protect the data, so keep them to the adults.

## Costs

The Firebase free plan allows 50,000 reads and 20,000 writes a day. Three children picking lunches uses a tiny fraction of that, so this stays free. GitHub Pages is free for public repositories.
