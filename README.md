# Reading Room

A single-file web app that shows one piece of writing at a time and makes you sit with it before you can move on. Built to test one specific hypothesis: if a reading app enforces focus on discovered content, will you choose to stay past the point you're allowed to stop?

**[Open the app →](https://sd-mp.github.io/reading-room/)**

---

## How it works

- One card at a time — a focus timer fills before you can move on
- Dwell time is scaled to the card's length (~200 wpm), clamped to a 15s floor and 90s ceiling
- Long articles are paginated — swipe right to read more
- Sources rotate by category so you get variety across types
- Sessions are logged locally — opens, time per card, and a "worth it / meh" verdict
- No notifications, no accounts, no algorithm

## Categories & sources

On first launch a picker lets you choose which categories appear in your feed. Tap ⚙ in the header to change them at any time.

| Category | Sources |
|---|---|
| News | Guardian, Letters from an American, Popular Information |
| Sport | Guardian Sport |
| Technology | Guardian Technology, Ars Technica, MIT Technology Review, Platformer |
| Business | Guardian Business, Fast Company, No Mercy No Malice |
| Ideas & Essays | The Marginalian, Astral Codex Ten, Overcoming Bias |
| Knowledge | Wikipedia, The Conversation, ProPublica |

## Running it

No build step. Open `index.html` in a browser or visit the GitHub Pages URL above.

To use as a mobile app: open in Safari → Share → Add to Home Screen.

## Guardian API key

The app uses the Guardian's free `test` key by default, which is rate-limited. If news stops loading, get a free personal key (2-min signup, no card) at [open-platform.theguardian.com](https://open-platform.theguardian.com/access/) and replace `GUARDIAN_KEY` at the top of `index.html`.

## Structure

| File | Purpose |
|------|---------|
| `index.html` | The entire app — HTML, CSS, and JS |
| `PROJECT_BRIEF.md` | Hypothesis, success criteria, and working principles |
| `NEXT_STEPS.md` | Features to consider after the 10-day test |
