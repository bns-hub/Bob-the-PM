# Project notes — gacha timeline (bannerschedule.netlify.app)

Add new entries at the top, do not delete old ones.

> [!note] This repository is public
> Nothing here is sensitive. The project is a public single-page timeline of gacha game banner schedules, deployed openly.

---

## 2026-09-22 Weekly pipeline closed, live site brought current

**What we already know, confirmed, not guessed:**

- **Where everything lives.** Repository `bns-hub/Projects`, folder `gacha-timeline/`. The whole app is one file, `gacha-timeline/index.html`, around 2,700 lines, no build step and no framework. `netlify.toml` points Netlify's base directory at that folder.
- **How it deploys.** Netlify auto-deploys to **bannerschedule.netlify.app** on every push to `main`. There is no deploy command to run and no manual step. A second Netlify project, `chronoschedule`, also builds from this repository and also posts checks on pull requests. Only `bannerschedule` is the gacha timeline.
- **The weekly checker works and should be left alone.** `.github/workflows/gacha-timeline-check.yml` runs every Monday 01:00 UTC, which is 09:00 Singapore time. It runs `gacha-timeline/scripts/check-sources.mjs`, which loads 24 official and community pages in a headless browser, hashes the banner-relevant text, compares each hash against `gacha-timeline/.audit-state.json`, and opens a **draft pull request** when any hash differs. It deliberately uses no API key and no reasoning model.
- **The gap this phase closed.** The Action only ever flagged drift. It never edited `index.html`, and nobody was closing that loop, so the live site sat stale while draft pull requests piled up. That was the one real problem, and it is a human-in-the-loop problem, not a code bug.
- **The in-browser checker is gone, permanently.** `index.html` used to carry a "⟳ Check all banner info" button that fetched those same sources from the visitor's browser. It could not work: Reddit, X, YouTube and the HoYoverse pages all block cross-origin browser requests. Removed in pull request #14 along with the whole subsystem behind it, roughly 950 lines. **Do not rebuild it.** The headless Action is the replacement and is not subject to those restrictions.
- **What replaced it in the page.** The `scheduleSources` array stays, rendered as a plain static list of links behind the "Show official sources" toggle, each with game, label, URL, note, and an "unofficial community source" badge where it applies.
- **The honesty convention, and it matters more than the data.** Anything not backed by an official notice uses category `tentative` and the exact status string `Unofficial / community-estimated · verify against official post`, which renders in the purple provisional style. Estimated dates say so in the row subtitle *and* in the detail panel. Where a date genuinely does not exist anywhere, the bar end is called a display placeholder in plain words rather than being passed off as a real date. **Match this wording for any future entry.** No invented dates or numbers, ever.
- **Data shape.** `const games = [...]`, one object per game, each with `patches` (bars across the top) and `rows` (each row holds `items` of category `banner`, `tentative`, `selector`, `event` or `maintenance`). Patch `status:"pending"` is the only patch status that changes styling.
- **Mobile jump-to-present was already fixed** before this phase, by a hand-written `animateScrollTo` using `requestAnimationFrame`. Native `scrollTo({behavior:"smooth"})` does not reliably animate on real phones, especially just after a finger has touched the same scrollable element. Do not revert it to the native call.
- **How to test changes.** Chromium and Playwright are available in the cloud sessions. Load `index.html` from `file://` at 390×844 and 1400×900, check for zero console and page errors, confirm the expected rows render, and click a row to check the detail panel. That caught nothing this time, which is the point.
- **Content applied this phase**, researched and verified by an earlier session on 22 Sep 2026 against Gematsu, HostedGG, dotesports and Prydwen, and not re-researched:
  - Honkai: Star Rail Version 4.6 "Dance With the Beast Before Moonrise", official launch 28 Sep 2026, patch end ~10 Nov a community estimate. Two community-reported phases.
  - Reverse: 1999 Version 3.8 "The Temporal Scale", official launch 24 Sep 2026, broadcast rewards confirmed, two banner phases with estimated dates.
  - Chaos Zero Nightmare: **no official Season 5 exists** as of 22 Sep 2026. Two community-tracker reruns added, Narja + Gaya and Sereniel + Peko.
- **Stale draft pull request #13 is closed.** It only ever changed `.audit-state.json`. That snapshot was folded into pull request #14 instead, so the Monday run compares against the 20 Sep hashes and will not re-flag `hsr-hoyolab`, `hsr-hoyolab-official`, `hsr-news` and `re-steam` a second time. **This is the pattern to reuse** whenever a weekly draft pull request is superseded: take its `.audit-state.json` with the real change, then close it.

**What we are choosing to leave open, or unsure of, for now:**

- **The Pearl question.** The verified brief lists the Star Rail phases as "Pearl + Evanescia" then "Pearl + Mortenax Blade", with Pearl leading both. That was reproduced exactly as briefed rather than silently corrected, and flagged on pull request #14. It may be a typo for a different Phase II lead. Check it against the official Version 4.6 banner post.
- Every community-estimated date added this phase. All are labelled, none is official, and all need replacing once official notices appear. The Sereniel rerun end date in particular is a pure display placeholder.
- Whether the weekly draft pull requests should stay a manual review step forever, or whether a session should be scheduled each Monday to triage them. Left manual for now, because the whole design deliberately keeps a human between "a source changed" and "the timeline says something new".
- Whether the `chronoschedule` Netlify project is still wanted, or is a leftover that just adds noise to pull request checks.

**The one goal for this phase:**

- Get bannerschedule.netlify.app current and correctly labelled, and make the weekly Action the only checking mechanism.

**Anything the next session should NOT re-ask, because it is already settled:**

- Do not rebuild the in-page "Check all banner info" button or any browser-side source fetching. It was removed on purpose and the reason is cross-origin blocking, not a bug worth fixing.
- Do not change the weekly workflow or `check-sources.mjs`. That part works.
- Do not replace `animateScrollTo` with native smooth scrolling.
- Do not add a build step, a framework or a package manager to `gacha-timeline/`. It is one static HTML file by design.
- Do not invent or tidy up a date to make a bar look neater. Label it instead, using the wording above.
- Do not delete past banner rows. The data block says to retain history, and the timeline shows 90 days back.
