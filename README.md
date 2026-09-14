# ACC Squamish — Static Site

A static HTML/CSS site for accsquamish.ca, based on
[github.com/accsquamish/accsquamish.ca](https://github.com/accsquamish/accsquamish.ca).

## Structure

```
index.html            Home
volunteer/index.html  Volunteer
resources/index.html  Resources
courses/index.html    Courses
donate/index.html     Donate
styles.css
images/
CNAME                 accsquamish.ca (for GitHub Pages custom domain)
```

Each inner page lives in its own folder as `index.html`, so links are clean
directory-style URLs with no `.html` extension: `/volunteer/`, `/resources/`,
`/courses/`, `/donate/`.

**Important:** all internal links (nav, footer, logo, stylesheet, hero
image) use root-relative paths (e.g. `/styles.css`, `/images/logo.png`,
`/volunteer/`). This is the right approach once the site is hosted at a
domain root (GitHub Pages with the included `CNAME`, Netlify, etc.), but it
means you can't just double-click `index.html` and browse around locally —
root-relative paths don't resolve under the `file://` protocol. To preview
locally, run a simple local server from this folder, e.g.:

```
python3 -m http.server 8000
```

then visit `http://localhost:8000/`.

The Landmark Names, Member Lookup, and Rental pages from earlier drafts have
been removed to match the current source repo.

## What was and wasn't carried over from the original WordPress site

**Carried over as static content:** all page text, headings, and lists.

**Now working as real, static-friendly embeds:**
- The **Volunteer** page's application form is a live HubSpot form
  (`hbspt.forms.create`, portal `20246688`) — no backend required, since
  HubSpot hosts the form itself.

**Still just a placeholder + contact info (needs a live server/backend on
the original site to work):**
- The **Donate** page's CanadaHelps payment form — replaced here with
  Stripe Payment Links for $5 / $10 / $25, plus an email link for other
  amounts or ways to give.

**Images:** the logo (`images/logo.png`) and hero photo (`images/hero-trip.jpg`)
are both local files now — nothing depends on the original accsquamish.ca.
Fonts (Fraunces / Public Sans) load from Google Fonts.

**Background photo:** the homepage hero uses a web-optimized copy of the
trip photo (resized to 1600px wide, progressive JPEG quality 68 — 257 KB,
down from the original 761 KB) with a dark gradient overlay for text
contrast.

**Upcoming Events calendar:** the homepage has an "Upcoming Events" section
(`index.html`, `#events`) that fetches and parses the club's ICS feed
directly in the browser:
`https://api.ezumee.services/group/ical/2409412b-a714-4644-8a38-d274283b6e1f`

This is a plain client-side `fetch()` + a small hand-rolled ICS parser (no
library needed) — it lists the next 8 upcoming events with date, time, and
location, each linking to its own event page (read from the `URL` property,
falling back to a link found in `DESCRIPTION`). Caveats:
- It only works if `api.ezumee.services` sends CORS headers allowing
  browser reads from other origins. If not, it falls back to a short
  message pointing to Groups Place.
- Recurring events (`RRULE`) are shown at their first occurrence only.

**Join / registration links:** every "Join" link (nav and footer) plus the
homepage's "sign up here" and "Register your ACC Membership online" links
point to `https://app.alpineclubofcanada.ca/registration`.
