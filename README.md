# outhaul-site

Marketing landing page for [Outhaul](https://github.com/outhaul-dev/outhaul) — the self-hosted, git-push-to-deploy PaaS in a single Go binary.

Served via **GitHub Pages** at **[outhaul.sh](https://outhaul.sh)**.

## What's here

| File        | Purpose                                                                       |
| ----------- | ----------------------------------------------------------------------------- |
| `index.html`| The entire landing page — self-contained, no build step. HTML + inline CSS/JS.|
| `install`   | Installer script served at `https://outhaul.sh/install` (see below).          |
| `CNAME`     | Custom domain for GitHub Pages (`outhaul.sh`).                                 |
| `.nojekyll` | Disables Jekyll so Pages serves every file — including `install` — untouched. |

There is no framework and no build tooling. The page is a single static HTML file with all styles and scripts inline, so it can be edited directly and previewed by opening it in a browser.

## Local preview

Just open the file:

```sh
xdg-open index.html      # Linux
open index.html          # macOS
```

Or serve it (useful for testing the `/install` path locally):

```sh
python3 -m http.server 8000
# → http://localhost:8000
```

## Deployment (GitHub Pages)

The site deploys automatically from `main`.

1. **Settings → Pages** → Source: **Deploy from a branch**, branch `main`, folder `/ (root)`.
2. **Custom domain** — `outhaul.sh` (already set via the `CNAME` file). At the DNS registrar for `outhaul.sh`, point the apex domain at GitHub's Pages IPs with **A records**:

   ```
   185.199.108.153
   185.199.109.153
   185.199.110.153
   185.199.111.153
   ```

   Optionally add a `CNAME` record for `www` → `outhaul-dev.github.io`.
3. **Enforce HTTPS** — tick it once GitHub finishes provisioning the Let's Encrypt certificate (can take a few minutes to an hour after the domain is first set).

Pushing to `main` publishes within a minute or two.

## The install one-liner

The page advertises:

```sh
curl -fsSL https://outhaul.sh/install | sh
```

For this to work, `https://outhaul.sh/install` must serve the **script bytes directly** — it cannot be a redirect, since `curl` piping to `sh` won't follow an HTML redirect. GitHub Pages serves the root-level `install` file at exactly that path (content-type is irrelevant to `curl | sh`).

> **`install` is currently a placeholder.** It is a safe no-op: it prints a "not ready yet" message and exits non-zero, so piping it to `sh` does nothing. Replace it with the real installer when the binary ships.

**Caching note:** GitHub Pages serves through a CDN with `Cache-Control: max-age=600`, so edits to `install` (and any other file) can take up to ~10 minutes to propagate.

## Editing the page

Everything lives in `index.html`. Copy is intentionally scoped to features that genuinely ship today — avoid claiming anything that isn't ready (teams/RBAC, metrics history, multi-node, registry, Compose blue-green, etc.).
