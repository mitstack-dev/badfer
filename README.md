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

## CI

`mitstack build` (in the companion `badfer-ci` repo) builds and pushes the image; the
`mitstack-delivery` workflow here only *follows* that delivery by polling the mitstack
dispatcher.

The generated `mitstack-delivery.yml` ships with `MITSTACK_DELIVERY_URL_BASE` empty, which
makes it poll a hostless URL and fail after the full 30-minute timeout on every push. The
base URL now comes from a repo variable instead:

```bash
gh variable set MITSTACK_DELIVERY_URL_BASE -R mitstack-dev/badfer \
  --body https://lywrjyiga5dxv2lfuz4jbe5txi0wypki.lambda-url.eu-central-1.on.aws
```

That variable is repo config, so it survives scaffold regeneration — but the three blocks
marked `PATCH n/3` in `.github/workflows/mitstack-delivery.yml` do not. If mitstack
regenerates that file, re-apply them. The workflow now exits with a warning (not a failure)
when the dispatcher is unset or unreachable, so a mitstack-side outage can no longer turn a
good build red.

## Local run

```bash
docker build -t waterline . && docker run --rm -p 8080:8080 waterline
# http://localhost:8080
```
