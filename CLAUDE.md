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
├── cobbld98.html     # "cobbld 98" retro-desktop easter egg (bundled React, see below)
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

### `cobbld98.html` (the "cobbld 98" easter egg)

A novelty retro Windows-98 desktop (Start menu, Paint, a terminal, Minesweeper, a
demo Contact app, etc.). It is **not** hand-built like the rest of the site: it is
a **compiled, self-extracting React bundle** (one ~0.5 MB file whose assets are
base64-inlined and rebuilt into blob URLs at load) that **pulls React/ReactDOM/
Babel from unpkg.com at runtime**, so it needs a network connection and does NOT
render offline via `file://`. Do not try to make it follow the vanilla
single-file convention; treat it as a dropped-in artifact.

The app's source IS readable inside the `<script type="__bundler/template">` JSON
blob (authored JSX/JS that Babel compiles in-browser, not minified), but it is
stored JSON-escaped (quotes as `\"`, closing-tag slashes as `/`), so edit it
with a small script, not by hand. A few cobbld customizations are layered on top of
the exported bundle and must be **re-applied if the bundle is ever re-exported**:

1. Outer `<head>`: title, description, favicon, `theme-color`, `noindex`.
2. A post-swap re-brand snippet in the loader (the bundle replaces `<html>` on
   boot, so the tab title and icon are re-applied there).
3. In the template: a `goHome` handler plus a "Back to cobbld.com" entry in the
   Start menu and on the shutdown screen (this is the way back to the main site).
4. Icon-renaming is removed. The desktop-icon rename `<input>` (inside the
   `<sc-for as="i">` loop, opened by the context-menu "Rename" action) bound its
   `onfocus`/`onblur`/`onkeydown` handlers in a way this `<sc-for>` + `<sc-if>`
   runtime mis-scopes, so focusing/blurring the field threw visible
   `ReferenceError`s and the rename never committed (broken in the original
   export; reproduces only in a real browser, not headless, so it cannot be
   verified locally). The rename `<input>`, its per-item bindings in
   `desktopIconObj`, and the "Rename" context-menu action were removed; the
   `startRename`/`onRenameCommit` methods remain defined but unused. If you
   re-export and want renaming back, it needs a real fix, not just re-adding it.

It is **noindex and intentionally left out of `sitemap.xml`** (same as `work.html`).
The entry point is a "cobbld 98" link in the footer "Explore" column of both
`index.html` and `work.html`, opening in the same tab. Visitors return via the
desktop itself (Start menu, "Back to cobbld.com", also offered on the shutdown
screen).

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
  a custom `.cursor-dot` (disabled on touch / reduced-motion), and full
  `@media (prefers-reduced-motion: reduce)` handling. The home hero's kinetic marquee
  was removed; an interactive terminal-style **build console** was built then parked
  (inert; markup commented out in `index.html`) for a future dedicated page. See NOTES.md.
- **Responsive:** must stay clean from 380px up with no horizontal overflow.

## Page structure

**`index.html`** (section ids / anchors):
`#nav` + `#mobileMenu` → `#hero` (`#top`, kinetic headline, sub + CTAs) →
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
