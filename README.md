# David Schumann LLC — website

A simple one-page marketing site: photovoltaic investments, battery storage, custom IT solutions, and a short **Legal notice**. Static HTML and CSS, suitable for **GitHub Pages** with a **Namecheap** domain.

## Multiple domains on GitHub Pages

**Yes — GitHub Pages works with this setup**, and **each repository can use its own custom domain.**

- Your existing site on **schumann.com.de** stays tied to **whatever repository** you configured under **Settings → Pages → Custom domain** for that repo.
- Create a **second repository** for this LLC site (or keep using this one), enable Pages on it, and set **Custom domain** to **davidschumannllc.com**.
- In Namecheap, point **davidschumannllc.com** using **A** records (apex) or **CNAME** (`www` → `YOUR_USERNAME.github.io`) exactly like your first domain — but **only for this domain’s DNS zone**. DNS for `schumann.com.de` does not need to change.

Limits worth knowing: one **custom domain per Pages site** (apex + `www` can be configured together in practice via redirects); different repos = different sites = different domains. Your GitHub account can host many Pages sites.

## Local preview

Open `index.html` in a browser, or serve the folder:

```bash
cd /path/to/llc_website
python3 -m http.server 8080
```

Then visit `http://localhost:8080`.

## Deploy with GitHub Pages

### 1. Push this repository to GitHub

Create a new repository (for example `llc_website`), then:

```bash
git add index.html styles.css README.md CNAME favicon.svg
git commit -m "Add David Schumann LLC one-pager"
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
git push -u origin main
```

Use your real username and repository name in place of `YOUR_USERNAME` / `YOUR_REPO`.

### 2. Turn on GitHub Pages

1. On GitHub, open the repo → **Settings** → **Pages** (under “Code and automation”).
2. Under **Build and deployment**, set **Source** to **Deploy from a branch**.
3. Choose branch **`main`** (or `master`) and folder **`/ (root)`**, then **Save**.

After a minute or two, the site is available at:

- **Project site:** `https://YOUR_USERNAME.github.io/YOUR_REPO/`
- **User/org site:** If the repo name is exactly `YOUR_USERNAME.github.io`, the site is at `https://YOUR_USERNAME.github.io/`

If assets look wrong on a project site, ensure links use **relative** paths (this project uses `styles.css` relative to `index.html`, which is correct).

### 3. Custom domain (Namecheap) + HTTPS

#### Option A — Apex domain (`example.com`)

In Namecheap: **Domain List** → **Manage** → **Advanced DNS**.

1. Remove conflicting **URL Redirect** or parked records if they clash with GitHub.
2. Add **A Records** (Host `@`) pointing to GitHub Pages IPs:

   | Type | Host | Value           | TTL    |
   | ---- | ---- | --------------- | ------ |
   | A    | `@`  | `185.199.108.153` | Automatic (or 300 s) |
   | A    | `@`  | `185.199.109.153` | … |
   | A    | `@`  | `185.199.110.153` | … |
   | A    | `@`  | `185.199.111.153` | … |

3. On GitHub: **Settings** → **Pages** → **Custom domain** → enter `example.com` → **Save**. Wait for DNS check; GitHub may create or update a `CNAME` file in the repo for you.
4. Enable **Enforce HTTPS** once the certificate is issued (often after DNS propagates, within minutes to 48 hours).

#### Option B — `www` subdomain

1. In Namecheap **Advanced DNS**, add a **CNAME Record**:

   - **Host:** `www`
   - **Target:** `YOUR_USERNAME.github.io` (no `https://`, no trailing path)

2. In GitHub **Pages**, set custom domain to `www.example.com` (or add both apex and `www`—GitHub documents supporting one primary; many teams redirect one to the other in Pages settings).

#### Optional: `CNAME` file in the repo

If GitHub does not add it automatically, commit a file named **`CNAME`** in the repository root whose **only line** is your canonical hostname, for example:

```text
www.example.com
```

or

```text
example.com
```

Match whatever you entered under **Custom domain** in GitHub.

### 4. Namecheap-specific tips

- DNS changes can take **30 minutes to several hours** to propagate; use a DNS checker if the domain does not resolve.
- Turn **off** Namecheap’s **URL Redirect** for `@` if you want GitHub to serve the site directly via A records.
- **Email forwarding** / MX records are unaffected if you only add A/CNAME for the web host; do not delete MX records your mail provider needs.

## Content notes

- Hero and section images load from **Unsplash** via HTTPS. Replace URLs in `index.html` with your own photography or hosted assets anytime.
- **Legal notice** is a short U.S.-style block (entity, address, site, email). Adjust if your counsel asks for more.

## Files

| File        | Purpose                          |
| ----------- | -------------------------------- |
| `index.html`| Structure and copy               |
| `styles.css`| Layout, typography, theme        |
| `favicon.svg` | Tab icon (simple SVG mark)    |
