# Tracking plan

Mixpanel tracking for the portfolio at https://bhagat7.github.io (project token set in `index.html`, US data residency).
Event names use Title Case "Object Verb". Property names use snake_case.

## Events

| Event | Fires when | Properties |
|---|---|---|
| `$mp_web_page_view` (automatic) | Every page load, via `track_pageview: true` in `mixpanel.init`. | Set by Mixpanel (URL, referrer, etc.) |
| `Resume Downloaded` | Visitor clicks the "Download resume" button. | `location`: `"hero"` |
| `Contact Clicked` | Visitor clicks any email (`mailto:`) or LinkedIn link. | `channel`: `"email"` or `"linkedin"`<br>`location`: `"hero"` or `"footer"` |
| `Case Study Viewed` | At least 50% of an `<article class="case">` is visible. Once per case study per page load. | `case_study`: the article's `<h3>` text |
| `Section Viewed` | A section's top edge scrolls into the top 80% of the screen. Once per section per page load, for `#work`, `#experience`, `#skills`, `#education`, `#contact`. | `section`: the element id, e.g. `"skills"` |

## How `location` is decided

A link inside the `<footer>` is `"footer"`. Any other link is `"hero"` (the only contact links outside the footer are in the header).

## Excluding your own visits

- Open the site with `?notrack` (e.g. `https://bhagat7.github.io/?notrack`) to opt that browser out. The choice is stored in localStorage and lasts until you opt back in.
- Open it with `?track` to opt back in.
- Browsers with Do Not Track switched on are also not tracked (Mixpanel default).
- Opt-out is per browser and per device, so repeat it on each one you use.

## Notes

- "Once per page load" means a reload counts again. Count distinct users, not events, to measure reach.
- `Section Viewed` uses a lenient rule on purpose: `#work` is several screens tall, so a 50% rule would never fire for it.
- On a 390px-wide phone the first case study ("Product analytics dashboard", with its two charts) is about 1,660px tall, so 50% visible needs roughly 830px of visible screen. On shorter phone screens it can never fire, so expect `Case Study Viewed` to under-count for that study on mobile.
- If Mixpanel is blocked by an ad blocker, no events are sent and the page behaves normally. Expect some under-counting for visitors with blockers.
