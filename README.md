# badfer — Waterline

A water-reminder app: log what you drink, watch the glass fill, get nudged on a timer.
Single static page (`index.html`), served by nginx:alpine on port 8080, deployed by mitstack.

## Features

- **Vessel readout** — animated glass that fills to your daily goal, with tick marks and a "goal met" stamp.
- **One-tap pours** — glass / mug / bottle / flask presets, a custom amount, and undo. Keys `1`–`4` pour, `U` undoes.
- **Reminders** — countdown dial on a 20 min – 2 hr interval. Browser notification when backgrounded, chime + on-screen alert when not. Snooze 10 min, and an option to stop once the goal is met.
- **Goal calculator** — suggests a target from body weight (~33 ml/kg) plus an activity top-up.
- **Units** — millilitres or US fluid ounces, applied everywhere including the presets.
- **History** — 7-day bar chart against the goal line, current streak, best streak, 7-day average.
- **Today's log** — timestamped entries, individually removable. Rolls over at midnight.

State lives in `localStorage` on the device — no backend, no accounts, no network calls.

## Local run

```bash
docker build -t waterline . && docker run --rm -p 8080:8080 waterline
# http://localhost:8080
```
