# India Job & Internship Finder — static site

A single-file tool for students and professionals job-hunting in India. Publish
it directly on GitHub Pages — no backend, no build step, no API key required
to get started.

## What it does

- **Live MNC listings** — fetches directly from ~50 companies' own applicant
  tracking systems (Greenhouse, Lever, Ashby) — the same data their official
  careers pages show — and keeps only postings located in India (or explicitly
  remote, if you opt in).
- **Internships stay internships** — student/fresher mode applies a hard
  category filter (title must match internship/trainee/campus-hire/fresher
  patterns, and explicitly excludes senior/staff/lead/manager-level titles)
  *before* ranking. Professional mode does the opposite: internship-labeled
  postings are excluded outright. This is a strict filter, not just a ranking
  preference — it fixes the case where an internship search could otherwise
  surface an experienced-level opening just because the keywords matched.
- **Matches to your description** — a small TF-IDF + cosine-similarity engine
  (vanilla JS, runs in your browser) ranks the filtered listings.
- **Big MNC & IT-major career pages** — a directory of employers (Google,
  Microsoft, Amazon, IBM, Accenture, TCS, Infosys, Wipro, Cognizant, HCLTech,
  Capgemini, Deloitte, EY, JPMorganChase, SAP, Cisco) whose hiring systems
  don't expose a public API, so they're linked directly instead of fetched.
- **Staffing agency directory** — a static, hand-checked list of established
  agencies that place candidates into MNCs in India (Randstad, TeamLease,
  Quess Corp, CIEL HR, ABC Consultants, Michael Page, Hays, NLB Services, and
  more), each linking to their official site.
- **Map view** — after a search, click "Show on map" to see matched openings
  plotted by city (Leaflet + OpenStreetMap, no API key needed), with a
  "jump to a city" search box to pan/zoom to any matched location worldwide.
- **College/university field** — feeds into "find HR on LinkedIn" and "campus
  hiring page" search links for each result (see note on contacts below).
- **Optional Adzuna integration** — add your own free Adzuna API key (from
  developer.adzuna.com) in "More options" for broader India job-market
  coverage beyond the curated MNC list. Stored only in your browser.
- **CSV export** of your matched results.

## On finding "HR contacts"

This tool deliberately does **not** scrape or list individual people's private
contact details (personal emails, phone numbers). That's a privacy and
platform-terms issue regardless of intent. Instead, for every job it gives you:

1. The **official Apply link**, pulled live from the company's own hiring
   system — the genuine channel.
2. A **pre-built LinkedIn people-search link** (company + "Talent Acquisition
   / Recruiter / HR / Campus" + your college, if given) so you can find and
   message the right person yourself, through LinkedIn's own search.
3. A **pre-built Google search link** for the company's campus-hiring page.

## Publish it on GitHub Pages

1. Create a GitHub repository (or use an existing one).
2. Add `index.html` to the repo root (or a `/docs` folder).
3. Push to GitHub.
4. Go to **Settings → Pages**. Under "Build and deployment", set **Source** to
   **"Deploy from a branch"**, then pick the branch (usually `main`) and
   folder (`/ (root)` or `/docs`). Click Save.
5. Wait 1–2 minutes, then refresh Settings → Pages — it'll show
   "Your site is live at `https://yourusername.github.io/your-repo/`".

This is a single static file with no build step, so **"Deploy from a
branch" is the right source** — not "GitHub Actions". If a
`.github/workflows/*.yml` file exists in the repo (added manually or by a
template), delete or disable it; a Pages Actions workflow isn't needed here
and can conflict with branch-based deployment.

### Troubleshooting

**`Error: Timeout reached, aborting! / Canceling Pages deployment...`**
This is a known, recurring issue on GitHub's own infrastructure with the
`actions/deploy-pages` action — it shows up across many unrelated repos, even
ones with a single tiny file, and isn't caused by anything in this project.
Fixes, in order of reliability:
1. Switch **Settings → Pages → Source** to **"Deploy from a branch"** (see
   above). This skips the Actions deploy step entirely and is the simplest
   fix for a no-build static site like this one.
2. If you want to keep the Actions-based flow: go to the **Actions** tab →
   the failed run → **"Re-run failed jobs."** Multiple reports confirm a
   retry or a fresh commit clears it — it's a transient runner-allocation
   issue, not a configuration problem.
3. Check [githubstatus.com](https://www.githubstatus.com) for an active
   Pages incident.

**Page loads but shows a blank/broken layout**
Hard-refresh (Ctrl/Cmd+Shift+R) — Google Fonts, Leaflet's CSS/JS, and the map
tiles all load from CDNs on first paint, and a cached partial load can look
broken. If it persists, open the browser console (F12) for the actual error.

**Map doesn't show any pins after a search**
The map only plots jobs whose location matched a city in `CITY_COORDS` in
`index.html`. Jobs with a location string outside that list still show up in
the results list, just without a map marker. Add more cities to
`CITY_COORDS` if needed.

**No jobs show up at all**
Some browsers/networks block third-party API calls. Open the browser console
(F12) and check for CORS or network errors — if Greenhouse/Lever/Ashby are
reachable, at least some sources should return results even if a few
individual company tokens are stale.

## Extending it

- **Add a company**: append `{ name, ats, token }` to the `SOURCES` array near
  the top of the `<script>` block. `ats` is `"greenhouse"`, `"lever"`, or
  `"ashby"` — find a company's token from its careers page URL
  (`boards.greenhouse.io/{token}`, `jobs.lever.co/{token}`, or
  `jobs.ashbyhq.com/{token}`).
- **Add a staffing agency**: append `{ name, url, blurb }` to the `AGENCIES`
  array.
- **Change the India location filter**: edit `INDIA_KEYWORDS`.

## Notes

- If a company's board token is wrong or the ATS API changes, that source
  just silently returns no jobs — it won't break the page or show wrong data.
- RemoteOK (used only if you check "include global remote roles") sometimes
  blocks direct browser requests; the page falls back to a public CORS relay
  automatically.
- This is a hobby/personal tool. Always verify a listing on the company's own
  site before applying, and never pay anyone for a job offer or interview.
