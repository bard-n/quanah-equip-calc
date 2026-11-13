# Gear Passive Planner

A single-file web tool that picks the best gear setup for your +1 passive-line goals.

You tell it what you want equipped (e.g. "Boss Damage ×3, Crit DMG ×1, Leap ×1") and list the items in your inventory with their passive lines. It finds the combination of items — at most one per equip slot — that meets every goal while equipping as few passive lines as possible.

## Features

- **Goal first** — set a target (0–3 lines) for each passive, then find the best setup in one click.
- **Optimal solver** — dynamic programming over the equip slots; guaranteed to meet the goal with the fewest equipped lines, if any combination can do it.
- **Closest setups** — when the goal can't be met, it ranks the top 10 setups that come closest and tells you exactly which passive lines you'd need to complete each one (and whether a free slot is left for them).
- **Clear diagnostics** — distinguishes "this goal is impossible even using every slot" from "each goal is possible alone, but not together", and shows the max achievable per passive.
- **Autosave** — your items and goals are stored in your browser (`localStorage`); nothing is sent anywhere.
- **Offline** — one self-contained HTML file, no build step, no dependencies.

## Usage

Open `gear-passive-planner.html` in any modern browser. That's it.

To share it as a website, enable GitHub Pages: **Settings → Pages → Deploy from branch → `main` → `/ (root)`**. The planner will then be live at `https://<your-username>.github.io/<repo-name>/`.

## Customizing for your game

Open the file and edit the config block at the top of the `<script>` section:

- `SLOTS` — the equip slot names (default: Hat, Top, Bottom, Shoes, Gloves, Cape).
- `PASSIVES` — the passive line names. **Append new ones at the end**: items and goals are saved as indices into this list, so inserting in the middle would scramble existing saved data.
- `SLOT_COLORS` — badge color per slot.

## How it works

- **Best setup:** memoized DFS over slots with state = per-passive counts clamped to their goals; minimizes total equipped lines subject to meeting every goal. A memo-size guard bails out gracefully if the goal set is too large to search.
- **Closest setups:** the same DP shape, but each state keeps its top 10 distinct suffixes ranked by (fewest missing lines, then fewest equipped lines).
