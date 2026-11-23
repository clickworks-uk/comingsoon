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
