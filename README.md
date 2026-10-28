# Lumana holding site

A static placeholder for www.lumana-beauty.com until the real app launches. Plain HTML/CSS, no build step, no tracking, no analytics, no cookies.

| URL | File |
| --- | --- |
| `/` | `index.html` (Spanish first, English section below, anchor toggle, no JavaScript) |
| `/privacy-policy` | `privacy-policy/index.html` |
| `/sms-terms` | `sms-terms/index.html` |

The two legal pages carry the same text as `src/legal/text.ts` in the main app. They are generated: after any change to that file, run `npm run holding:legal` in `lumana-web` (script: `scripts/build-holding-legal.mjs`). `tests/holding-legal.test.ts` fails if the pages in this folder are out of date. Don't edit the two legal `index.html` files by hand. (Change the text itself only with Annie's and counsel's approval and a new "Last updated" date.)

`CNAME` (`www.lumana-beauty.com`) and `.nojekyll` are needed by GitHub Pages. Leave them in place. Fonts are self-hosted (SIL OFL, licences in `fonts/`).

## Before you publish

- Phone (816) 914-1322 and hola@lumana-beauty.com (confirmed working) are already in `index.html`, matching the legal pages.
- Counsel review of the privacy policy is still pending (LAUNCH_INPUTS #80). The public text has no placeholder markers; if counsel changes the wording, edit `src/legal/text.ts` in the main app, bump the "Last updated" date, and regenerate these two pages.

## 1. Publish with GitHub Pages (free)

> **Important:** `lumana-web` is a **private** repository and must stay private. Do not turn on Pages for it. Publish from a **separate, PUBLIC repository that contains only the files in this `holding-site/` folder** (nothing else from `lumana-web`: no source, docs, handoffs or env files). GitHub Pages on a free organization plan needs a public repo.

1. Sign in to GitHub as a member of the **Lumana-beauty** organization and open https://github.com/organizations/Lumana-beauty/repositories/new.
2. Create a repository named `holding-site`, **Public**, with no README, `.gitignore` or licence. (A public repo is fine: it only holds public text and logos.)
3. Upload the **contents** of this `holding-site/` folder so `index.html`, `CNAME`, `.nojekyll`, `style.css`, `brand/`, `fonts/`, `privacy-policy/` and `sms-terms/` sit at the repo root. This README and `screenshots/` can be left out. Easiest from a terminal:
   ```
   cd holding-site
   git init -b main
   git add -A
   git commit -m "Lumana holding site"
   git remote add origin https://github.com/Lumana-beauty/holding-site.git
   git push -u origin main
   ```
   Run these inside a copy of the `holding-site/` folder (or the folder itself after deleting its `screenshots/` folder if you don't want them public). The `git init` creates a brand-new repo, not part of `lumana-web`.
4. In the new repo go to **Settings → Pages**. Under **Build and deployment**, set **Source: Deploy from a branch**, **Branch: `main`**, folder **`/ (root)`**, then Save.
5. Under **Custom domain** enter `www.lumana-beauty.com` and Save. (It will show a DNS check error until step 2 below is done. That is expected.)
6. Check the preview at `https://lumana-beauty.github.io/holding-site/` before touching DNS. The pages use relative links, so they work there too.

## 2. GoDaddy DNS changes

GoDaddy → **My Products → lumana-beauty.com → DNS → Manage DNS** (the DNS Records tab).

**Change only these records:**

1. **Delete** the existing `A` record for Name `@` that points to GoDaddy's WebsiteBuilder / "Coming Soon" address (often shown as `WebsiteBuilder Site`). Delete any other `A` record on `@` as well. Also delete any `AAAA` record on `@` that you don't recognize as GitHub's.
2. **Add four `A` records**, all with Name `@`, TTL 1 hour (or the default):

   | Type | Name | Value |
   | --- | --- | --- |
   | A | @ | `185.199.108.153` |
   | A | @ | `185.199.109.153` |
   | A | @ | `185.199.110.153` |
   | A | @ | `185.199.111.153` |

3. **Edit or add the `CNAME` record** for Name `www`: Value `lumana-beauty.github.io` (the org name in lowercase, with no repo name and no `https://`). If `www` currently points to `@` or to a GoDaddy host, replace that value.

**Do NOT touch** any `MX`, `TXT` (including SPF, `_dmarc`, DKIM and verification records), or other `CNAME` records (for example `autodiscover`, `email`, DKIM selectors). They run Annie's email. If GoDaddy prompts to "reset" or "restore default" records, decline.

If GoDaddy says the domain is connected to Website Builder and won't let you edit `@`, disconnect the site from the domain first (Website Builder → Settings → Domain) or switch the domain's forwarding/parking off. That doesn't affect email.

## 3. Finish and verify

1. DNS can take minutes up to a few hours. In the repo's **Settings → Pages**, the check next to the custom domain should turn green.
2. When **Enforce HTTPS** becomes available, tick it. GitHub issues the certificate automatically. This can take up to about an hour after the DNS check passes.
3. Test: `https://www.lumana-beauty.com/`, `https://www.lumana-beauty.com/privacy-policy` and `https://www.lumana-beauty.com/sms-terms`. `https://lumana-beauty.com` (no `www`) should redirect to the `www` address.
4. Confirm email still works by sending a test message to and from an @lumana-beauty.com address.

## Launch day

When the real app goes live on www.lumana-beauty.com, the app must serve `/privacy-policy` and `/sms-terms` at those same paths (it already does). Then point DNS at the app's host the same way (same records only, same hands-off rule for email records), and archive this repo.
