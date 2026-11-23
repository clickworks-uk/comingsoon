# Prompt for Claude Cowork — verify and harden getclickworks.com

Paste everything below the line into Cowork, with Vercel open and
getclickworks.com reachable in a browser tab.

---

My holding page is live at **https://getclickworks.com**. I want you to check
it is actually secure, and to review the security settings in Vercel. Report
what you find before changing anything.

## Context

- **Vercel project:** `comingsoon`, id `prj_4KPjCJ4RQv2V7meJhMVN0DZp5xNA`
- **Team id:** `team_A9qQ1aNWCkCvt37S28lOx60i` (shown as *Whitehawk Agency's
  projects* or *clickworks-uk* depending on whether it has been renamed)
- **Repo:** `clickworks-uk/comingsoon`, branch `main`
- The page is a single static `index.html`. **No JavaScript, no forms, no
  third-party resources.**
- Security response headers are already set in `vercel.json` in the repo and
  deployed. I need you to confirm they are actually arriving in the browser.

## Part 1 — verify what is actually being served

Open **https://getclickworks.com** and check the response headers (browser
devtools → Network → the document request → Response Headers, or
https://securityheaders.com).

Tell me the actual value you see for each of these, or say "missing":

| Header | Expected |
|---|---|
| `content-security-policy` | `default-src 'none'; img-src 'self'; style-src 'self' 'unsafe-inline'; font-src 'self'; base-uri 'none'; form-action 'none'; frame-ancestors 'none'; upgrade-insecure-requests` |
| `strict-transport-security` | `max-age=31536000; includeSubDomains` |
| `x-content-type-options` | `nosniff` |
| `x-frame-options` | `DENY` |
| `referrer-policy` | `strict-origin-when-cross-origin` |
| `permissions-policy` | denies accelerometer, camera, geolocation, gyroscope, magnetometer, microphone, payment, usb |
| `cross-origin-opener-policy` | `same-origin` |

Also confirm:

- `https://getclickworks.com` redirects to `https://www.getclickworks.com`
- `http://getclickworks.com` redirects to `https://`
- The padlock shows a valid certificate, issued to the right domain
- The browser console shows **no CSP violation errors**. If it does, tell me
  exactly what was blocked — that means the policy is too tight for something
  on the page and I need to fix it in the repo, not in Vercel.
- The page renders properly: white "clickworks" logo on a dark blue and
  magenta gradient, "Coming soon", a sentence, four social icons

## Part 2 — review Vercel's security settings, do not change them yet

Go to the `comingsoon` project → **Settings**, and tell me the current state
of each. **Report first. Do not change anything in this part without asking
me.**

- **Deployment Protection** → Vercel Authentication. I expect
  *"Standard Protection"* / all except custom domains.
- **Firewall** → whether Attack Challenge Mode is on or off, and what the
  default managed rulesets are doing.
- Whether the plan offers a Web Application Firewall / custom rules, and
  whether any are configured.
- **Deployment Protection** → whether Protection Bypass for Automation is
  enabled, and whether any bypass tokens exist.

## Things I specifically do NOT want you to turn on

Read these before touching any toggle:

1. **Do not enable Password Protection or change Vercel Authentication to
   cover custom domains.** This is a public holding page — locking it would
   defeat the entire point. The current setting (protect previews, leave the
   custom domain public) is deliberate and correct.
2. **Do not leave Attack Challenge Mode on.** It makes every visitor pass a
   browser challenge. It is for when a site is actively under attack, not a
   standing setting. If it is currently on, tell me — I did not turn it on.
3. **Do not add HSTS preload** or submit the domain to the preload list. It
   is close to irreversible and would force HTTPS on every future subdomain
   of getclickworks.com. That is a decision for later, deliberately.
4. **Do not change the DNS at IONOS.** It is correct and verified. Nothing in
   this task needs a DNS change.
5. **Do not edit the repo.** If a header needs changing, tell me what and why
   and I will change it in `vercel.json` myself.

## Part 3 — tell me what you would change

After reporting, give me a short list of anything you think is genuinely
worth changing, with the reason, and wait for me to say yes. For a static
page with no JavaScript, no forms and no user data, I expect that list to be
short or empty — if you are proposing a lot, explain why each one matters
here specifically rather than in general.
