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

### CTA URLs — centralized, never inlined

The two CTA URLs live in **one place** and are referenced everywhere via the
`macros` plugin:

- Defined in `mkdocs.yml` under `extra:` — `vamsys_join_url` and `discord_url`.
- Used in page bodies as markdown buttons:
  `[Join on vAMSYS]({{ vamsys_join_url }}){ .md-button .md-button--primary }`.
- The footer `extra.social` Discord link uses the **literal** URL (macros don't
  expand inside `mkdocs.yml`) — keep it in sync with `discord_url`.

> **Do not** write CTAs as raw HTML `<a href="<TOKEN>">` or as
> `[text](<TOKEN>)`. A bare `<TOKEN>` becomes a *relative autolink*, so the
> button 404s against the current page and `--strict` does **not** catch it.
> That was the original dead-CTA bug. Always use the `{{ macro }}` form above.

### Build guard against stray tokens

CI (`.github/workflows/deploy.yml`) fails the build if any unreplaced token
remains, via `grep -rniE '<[A-Z_]+_URL>|vafricaairlines\.com/<|>>|<<INSERT'`
over `docs/` and `mkdocs.yml`. `--strict` can't see these; the guard is the
safety net. If you add a new placeholder token, make sure it's replaced (or
extend the guard) before merging to `main`.

## Design / copy

- Final marketing copy, real branding/colors, and logo art are **out of scope**
  for scaffolding — placeholders only. Brand colors live in
  `docs/assets/stylesheets/extra.css` and the `theme.palette` in `mkdocs.yml`.

## Out of scope (do not build)

- Live flight maps, leaderboards, dashboards — anything duplicating vAMSYS.
- Operations-manual content (belongs in the private `operations` repo).
- Auth/SSO gating — this is a public site.
