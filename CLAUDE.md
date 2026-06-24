# CLAUDE.md

Guidance for working in the **cobbld** marketing site repo.

## What this is

cobbld is a small studio that builds custom software and automation for **small
businesses**, using AI where it helps. Positioning line: *"A force multiplier for
the underdog."* This repo is the marketing site: a static, dependency-free set of
hand-built HTML pages (no framework, no build step).

## Web3Forms lead capture

The home page contact form posts to **Web3Forms**. A live access key (registered
to **hello@cobbld.com**) is set on the hidden `name="access_key"` input in
[`index.html`](index.html), so valid submissions are emailed as leads.

The submit handler in `index.html` gates on the key: if the value still contains
`REPLACE` it uses the mailto fallback (mailto hello@cobbld.com); otherwise it
POSTs to `data-endpoint` (`https://api.web3forms.com/submit`) and shows the
success state, falling back to mailto only if that request fails. To rotate the
key, swap the `access_key` value. To switch providers (e.g. Formspree), change
`data-endpoint` on the `<form id="contactForm">` and adjust the hidden fields.

## Repository layout

```
.
├── index.html        # Home page (the live site root)
├── work.html         # Work page: example/demo systems cobbld builds
├── favicon.svg       # Logo mark (three offset rounded squares = "cobble together")
├── CLAUDE.md         # This file
├── .gitignore        # Ignores local .claude/ tooling
├── mockups/          # Design-exploration ARCHIVE (not the live site)
│   ├── index.html    #   Chooser page comparing the three directions
│   ├── forge.html    #   Direction A: warm editorial (Fraunces)
│   ├── circuit.html  #   Direction B: technical / live-ops (Archivo)
│   └── street.html   #   Direction C: bold kinetic (Unbounded) — the one chosen
└── .claude/          # Local tooling, git-ignored (launch.json for the preview server)
```

The **live site is `index.html` + `work.html` at the repo root.** `mockups/` is a
record of the design directions that were explored; `mockups/street.html` is an
older snapshot of the chosen design and has since diverged from `index.html` (do
not treat it as canonical).

## Design system ("Street")

Both live pages share one system. Keep them consistent.

- **Fonts (Google Fonts):** Unbounded (display), Onest (body), DM Mono (labels).
  Do NOT introduce other fonts, especially the common AI defaults (Inter,
  Bricolage Grotesque, Hanken Grotesk, Space Grotesk, JetBrains Mono, Geist,
  Playfair, Instrument Serif, Caveat).
- **Palette (CSS variables in `:root`):** `--bone #f2efe9`, `--ink #11100d`,
  `--tang #ff5b1e` (electric tangerine), `--acid #f2e205`.
- **Build style:** each page is ONE self-contained file with a single inline
  `<style>` and a single inline `<script>`. Vanilla JS only. Only external
  resource is Google Fonts. Must work via `file://` and via a static server.
- **Shared behaviors** (kept in sync across both pages): fixed nav that hides on
  scroll-down + a mobile hamburger menu, `.reveal` IntersectionObserver scroll-ins,
  a custom `.cursor-dot` (disabled on touch / reduced-motion), a kinetic marquee,
  and full `@media (prefers-reduced-motion: reduce)` handling.
- **Responsive:** must stay clean from 380px up with no horizontal overflow.

## Page structure

**`index.html`** (section ids / anchors):
`#nav` + `#mobileMenu` → `#hero` (`#top`, kinetic headline + ticker) →
`#build` ("a few things we build", horizontal-scroll gallery with a stacked mobile
fallback) → `#leak` (interactive "find the leak" picker) → `#thesis` (tangerine
band) → `#how` (four steps + payback banner) → `#cta` (contact form) → `footer`.

**`work.html`** (a "what we build" overview, not a portfolio): `#nav` +
`#mobileMenu` → hero ("Start small. Grow into custom software.") → three `.tier`
color-block sections (Quick wins / Core builds / Scale) that mirror the home
`#build` content, each with an abstract decorative SVG (no fabricated product
demos or client data) → honest note → `#cta` → `footer`. Its nav/footer and
shared JS mirror `index.html`; update both together.

## Copy & content rules (important)

The user has set firm guidelines. Re-read [memory or] enforce these:

- **No fabricated statistics.** No invented metrics, ROI, "$ recovered",
  percentages, or hours-saved. There are no real clients yet. Sample data inside a
  clearly-labeled product mockup (e.g. the "Sample quote") is fine; claimed
  outcomes are not.
- **No real-sounding client names** presented as customers. Product UIs are
  examples/demos, not case studies.
- **Audience is "small businesses."** Do not use "operator(s)". Do not lock into
  trade verticals (HVAC, plumbing, etc.). Describe capabilities, not niches.
- **cobbld is not an AI company.** Lead with the business outcome; mention AI lightly
  as the method. Keep AI references sparse.
- **Services are examples, not an exhaustive menu.** Keep the "if it slows you down,
  we can probably build it" framing.
- **No em dashes** anywhere in copy. Plain, human, confident voice; no hype.

## Running / previewing

It is a static site. Either open the HTML files directly in a browser, or serve
the folder:

```
python -m http.server 4178
# then open http://localhost:4178/index.html
```

(`.claude/launch.json` configures the same static server for the Claude Code
preview tool; it is git-ignored.)
