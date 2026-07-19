# CLAUDE.md — vAfrica Airlines website

Conventions for future Claude Code runs in this repo. Read this before making
content or feature changes.

## What this repo is

The **public marketing/recruitment website** for vAfrica Airlines, a
vAMSYS-based virtual airline. It is one of three org repos:

- `website` — this repo (public site).
- `operations` — private operations manual (also Material for MkDocs). **Do not**
  put operations-manual content here.
- `.github` — org profile + shared health files.

## Core strategy — funnel first

The site's only job is to **drive traffic TO vAMSYS** (primary CTA) and Discord
(secondary CTA). It is a funnel, not a platform.

- **Every page** should lead toward the vAMSYS join CTA.
- **Do NOT replicate vAMSYS features** — no live maps, leaderboards, pilot
  dashboards, or flight tracking. That splits attention for an inferior copy.
  Those live on vAMSYS; link to them, don't rebuild them.
- Keep it lean. Fewer, sharper pages beat more pages.

## Tech stack (use exactly this)

- **MkDocs** + **Material for MkDocs** theme.
- Python deps pinned in `requirements.txt`.
- Deploy: **GitHub Pages via GitHub Actions** (`.github/workflows/deploy.yml`,
  using `upload-pages-artifact` → `deploy-pages`). The built `site/` is **never
  committed** (it's in `.gitignore`).
- Mirror the `operations` repo's Material conventions where sensible so the two
  sites feel like one brand.

## Build requirement

- Builds must pass **`mkdocs build --strict`** (set in `mkdocs.yml` and CI).
  Strict turns broken internal links / nav errors into failures. Run it locally
  before committing.

## Placeholder-token convention

Real destinations and copy are **Miguel's to fill in**. Use these exact tokens
so they're find-and-replaceable — never guess real URLs, domains, or copy:

| Token                  | Meaning                                    |
| ---------------------- | ------------------------------------------ |
| `<CUSTOM_DOMAIN>`      | The site's custom domain (e.g. www.…)      |
| `<VAMSYS_JOIN_URL>`    | vAMSYS join/apply URL (primary CTA)        |
| `<DISCORD_INVITE_URL>` | Discord invite link (secondary CTA)        |
| `<AIRLINE_TAGLINE>`    | One-line value prop / tagline              |

- Mark spots needing real copy with `<!-- TODO: Miguel -->`.
- External CTA links are written as raw HTML `<a>` anchors so the `<TOKEN>`
  survives markdown parsing and `--strict` link validation. Internal page links
  use normal markdown relative links (which strict *does* validate — keep them
  correct).

## Design / copy

- Final marketing copy, real branding/colors, and logo art are **out of scope**
  for scaffolding — placeholders only. Brand colors live in
  `docs/assets/stylesheets/extra.css` and the `theme.palette` in `mkdocs.yml`.

## Out of scope (do not build)

- Live flight maps, leaderboards, dashboards — anything duplicating vAMSYS.
- Operations-manual content (belongs in the private `operations` repo).
- Auth/SSO gating — this is a public site.
