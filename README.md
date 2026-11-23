# mailgoose public site

The public website at [mailgoose.app](https://mailgoose.app): the landing page,
the policy pages, the alpha tester guide and, later, the knowledge base. Plain
HTML and CSS with no build step, published by GitHub Pages. By Gremlin
Technologies.

The app itself (API, workers, web app, CLI) lives in a separate, private
repository. Nothing here needs AWS or any secret.

## Layout

Everything under `site/` is published as-is; its folders are the site's URLs.

| Path | URL | Notes |
| --- | --- | --- |
| `site/index.html` | `/` | The landing page. |
| `site/pricing/` | `/pricing/` | The pricing page. Its numbers come from the app (`DefaultPacks`, `PLAN_PRICES`, the free tier); change them there first. |
| `site/price-comparison/` | `/price-comparison/` | mailgoose next to Mailchimp, Brevo, MailerLite and Kit. Our prices are worked out in the page from one block of constants; competitor figures are dated and each links to its source. Re-check them before changing the date. |
| `site/404.html` | any unknown path | |
| `site/terms/`, `site/privacy/`, `site/anti-spam/`, `site/dpa/` | `/terms/` and so on | **Generated** from the app's policy pages. Don't edit by hand. |
| `site/alpha/` | `/alpha/` | The alpha tester guide. Hand-written, unlisted (nothing links to it, `noindex`), linked from tester invites. It documents only what's live; move items out of "Coming soon" as they ship. |
| `site/assets/tokens.css` | | **Generated** from the app's design tokens (light only). Don't edit by hand. |
| `site/assets/site.css` | | The site's own layout, built on the tokens. |
| `site/assets/` | | Favicons, Inter (`fonts/`, with its licence), the social card, and `brand/` (downsized logos). |
| `site/docs/` | `/docs/` | **Reserved** for the knowledge base. Not started yet. |

### The knowledge base (later)

It goes under `site/docs/` and is served at `mailgoose.app/docs/`, so it shares
the site's tokens, stylesheet and header. Start it as plain HTML pages like the
rest of the site. If it outgrows hand-written HTML, a static generator can
build into `site/docs/` in the Pages workflow; that's a decision for then, not
now.

### Generated files

The design tokens and the policy pages come from the app repository, so the
site always says what the app says. Their generators live there. With both
repositories checked out side by side, run from the app repository's root:

```sh
node scripts/site/tokens.mjs     # writes ../mailgoose-public/site/assets/tokens.css
node scripts/site/policies.mjs   # writes ../mailgoose-public/site/<policy>/index.html
```

Then open a PR here with the result.

## Look and feel

The site follows the app's design system: white pages, near-black text, the
periwinkle blue only for links and buttons, hairline borders. It's light only:
no dark-mode styles, no `prefers-color-scheme`, no light-on-dark logo.

## Preview locally

```sh
cd site && python3 -m http.server 8000
```

Then open http://localhost:8000.

## Publishing

`.github/workflows/pages.yml` publishes `site/` on every push to `main` that
changes it, and can be run by hand from the Actions tab.

One-time setup, in this repository's Settings > Pages:

1. **Source:** GitHub Actions.
2. **Custom domain:** `mailgoose.app`. A custom domain can be on one
   repository at a time, so remove it from any other repository's Pages first.
3. **Enforce HTTPS:** tick it once it's clickable (it stays greyed out until
   GitHub has issued the certificate).

DNS for mailgoose.app is at GoDaddy and points at the GitHub account, not at a
repository, so moving the site between repositories of the same account needs
no DNS change. For reference, the records GitHub Pages needs:

- Four `A` records on `@`: `185.199.108.153`, `185.199.109.153`,
  `185.199.110.153`, `185.199.111.153`.
- Four `AAAA` records on `@`: `2606:50c0:8000::153`, `2606:50c0:8001::153`,
  `2606:50c0:8002::153`, `2606:50c0:8003::153`.
- `CNAME` `www` to `gremlin-technologies.github.io`.

Leave every other record alone (mail, Microsoft 365, SPF, DKIM, DMARC, and the
`NS` records that delegate `staging` to Route 53).

Check it's live:

```sh
dig +short mailgoose.app A                 # the four 185.199.x.153 addresses
curl -sI https://mailgoose.app | head -1   # HTTP/2 200
```
