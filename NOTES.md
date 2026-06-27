# cobbld site — working notes

Scratch notes and parked work for the cobbld marketing site. Agent/project guidance
lives in `CLAUDE.md`; this file is for decisions, parked features, and TODOs.

---

## Parked: interactive "build console" (terminal) — 2026-06-27

**Status:** built and working, then stashed. Not rendered anywhere on the live site.

**Why parked:** the console's copy and its `scope` problem-picker felt too broad /
generic. We want concrete numbers and real categories from actual business clients
before committing to the content. Holding it until then.

**Future:** it deserves its own dedicated page (not the home hero). Revisit once we
have real client data so the categories and recommendations can be specific.

**Where the code lives (all in `index.html`):**
- HTML markup: commented out at the end of the hero (search `STASHED: interactive
  build console`).
- CSS: the `.console-*` block (search `STASHED: build console (terminal) styles`).
- JS: the console IIFE (search `STASHED: build console (terminal) logic`). It is
  inert because it early-returns when `#console` is not in the DOM.

**To restore:** un-comment the hero markup block; it self-boots on scroll-in (CSS
and JS are already present). For its own page, lift all three pieces into the new
page instead.

**TODO before reviving:** get concrete numbers + real categories from clients, then
rewrite the copy below to be specific, not broad.

### Full copy + logic spec (editable while parked)

Markers: `$ …` = self-typing command line · `✓ …` = success line · `→ …` =
tangerine tagline · `[bar]` = progress bar · `( )` = blank line.

**Fixed UI strings**
```
Caption above console:   Build console · tap or type a command
Title bar label:         cobbld build console
Status (top right):      open for work
Prompt path:             ~/your-business
Prompt sign:             $
Input placeholder:       type a command…  try: scope
Version tag (boot):      v1.0
```

**Commands + aliases**
```
quick-win   <- quick-win, quickwin, quick, quick win, 1
core        <- core, 2
scale       <- scale, 3
scope       <- scope, estimate, diagnose, where, start-here, find-leak
start       <- start
help        <- help, ?, --help, commands
clear       <- clear, cls, reset
menu/back   <- menu, back
(a leading "cobbld " is stripped, so "cobbld scope" == "scope")
Default chips:  quick-win . core . scale . scope . help . clear
Unknown cmd ->  command not found: <x>  /  try: help
```

**Boot**
```
cobbld build console   v1.0
  a force multiplier for the underdog.
( )
$ cobbld init
  reading your business.........  ok
  finding what slows you down...  ok
  ready.
( )
  pick a command, or type one
```

**quick-win / core / scale**
```
$ cobbld quick-win
  websites . booking . small automations
  building...
[bar]
✓ ships fast. built to pay for itself.
→ stop missing calls. start booking jobs.

$ cobbld core
  internal tools . job tracking . quoting . dashboards
  assembling...
[bar]
✓ the system your business actually runs on.
→ one place where the work finally lives.

$ cobbld scale
  custom apps . integrations . bespoke builds
  compiling...
[bar]
✓ when off-the-shelf can't cut it.
→ built around how you work, not the other way.
```

**scope** (problem picker)
```
$ cobbld scope
  what's slowing you down? pick one
```
Chips + outputs — chip label | bottleneck line | recommended build | tier | detail:
```
no website yet          | no real website, or one that just sits there      | A website that works      | Quick win  | a clean, fast site that brings in work instead of sitting there.
hard to book / reach    | customers can't easily book or reach you          | Booking and enquiry flows | Quick win  | a simple way to book time or send an enquiry, minus the back and forth.
lives on paper          | it all lives on paper and in your head            | An internal tool          | Core build | one place that holds your customers, jobs and notes.
admin eats evenings     | admin and quoting eat your evenings               | Quoting and admin tools   | Core build | quotes, invoices and follow-ups handled, so they stop eating your nights.
off-the-shelf won't fit | off-the-shelf software doesn't fit how you work   | A custom app              | Scale      | when the tools you can buy don't fit, we build the exact one you need.
```
After picking:
```
$ cobbld scope <key>
  reading the bottleneck...  ok
  bottleneck: <bottleneck line>
  recommended first build:
    > <recommended build>
      <detail line>
[bar]
✓ <recommended build> . <tier>
```
…then chips: `→ start this build` · `scope again` · `‹ menu`

**start / help / clear / menu**
```
$ cobbld start
  opening a work order...
✓ let's build something that pays for itself.
→ taking you to the form
  (then scrolls to the contact form and focuses the name field)

$ cobbld help
  quick-win   ship fast. built to pay for itself.
  core        the system you run on.
  scale       bespoke builds + integrations.
  scope       find your first build.
  clear       wipe the log.

clear  -> wipes the log, shows: pick a command, or type one
menu   -> same line + resets to the default chips
```
