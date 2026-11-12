# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Spendly — a Flask expense tracker built as a step-by-step teaching project. Only the public pages exist so far; most backend features are deliberately stubbed for later steps.

## Commands

```
python -m venv venv && venv\Scripts\activate   # venv/ is gitignored
pip install -r requirements.txt
python app.py                                  # dev server, debug on, http://127.0.0.1:5001 (not 5000)
pytest                                         # pytest + pytest-flask are installed; no tests exist yet
pytest path/to/test_file.py::test_name         # single test
```

No linter or build step is configured.

## Architecture

- `app.py` is the single Flask module: all routes live here and render Jinja templates. There is no blueprint structure.
- **Implemented routes:** `/` (landing), `/register`, `/login`, `/terms`, `/privacy` (static pages only; the register/login forms have no POST handling yet).
- **Placeholder routes** (`/logout`, `/profile`, `/expenses/add`, `/expenses/<id>/edit`, `/expenses/<id>/delete`) return plain strings labelled with the step that will implement them ("coming in Step N"). Replace the string when implementing; keep the route names, since templates link via `url_for(...)`.
- `database/db.py` is a comment-only stub describing the intended `get_db()` (SQLite, `row_factory`, foreign keys on), `init_db()` (`CREATE TABLE IF NOT EXISTS`) and `seed_db()`. The DB file `expense_tracker.db` is gitignored.
- `templates/base.html` holds the navbar and footer (including the Terms/Privacy links) and defines `title`, `head`, `content` and `scripts` blocks. Every page extends it. Footer links belong in `base.html`, not in individual pages.
- `static/css/style.css` is the only stylesheet, themed through `:root` variables (`--ink`, `--paper*`, `--accent*`, fonts DM Serif Display / DM Sans). Reuse those variables. Sections are separated by banner comments (Hero, Dashboard mock, Video modal, Buttons, Features, Auth, Footer, Legal pages, Responsive).
- `.btn-primary` / `.btn-ghost` are shared by the landing, CTA and auth pages. The hero restyles them through `.hero-actions` overrides, so change the hero buttons there rather than editing the shared rules.
- Legal pages (`terms.html`, `privacy.html`) share the `.legal-*` classes; a new legal page should reuse them.
- `static/js/main.js` is an empty stub loaded on every page. Page-specific JS goes inline in the page's `{% block scripts %}` (e.g. the video modal in `landing.html`). The project uses no JS libraries or frameworks; keep it vanilla.

## Placeholders to replace before launch

- `landing.html`: the YouTube video ID in the `data-src` of `#video-modal-iframe`.
- `privacy.html`: the contact email `support@spendly.example`.
