# mviewmockups — Project Context

> **⚠️ PUBLIC REPO.** GitHub Pages serves this repo publicly. Never commit credentials,
> database hosts/IPs, member IDs, or customer emails here. Internal build details live in
> the **private** `mvest2019/mviewcrm` repo → `docs/dossier-implementation.md` (data-source
> map, pipeline spec, cost model) and `docs/dossier-pitch-ryan.docx` (executive pitch).

## What this repo is

Static UI mockups for **Mineral View** (mineralview.com), used to demo product concepts
internally before building them in the real Next.js portal. No build step, no framework —
plain HTML/CSS/JS files, one page per file.

**Live site:** https://manjushakarpe1982.github.io/mviewmockups/

## ⚠️ Deploy: TWO branches, always push BOTH

GitHub Pages serves the **`gh-pages`** branch, NOT `main`. Every content change must land on both:

```sh
git add <files> && git commit -m "..." && git push origin main
git checkout gh-pages && git checkout main -- <files> && git add -A
git commit -m "Sync to Pages branch" && git push origin gh-pages
git checkout main
```

(Alternative: repoint Pages to `main` in repo Settings → Pages — discussed, not done yet.)
Deploys go live ~1–2 minutes after push. `context.md` stays on `main` only (don't sync it
to gh-pages — no need to web-serve it).

## Page inventory

| Page | What it is |
|---|---|
| `index.html` | Hub listing all mockups |
| `registration.html` | Registration-flow redesign (homepage → 3-step signup → claim → MVestimate). Predates the dossier work. |
| `login.html` | Demo login. Credentials shown on page: `demo@mineralview.com` / `dossier2026`. Sets `sessionStorage.mv_demo=1`. |
| `dashboard.html` | Portal dashboard replica — **owner: Shannon Deckert** (10 leases). Ticker, owner bar, MVestimate card, "Your Mineral Report" entry card, activity strip, bell badge, lease table. |
| `dossier.html` | **"Your Mineral Report" — Shannon Deckert.** The flagship dossier mockup: summary tiles, Leaflet map, per-tract production sparklines, needs-a-look cards, value table, activity alerts, glossary. |
| `dashboard-ryan.html` | Dashboard — **owner: Ryan Cochran** (105 leases; top-10 table, portfolio-scale stats). |
| `dossier-ryan.html` | Ryan's report — portfolio-scale variant: county rollup table, top-6 producer sparklines, 49-pin map, aggregated value table. |

**Auth guard:** every post-login page redirects to `login.html` unless `sessionStorage.mv_demo === '1'`.
Bypass for direct links / screenshots: append `?demo=1`.

**Owner switcher:** `<select class="ownersel">` (report pages, top-right) and `<select class="switch">`
(dashboards, owner bar) navigate between the Shannon and Ryan versions of each page. Adding an owner =
create `dashboard-<name>.html` + `dossier-<name>.html` and add an `<option>` to all switchers.

## Design system (mirror of the live portal)

- **Font:** Lexend Deca (Google Fonts — fine here; the claude.ai artifact copy can't use CDNs)
- **Tokens:** navy `#0B1220`, teal `#00C4A7` / dark `#00A38B` / deep `#008F7A`, mint bg `#F4FBF9`,
  ink `#22313F`, muted `#7A8894`, orange (NEW badges) `#FF7A1A`, amber (warnings) `#B45309`
- **Charts:** single-hue teal line `#008F7A`, amber endpoint dot `#B45309` + last-value label —
  palette validated for contrast + color-blind safety (dataviz validator)
- **Maps:** Leaflet 1.9.4 + Esri basemaps (Streets default — same family as mineralview.com/map,
  which runs ArcGIS JS 4.28). Well pins = circleMarkers colored by status (`on` #008F7A /
  `off` #9AA7A4 / `warn` #E08A2E); recent filings = pulsing blue diamond divIcons (approximate,
  placed at RRC nearest-town); layer control + `showOnMap(id)` deep-links from alert cards.
- **Language rules:** plain Texas-owner English, no jargon; brand is always "Mineral View";
  every dollar figure labeled recorded-fact vs estimate ("not an appraisal, not an offer").

## Data provenance (all real, none mocked)

Report content is generated from **real Texas RRC public records + Mineral View's internal data**
via read-only queries (connection details in the private repo doc). Production is complete through
**April 2026** (operators file ~2–3 months behind — stated in every report header). Valuations are
whole-lease MVestimate × the owner's recorded decimal interest. Known data stories baked into the
mockups on purpose:

- Shannon: Morris lease has no state production record; Alvis maps to a different unit in a
  different county (map shows both locations) — the "needs a look" trust-builder pattern.
- Ryan: one lease excluded from totals due to a corrupt valuation-model doc (flagged to the data
  team); Dierking Unit has a real spring production dip (the "production watch" alert pattern).

To refresh numbers: extraction scripts live in the private CRM repo world (see
`dossier-implementation.md` §3.8) — regenerate the digest, then hand-update the HTML (or run the
future pipeline). Do not invent numbers directly in HTML.

## Changelog

**2026-07-07 — Dossier demo built (full day)**
- Added login → dashboard → "My Report" flow (`login/dashboard/dossier.html`), portal-brand styling
- Dossier v1 for Shannon Deckert: tiles, sparklines, value table, recorded-vs-estimate box
- Map section: real well coordinates, status colors, 1-mile rings, Alvis dual-location callout
- Activity alerts: real permit/completion/production events; "Show on map" links; alert pins with
  layer toggle; Esri basemap switcher (Streets/Satellite/Topo/OSM)
- Dashboard: "Your Mineral Report" entry card, activity strip, bell badge → report alerts
- Ryan Cochran's portfolio-scale dossier + dashboard (105 leases, county rollup, top producers)
- Working owner switcher across all pages; full owner names
- Centered content layout on wide screens
- Learned: Pages serves gh-pages (main-only pushes don't deploy)

**Earlier**
- Registration-flow mockup (`registration.html`) + hub page

## Open items

- [ ] Repoint Pages to `main` (or keep double-pushing)
- [ ] Production version: reuse the site's real ArcGIS map component instead of Leaflet;
      deep-link wells to mineralview.com/map (needs `?api=` URL-param support there)
- [ ] Consider re-anonymizing Shannon ("Shannon D.") if the link circulates beyond the team
- [ ] Next mockup candidates: free-vs-paid gated report view (locked sections), monthly-report
      archive tab, "report changed" email template
