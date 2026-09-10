# Spot the Face

A "find the person in the crowd, as fast as you can" mini game. Pure HTML/CSS/JS
+ JSON — no build step, no backend. Deploys straight to GitHub Pages.

## How it works

- `index.html` — the whole app (login, home, game, results, leaderboard).
- `images/grid.jpg` — the big crowd photo everyone searches.
- `images/overlay.png` — the "target" image that gets placed on top of one
  face in the grid at the start of each round. **This is currently a
  placeholder crosshair marker — replace it with an actual photo/crop of
  the person you want players to find.** Keep it roughly square.
- `data/cells.json` — a machine-generated map of every face's bounding box
  in `grid.jpg` (`minX`/`maxX`/`minY`/`maxY`, in the image's natural pixel
  coordinates). It was generated automatically from your uploaded image by
  detecting the grid lines (16×16 grid, ~38.25px per cell). Each round, one
  cell is picked at random and the overlay image is placed exactly over it.
- `data/config.json` — the target's name, the on-screen instruction text
  ("Tap on the photo of ___"), and image paths. Edit this to change the copy
  without touching any code.
- `data/leaderboard.json` — a *seed* leaderboard, shipped with the site.

## About the leaderboard (read this)

GitHub Pages is static hosting — there's no server to write to, so a truly
shared, always-up-to-date leaderboard across every visitor isn't possible
with HTML + JSON alone. Here's what this build actually does:

- Every play is saved to the browser's `localStorage` on that device, and
  combined with whatever is in `data/leaderboard.json` for display.
- That means: on one device, a player's own history persists across visits
  (the "Hey `<name>`, play now" screen). Scores from *other* devices won't
  show up automatically, since nothing gets written back to the repo.
- On the leaderboard screen there's an **"Export leaderboard.json"**
  button. It downloads the current combined list (seed file + everything
  played on that device) as a `.json` file. If you want a shared,
  slowly-updated leaderboard, periodically export it from your own device
  and commit it over `data/leaderboard.json` — new visitors will see it as
  their starting seed.
- If you want a *real* live shared leaderboard, you'd need a tiny backend
  (e.g. a free Cloudflare Worker / Firebase / a GitHub Action that appends
  to a JSON file via a form submission). That's outside "only HTML and
  JSON" so it's not included here, but the code is structured so it's a
  single fetch/post call away if you add one later — see `finishGame()` in
  `index.html`.

## Editing the grid / target

- To point at a different face, just change which `data/cells.json` entry
  gets used — or regenerate the file if you use a different photo (grid
  size, columns/rows, and cell size all live at the top of `cells.json`).
- To restrict which faces can be picked (e.g. only the ones that actually
  look plausible with your overlay), trim the `cells` array down to just
  the ones you want as possible spawn points.

## Running locally

Any static file server works, e.g.:

```
cd site
python3 -m http.server 8000
```

Then open `http://localhost:8000`.

## Deploying to GitHub Pages

1. Push this folder's contents to a repo (root, or a `/docs` folder).
2. In the repo: **Settings → Pages → Deploy from a branch**, pick the
   branch and folder you used.
3. Done — GitHub serves `index.html`, `data/`, and `images/` as-is.

The included `.nojekyll` file stops GitHub Pages' Jekyll processor from
ignoring folders starting with an underscore, etc. — harmless to keep even
though nothing here needs it.
