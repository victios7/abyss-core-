

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
