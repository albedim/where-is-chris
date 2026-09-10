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
