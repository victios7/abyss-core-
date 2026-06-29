

# ABYSS::CORE — GitHub Pages Deployment

## What you get

A **100% static, serverless** version of ABYSS::CORE that runs entirely in the browser.  
No server, no Node.js, no backend — just one HTML file.

| Feature | GitHub Pages version | Replit version |
|---|---|---|
| Argon2id | ✓ (WASM via CDN) | ✓ (native Node.js) |
| Layer 1 cipher | AES-256-GCM | ChaCha20-Poly1305 |
| Layer 2 cipher | AES-256-GCM (key 2) | AES-256-GCM |
| HMAC | SHA-512 | SHA3-512 |
| Seed version | v3 (static) | v2 (server) |
| IP admin panel | ✗ | ✓ |
| File encryption | ✓ | ✓ |
| .abyss export | ✓ | ✓ |
| Oracle AI | ✓ (Pollinations API) | ✓ |

> ⚠️ Seeds from the Replit server (v2) are NOT compatible with the GitHub Pages version (v3).
> They use different KDF info strings and cipher chains. Each version decrypts only what it encrypted.

---

## Deployment — Exact Steps

### 1. Create the GitHub repository

```bash
# Option A: via GitHub CLI
gh repo create abyss-core --public --clone
cd abyss-core

# Option B: manually
# Go to https://github.com/new
# Name: abyss-core
# Visibility: Public (required for free GitHub Pages)
# Click "Create repository"
# Then clone it:
git clone https://github.com/YOUR_USERNAME/abyss-core.git
cd abyss-core
```

### 2. Copy the file

Copy `index.html` from this folder into the root of your repository:

```bash
# From the github-pages/ folder in this project:
cp github-pages/index.html /path/to/abyss-core/index.html
```

Or download it manually and place it at the repo root.

### 3. Push to GitHub

```bash
cd abyss-core
git add index.html
git commit -m "Deploy ABYSS::CORE"
git push origin main
```

### 4. Enable GitHub Pages

1. Go to your repository on GitHub
2. Click **Settings** → **Pages** (left sidebar)
3. Under **Source**, select **Deploy from a branch**
4. Branch: `main` · Folder: `/ (root)`
5. Click **Save**

### 5. Access your site

After ~30–60 seconds, your site will be live at:

```
https://YOUR_USERNAME.github.io/abyss-core/
```

That's it. No build step, no npm install, no config files.

---

## Custom Domain (optional)

1. Buy a domain (e.g. `abyss-core.io`)
2. In your DNS provider, add a CNAME record:
   ```
   CNAME  www  YOUR_USERNAME.github.io
   ```
3. In GitHub repo Settings → Pages → Custom domain, enter `www.abyss-core.io`
4. Check **Enforce HTTPS**

---

## Security notes

- All cryptography runs in the browser via **SubtleCrypto** (Web Crypto API) — no data is ever sent to any server
- Argon2id parameters: 128 MB memory, 5 iterations, parallelism 4 — identical to server version
- The HMAC-SHA-512 integrity check prevents silent data corruption or tampering
- The Oracle AI chat sends ciphertext statistics (NOT the ciphertext itself) to Pollinations AI for analysis
- PIN is never included in the `.abyss` export file — store it separately

---

## Troubleshooting

| Problem | Fix |
|---|---|
| Site shows 404 | Wait 60s after enabling Pages; make sure `index.html` is at repo root, not in a subfolder |
| `argon2 is not defined` | CDN may be blocked; check browser console and network tab |
| Encryption very slow | Argon2id with 128MB/5 iterations takes 8–20 seconds — this is by design |
| Oracle AI fails | Pollinations API may be rate-limited; try again after 10s |
| Wrong seed version error | You are trying to decrypt a server-side (v2) seed with the static (v3) engine — use the Replit app instead |
