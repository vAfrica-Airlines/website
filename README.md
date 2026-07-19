# vAfrica Airlines — website

Public marketing and recruitment site for **vAfrica Airlines**, a vAMSYS-based
virtual airline. Built with [MkDocs](https://www.mkdocs.org/) and the
[Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) theme, and
deployed to GitHub Pages via GitHub Actions.

> **Strategy in one line:** this site is a funnel that drives visitors to
> **vAMSYS** (join) and **Discord** (community). It does not replicate vAMSYS
> features. See [`CLAUDE.md`](CLAUDE.md) for the full conventions.

---

## Local development

Requires **Python 3.x**. Use a virtual environment:

```bash
# from the repo root
python3 -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
pip install -r requirements.txt

# live-reload preview at http://127.0.0.1:8000
mkdocs serve

# production build with strict checks (must pass before you commit)
mkdocs build --strict
```

`mkdocs build --strict` fails on broken internal links or nav errors — the same
check CI runs.

## Project layout

```
website/
├── mkdocs.yml                     # site config, theme, nav, plugins
├── requirements.txt               # pinned Python deps
├── README.md                      # this file
├── CLAUDE.md                      # repo conventions (funnel strategy, tokens…)
├── .github/workflows/deploy.yml   # build + deploy to GitHub Pages
├── docs/
│   ├── index.md                   # Home: hero + join CTA
│   ├── about.md                   # About the airline
│   ├── join.md                    # Recruitment funnel (key conversion page)
│   ├── faq.md                     # FAQ (collapsible admonitions)
│   ├── contact.md                 # Contact / Discord
│   ├── CNAME                      # custom domain (vafricaairlines.com)
│   └── assets/
│       ├── images/                # logo.svg, favicon.svg (placeholders)
│       └── stylesheets/extra.css  # brand color overrides
└── overrides/                     # Material theme override dir (custom_dir)
```

The built site is generated into `site/` and is **not** committed (see
`.gitignore`); CI builds and publishes it.

## How deployment works

`.github/workflows/deploy.yml` runs on every push to `main`:

1. Checks out the repo and sets up Python.
2. `pip install -r requirements.txt`.
3. `mkdocs build --strict`.
4. Uploads `site/` as a Pages artifact and deploys it with the official
   `actions/deploy-pages` flow (permissions `pages: write`, `id-token: write`,
   with a `concurrency` guard).

You can also trigger it manually from the **Actions** tab (`workflow_dispatch`).

### One-time GitHub Pages setup (Miguel)

1. **Settings → Pages → Source:** set to **"GitHub Actions"**.
2. **Settings → Pages → Custom domain:** enter your domain (this keeps
   `docs/CNAME` in sync).
3. Enable **Enforce HTTPS** once the certificate is issued.

### Custom domain DNS (Miguel)

**Configured:** the site's custom domain is **`vafricaairlines.com`** (apex),
set in Pages settings with apex `A` records pointed at GitHub. Reference kept
below.

GitHub Pages custom-domain docs:
<https://docs.github.com/pages/configuring-a-custom-domain-for-your-github-pages-site>

- **Apex domain** (`vafricaairlines.com`): add `A` records to GitHub Pages' IPs
  (and `AAAA` for IPv6), or an `ALIAS`/`ANAME` if your DNS host supports it:

  ```
  A     185.199.108.153
  A     185.199.109.153
  A     185.199.110.153
  A     185.199.111.153
  ```

- **`www` subdomain** (optional, `www.vafricaairlines.com`): add a `CNAME`
  record pointing to `vafrica-airlines.github.io`. GitHub will then redirect
  `www` → apex automatically. Not required since the apex is already set.

---

## Placeholders Miguel must replace

This is scaffolding. Search the repo for each token below and replace **all**
occurrences with real values. Also replace the placeholder assets.

| Token / item          | Where it appears                                   | Replace with                          |
| --------------------- | -------------------------------------------------- | ------------------------------------- |
| ~~`<CUSTOM_DOMAIN>`~~ | done — set to `vafricaairlines.com`                | ✅ replaced                            |
| `<VAMSYS_JOIN_URL>`   | `docs/index.md`, `about.md`, `join.md`, `faq.md`, `contact.md` | The vAMSYS join/apply URL             |
| `<DISCORD_INVITE_URL>`| `docs/index.md`, `join.md`, `contact.md`, `mkdocs.yml` (`extra.social`) | Your Discord invite link              |
| `<AIRLINE_TAGLINE>`   | `docs/index.md`                                    | Your one-line tagline / value prop    |
| `<!-- TODO: Miguel -->` blocks | throughout `docs/`                        | Real page copy                        |
| `docs/assets/images/logo.svg` | header logo                               | Real logo art                         |
| `docs/assets/images/favicon.svg` | browser tab icon                       | Real favicon                          |
| Brand colors          | `docs/assets/stylesheets/extra.css`, `theme.palette` in `mkdocs.yml` | Real brand palette   |

Find every token at once:

```bash
grep -rn -e '<VAMSYS_JOIN_URL>' -e '<DISCORD_INVITE_URL>' \
        -e '<AIRLINE_TAGLINE>' -e 'TODO: Miguel' .
```

## Optional: social cards (Open Graph images)

Material can auto-generate social/OG preview images via its `social` plugin. It
needs the Cairo/Pango system libraries and the `pillow`/`cairosvg` Python deps.
To enable: uncomment the `social` plugin in `mkdocs.yml`, the deps in
`requirements.txt`, and the apt-get step in `deploy.yml`.
