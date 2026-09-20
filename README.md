# dlbirthdoula.com

Static site for Dana Lenahan | Birth Doula. No build step, no framework, no dependencies —
two HTML files with inline CSS/JS. Hosting is free on Netlify; the only cost is the domain.

## Files

- `index.html` — the whole site (hero, about, services, facts, testimonials, contact form)
- `thanks.html` — where the contact form lands after a successful submission
- `images/` — optional; see below

## Deploy

1. Create a new GitHub repo (e.g. `dlbirthdoula`) and push these files to the root.
2. In Netlify: **Add new site → Import an existing project → GitHub**, pick the repo.
3. Build command: leave **empty**. Publish directory: `.` (the repo root). Deploy.
4. **Domain settings → Add custom domain** → `dlbirthdoula.com`. Point the domain's
   nameservers at Netlify (or add the A/CNAME records Netlify shows). HTTPS is automatic
   and free once DNS resolves.

Every push to `main` redeploys automatically.

## Contact form → Dana's inbox

The form uses Netlify Forms, which is free up to 100 submissions/month. It's already wired
up (`data-netlify="true"`, a hidden `form-name` field, and a honeypot for spam).

After the first deploy:

1. Netlify dashboard → **Forms** → you should see a form named `contact`.
2. **Forms → Settings → Form notifications → Add notification → Email notification**.
3. Enter Dana's email address. Every submission emails her and is also stored in the dashboard.

Optionally set the reply-to so Dana can just hit reply: in the notification settings, Netlify
lets you pick the `email` field as the reply-to address.

If you ever outgrow 100 submissions/month, swap the form `action` and attributes for a
Formspree or Getform endpoint — the field names won't need to change.

## Adding photos

Dana's portrait is **embedded directly in `index.html`** as a base64 data URI, so the page is
fully self-contained and renders even when opened as a bare file. A full-resolution copy also
lives at `images/dana.jpg`.

If you'd rather load it as a normal file (better caching, ~155KB smaller HTML), find the
`<img id="portrait" ...>` tag and replace the giant `src="data:image/jpeg;base64,..."` value
with `src="images/dana.jpg"`. Nothing else changes.

The portrait sits inside an arch shape, with a second arch in the sea-green gradient offset
behind it.

The hero is a gradient by default and looks finished that way. If you want a photo behind it,
drop in `images/hero.jpg` — a wide, calm water or landscape shot, ~2400px wide and under 400KB.
It appears automatically on the next deploy, no code changes needed. Compress first
(squoosh.app) so the page stays fast.

## Editing copy

Everything is plain text in `index.html`. Search for the section comments (`<!-- SERVICES -->`,
`<!-- TESTIMONIALS -->`, etc.) to find what you want to change.

To add a testimonial, copy an existing `<figure>` block and change the text and `<figcaption>`.

## Notes on the design

- Palette: deep tidal green, sea glass, wet sand, shell white, clay — water and earth.
- Type: Fraunces (display) over Jost (body), loaded from Google Fonts.
- Services are grouped **Before / During / After** because that's the real shape of the work.
- Motion is limited to a slow wave drift and gentle scroll reveals; both respect
  `prefers-reduced-motion`.

## Social preview image (OG image)

`images/og.png` (1200×630) is what Facebook, iMessage, LinkedIn, and X show when
someone shares a link. It is generated from `og-template.html`, not hand-designed,
so it can be regenerated whenever the photo or tagline changes:

```bash
python3 -m http.server 8901          # from the repo root
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" \
  --headless --disable-gpu --hide-scrollbars --force-device-scale-factor=1 \
  --window-size=1200,630 --virtual-time-budget=6000 \
  --screenshot="$PWD/images/og.png" "http://localhost:8901/og-template.html"
```

Edit `og-template.html` first if the wording or crop needs to change
(`object-position` on the photo controls the crop).

After deploying a new version, caches on Facebook and LinkedIn hold the old image —
re-scrape at https://developers.facebook.com/tools/debug/ to force a refresh.
