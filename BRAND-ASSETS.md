# Brand assets — manifest

Web-ready brand artifacts for the vAfrica Airlines site, generated from the
brand system in `Github/brand/`. Palette and type: teal `#009B8D` /
cyan `#02B4DC` / burnt-orange `#BA4D00` / rust `#7D3814` / navy `#0B2628` /
cream `#EFEDEC`; display **Archivo**, body **Hanken Grotesk**, mono **IBM Plex Mono**.

| File | Size / format | Used by |
| --- | --- | --- |
| `docs/assets/images/logo.svg` | vector | `theme.logo` — header mark (compact roundel, no plane, reads at ~24 px) |
| `docs/assets/images/favicon.svg` | vector | `theme.favicon` — Africa on the sunset disc, legible at 16 px |
| `docs/assets/images/favicon.ico` | 16/32/48 | legacy favicon fallback |
| `docs/assets/images/apple-touch-icon.png` | 180×180 | `overrides/main.html` `apple-touch-icon` link |
| `docs/assets/images/roundel.svg` | vector | full emblem (Africa + airliner) for large/marketing use |
| `docs/assets/images/logo-lockup-light.svg` | vector | horizontal wordmark, **light** text — for dark backgrounds |
| `docs/assets/images/logo-lockup-dark.svg` | vector | horizontal wordmark, **dark** text — for light backgrounds |
| `docs/assets/images/og-image.png` | 1200×630 | default Open Graph / Twitter card (`overrides/main.html`) |
| `docs/assets/images/hero.png` | 1600×760 | home hero banner (`docs/index.md`, `.vafrica-hero`) |
| `docs/assets/stylesheets/extra.css` | — | `:root` brand tokens + Material palette/type overrides |

## Notes for final production

- **Wordmark face:** the lockup / OG / hero text is currently set in a metric-
  compatible grotesk (build environment lacked Archivo). The canonical display
  face is **Archivo** — re-export the wordmark in Archivo 800 for pixel-final art.
- **Africa silhouette** was vector-traced from the existing brand art, then
  simplified. The airliner uses a clean geometric glyph rather than the original
  painterly plane.
- **Social cards** use static default meta in `overrides/main.html` (no Cairo/
  Pango deps). To switch to per-page auto-generated cards later, enable the
  Material `social` plugin (see `requirements.txt` / `mkdocs.yml`).
