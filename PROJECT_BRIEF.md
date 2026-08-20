# Reading Room — Project Brief

A single-file web app that shows one piece of writing at a time and makes you sit
with it for a set period before you can move on. It pulls from a mix of sources —
current news, knowledge, and essays — and logs how you use it.

This document is the source of truth for **what we're trying to learn** and **how
we'll work**. It is not a product spec. Read the "Why this is a test" section before
adding anything.

---

## The hypothesis we're actually testing

The bet is **not** "a nicer content feed will do well." That space is crowded and
mostly commoditised. The specific, narrower bet is:

> If a reading app *enforces focus* on discovered content, will a real person
> (me) choose to dwell **voluntarily** — reading past the point they're allowed to
> stop — rather than reflexively skipping?

Everything about the app exists to answer that one question. The forced-focus timer
is the hypothesis. The sources, layout, and polish are just the conditions needed to
test it fairly.

---

## Why this is a test, not a product (yet)

Honest context, kept here so we don't drift into building a company by accident:

- Every obvious framing of "help people consume information better" is already
  occupied — algorithmic article feeds (Artifact, built by Instagram's founders,
  shut down for lack of market), "TikTok but educational" (Nibble, Deepstash),
  "Duolingo for knowledge" (Brilliant, Imprint, Headway). None broke out at scale.
- The recurring reason is demand, not execution: this kind of product is usually a
  **vitamin** (nice-to-have, guilt-driven, abandoned) rather than a **painkiller**
  (urgent, with a payoff you feel).
- The one open, interesting question is whether forced focus can create a payoff a
  user *feels* — a reason to come back that isn't just "I should read more."
- The only way to find that payoff is to build the smallest real version and live
  with it. Hence: a cheap prototype and a disciplined 10-day test, not a launch.

**If the test fails, that is a genuine result, cheaply bought.** Do not treat a
failing scorecard as a bug to engineer around.

---

## Success criteria — the 10-day scorecard

Locked in advance so they can't be quietly moved later. The app logs what's needed
to grade all three.

1. **Frequency** — I open it unprompted on **at least 7 of 10 days**. "Unprompted"
   is guaranteed by design: the app sends **zero notifications**, so every open counts.
2. **Voluntary pull** — in several sessions I stay **past the forced minimum**: the
   app let me stop and I kept going anyway. This is the key signal. Time spent *while
   forced* proves nothing — the timer manufactures it. Only **voluntary** minutes count.
3. **Payoff** — at least once I end a session, tap "worth it," and can name what I got.

Grading bar: pass = all three. A pretty demo that I stop opening by day 5 is a fail,
regardless of how good it looks.

---

## How it works (current architecture)

- **One static HTML file** (`index.html`), no build step, no backend. Hosted free on
  GitHub Pages. Add-to-home-screen makes it behave like an app.
- **Sources**, pulled in parallel and mixed:
  - *Current news* — The Guardian content API (returns full article body text).
  - *Knowledge* — Wikipedia random article extracts (CORS-friendly, no key).
  - *Ideas* — full-text essay RSS (Aeon, The Marginalian).
- **Variety** — items go into per-source buckets and a round-robin picker rotates
  between them, so consecutive cards come from different sources (all Guardian
  sections count as one group so news can't dominate).
- **The mechanic** — one card at a time. A focus line fills over a **forced dwell**
  period; the "Next" control is locked until it completes. Dwell is **scaled to the
  card's length** (≈200 wpm) and clamped to a **15s floor / 90s ceiling**, so you're
  never waiting on a finished card and never forced longer than the text.
- **Logging** — per-session open time, seconds per card, and a "worth it / meh"
  verdict, stored in `localStorage`. A scorecard view summarises days opened,
  voluntary minutes, and worth-it taps; CSV export for the full log.
- **No notifications**, deliberately.

---

## Known issues / limitations

- **Guardian "test" key** is shared and rate-limited. If news stops loading, swap in
  a free personal key (2-min signup) — the key and instructions are commented at the
  top of `index.html`.
- **Wikipedia extracts** are sometimes short, so those cards sit near the 15s floor.
- **Mixed source lengths** make pacing slightly uneven (a long essay next to a short
  extract). Acceptable for the test; revisit only if it drives me away.
- **Data is per-browser, per-URL.** Use the same phone + browser for all 10 days, and
  don't clear site data mid-test, or the scorecard resets.

---

## The plan

### Phase 0 — Sync (do first)
Get the repo to the current build, then push, so local / remote / live all match.

### Phase 1 — Get to "usable," then stop
"Usable" is a deliberately low bar, defined here so we know when to stop:
- Cards load reliably with real content across sources.
- The forced-focus mechanic works and pacing feels right.
- The scorecard records the three metrics correctly.

That's the whole definition of done for the prototype. **Do not add** genres,
playlists, accounts, recommendation, or styling beyond legibility. Those are things
you add to something that already passed a test — not before.

### Phase 2 — Run the 10 days
Use it for real. **No feature work during the test** — changing the app mid-test
invalidates the result. If something mildly annoys you, note it; don't fix it.

### Phase 3 — Read the scorecard honestly
- **Pass** (all three criteria) → now the interesting question is live: *what* made
  me come back, and can that payoff be sharpened for other people. That's the door to
  a real product, and where the strategy work restarts.
- **Fail** → equally valuable. The likely lesson is that forced focus alone isn't a
  payoff, and the idea needs a real destination (a deadline, a decision, a goal) or
  should be set down. Either way, cheaply learned.

---

## Working principles (guardrails)

- **The lesson comes from use, not polish.** Resist the urge to keep building.
- **Content-experience bugs are not mechanic problems.** Thin cards, pacing, and
  variety were all fixable details; through all of them the forced-focus mechanic
  held up. Keep that distinction — the mechanic is what's on trial.
- **Scope creep is the enemy of a clean test.** Every feature added before the 10
  days muddies the signal about whether the core idea works.
- **The unanswered question** worth keeping in view: what gives forced focus a payoff
  a person can *feel* — beyond relieving guilt about how they spend their time.

---

## Notes for Claude Code

- The entire app is `index.html`. Vanilla HTML/CSS/JS, no dependencies, no build.
- Deploy = commit and push to `main`; GitHub Pages redeploys in ~1 minute.
- Please show diffs before applying, and keep changes minimal and legible.
- Your job here is to help reach Phase 1 ("usable") efficiently — **not** to expand
  scope. If a request would add features beyond the definition of done above, flag it
  and ask whether it should wait until after the 10-day test.
