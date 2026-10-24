# BudFin on Netlify

public/index.html            the app (static)
netlify/functions/api.mjs    backend: email-code login, database, AI proxy (Netlify Functions + Netlify Blobs)

## Deploy (needs Git or the Netlify CLI; drag-and-drop does not run functions)
1. Push this folder to GitHub, then Netlify > Add new site > Import from Git. Or: `npm i -g netlify-cli && npm i && netlify deploy --prod`.
2. Site settings > Environment variables:
   - SESSION_SECRET   long random string (required)
   - GMAIL_USER and GMAIL_APP_PASSWORD   to email login codes through Gmail (turn on 2-step verification, then create an App Password at myaccount.google.com/apppasswords; use the 16-letter app password, not your normal password)
   - or RESEND_API_KEY and MAIL_FROM   (Resend only emails other people after you verify a sending domain)
   - LOCATION_KEY   long random string used to encrypt stored locations (recommended; falls back to SESSION_SECRET)
   - ADMIN_EMAILS     comma-separated admin emails (can moderate and delete anything)
   - ALLOWED_DOMAIN   defaults to iitr.ac.in
   - ANTHROPIC_API_KEY (optional) for the AI buddy; AI_MODEL optional
   - DEV_OTP=1        testing only: shows the code on screen instead of emailing it. Never leave on in production.
3. Redeploy, open the site, sign in with an @iitr.ac.in email.

Local test: `netlify dev` (use a .env file with the same variables).


## Privacy notes
- The phone blurs the GPS position (random offset up to 75 m) before upload; exact coordinates never leave the device.
- The server stores only the blurred point, AES-256-GCM encrypted, for at most 3 hours, and never sends coordinates to any client or admin. Clients only receive a rounded distance for people inside both users' radius (100 m to 2 km, default 500 m).
