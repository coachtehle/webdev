# Deploying The Liminal Leadership Institute site

This is a static site: three HTML pages (`index.html`, `privacy.html`, `terms.html`), all CSS embedded inline, images in `assets/`. No build step, no server, no database. That makes it about as cheap and simple to host as a website gets.

Recommended stack: **Cloudflare Pages** (hosting, free) + **Cloudflare Registrar** (domain, at-cost pricing, no markup). Using both under one Cloudflare account also makes DNS close to automatic (see Part 3).

---

## Part 1 — Get the code somewhere Cloudflare can see it

You have two options. Pick one.

### Option A — Direct Upload (fastest, no GitHub account needed)
1. Zip the contents of this folder (not the folder itself — the files should be at the root of the zip).
2. Create a free account at https://dash.cloudflare.com/sign-up
3. In the dashboard: **Workers & Pages → Create → Pages → Upload assets**
4. Upload the zip. Cloudflare deploys it immediately and gives you a URL like `your-site.pages.dev`.
5. **Downside:** every future change means re-zipping and re-uploading by hand.

### Option B — Git-based deploy (recommended if you'll keep updating the site)
1. Create a free GitHub account if you don't have one: https://github.com/signup
2. Create a new repository (public or private, either works) — do **not** initialize it with a README.
3. Back in this folder, connect it to that new repo and push:
   ```bash
   git remote add origin https://github.com/YOUR-USERNAME/YOUR-REPO.git
   git branch -M main
   git push -u origin main
   ```
   (GitHub will prompt you to authenticate the first time — follow its instructions.)
4. In Cloudflare: **Workers & Pages → Create → Pages → Connect to Git**, authorize GitHub, pick the repo.
5. Build settings: leave **Build command** blank and **Output directory** as `/` (root) — there's nothing to build.
6. Deploy. From now on, every `git push` automatically redeploys the live site.

---

## Part 2 — Buy the domain

There is no such thing as a one-time "lifetime" domain — every registrar charges an annual renewal, that's set by the domain registries themselves, not the seller. What you're choosing is who charges you the least for that renewal.

1. In the Cloudflare dashboard: **Domain Registration → Register a Domain**.
2. Search your desired domain. Cloudflare sells at cost (no markup), typically ~$9-10/year for `.com`.
3. Complete the purchase (you'll need your own payment method — this part has to be done by you directly).

If you'd rather buy elsewhere (Namecheap, Porkbun, etc.), that's fine too — see Part 3B below for what changes.

---

## Part 3 — Connect the domain to the site

### 3A — If you bought the domain through Cloudflare (simplest)
1. In your Pages project: **Custom domains → Set up a custom domain**.
2. Enter your domain (e.g. `theliminalleadershipinstitute.com`).
3. Cloudflare already controls DNS for this domain (since it's registered there), so it adds the required record automatically. Usually live within a few minutes.
4. Repeat for the `www` subdomain if you want both `example.com` and `www.example.com` to work.

### 3B — If you bought the domain somewhere else
You have two ways to connect it:

**Easiest: move DNS management to Cloudflare (still free, doesn't require moving the registration itself)**
1. Add the domain as a new "site" in your Cloudflare dashboard (free plan).
2. Cloudflare gives you two nameservers (e.g. `aida.ns.cloudflare.com`).
3. Log into your original registrar (Namecheap, etc.) and replace its nameservers with the two Cloudflare gave you. This can take a few hours to propagate.
4. Once active, follow the same steps as 3A.

**Alternative: keep DNS at your original registrar, add a manual record**
1. In your Pages project's **Custom domains** tab, Cloudflare will show you the exact CNAME target to use (something like `your-site.pages.dev`).
2. In your registrar's DNS settings, add:
   - Type: `CNAME`
   - Name: `www` (or `@` if your registrar supports CNAME flattening at the root — not all do)
   - Value: the `.pages.dev` target Cloudflare shows you
3. For the bare root domain (`example.com` without `www`), most non-Cloudflare registrars can't point a root domain via CNAME — you'd typically redirect root → `www` instead, or use the registrar's "ALIAS"/"ANAME" record type if it offers one.

---

## What's already done on the code side
- Git repository initialized, dead template files removed (unused framework CSS/JS, an old draft HTML, orphaned icon assets).
- `.gitignore` excludes local tool config.
- Nothing in this repo requires environment variables, API keys, or a database — there's nothing secret to protect.

## What you still need to do yourself
- Create the Cloudflare (and optionally GitHub) account.
- Choose and pay for the domain.
- Tell me once you've picked a path and I'll help with anything on the code/config side (e.g. adjusting the contact form to actually submit somewhere, since right now it has no backend).
