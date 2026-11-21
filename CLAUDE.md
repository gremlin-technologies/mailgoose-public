# mailgoose public site

The public website at mailgoose.app, by Gremlin Technologies. Plain HTML and
CSS under `site/`, no build step, published to GitHub Pages by
`.github/workflows/pages.yml`. See README.md for the layout.

## Working rules

- **Branches and PRs.** Work on a branch and open a pull request into `main`.
  Never push to `main`, and never merge your own PR.
- **Reviewer merges.** A separate reviewer agent may merge a PR that only
  touches `site/`, `README.md` or `CLAUDE.md` once it does what its
  description says and follows these rules. PRs that touch `.github/` are
  reviewed the same way but left for the owner to merge.
- **No personal names.** Don't name the owner anywhere in this repository:
  pages, code, commit messages, PRs or issues. Write "Gremlin Technologies",
  or "the owner" where a role reads better.
- **No secrets.** Nothing here needs a key or token; keep it that way.
- **Generated files.** `site/assets/tokens.css` and the four policy pages
  (`site/terms/`, `site/privacy/`, `site/anti-spam/`, `site/dpa/`) are written
  by scripts in the app repository. Don't edit them by hand; change the app
  and regenerate them.
- **Light only.** No dark-mode styles, `prefers-color-scheme`, `data-theme`
  or light-text logo on the site.
- **Unlisted pages stay unlisted.** `site/alpha/` carries `noindex, nofollow`
  and nothing links to it.
- **The knowledge base goes under `site/docs/`**, served at
  mailgoose.app/docs/. It isn't started; don't add a framework for it without
  the owner's say.
