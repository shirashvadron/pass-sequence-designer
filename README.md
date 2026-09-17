# Pass Sequence Designer

A browser tool for designing and simulating football-pass sequences for a sequential
working-memory task. Lay players out on an 11-inch tablet screen, build the pass order,
and play it back at a set pass speed.

**Live site:** https://USERNAME.github.io/pass-sequence-designer/

## What it does

- **Drag players** anywhere on a 16:10 play area (the tablet's own aspect ratio).
- **Add or remove players**, up to 12.
- **Build the pass order** by tapping players on the pitch, in sequence.
- **Play it back** — the ball animates along the pass arcs.
- **Control the timing** — pass speed in screen-widths per second, and the wind-up pause
  before each release in milliseconds.
- **Read the geometry live** — mean, shortest and longest pass distance; the closest pair
  of players against a minimum-separation threshold; how many players are never touched
  and how many are touched more than once; total sequence duration.
- **Export** the layout and sequence as JSON, ready to hand to a developer.
- **Save setups** in your browser and reload them later.

## Why the readouts matter

Two of them carry design decisions rather than trivia.

**Closest two players.** Positions closer than `0.19` screen-widths make tap targets
overlap and the ball's path hard to follow. The panel flags any offending pair, and
**Spread out** nudges everyone apart until the threshold is met.

**Players never touched / touched twice+.** If a sequence touches every player exactly
once, the final position can be recovered by elimination rather than from memory. The
tool says so when that happens. Allowing a repeat, or leaving a player out, closes that
route.

**Mean pass distance** matters when comparing difficulty levels. Pass speed is specified
in screen-widths per second, so a longer pass takes longer; if mean pass distance drifts
between levels, total sequence duration drifts with it for reasons unrelated to how many
passes there are. Keeping the mean matched across levels keeps the comparison clean.

## Units

Positions are normalised to the screen — `x` as a fraction of width, `y` of height — so a
layout holds at any resolution. Distances are expressed in **screen-widths**: the distance
between two points divided by the screen width. Pass speed uses the same unit per second.

## Export format

```json
{
  "task": "19-football-passes",
  "screen": { "aspect": "16:10" },
  "playerCount": 8,
  "passSequenceLength": 7,
  "passSpeed": 0.15,
  "passSpeedUnit": "screen-widths/second",
  "windUpMs": 167,
  "meanPassDistance": 0.33,
  "players": [{ "id": "P1", "x": 0.14, "y": 0.24 }],
  "sequence": ["P2", "P1", "P3"]
}
```

## Presets

Three levels are built in, differing only in sequence length and player count:

| Preset | Passes | Players |
| --- | --- | --- |
| Short | 6 | 7 |
| Medium | 7 | 8 |
| Long | 8 | 9 |

All three draw their players from one shared nine-slot layout, so positions are identical
across levels and only the count changes.

## Running it

It's a single static file with no build step and no dependencies. Open `index.html` in a
browser, or serve the folder:

```
python3 -m http.server 8000
```

Fonts load from Google Fonts; everything else is inline. Saved setups live in the
browser's `localStorage` — they stay on that device and are never uploaded anywhere.

## Deploying to GitHub Pages

1. Push this folder to a repository.
2. **Settings → Pages → Build and deployment**, source **Deploy from a branch**,
   branch `main`, folder `/ (root)`.
3. The site appears at `https://USERNAME.github.io/pass-sequence-designer/` within a minute
   or two.

---

© 2026 Shira Shvadron. All rights reserved.
