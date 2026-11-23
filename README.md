# clickworks — coming soon

The holding page for Clickworks, deployed on Vercel as a static site.

## What this is

One static `index.html` with its CSS inline, plus the wordmark and the Inter
variable font in `assets/`. There is no build step, no `package.json` and no
dependencies — deliberately. A holding page is the only thing online while it
is up, so it has nothing that can rot or fail to build.

The typeface, the logo and the background gradient are lifted verbatim from
the main site (`clickworks-website`), so the two look like the same brand.

## Editing it

Change the copy in `index.html` and push. Vercel redeploys on every push to
the default branch; there is nothing to build.

The things most likely to change:

| What | Where |
|---|---|
| "Coming soon" | `.lede` paragraph |
| The sentence under it | `.blurb` paragraph |
| Social links and handle | the `.socials` block |

## Adding a contact address

There is no email on the page, because Clickworks has not published one yet.
When there is one, add it above the socials:

```html
<a class="contact" href="mailto:hello@example.com">hello@example.com</a>
```

## Vercel

Framework preset: **Other**. No build command, no output directory, no
environment variables. Vercel serves the repo root as static files.

## Font licence

Inter is used under the SIL Open Font License — see
`assets/Inter-LICENSE.txt`.

## Security headers

`vercel.json` sets the response headers for every path. Because this page has
**no JavaScript, no forms and no third-party resources**, the Content Security
Policy can be far stricter than a normal site's: `default-src 'none'` with
only `img-src`, `style-src` and `font-src` opened up to `'self'`, and
`script-src` left denied by the default.

`style-src` keeps `'unsafe-inline'` because the CSS lives in a `<style>` block
in `index.html`. That allowance cannot be exploited here — the page is wholly
static with no scripts and no user input, so there is no way to inject a
style. Moving the CSS to its own file would let that be dropped, at the cost
of an extra render-blocking request.

If you ever add JavaScript, an embed, analytics or a form to this page, the
CSP will block it and the page will look broken. Widen the policy in
`vercel.json` in the same commit.

`Strict-Transport-Security` is set with `includeSubDomains` but **without**
`preload`. Preloading is close to irreversible and would force HTTPS on every
future subdomain of `getclickworks.com`; worth doing deliberately later, not
by default.
