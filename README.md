# ACC Squamish — Static Site

A static HTML/CSS conversion of accsquamish.ca (a WordPress site).

## Pages included
- `index.html` — Home
- `volunteer.html` — Volunteer
- `resources.html` — Resources
- `courses.html` — Courses
- `donate.html` — Donate
- `landmarks.html` — Landmark Names (Sḵwx̱wú7mesh place names)
- `member-lookup.html` — Member Lookup
- `rental.html` — Rental

Open `index.html` in any browser, or upload the whole folder to any static
host (Netlify, GitHub Pages, S3, etc.) — no build step or server required.

## What was and wasn't carried over

**Carried over as static content:** all page text, headings, lists, the
landmarks table, and the navigation/footer structure.

**Not carried over (these needed a live server/backend on the original site):**
- The **Donate** page's embedded CanadaHelps payment form.
- The **Member Lookup** page's `[acc_membership_lookup]` plugin, which queries
  the Alpine Club of Canada's membership database.
- The **Volunteer** page's application form widget.
- The **Rental** page was empty/under-construction on the live site as well.

Each of those spots has a placeholder box with a link or contact email
instead, since a static site can't run server-side lookups or process
payments on its own. If you want any of these to actually work, they'd need
to be re-embedded as third-party widgets (CanadaHelps, Google Forms, etc.)
that don't require your own backend.

**Images:** the logo is now a local file too (`images/logo.png`, supplied
directly), so nothing on the site depends on the original accsquamish.ca
anymore. The hero photo is also local (see below). Fonts (Fraunces /
Public Sans) load from Google Fonts.

**Upcoming Events calendar:** the homepage now has an "Upcoming Events"
section (`index.html`, `#events`) that fetches and parses the club's ICS
feed directly in the browser:
`https://api.ezumee.services/group/ical/2409412b-a714-4644-8a38-d274283b6e1f`

This is a plain client-side `fetch()` + a small hand-rolled ICS parser (no
library needed) — it lists the next 8 upcoming events with date, time, and
location. Two caveats, since this is a static site with no backend:
- It only works if `api.ezumee.services` sends CORS headers allowing
  browser reads from other origins. If it doesn't, the fetch will fail and
  the section falls back to a short message pointing to Groups Place —
  test it once the site is hosted somewhere with a real URL (`file://`
  pages can't fetch cross-origin at all).
- Recurring events (`RRULE`) are shown at their first occurrence only; the
  parser doesn't expand recurrence rules.
- Each event card links to that event's own page, read from the `URL`
  property in the ICS feed (falling back to the first link found in
  `DESCRIPTION` if `URL` is absent). Events with no link anywhere render as
  plain (non-clickable) cards.

**Join / registration links:** every "Join" link (nav and footer) plus the
homepage's "sign up here" and "Register your ACC Membership online" links
now point to `https://app.alpineclubofcanada.ca/registration`.

If the CORS fallback triggers and you want it working properly, the fix is
almost always on the calendar API side (enabling `Access-Control-Allow-Origin`)
or routing the fetch through a small serverless proxy you control.

**Background photo:** the homepage hero uses a locally bundled, web-optimized
copy of the trip photo at `images/hero-trip.jpg` (resized to 1600px wide,
re-encoded as progressive JPEG at quality 68 — 257 KB, down from the
original 761 KB) with a dark gradient overlay for text contrast. If there
are other background photos you'd like on the Volunteer/Resources/Courses/
Donate pages, send them over and I'll optimize and wire those in the same
way.
