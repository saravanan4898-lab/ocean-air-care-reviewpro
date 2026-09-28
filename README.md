# Ocean Air Care · ReviewPro

A premium, **mobile-first review assistant PWA** for Ocean Air Care.
Customers scan a QR code, pick the service they received, rate their experience, tap-to-copy a professionally written review suggestion, and jump to the Google review page to submit.

> Every customer can proceed to Google regardless of rating.

---

## Tech stack

| Layer | Tech |
| --- | --- |
| Frontend | **React 18 + Vite** |
| Styling | Tailwind CSS (custom brand tokens) |
| Motion | Framer Motion |
| Icons | lucide-react |
| Backend | **Firebase** — Auth + Firestore |
| PWA | `vite-plugin-pwa` |
| QR | `qrcode` |
| CSV | `papaparse` |
| Hosting | **GitHub Pages** |

## Modules

The project is organised module-by-module with dedicated Git commits:

1. **Project scaffold** — Vite + Tailwind + PostCSS + PWA config
2. **Data layer** — Firestore/localStorage adapter, seed data (540+ templates)
3. **Auth** — Firebase Auth wrapper with a local dev-mode fallback
4. **Shared UI** — AppShell, TopBar, Page, RatingStars, Toaster
5. **Customer flow** — Landing → Service select → Rating → Review library → Thank you
6. **Admin flow** — Login → Overview → Reviews CRUD → Categories → QR generator
7. **PWA + deployment** — manifest, service worker, GitHub Pages workflow

## Getting started

```bash
cd frontend
yarn install
yarn start   # http://localhost:3000
```

## Firebase setup

The app runs in **local dev mode** (localStorage) if Firebase credentials are missing.
To enable Firebase (recommended for production):

1. Create a Firebase project at https://console.firebase.google.com
2. **Enable Email/Password sign-in** — Authentication → Sign-in method
3. **Create your Firestore database** — start in production mode
4. **Register a Web app** — copy the config
5. Fill `frontend/.env`:
   ```env
   VITE_FIREBASE_API_KEY=...
   VITE_FIREBASE_AUTH_DOMAIN=...
   VITE_FIREBASE_PROJECT_ID=...
   VITE_FIREBASE_STORAGE_BUCKET=...
   VITE_FIREBASE_MESSAGING_SENDER_ID=...
   VITE_FIREBASE_APP_ID=...
   ```
6. **Deploy security rules**: `firebase deploy --only firestore:rules`
7. **Create your admin user** via Firebase Authentication console.

## Google Reviews link

Set `VITE_GOOGLE_PLACE_ID` in `frontend/.env` with your Ocean Air Care Google Place ID.
Find yours at https://developers.google.com/maps/documentation/places/web-service/place-id.

## Admin

- URL: `/admin/login`
- **Local dev credentials** (only used when Firebase is not configured):
  - Email: `admin@oceanaircare.local`
  - Password: `OAC-Admin-2026!`
- **Production**: Use the Firebase user you create in the console.

## Deployment (Firebase Hosting)

This project is configured to deploy to Firebase Hosting at site `ocean-air-care-ff757-1bb98`.

### One-time setup

```bash
# 1. Install Firebase CLI (once, globally)
npm install -g firebase-tools

# 2. Sign in
firebase login
```

### Every deploy

```bash
# From the repo root
cd frontend && yarn install && yarn build && cd ..

# Deploy the built frontend + Firestore rules
firebase deploy --only "hosting:ocean-air-care-ff757-1bb98,firestore:rules"
```

Your live site will be available at:

- `https://ocean-air-care-ff757-1bb98.web.app`
- `https://ocean-air-care-ff757-1bb98.firebaseapp.com`

### Alternative: GitHub Pages (manual static deploy)

The app builds as a fully static site (React SPA + Firebase/Gemini called from the browser).
The build uses **relative asset paths** and **hash-based routing** (`#/...`), so it works on
any GitHub Pages URL — project site or user site — with zero server configuration.

```bash
# 1. Clone your repo and install
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>/frontend
yarn install

# 2. Configure environment (copy values from your existing setup)
cp .env.example .env     # fill Firebase config + Gemini key + Place ID

# 3. Run locally
yarn dev                 # http://localhost:3000

# 4. Build static site
yarn build               # outputs frontend/dist/

# 5. Publish dist/ to the gh-pages branch
yarn deploy              # uses the 'gh-pages' package; needs your git/GitHub login
```

Then in GitHub: **Repo → Settings → Pages → Source: "Deploy from a branch" →
branch `gh-pages`, folder `/ (root)` → Save.** Your site goes live at
`https://<your-username>.github.io/<repo-name>/` within a few minutes.

**After the first deploy, complete these one-time steps:**

1. **Firebase authorized domains** — Firebase Console → Authentication → Settings →
   Authorized domains → add `<your-username>.github.io`. Without this, admin login fails.
2. **Gemini API key restriction** — Google Cloud Console → APIs & Services → Credentials →
   your Gemini key → Application restrictions → HTTP referrers → add
   `https://<your-username>.github.io/*`. (Also rotate the key if it was ever shared publicly.)
3. **SEO URLs** — update the canonical/OG/JSON-LD URLs in `frontend/index.html` and the
   URLs in `frontend/public/robots.txt` + `frontend/public/sitemap.xml` to your new
   GitHub Pages URL, then `yarn build && yarn deploy` again.
4. **Search Console** — add the new GitHub Pages URL as a new property and re-verify.
5. **Regenerate QR codes** — old QR codes point to the previous hosting domain; generate
   fresh ones from Admin → QR generator after migration.

Note: `.env` files are git-ignored and never pushed to GitHub — keep your real keys only
in your local `frontend/.env`. The built `dist/` bundle necessarily contains the Firebase
web config (public by design); protect the Gemini key with the referrer restriction above.

## Compliance

Review templates encourage **honest** feedback and are written to comply with Google's review policies. They are only *suggestions* — customers may edit or replace the text before submitting to Google.

## Business details

- Business: **Ocean Air Care**
- Theme: Blue & White
- Phone / WhatsApp: **9941103715**
- Language: English

---

## License

MIT © Ocean Air Care
