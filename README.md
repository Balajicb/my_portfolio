# Balaji K — Portfolio Website

A minimal, single-page portfolio with a Node.js backend for the contact form.

```
portfolio/
├── index.html          ← the whole site (frontend)
├── assets/
│   └── resume.pdf       ← put your real resume here
├── server/               ← Node.js/Express backend (contact form email)
│   ├── server.js
│   ├── package.json
│   └── .env.example
└── README.md
```

## 1. Add your details

Open `index.html` and replace the placeholders:

- `https://linkedin.com/in/your-linkedin` → your real LinkedIn URL (appears twice)
- `https://github.com/your-github` → your GitHub URL
- `you@example.com` → your real email
- Drop your resume file into `assets/resume.pdf` (create the `assets` folder if it's not there yet)

## 2. Run the backend locally (for the contact form)

The contact form needs a small backend to actually send you an email.

```bash
cd server
npm install
cp .env.example .env
# then edit .env with your real email + app password
npm start
```

This starts the API at `http://localhost:4000`. The frontend already points to
`http://localhost:4000/api/contact` — that's fine for local testing.

**Gmail users:** you can't use your normal Gmail password here. Create an
"App Password" instead: https://myaccount.google.com/apppasswords
(requires 2-Step Verification to be turned on for your Google account).

## 3. Deploy the frontend (free)

Easiest options, both free:

**Option A — Vercel**
1. Create a free account at vercel.com
2. Install the CLI: `npm i -g vercel`
3. From the `portfolio/` folder, run: `vercel`
4. Follow the prompts — you'll get a free URL like `balaji-k.vercel.app`

**Option B — Netlify**
1. Create a free account at netlify.com
2. Drag and drop the `portfolio/` folder onto the Netlify dashboard ("Deploys" tab)
3. You'll get a free URL like `balaji-k.netlify.app`

Both let you later attach a custom domain for free (see step 5).

## 4. Deploy the backend (free)

The contact form's backend needs to live somewhere too — it won't work on
Vercel/Netlify by itself since those are static hosts.

**Option A — Render.com (recommended, simplest free tier)**
1. Push the `server/` folder to a GitHub repo
2. Create a free account at render.com → "New Web Service"
3. Connect your repo, set:
   - Build command: `npm install`
   - Start command: `npm start`
4. Add your `.env` values under Render's "Environment" tab (do NOT commit `.env` to GitHub)
5. Render gives you a URL like `https://your-app.onrender.com`

**Option B — Railway.app** — similar free-tier flow to Render.

Once deployed, open `index.html` and update this line near the bottom:

```js
const CONTACT_API_URL = 'http://localhost:4000/api/contact';
```

Change it to your deployed backend URL, e.g.:

```js
const CONTACT_API_URL = 'https://your-app.onrender.com/api/contact';
```

Then redeploy the frontend so it picks up the change.

## 5. Free domain options

- **Free subdomain (easiest):** Vercel/Netlify already give you a free
  `.vercel.app` / `.netlify.app` subdomain — good enough to put on a resume.
- **js.org (nice fit for a JS developer):** free `yourname.js.org` subdomains
  are available for JavaScript-related projects — see https://js.org
- **Freenom-style free TLDs** (`.tk`, `.ml`, etc.) exist but have become
  unreliable/less recommended recently — a free subdomain from Vercel/Netlify
  or js.org is generally the safer bet.
- If you ever want a "real" `.com`/`.dev` domain, they're usually $8–15/year
  from providers like Namecheap or Google Domains, and can be pointed at your
  free Vercel/Netlify hosting.

## Notes

- The contact form backend is intentionally minimal — no database, just
  sends an email via Nodemailer when someone submits the form.
- CORS is open (`app.use(cors())`) for simplicity. If you want to lock it
  down to only your domain later, pass an options object to `cors()`.
- Everything here is plain HTML/CSS/JS + Node/Express — no build step needed.
