# TOG Attendee Pass ("I'm attending" generator)

Lets approved attendees of Claude Impact Lab × Cerebras Community (10–11 Oct, Global Skills Park, Bhopal) make a 1080×1350 LinkedIn pass.
They upload a photo, enter name / role / 3 tags, pick a style (Cream / Terracotta / After dark), download a PNG and copy a ready-made caption.

- Single static file: `index.html` (~64 KB). No build step and no backend. All logos, mascots and the Luma QR are embedded as data URIs.
- Photos never leave the user's device; everything is drawn on a `<canvas>`.
- Fonts load from Google Fonts: Anton, Bricolage Grotesque, Instrument Serif and JetBrains Mono.

## Files
```
tog-pass/
├── index.html          ← the whole site (deploy this)
├── README.md
└── worker/             ← only for the Cloudflare Workers option
    ├── worker.js       ← same page, served by a Worker
    └── wrangler.toml   ← name = "theoriginguild"
```

## Deploy (pick one)

### A. Cloudflare Pages (no install, about 1 minute)
1. dash.cloudflare.com → Workers & Pages → Create → Pages → **Upload assets**
2. Project name: `theoriginguild` → drag in the `tog-pass` folder → Deploy
3. URL: **https://theoriginguild.pages.dev**

### B. Cloudflare Workers (CLI)
```bash
cd tog-pass/worker
npx wrangler login
npx wrangler deploy
```
URL: `https://theoriginguild.<your-subdomain>.workers.dev`

Or use the dashboard: Create Worker → name it `theoriginguild` → Edit code → paste the contents of `worker.js` → Deploy.

### C. Vercel
```bash
cd tog-pass
npx vercel --prod      # project name: theoriginguild
```
URL: `https://theoriginguild.vercel.app` (if the name is free)

### D. Netlify
Go to app.netlify.com/drop and drag in the `tog-pass` folder. Then Site settings → change the site name to `theoriginguild`.

### E. GitHub Pages
Push `index.html` to a repo, for example `vishal4742/attendee-pass`. Then Settings → Pages → Deploy from branch `main`.

### Custom domain (optional)
On Cloudflare Pages: project → Custom domains → add e.g. `pass.theoriginguild.com`.

## Where to edit things (search inside index.html)
| What | Search for |
|---|---|
| LinkedIn caption text | `$('caption').value=` |
| Card styles / colours | `const THEMES=` (cream, terra, ink) |
| Dates, timings, "$100 Claude credits" | `day('DAY 1` and `$100 CLAUDE CREDITS` |
| Bottom band text / Luma link | `CLAUDE IMPACT LAB × CEREBRAS` and `REGISTER: LUMA.COM` |
| Page headline | `Got approved?` |
| Images (logos, mascots, QR) | `const A={` (base64 data URIs: tog, claude, cerebras, clawd, frog, qr) |
| Download filename | `TOG-ImpactLab-` |

If you edit `index.html` and also use the Worker option, regenerate `worker.js`:
```bash
node -e "const fs=require('fs');const h=fs.readFileSync('index.html','utf8');fs.writeFileSync('worker/worker.js','const HTML='+JSON.stringify(h)+';\nexport default{async fetch(){return new Response(HTML,{headers:{\"content-type\":\"text/html;charset=utf-8\",\"cache-control\":\"public,max-age=300\"}})}};\n')"
```

## Share flow
1. Add the live link to the Luma approval message: "You're in. Make your pass and post it: <link>"
2. Send one Luma blast to everyone already approved, and post the link in the WhatsApp group with your own pass as an example.
3. The caption includes #ClaudeBhopal and asks people to tag The Origin Guild. Reshare the best posts from the TOG page.

## Rules baked in
- Logos are used exactly as provided. The connector is "×". The word "partnership" is never used.
- QR → https://luma.com/claude-ozni (verified).
