# Sage passports — hosting guide

Each passport is a self-contained HTML file. There is no build step or dependency.

| Passport | File | Who it is for |
| --- | --- | --- |
| Luke's work experience passport | [LukeSagePassport.html](LukeSagePassport.html) | A five-day work experience visit |
| Lucas's onboarding passport | [LucasSagePassport.html](LucasSagePassport.html) | An Apprentice Enablement Engineer's first six months |

Lucas's passport sits alongside his First Month Workbook and [Confluence onboarding hub](https://confluence.sage.com/spaces/SFLOC/pages/916586929/Lucas). Confluence holds the detailed training tracker, learning resources and weekly log. The passport focuses on practice, introductions, evidence and growing independence.

## Host it on GitHub Pages

1. Add each HTML file to the repo root. Keep the named files so both passports have their own URL.
2. **Settings → Pages → Build and deployment**: source = *Deploy from a branch*,
   branch = `main`, folder = `/ (root)`. Save.
3. After Pages publishes the branch, open:
   - `https://adelesmith-sage.github.io/work-experience-passports/LukeSagePassport.html`
   - `https://adelesmith-sage.github.io/work-experience-passports/LucasSagePassport.html`

## Luke's password gate

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

The password in Luke's file only selects a local save slot. It does not protect the page. To reuse that format for another student, copy the file, update the content, and change the password and save slot.

Lucas's passport has no password gate. It saves under its own browser storage key and opens directly, so he can jump straight to the current phase. Its introductory meeting notes are saved only in that browser unless he exports them.

## How saving works

- Answers are stored in the browser's `localStorage` (Luke per save slot; Lucas under a separate key).
- They persist across closing the tab and restarting the machine.
- **Export / Import** buttons let the user download a backup and reload it on another device. Luke's controls appear after unlock; Lucas's appear immediately.
- **Print → Save as PDF** produces a clean, filled-in copy. Backup controls are hidden in print, as is Luke's lock screen.

## Honest limitations (please read)

- **The HTML is public when hosted on GitHub Pages.** Luke's password is visible in the source and is not security. Do not put confidential content in either HTML file.
- **Local answers are not account-protected.** Do not type customer data, ticket contents, passwords, keys or other personal information into either passport or its exported JSON file. Keep evidence in an approved internal system.
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
