# Publishing karaluo.com — GitHub Pages + Squarespace Domains

Your site is a single self-contained `index.html` with `headshot.jpg`, `CV.pdf`, and a
`CNAME` file already set to **karaluo.com**. Follow these steps in order. Total time ~20
minutes of work + up to a few hours for DNS to propagate.

> If your domain is actually on **Square / Weebly** (not Squarespace), every DNS record
> below is still identical — only the menu names differ. Jump to Step 4 notes.

---

## Step 1 — Put the site on GitHub

GitHub serves a personal site from a repo named **exactly** `USERNAME.github.io`
(replace `USERNAME` with your GitHub username, all lowercase).

1. Sign in at https://github.com (create a free account if needed).
2. Click **+** (top right) → **New repository**.
   - **Repository name:** `USERNAME.github.io`
   - **Public** (required for free Pages)
   - Do **not** add a README, .gitignore, or license.
   - Click **Create repository**.
3. Open **Terminal** on your Mac and run these commands (copy/paste line by line).
   The first command clears a partial git folder I created in the sandbox so you start clean:

   ```bash
   cd ~/Documents/Claude/Projects/"Berkeley Application"/website
   rm -rf .git
   git init -b main
   git add -A
   git commit -m "Initial commit: karaluo.com"
   git remote add origin https://github.com/USERNAME/USERNAME.github.io.git
   git push -u origin main
   ```

   If prompted for a password, use a **Personal Access Token** (GitHub no longer accepts
   account passwords): github.com → Settings → Developer settings → Personal access tokens
   → Tokens (classic) → Generate, with the `repo` scope. Paste it as the password.

After the push, your files should appear in the repo on github.com.

---

## Step 2 — Turn on GitHub Pages

1. In the repo, go to **Settings** → **Pages** (left sidebar).
2. Under **Build and deployment** → **Source**, choose **Deploy from a branch**.
3. Set branch to **main** and folder to **/ (root)**. Click **Save**.
4. Wait ~1 minute. The site goes live at `https://USERNAME.github.io` first — open it to
   confirm it looks right before wiring up the custom domain.

Because the repo contains a `CNAME` file, GitHub will also show **karaluo.com** under
"Custom domain" automatically. If it doesn't, type `karaluo.com` into that box and Save.

---

## Step 3 — Verify your domain (recommended, prevents takeovers)

1. GitHub → your profile **Settings** → **Pages** → **Add a domain** → enter `karaluo.com`.
2. GitHub gives you a `TXT` record like `_github-pages-challenge-USERNAME` with a code value.
3. Add that TXT record in Squarespace (Step 4 shows where), then click **Verify** on GitHub.

This is optional but worth doing. You can also skip straight to Step 4 and add it later.

---

## Step 4 — Point karaluo.com at GitHub (Squarespace DNS)

1. Go to https://account.squarespace.com → **Domains** → click **karaluo.com**.
2. Open **DNS** / **DNS Settings** → **Custom Records** (or "Add Record").
3. **Delete** any existing parking/forwarding `A` or `CNAME` records for `@` and `www`
   that point to Squarespace's own servers (leave MX/email records alone).
4. Add these records exactly:

   **Four A records** — Host `@`, Type `A`:

   | Host | Type | Value |
   |------|------|-------------------|
   | @ | A | 185.199.108.153 |
   | @ | A | 185.199.109.153 |
   | @ | A | 185.199.110.153 |
   | @ | A | 185.199.111.153 |

   **Four AAAA records (IPv6, optional but recommended)** — Host `@`, Type `AAAA`:

   | Host | Type | Value |
   |------|------|---------------------|
   | @ | AAAA | 2606:50c0:8000::153 |
   | @ | AAAA | 2606:50c0:8001::153 |
   | @ | AAAA | 2606:50c0:8002::153 |
   | @ | AAAA | 2606:50c0:8003::153 |

   **One CNAME for www** — so www.karaluo.com redirects to the apex:

   | Host | Type | Value |
   |------|------|----------------------|
   | www | CNAME | USERNAME.github.io. |

   **(If doing Step 3)** the verification TXT record:

   | Host | Type | Value |
   |------|------|-------|
   | _github-pages-challenge-USERNAME | TXT | (code GitHub gave you) |

5. Save.

> **On Square / Weebly instead?** Square's DNS editor calls these the same things:
> add the four `A` records on the root/`@` host with the four IPs above, and a `www`
> `CNAME` to `USERNAME.github.io`. If Square only allows domain *forwarding* and not raw
> A records, you'll need to either transfer DNS to a provider that allows custom records
> (e.g., Cloudflare, free) or use Square's CNAME-only setup pointing `www` to
> `USERNAME.github.io` and forwarding the apex to `www`.

---

## Step 5 — Wait, then enforce HTTPS

1. DNS changes take anywhere from a few minutes to a few hours (sometimes up to 24h).
2. Back on GitHub → Settings → Pages, once the domain check passes, tick
   **Enforce HTTPS**. (The box is greyed out until GitHub finishes issuing the free
   TLS certificate — just wait and refresh.)
3. Visit **https://karaluo.com** — done.

---

## Updating the site later

Edit `index.html` (or swap `CV.pdf` / `headshot.jpg`), then in Terminal:

```bash
cd ~/Documents/Claude/Projects/"Berkeley Application"/website
git add -A
git commit -m "Update site"
git push
```

Changes are live within a minute.

---

## Quick checklist

- [ ] Repo named `USERNAME.github.io`, pushed
- [ ] Pages enabled (Deploy from branch → main / root)
- [ ] `USERNAME.github.io` loads correctly
- [ ] 4 A records + 4 AAAA + www CNAME added at Squarespace
- [ ] Custom domain `karaluo.com` shows in GitHub Pages settings
- [ ] Enforce HTTPS ticked
- [ ] https://karaluo.com loads
