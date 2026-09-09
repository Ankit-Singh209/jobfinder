# India Job & Internship Finder — static site

## Engineering discipline filter and direct platform search links

Two more additions that directly address the software-vs-hardware coverage
gap:

- **Engineering field dropdown** in the search form (Computer Science/IT,
  Electronics & VLSI, Mechanical, Civil, Electrical, Chemical, Aerospace,
  Industrial). Picking a non-software discipline biases the live-search
  ranking toward that field where possible, and flags the result as needing
  the platform links below.
- **"Search directly on India's job platforms" panel** — appears above every
  search's results with accurate, pre-built search links to **Naukri**,
  **Internshala**, **Indeed India**, and **LinkedIn Jobs**, using your
  description/discipline/city. This is the honest fix for disciplines the
  live search structurally can't reach: none of these four platforms expose
  a public API, so results aren't fetched into the page, but the links are
  correctly built and take you straight to real, live results on each site —
  which collectively cover far more of the Indian job market (all
  disciplines, all company sizes) than the ~68-company live source list ever
  could.

## What's new in this redesign

Full UI rebuild into an actual job-board layout, on top of the same live
data engine as before:

- **Tabbed navigation** — Find Jobs / Saved / Directories, like a real site
- **Dark mode**, remembered across visits (or follows your OS preference)
- **Saved jobs** — star any listing, kept in your browser via local storage,
  browsable in its own tab
- **Sort** — best match, newest first, or company A–Z
- **Company name filter** — narrow results by typing a company name
- **Pagination** — "Load more" instead of one long dump of results
- **Company logos** — pulled from Clearbit's public logo API where a domain
  can be guessed from the company name; falls back to a clean initial-letter
  badge if the logo doesn't load, so nothing ever shows a broken image
- **"New" badge** on listings posted in the last 10 days
- **List/Map toggle** moved into the results sidebar alongside other filters


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
- **Semiconductor, VLSI & Networking Hardware career pages** — a separate
  directory (Intel, Qualcomm, NVIDIA, AMD, Texas Instruments, Synopsys,
  Cadence, Micron, Broadcom, Tessolve) for the same reason: this hiring is
  concentrated at companies that don't use Greenhouse/Lever/Ashby, so a live
  search for "VLSI" or "network engineer" against the ~50 mostly-SaaS
  companies in the live source list will correctly find nothing — this
  directory is where that hiring actually is.
- **Matching requires genuine overlap, not just fuzzy similarity** — a job
  must share at least one of the *distinguishing* terms from your description
  (not generic filler like "team" or "experience") to be shown at all. This
  is what stops, e.g., a "network engineering" search from surfacing an
  unrelated "AI Implementation Engineer" posting just because both mention
  "engineering."
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

## Why some searches (VLSI, core networking) come back empty

The live source list is ~68 companies on Greenhouse, Lever, or Ashby — and
that's a real structural constraint, not a bug. Companies that expose those
public ATS APIs skew heavily toward software/SaaS/fintech, regardless of
which country they're headquartered in. Semiconductor and chip-design
employers (Qualcomm, Intel, NVIDIA, Texas Instruments, Broadcom, Synopsys,
Cadence) and core networking-hardware vendors (Cisco, Arista, Juniper) run
their own proprietary hiring systems (Workday, SuccessFactors, custom
portals) with no public feed to pull from — so a live search for "VLSI" or
"network engineer" correctly returns nothing from this source list, and the
app tells you why and points to the dedicated career-page directory instead
of silently failing or, worse, showing an unrelated result.

**Adding companies from more countries doesn't change this** — it broadens
general tech/software coverage, but VLSI and core-networking hiring will
still route to the directory regardless of which countries are represented
in the live sources, because it's about which ATS a company uses, not where
it's headquartered.

### Country coverage in the live sources

The ~68 live sources now include companies headquartered in the US, Europe
(Revolut, N26, GoCardless, Typeform, Celonis, Personio, Pleo), Canada
(Wealthsimple, Hootsuite, Clio), Japan (Mercari, SmartNews), and India
(Groww, Postman, Contentstack), all filtered down to their India-located
postings.

**China and Russia are deliberately not included.** Major employers in both
countries run their own hiring systems rather than Greenhouse/Lever/Ashby, so
there's no genuine public API source to pull from — adding entries for them
would mean guessing at tokens that don't exist, which would just add dead
weight rather than real coverage. If you know of a specific company from
either country with a genuine public Greenhouse/Lever/Ashby board, add it to
`SOURCES` yourself (see below) and it'll work the same way.

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
