# huzefa26.github.io

Personal portfolio and blog, built with [Hugo](https://gohugo.io) and the [yassi](https://github.com/yassi/hugo-theme-yassi) theme.

## Tech stack

- **Hugo** (v0.163.3, extended) — static site generator.
- **yassi** theme — included as a git submodule under `themes/yassi`.
- Project-level layout/CSS overrides for the homepage, posts list, and experience timeline.

## Local development

```bash
hugo server -D   # -D includes draft content
```

Site: http://localhost:1313/

## Production build

```bash
hugo --minify --gc
```

Output goes to `public/` (gitignored; never committed). GitHub Actions generates this on deploy.

## Deploy

Deploys are automatic via GitHub Actions on every push to `main`:

1. Workflow `.github/workflows/hugo.yml` builds the site with `--minify --gc`.
2. Deploys `public/` to GitHub Pages via `actions/deploy-pages`.

One-time setup (GitHub web UI): **Settings → Pages → Source: GitHub Actions**.

The site is served at `https://huzefa26.github.io/`.

## Repo structure

```
content/
  _index.md              Homepage bio (cover intro)
  posts/                 Blog posts (Markdown)
  projects/              Project pages (Markdown; `featured` flags carousel entries)
data/
  experience.toml        Experience & Education timeline (rendered on the homepage)
layouts/
  _default/home.html     Homepage override (cover + experience + featured projects)
  _default/list.html     Section-list override (projects grid + posts dividers list)
  partials/experience.html  Renders data/experience.toml as a timeline
  partials/head/css.html Loads theme CSS + assets/css/custom.css
assets/css/custom.css    Custom styles layered on top of the theme
static/
  cv.pdf                 CV PDF (linked from the navbar)
  images/                Avatar and project cover images
themes/yassi/            Theme source (git submodule; don't edit in place)
hugo.toml                Site config: identity, socials, menu, colors
```

## Editing content

- **New post:** drop a `.md` file in `content/posts/` with the front matter shown in existing posts (title, date, summary, tags, categories).
- **New project:** same, in `content/projects/`; set `featured = true` and `weight` to appear in the homepage carousel.
- **Experience & Education:** edit `data/experience.toml` — add `[[entries]]` blocks with `date`, `title`, `subtitle`, `bullets`, `type` ("experience" or "education").

## Config notes

- **Accent color:** `hugo.toml` has the theme default light blue active and three commented-out swatches (terracotta, teal, indigo). Uncomment one pair (`accentColor` + `accentColorDark`) to preview.
- **Socials:** `[params.social]` holds GitHub, Twitter/X, LinkedIn, and a `mailto:` email placeholder — fill in real values.
- **CV link:** the navbar "CV" entry points to `/cv.pdf`; replace `static/cv.pdf` with your real resume.

## Versioning

Releases are tagged SemVer (initial: `v0.1.0`). Bump patch for content fixes, minor for features.

## License

None currently (all rights reserved). Add one if you decide how open you want the content to be.