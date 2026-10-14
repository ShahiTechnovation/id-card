# Claude Code Workshop Pass ("I'm attending" generator)

Lets attendees of the Claude Code Workshop (Thu 15 Oct, 1:30–4:30 PM, BIBA Indrapuri, Bhopal · https://luma.com/claude-3brg) make a 1080×1350 LinkedIn pass.
They upload a photo, enter name / role / 3 tags, pick a style (Cream / Terracotta / After dark), download a PNG and copy a ready-made caption.

- One static HTML file per pass (~29 KB). No build step and no backend. The Claude logo, the Clawd mascot and the Luma QR are embedded as data URIs.
- Photos never leave the user's device; everything is drawn on a `<canvas>`.
- Fonts load from Google Fonts: Anton, Bricolage Grotesque, Instrument Serif and JetBrains Mono.

## Files
```
index.html               ← attendee pass ("I'm attending"), the one served at flex.withclaude.in
host/index.html          ← "I'm hosting" pass
volunteer/index.html     ← "I'm volunteering" pass
tog-pass/, tog-pass.zip  ← archive of the 10–11 Oct Claude Impact Lab × Cerebras pass
```
`host/` and `volunteer/` are the attendee page with the headline, caption and download name swapped. If you change `index.html`, make the same change in both.

## Deploy
The live site runs on Vercel and was deployed from the CLI, not from GitHub, so pushing to this repo does not update it. From the repo root:
```bash
npx vercel --prod      # first run asks which project to link: pick the one that owns flex.withclaude.in
```
`host/` and `volunteer/` have their own `vercel.json`, so run the same command inside each folder if you want those live too.

Any static host also works (Cloudflare Pages upload, Netlify drop): upload the folder.

## Where to edit things (search inside index.html)
| What | Search for |
|---|---|
| LinkedIn caption text | `$('caption').value=` |
| Card styles / colours | `const THEMES=` (cream, terra, ink) |
| Date, time, venue | `block('THU` and `block('VENUE` |
| Top bar text | `CLAUDE COMMUNITY` and `BHOPAL · 15 OCT` |
| Bottom band text / Luma link | `CLAUDE CODE WORKSHOP · BHOPAL` and `REGISTER: LUMA.COM` |
| Page headline | `Got your spot?` |
| Images (logo, mascot, QR) | `const A={` (base64 data URIs: claude, clawd, qr) |
| Download filename | `Claude-Code-Workshop-` |

The QR is drawn at 4 px per module (31 modules × 4 = 124 px) so it stays sharp. If a new link gives a QR with a different module count, update the size on the `// QR` line.

## Share flow
1. Add the live link to the Luma approval message: "You're in. Make your pass and post it: <link>"
2. Send one Luma blast to everyone approved, and post the link in the Claude Community WhatsApp group with your own pass as an example.
3. The caption includes #ClaudeBhopal and asks people to tag Aniket Sahu and Claude. Reshare the best posts.

## Rules baked in
- Logos are used exactly as provided.
- The pass only says what the Luma page says: no credits, no prizes.
- QR → https://luma.com/claude-3brg (checked by scanning the rendered pass).
