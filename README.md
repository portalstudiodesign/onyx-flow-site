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
3. **The launch countdown.** The hero shows a days/hours/minutes counter beside the Play
   button. Its target is hard-coded in the inline script at the bottom of `index.html`:

   ```js
   var target = new Date('2026-09-02T09:20:00+03:00').getTime();
   ```

   That is 14 days from the start of closed testing (25 testers, started 19 August 2026 at
   09:20 Romanian time). The offset is written out explicitly, so the counter reads the same
   in every timezone; Romania stays on UTC+3 until the last Sunday of October. When the date
   passes, the counter hides itself and the page simply reads "Coming to Google Play" — so a
   slipped launch degrades quietly rather than showing zeroes. Change the date there if the
   window moves, and delete the `.countdown` block once the game is actually live.

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

## Live site

**<https://portalstudiodesign.github.io/onyx-flow-site/>**

Privacy policy URL for the Play Console listing — use the full path, GitHub Pages has no
redirect layer:

```
https://portalstudiodesign.github.io/onyx-flow-site/privacy.html
```

## Publishing an update

The folder is its own git repo (`portalstudiodesign/onyx-flow-site` — deliberately *not* named
`onix-flow`, which is the private repo), separate from the parent directory — the parent's `.gitignore` ignores it, so the game source can never be pushed by
accident. Edit the files, then:

```bash
git -C onyxflow-site add -A && git -C onyxflow-site commit -m "Update copy" && git -C onyxflow-site push
```

GitHub Pages rebuilds in roughly a minute. `.nojekyll` is present so files are served exactly as
committed.

## Moving to a custom domain later

1. Buy the domain, then add a `CNAME` file to this repo containing just the domain.
2. At the registrar, point the apex at GitHub's IPs (`185.199.108–111.153`) or add a
   `CNAME` record for `www` → `portalstudiodesign.github.io`.
3. In the repo's Settings → Pages, set the custom domain and tick "Enforce HTTPS".
4. **Update the privacy-policy URL in the Play Console listing** to the new domain.

`netlify.toml` is kept in the repo for the day you move to Netlify instead; it has no effect on
GitHub Pages, which is why the `/privacy` shortcut does not work there.

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
