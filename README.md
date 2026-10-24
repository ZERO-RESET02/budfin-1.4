# BudFin on Netlify

public/index.html            the app (static)
netlify/functions/api.mjs    backend: email-code login, database, AI proxy (Netlify Functions + Netlify Blobs)

## Deploy (needs Git or the Netlify CLI; drag-and-drop does not run functions)
1. Push this folder to GitHub, then Netlify > Add new site > Import from Git. Or: `npm i -g netlify-cli && npm i && netlify deploy --prod`.
2. Site settings > Environment variables:
   - SESSION_SECRET   long random string (required)
   - GMAIL_USER and GMAIL_APP_PASSWORD   to email login codes through Gmail (2-step verification on, then an App Password from myaccount.google.com/apppasswords; use the 16-letter app password)
   - or RESEND_API_KEY and MAIL_FROM
   - LOCATION_KEY   long random string used to encrypt stored locations (recommended; falls back to SESSION_SECRET)
   - ADMIN_EMAILS   comma-separated admin emails
   - ALLOWED_DOMAIN defaults to iitr.ac.in
   - DEV_OTP=1        testing only: shows the code on screen. Never leave on in production.
3. Redeploy, open the site, sign in with an @iitr.ac.in email.

## What changed in this version
- Groups: main page lists only groups within your "group distance" (Settings, 100 m to 5 km, default 500 m); search shows every group. Hide (per user, restorable) and delete (owner/admin, removes chat and members). Groups are pinned to the creator's location when created.
- Distance: no more random blur. The phone uploads the live GPS fix (best accuracy of ~9 s), the server stores it AES-256-GCM encrypted and only ever returns a rounded distance. Position refreshes every ~45 s while "Free to chill?" is on; people not updating for 15 min drop off the map.
- Recruit: organisers choose what applicants must provide (resume link, skills, availability, experience, why, one custom question). People send a request; the organiser accepts or rejects. Direct joining of recruit teams is blocked on the server.
- Block list in Settings with Unblock; hidden groups with Unhide (both synced to the account).
- Log out. All seeded demo groups, ideas and lost & found items are purged on first sync, and nothing is seeded any more.
- Notifications: chat messages (groups and DMs), idea replies, join requests and decisions, new groups nearby, new members, lost & found matches. Bell inbox, in-app toasts, browser alerts while the app is open or backgrounded.
- Online / Offline status (Offline hides you and stops location sharing). Closing reminder: back button shows a "your location is visible" prompt with a switch-off button; closing the tab shows the browser's generic confirm; a background alert fires if the app stays hidden for a minute.

## Privacy notes
- Coordinates are never sent to any client or admin; only a rounded distance for people/groups inside the relevant radius.
- Stored location expires after 3 hours at most.
