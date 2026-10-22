# Publishing the AKB Training Lounge

The site is a static folder. No build step, no server, no database — just HTML,
one stylesheet, one script, and three logo files. That makes it portable to
essentially any host.

**It is fully self-contained.** Every internal link is relative and every logo
is a local file, so the folder works from any root. The only external calls are
Google Fonts (Cormorant + Jost) and four plain links out to the restaurant's
main site.

---

## Option 1 — Kortix Apps (preferred)

**Currently blocked.** Apps is not enabled for this project:

```
✗  Apps is not enabled for this project. Enable it in Settings → Feature flags.
```

A project manager has to turn it on in **Settings → Feature flags**. An agent
cannot enable it. Once it's on:

```bash
kortix apps deploy ./akb-brand --app akb-training-lounge
```

That packs the folder, uploads it, and prints a live URL. To put it on a real
domain:

```bash
kortix apps deploy ./akb-brand --app akb-training-lounge \
  --domain training.americanakitchenandbar.com
```

Useful afterwards:

```bash
kortix apps list                      # slugs, status, URLs
kortix apps show akb-training-lounge  # source, domains, last deploy
kortix apps logs akb-training-lounge  # deployment logs
```

A deploy is a snapshot — editing files afterwards changes nothing live until
you deploy again.

---

## Option 2 — Netlify Drop (works right now, no account needed)

Fastest route if you want it in front of the team today.

1. Go to <https://app.netlify.com/drop>
2. Drag the `akb-brand` folder onto the page
3. You get a live URL in about ten seconds

Free, and you can claim it to a Netlify account later to set a custom domain.
This is also the easiest path to fixing the café form — see below.

---

## Option 3 — Any static host

The folder drops into any of these unchanged:

| Host | How |
|---|---|
| **Netlify** | Drag-and-drop, or connect the repo and set publish dir to `akb-brand` |
| **Vercel** | `vercel --prod` from inside `akb-brand/` |
| **Cloudflare Pages** | Connect the repo, build command empty, output dir `akb-brand` |
| **GitHub Pages** | Push `akb-brand/` contents to a `gh-pages` branch |
| **Existing WordPress host** | Upload via SFTP to a subdirectory or subdomain |

That last one is worth considering — the restaurant already has hosting for
americanakitchenandbar.com. A subdomain like `training.americanakitchenandbar.com`
would keep everything in one place.

---

## Still to do before a wide launch

### The Question Café form has no backend

`cafe.html` validates input and shows a confirmation, but nothing is stored.
Someone posting a question gets a success message and their question vanishes.
**Fix this before telling staff to use it.**

Easiest fix, if you host on Netlify — add two attributes to the form tag in
`cafe.html`:

```html
<form class="formwrap" id="askform" netlify name="cafe-questions">
```

Netlify then captures every submission and emails them to you. No other change
needed.

On another host, [Formspree](https://formspree.io) does the same thing:

```html
<form class="formwrap" id="askform" action="https://formspree.io/f/YOUR_ID" method="POST">
```

In both cases you'll also want to remove the `e.preventDefault()` line in the
submit handler at the bottom of `cafe.html`, so the form actually posts.

### The personas still have blanks

Send the GM, Accounting lead, and Project Manager to `/intake.html`. Fifteen
minutes between the three of them and the personas are finished. Until then the
AI answers like it would for any restaurant in New Jersey.

---

## Checking it locally

```bash
cd akb-brand && python3 -m http.server 4173
```

Then open <http://localhost:4173/>.

---

## What's in the folder

```
akb-brand/
├── index.html          Landing page
├── sessions.html       Three sessions + all materials
├── personas.html       Six AI personas, copy-to-clipboard
├── cafe.html           Question Café (form needs a backend)
├── intake.html         Collects the remaining figures
├── akb.css             Shared brand system — edit tokens here
├── assets/             Logos (white, black, icon)
└── sessions/
    ├── deck.css        Deck + handout styling
    ├── deck.js         Deck controller (arrows, F, P, swipe)
    ├── s1/             Deck, guide, LLM comparison, facilitator
    ├── s2/             Deck, blank template, facilitator
    └── s3/             Deck, card, worked example, leaderboard, facilitator
```

23 files, 344 KB.
