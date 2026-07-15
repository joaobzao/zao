# zao.pt redesign — design

## Context

The site shipped 2026-07-15: a single hand-written static page, live at `zao.pt` via a
Cloudflare Worker. It works, but it reads as a flat list — every project has the same
weight and one line of description.

Two things are wrong with that:

1. **It looks plain.** The content is broadly right; it just doesn't look designed.
2. **It flattens a three-tier career into one list.** João is a Principal iOS Engineer on
   huge team products *and* the co-founder of a business that leads its market. The
   current page can't tell those apart.

## Decisions taken during brainstorming

- **"Professional" means visual craft**, not more words. Content is close to right.
- **Stats only where they help.** Real store figures for the Blip apps; no figures
  elsewhere.
- **App icons, no screenshots, no headshot.** Icons are the biggest visual lever
  available; screenshots of Blip's products are someone else's marketing assets.
- **Rounded, dated figures.** Coarse numbers stay true for years; a date makes them
  honest rather than stale. No live fetching — the Worker serves static files, and
  scraping Play proved brittle.
- **PadelTeams figures are commercially sensitive** — describe the platform and the
  co-founder role, no metrics.

## Structure

Five bands. The size difference between the first three does the arguing.

```
Hero            João Zão · Principal iOS Engineer, Blip.pt · co-founder, PadelTeams

At Blip         ▓ Paddy Power     64px icon · name · stats row · store links
                ▓ Sky Bet
                ▓ Betfair
                [team-products note] · [Store figures, July 2026]

PadelTeams      ▓ featured, own band — co-founded business, no metrics

Also            ▪ Capas  ▪ DontPushMe  ▪ Identifiers    compact, 40px / none

Experience      timeline (unchanged)
Contact
```

**Why PadelTeams gets its own band.** It is not a side project. It is a co-owned business
where João builds the technology *and* runs the company. Grouping it with Capas would
repeat the current site's mistake of flattening tiers. Grouping it with Blip would imply
it is employment. It is neither, so it stands alone.

**Why the install count is absent from PadelTeams.** 100+ installs reads badly only if you
assume a consumer app. It is a B2B platform sold to *clubs* — clubs are the customers, and
installs are close to meaningless as a measure. This is not hiding a number; it is
declining to publish an irrelevant one.

## Content

### Hero

> **João Zão**
> Principal iOS Engineer at Blip.pt · Co-founder of PadelTeams — Porto, Portugal
>
> Thirteen years building mobile apps. I started on Android, moved to iOS, and never put
> either down — these days mostly Swift, with Kotlin Multiplatform for the things that
> should only be written once.

The role line gains the co-founder half. Engineer *and* founder is a stronger position
than either alone, and the current hero hides it entirely.

### At Blip

| App | App Store | Google Play | Installs |
|---|---|---|---|
| Paddy Power | 4.7★ (260k) | 4.3★ (44k) | 1M+ |
| Sky Bet | 4.7★ (68k) | 4.5★ (50k) | 1M+ |
| Betfair | 4.7★ (110k) | 4.4★ (25k) | 1M+ |

Each: 64px icon, name, "Sports betting", the stats row, Play + App Store links.

Band footer keeps the existing honesty note — *"The betting apps are team products — I
work on them, I didn't build them alone."* — plus a quiet `Store figures, July 2026`.

### PadelTeams

> Co-founded with António Fernandes. Competition and club management for padel clubs —
> Portugal's leading platform for running competitions and managing a club. We build the
> technology and run the business.

Both store links. No metrics.

### Also

- **Capas** — Portuguese newspaper front pages, daily. A Rust scraper publishes the day's
  front pages as JSON; a Kotlin Multiplatform app reads it. One codebase, both platforms.
  Mine end to end. Play + source links.
- **DontPushMe** — test iOS push notifications without the backend. SwiftUI and TCA.
- **Identifiers** — identifiers for UI testing in Swift, a reflection-based approach.

## Stats treatment

Inline and labelled — text, not chart furniture. No tiles, no meters, no charts, so no
visualization system is needed:

> App Store **4.7**★ (260k) · Google Play **4.3**★ (44k) · 1M+ installs

**Both stores, always.** A bare "4.7★" would silently mean the App Store figure — the
higher one in all three cases. That is cherry-picking, and it is checkable in ten seconds.

**Installs are marked Play-only.** Apple does not publish install counts; an unqualified
"1M+ installs" would imply a cross-platform total that does not exist.

## Visual system

Same restraint, more craft. No framework, no JS, one stylesheet, light + dark as now.

- **Type scale** — replace the current four near-identical sizes with a deliberate scale.
- **Icons** — rounded squares, hairline border. Apple ships square artwork; the rounding
  is ours. `loading="lazy"`, explicit `width`/`height` to avoid layout shift, `alt` naming
  the app.
- **Accent** — used deliberately: the star glyph and links only.
- **Rhythm** — generous space *between* bands, tighter *within* them. This is what makes
  the bands read as bands.

## Assets

Five icons, **committed to the repo** at `assets/`. Do not hotlink: Apple rotates
`mzstatic.com` URLs and the images would vanish silently.

| File | Source |
|---|---|
| `paddypower.jpg` | iTunes API `artworkUrl512`, id 382030091 |
| `skybet.jpg` | iTunes API `artworkUrl512`, id 428237841 |
| `betfair.jpg` | iTunes API `artworkUrl512`, id 552024276 |
| `padelteams.jpg` | iTunes API `artworkUrl512`, id 6478245060 |
| `capas.png` | Play CDN (`play-lh.googleusercontent.com`), Android-only app |

~14KB each from Apple. Verified fetchable 2026-07-16.

## Out of scope

- Screenshots, headshot, blog, analytics.
- Live stat fetching.
- Any change to DNS, the Worker, or the deploy path — all working, all left alone.
- `www` redirect and HSTS — already settled separately.

## Verification

- Render at 900px and 320px, light and dark; no horizontal scroll at 320 (test in a real
  320px iframe — headless Chrome clamps windows to 500px minimum and will lie).
- All 11 links resolve to the right target (9 store links, 2 GitHub). The betting Play
  links need `gl=GB`; they 404 from Portugal without it.
- Icons load, are not hotlinked, and carry alt text.
- Every figure on the page traces to the table above. Nothing unsourced.
- Deploy is automatic on push to `main`; verify the live page byte-matches the repo.
