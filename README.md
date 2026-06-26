# Sage Work Experience Passport — hosting guide

A single self-contained HTML file. Open it, enter a password, and each student gets
their own private copy that saves as they type. No build step, no dependencies.

## Host it on GitHub Pages

1. Create a repo (e.g. `work-experience-passports`) and add the HTML file.
   - If you want the URL to be the repo root, rename the file to `index.html`.
   - Otherwise keep `LukeSagePassport.html` and link to it directly.
2. **Settings → Pages → Build and deployment**: source = *Deploy from a branch*,
   branch = `main`, folder = `/ (root)`. Save.
3. Wait ~1 minute. Your site appears at:
   - `https://<your-username>.github.io/<repo>/` (if `index.html`), or
   - `https://<your-username>.github.io/<repo>/LukeSagePassport.html`

## Set the passwords

Open the file and find the `PROFILES` block near the bottom (inside the last
`<script>`). Each line is `"password" : "save-slot"`:

```js
const PROFILES = {
  "luke2026": "luke",
  "demo":     "demo"
};
```

- Give each student a different password **and** a different slot — that keeps their
  answers separate.
- Passwords are matched case-insensitively and trimmed of spaces.
- Delete the `demo` line if you don't want it.

To reuse this for the next student, copy the file, update the schedule/name content,
and change the password.

## How saving works

- Answers are stored in the browser's `localStorage`, per save-slot.
- They persist across closing the tab and restarting the machine.
- **Export / Import** buttons (bottom-right, after unlock) let a student download a
  backup file and reload it — handy for moving between devices or keeping a copy.
- **Print → Save as PDF** produces a clean, filled-in keepsake (the lock screen and
  controls are hidden in print).

## Honest limitations (please read)

- **This is obscurity, not security.** The password lives in the page source, and a
  GitHub Pages site is publicly readable even from a private repo. Anyone determined
  can read the passwords and the page. Don't store anything sensitive.
- **Data is per-browser/per-device.** A student filling it in on a work PC won't see
  those answers on their phone. Clearing browser data wipes it (use Export first).
- It won't save inside an in-editor preview that blocks storage — test on the real
  hosted URL or by opening the file directly in a browser.

## If you outgrow this

For genuine per-user privacy and cross-device sync you need a small backend. Lightweight
options that keep the same single-page front end:

- **Supabase** or **Firebase** — free tier, real auth + a database, a few lines of JS.
- A **serverless function + KV store** (Cloudflare Workers KV, Vercel + Upstash, or an
  AWS Lambda in front of DynamoDB/S3) — fits well if you'd rather stay on AWS.

The save/restore code already serialises everything to one JSON object per user, so
swapping `localStorage` for a backend `fetch` is a small change when you're ready.
