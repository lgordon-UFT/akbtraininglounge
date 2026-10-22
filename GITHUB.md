# Publishing with GitHub Pages

GitHub Pages hosts this site free, on a real URL, with no build step. Two ways
in — pick based on whether you'd rather click or type.

> **Note:** this sandbox has no GitHub access — no `gh` CLI and no credentials,
> and the git remote points at Kortix's server rather than GitHub. So the repo
> has to be created from your own account. Everything below is prepared and
> ready; you're doing the parts that need your login.

---

## Route A — Upload through the browser (no tools needed, ~5 minutes)

1. **Create the repo**
   Go to <https://github.com/new>
   - Name: `akb-training-lounge`
   - **Public** — required for Pages on a free plan. If the account has GitHub
     Pro or the restaurant has an Organization plan, Private works too.
   - Don't tick "Add a README" — the upload includes everything.
   - Click **Create repository**

2. **Upload the files**
   On the empty repo page, click **uploading an existing file**.
   Unzip the package first, then drag **the contents** of the folder in — not
   the folder itself. `index.html` must sit at the top level of the repo, not
   inside a subfolder.
   Commit straight to `main`.

3. **Turn on Pages**
   **Settings → Pages → Build and deployment → Source: GitHub Actions**
   (Not "Deploy from a branch" — the included workflow handles it.)

4. **Wait about a minute**
   The **Actions** tab shows the deploy running. When it goes green your site
   is at:
   ```
   https://<your-username>.github.io/akb-training-lounge/
   ```

That's it. Every future change you commit redeploys automatically.

---

## Route B — From a terminal

If you have git set up locally:

```bash
# unzip the package, then from inside that folder:
git init -b main
git add .
git commit -m "AKB Training Lounge"
git remote add origin https://github.com/<your-username>/akb-training-lounge.git
git push -u origin main
```

Then do step 3 above — **Settings → Pages → Source: GitHub Actions**.

---

## A custom domain

A URL like `training.americanakitchenandbar.com` reads far better to staff than
a `github.io` address, and it's free.

1. In the repo: **Settings → Pages → Custom domain**, enter the subdomain, Save.
2. With whoever manages the restaurant's DNS, add a CNAME record:
   ```
   training   CNAME   <your-username>.github.io
   ```
3. Back in Settings → Pages, tick **Enforce HTTPS** once the certificate issues
   (usually under an hour).

GitHub will add a `CNAME` file to the repo — leave it there.

---

## What's already prepared

| File | Why |
|---|---|
| `.github/workflows/pages.yml` | Deploys on every push to `main`. Also runnable by hand from the Actions tab. |
| `.nojekyll` | Stops GitHub running Jekyll over the files. Without it, anything in a folder starting with `_` gets silently dropped. |

Both are in the package. Don't delete them.

---

## If it doesn't work

| Symptom | Cause |
|---|---|
| 404 at the Pages URL | `index.html` isn't at the repo root — you uploaded the folder instead of its contents. |
| Site loads, no styling | Same cause. `akb.css` is being looked for one level up. |
| Actions tab shows a red X | Open the run and read the error. Usually Pages source is still set to "Deploy from a branch." |
| Nothing in Actions at all | Source isn't set to GitHub Actions yet — step 3. |
| Logos missing | The `assets/` folder didn't upload. Browser uploads sometimes skip nested folders — use Route B, or upload `assets/` separately. |

---

## Before you share the link with staff

**The Question Café form doesn't store anything.** It validates and shows a
confirmation, then the question disappears. That's worse than having no form.

GitHub Pages is static, so it can't receive form posts — you need an external
form service. [Formspree](https://formspree.io) is free for low volume. Sign
up, create a form, then in `cafe.html`:

```html
<form class="formwrap" id="askform" action="https://formspree.io/f/YOUR_ID" method="POST">
```

And in the script at the bottom of that file, remove the `e.preventDefault()`
line so the form actually submits.

Until that's done, either skip the Café in the launch announcement or tell
people to bring questions in person.

**The personas still have blanks.** Send the GM, Accounting lead, and Project
Manager to `/intake.html` — fifteen minutes between them finishes the job.
