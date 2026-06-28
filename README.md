# QA Engineer Portfolio

A lightweight, dependency-free portfolio site built with plain HTML, CSS, and a
little vanilla JavaScript. Designed to be published for free via **GitHub Pages**.

## Structure

```text
portfolio-site/
├── index.html        # Page content, structure & inline JS (theme, QA mode, scroll, contact form)
├── 404.html          # Custom test-runner themed not-found page
├── css/
│   ├── style.css     # Base styling (edit the :root variables to re-theme)
│   └── qa-mode.css   # "QA mode" re-skin, scoped under <html data-mode="qa">
├── assets/           # Photo, résumé (PDF), certification/school logos, favicon
├── .nojekyll         # Tells GitHub Pages to serve files as-is
└── README.md
```

### QA mode

A **"Turn on QA mode"** button in the footer flips the whole page into a
test-runner inspired theme (indigo accent, monospace `expect(...)` cues, a
pass/fail terminal block, and "passing" pills). It sets `<html data-mode="qa">`,
which `qa-mode.css` keys off, and the choice is remembered via `localStorage`.
Click **"Turn off QA mode"** to return to the standard view. QA mode has its own
light and dark palettes, so the navbar 🌙 / ☀️ toggle works there too.

## Customizing

1. Open `index.html` and edit your details — name, location, email, job history,
   highlights, certifications, education, and social links.
2. Assets live in the `assets/` folder:
   - `patrick-star-superhero.png` — the About photo
   - `pvictoriano-resume.pdf` — the résumé for the download buttons
   - `microsoft_logo.jpg`, `linux_logo.jpg`, `school.jpg` — certification/school logos
3. Want different colors? Edit the variables under `:root` (dark theme) and
   `[data-theme="light"]` (light theme) at the top of `css/style.css`. The QA-mode
   palette lives in `css/qa-mode.css`.

### Enable the contact form

The site has a working contact form, but static GitHub Pages can't process
submissions server-side. It uses [Formspree](https://formspree.io) (free tier):

1. Sign up and create a new form — Formspree gives you a form ID.
2. In `index.html`, set the form's `action` to `https://formspree.io/f/<your-id>`.
   (It's currently wired to an existing form ID, `xzdldlkl`.)

The form submits via JavaScript (AJAX), so visitors stay on the page and see an
inline **"✓ Thanks! Your message has been sent."** confirmation that clears on the
next click; submissions still arrive in your email inbox. Prefer no form? Just
delete the `<form>` block — the direct email link below it still works.

#### Spam protection

Two layers keep spam out, both compatible with the on-page AJAX flow:

- **Honeypot** — a hidden `_gotcha` field (off-screen via `.form__honeypot` in
  `css/style.css`). Real visitors never see it; bots that auto-fill it get
  silently dropped by Formspree. Works out of the box, no setup.
- **Cloudflare Turnstile** — a privacy-friendly CAPTCHA that Formspree verifies
  server-side. The widget sits to the left of the **Send message** button and a
  Site Key is already wired into the `<div class="cf-turnstile">` in
  `index.html`. To run it on your own form:
  1. In the [Cloudflare dashboard](https://dash.cloudflare.com), create a
     Turnstile widget and list the domains it may run on (add `localhost` too if
     you want it to render during local testing). This gives a **Site Key** and
     **Secret Key**.
  2. Put your **Site Key** in the `data-sitekey` attribute on the
     `<div class="cf-turnstile">` in `index.html` (replacing the existing one).
  3. In the Formspree dashboard, enable CAPTCHA → **Cloudflare Turnstile** and
     paste your **Secret Key**. (Leave Formspree's reCAPTCHA off — it forces a
     redirect that breaks the AJAX flow.)

  Turnstile Site Keys are domain-bound, so the widget only renders on a domain
  listed in its Cloudflare config. On any other host (e.g. `localhost` when it
  isn't allow-listed) it logs a harmless `400` and shows no widget.

### Light / dark mode

A toggle button (🌙 / ☀️) sits in the navbar. New viewers default to **dark
theme** (regardless of system preference); returning visitors keep their chosen
theme via `localStorage`. The default is applied pre-paint in `index.html`'s
`<head>` to avoid a flash. New viewers also start in **standard (non-QA)** mode.

### Scroll controls

Two floating buttons on the right edge (↑ / ↓) smooth-scroll to the top and
bottom of the page.

### Favicon

`assets/favicon.svg` is a simple checkmark icon. Replace it with your own SVG (or
swap in a `.png`/`.ico` and update the `<link rel="icon">` in `index.html`).

## Previewing locally

Just double-click `index.html`, or run a simple local server:

```bash
# Python 3
python -m http.server 8000
# then open http://localhost:8000
```

## Publishing with GitHub Pages

1. Create a new repository on GitHub (e.g. `portfolio` or `yourusername.github.io`).
2. Push this folder to it:

   ```bash
   git init
   git add .
   git commit -m "Initial portfolio site"
   git branch -M main
   git remote add origin https://github.com/yourusername/your-repo.git
   git push -u origin main
   ```

3. On GitHub, go to **Settings → Pages**.
4. Under **Build and deployment**, set **Source** to *Deploy from a branch*,
   choose the `main` branch and the `/ (root)` folder, then **Save**.
5. Wait ~1 minute. Your site will be live at:
   - `https://yourusername.github.io/your-repo/` — for a normal repo, or
   - `https://yourusername.github.io/` — if you named the repo `yourusername.github.io`.

That's it. Every time you push to `main`, the site updates automatically.
