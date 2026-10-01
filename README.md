# Boston Carriage — 2026 Website Redesign (full site)

This is the developer handoff for the new bostoncarriage.com. It contains every page of the current site, rebuilt as static HTML with one shared stylesheet and a small vanilla-JS file. There is no framework or build step: open `index.html` in a browser and click through.

## Pages

| New file | Replaces | Notes |
|---|---|---|
| `index.html` | `/` (homepage) | Hero with quote form, Logan steps, fleet, rates, FAQ |
| `logan-airport-car-service-bos.html` | same URL | Logan guide: how pickup works, **terminal-by-terminal meeting points**, waiting-time rules, fares to/from BOS, FAQ |
| `reservations.html` | `reservations.aspx` | **Visual template only**; see "Reservations" below |
| `about.htm` | same URL | |
| `policy.htm` | same URL | Full terms, reorganized with a table of contents and key-facts summary |
| `wipo.html` | same URL | **Template**: paste the existing WIPO content in (marked `TODO(dev)`) |

The file names match the live URLs, so existing links and search rankings carry over. The one exception is `reservations.html`, which stands in for `reservations.aspx`.

## Launch checklist

1. **Reservations (`reservations.aspx`).** The live booking flow is an ASP.NET WebForms app with 3 steps: Trip info, Select a car, Confirm. `reservations.html` shows how each step should look.
   - Keep the existing postback logic and restyle the `.aspx` markup with these classes: `.stepper`, `.panel-card`, `.field`, `.car-opt`, `.summary`.
   - The inline `<script>` at the bottom only switches between the steps for demo purposes. Remove it.
   - Vehicle capacities and prices are shown as `[X]` / `[Price]` placeholders; they should come from the system.
   - The original luggage-limit image (`/images/luggage.jpg`) can go inside the note in step 1.
2. **Link to the booking app.** Every "Reserve / Book / See my price" link points to `reservations.html`. Search and replace that with `reservations.aspx` when deploying.
3. **Homepage quote form.** It sends a GET request to `reservations.html` with the fields `pickup`, `dropoff`, `date`, `time` and `vehicle`. Map these to the booking app's parameters so step 1 is pre-filled.
4. **WIPO page.** Paste the current content of `wipo.html` into the `.prose` block.
5. **Photos.** Fleet cards (homepage) and the About page have labelled placeholders, each with a `TODO(dev)` comment. The existing images in `/images/` can be reused.
6. **Content for the owner to confirm:**
   - The terminal directions on the Logan page come from the current Policy page. Check them against Logan's current layout.
   - The current About page title says "In Business for 30 years", but the copy everywhere says "since 1999" (27 years). The new site uses "since 1999".
   - The Policy text has been lightly edited, for typos and plain English only; the terms themselves are unchanged. Have the owner give it a quick read.
7. **Shared header and footer.** These are duplicated in every file. If the site runs on a CMS or server includes, pull them into partials.

## Files

```
*.html / *.htm       6 pages
styles.css           All styles. Tokens at top; homepage; "Inner pages" section; responsive rules
main.js              Mobile menu, one-open-at-a-time FAQ, footer year
assets/fonts/        Instrument Serif + Geist (woff2, self-hosted, SIL OFL)
assets/images/       Empty; drop photos here
design-reference/    Full-page screenshots of every page, desktop (1440) and mobile (390)
```

## Design tokens

| Token | Hex | Use |
|---|---|---|
| `--ink` | #111820 | Header, heroes, dark panels, primary text |
| `--stone` | #F3F0E8 | Page background |
| `--sand` | #E9E4D8 | Alternate sections, notes |
| `--brass` | #D4A259 | Accent fills; always use ink text on top |
| `--brass-text` | #7A5418 | Accent for small text on light backgrounds (WCAG AA) |
| `--text-2` | #3A4048 | Body copy on light |
| `--on-dark-2` | #C9D0D7 | Body copy on dark |

**Type.** *Instrument Serif* is for headlines, prices and big numbers. *Geist* (400/500/600) is for everything else.

**Radius.** 12 px for fields, 20–24 px for cards, 32 px for large panels, and fully rounded (pill) for buttons and chips.

## Responsive behavior

- **Above 1180 px:** full desktop layout.
- **900–1180 px:** the header text links collapse and the phone and Reserve buttons stay. Two-column sections stack, and the booking summary moves below the form.
- **Below 900 px:** the header shows a call button and a menu button. On the homepage the quote form comes before the stats. The rates tables become compact lists, and the Policy table of contents becomes a horizontal row of scrollable pills.

## Accessibility

- Every form field has a real label, and fieldsets are used for grouped choices.
- The FAQ uses native `<details>`, so it works without JS.
- Includes a skip link, breadcrumbs, `aria-current` on the active nav item and current step, and visible focus rings.
- Touch targets are at least 44 px, and text contrast meets WCAG AA.

Tested in Chromium at 1440, 1024 and 390 px on every page with no horizontal overflow. Please also check Safari/iOS and Firefox.
