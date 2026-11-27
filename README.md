# eInvoices.ae website

The public pages of [eInvoices.ae](https://www.einvoices.ae), an extension for Manager.io accounting
software that sends and receives e-invoices under the UAE eInvoicing mandate. A static site built with
[Eleventy](https://www.11ty.dev/) and deployed to GitHub Pages. It is set up the same way as
`www.aussiebankfeeds.com`.

## Commands

```
npm install      # once
npm start        # local preview at http://localhost:8080, rebuilds on change
npm run build    # writes the site to _site/
```

Pushing to `main` builds and deploys the site (`.github/workflows/pages.yml`).

## Where things are

| Path | What it holds |
|---|---|
| `src/index.njk` | Wording of the English home page (`/`), as front matter |
| `src/ar/index.njk` | Wording of the Arabic home page (`/ar/`), as front matter |
| `src/_includes/home.njk` | The home page both of those fill in: hero, how it works, features, deadlines, pricing and FAQ |
| `src/404.njk` | Not-found page, in both languages because GitHub Pages serves one file for every missing address |
| `src/_includes/layout.njk` | Page shell: head, header and footer |
| `src/_data/strings.json` | Header and footer wording in each language, and the address of the app (`app`) the buttons open |
| `src/_data/site.json` | Address of this site (`url`), the company behind it (`company`), the support email, and the monthly price in AED (`price`) |
| `public/` | Files copied to the site as they are: stylesheet, icons, `robots.txt` |
| `eleventy.config.js` | Build configuration |

## Two languages

The site is in English and Arabic. A page says which with `lang` in its front matter (`en` when left
out); `lang: ar` makes the page right-to-left. `alternate` is the address of the same page in the
other language, used for the language link in the header and for `hreflang`.

Change the wording in `src/index.njk` and `src/ar/index.njk` together. `public/styles.css` uses
logical properties (`padding-inline-start`, not `padding-left`) wherever a side matters, so one
stylesheet serves both directions.

The price is `price` in `src/_data/site.json`: write `{price}` in home page wording and it is filled
in. The one exception is `description` in the front matter of the two home pages, where the number is
typed out.

## Mandate dates

The mandate dates on the home page (`deadlines` and `faq` in the front matter of both home pages)
come from UAE Ministerial Decision No. 244 of 2025 as amended. They have been moved once already, so
check them when the Ministry of Finance announces changes.

## Custom domain

The site is deployed by a GitHub Actions workflow, so the domain is set in the repository's
**Settings → Pages → Custom domain** (`www.einvoices.ae`), not by a `CNAME` file. DNS needs a `CNAME`
record for `www` pointing at `alfabasehq.github.io`, and the bare domain `einvoices.ae` pointing at
GitHub Pages' `A` records, which makes GitHub redirect it to `www`. Until that is done the site is
served from `alfabasehq.github.io/www.einvoices.ae/`, which the build allows for (see `--pathprefix` in
the workflow).
