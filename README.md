# Onyx Flow — site

Static landing page + privacy policy for **Onyx Flow** by Portal Design Studio
(internal project: Project NOVA). No build step, no dependencies, no external requests —
plain HTML/CSS/JS plus media.

```
index.html      landing page
privacy.html    privacy policy (Google Play requires a public URL for this)
styles.css      shared styles — the palette is copied from the game's NovaUIKit
favicon.svg     brand mark (onyx ring + amethyst crystal)
robots.txt
netlify.toml    deploy config: publish ".", /privacy → /privacy.html
assets/         real captures from the Galaxy A25 build + the Open Graph card
```

## Before publishing

1. **Store link.** When the game is live, swap the disabled
   `<span class="btn">Coming to Google Play</span>` in the hero for a real `<a>` to the listing,
   and drop the "In development" pill.
2. Set `og:url` / a canonical link once the final domain is known.

## The privacy policy and the ads

`privacy.html` is written for the shipping plan: **Google AdMob ads, and nothing else
collected.** It documents what AdMob receives (advertising ID, IP, device and app info, ad
interactions), links Google's own policies, and describes the EEA/UK consent flow and how to
reset the advertising ID.

Three things have to line up with it in the Play Console:

- **Data Safety form** — declare what AdMob collects. A mismatch between the form and this page
  is a common cause of rejection.
- **Consent (EEA/UK/Switzerland)** — Google requires a certified CMP. The policy says the
  consent choice can be changed later from the game's Settings, so that entry point has to
  exist in the app.
- **Target audience** — the policy states the game is not directed at children under 13. If you
  set a child-inclusive audience in Play, AdMob must run in child-directed mode and this page
  has to say so.

Re-check the policy before any release that adds an ad network, in-app purchases, cloud saves,
leaderboards, analytics or crash reporting. It is not legal advice — have it reviewed if you
want certainty.

## Local preview

```bash
python -m http.server 4321 --directory onyxflow-site
```

Then open <http://localhost:4321>.

## Deploy (Netlify)

The repository root is already linked to a *different* Netlify site, so create a new one rather
than deploying into that link:

```bash
netlify sites:create --name onyx-flow
```

```bash
netlify deploy --dir=onyxflow-site --prod --site onyx-flow
```

Drag-and-drop onto <https://app.netlify.com/drop> works just as well — the folder is
self-contained. Any static host (GitHub Pages, Cloudflare Pages, Vercel) will serve it as-is.

## Where the assets came from

Everything in `assets/` is a real capture of the game on the Samsung Galaxy A25, cut with ffmpeg
from the four screen recordings in `Downloads/onyx flow website  photo and videos/` (14 August
2026). Those recordings have no status bar, so the crop box is `392:800:0:0` on a 392×850
capture — it keeps the whole play area and drops only the on-screen dev buttons at the bottom.

| file | source | timestamp |
| --- | --- | --- |
| `shot-menu.webp` | video 1 — main menu | 5.0 s |
| `shot-ignition.webp` | video 1 — IGNITION combo, row clearing | 25.5 s |
| `shot-hot.webp` | video 1 — HOT combo at ×5, column clearing | 34.2 s |
| `shot-flow.webp` | video 1 — the moment Flow activates | 51.5 s |
| `shot-crystal.webp` | video 1 — board lit while holding Flow | 54.0 s |
| `skin-1.webp` | video 1 — Classic | 42.0 s |
| `skin-2.webp` | video 2 — Glassmorphism | 60.0 s |
| `skin-3.webp` | video 3 — Neon | 63.0 s |
| `skin-4.webp` | video 4 — Holographic | 45.0 s |
| `hero-loop.mp4` | video 1 — 14 s: clears → combo → Flow activation | 47.5–61.5 s |
| `studio-emblem.webp` | `logo portal design studios.png`, emblem cropped | — |
| `og.png` | Open Graph card, rendered from a throwaway page at 1200×630 | — |

**Skin mapping is assumed from the recording order** (the videos were sent as Classic,
Glassmorphism, Neon, Holographic). Confirm it before publishing — the four captions in the
"Four finishes" section are the only place it matters.

Not used: the two 14 August JPEG screenshots, because both have an AdMob test banner across
the bottom.
