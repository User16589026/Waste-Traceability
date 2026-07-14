<p align="center">
  <img src="INSEELOGO.png" alt="INSEE Ecocycle" height="56"/>
</p>

<h1 align="center">Waste Traceability Dashboard</h1>

<p align="center">
  End-to-end tracking for industrial waste disposal — from the moment a truck is weighed in,
  through lab certification and processing, to final kiln consumption.
</p>

<p align="center">
  <a href="https://user16589026.github.io/Waste-Traceability"><b>🔗 View the live dashboard</b></a>
</p>

---

## What this is

Every delivery of industrial waste handled by INSEE Ecocycle passes through seven tracked
stages before it's fully accounted for. This dashboard gives operations staff and customers
a single place to see, for any delivery or batch, exactly where it is in that process —
replacing what used to be a manual cross-reference across multiple SAP reports.

**Traceability Flow:** Weight In → Waste Analysis → Load In → Weight Out → Pre-process →
Finish Goods → Kiln Consumption

## Features

- **Ops Dashboard** — the full delivery register, with status breakdowns, monthly volume
  trends, and a searchable, filterable table of every delivery
- **Customer View** — a self-serve portal where a company can look up its own deliveries and
  export a report, without needing access to the internal Ops view
- **Delivery detail** — click into any delivery to see a step-by-step timeline with the
  underlying SAP data for each stage (weigh-in, lab results, load-in, blend batch, kiln)
- **Search & filters** — find a delivery by ID, batch, PO number, or customer; narrow by
  status or date range
- **Export** — download a filtered view as a PDF report or CSV

## How it's built

- Plain HTML / CSS / JavaScript — no framework, no build step, one file
- Hosted as a static site on GitHub Pages
- Data lives in [Supabase](https://supabase.com) (Postgres), refreshed from SAP export data
  by an internal pipeline that isn't part of this repo — this repo is just the frontend that
  reads it
- Read access is public but read-only (Row Level Security enforces this at the database level)

## Known limitations

- Some milestone dates (Pre-process, Finish Goods, Kiln Consumption) are recorded at the
  *batch* level rather than per delivery, since multiple deliveries can be combined into one
  batch before processing. Where a date would be misleading for a given delivery, it's shown
  as `—` instead of guessed.
- Cycle time (how long a delivery takes to reach processing) can only be calculated for
  deliveries where that batch-level date is known — currently a little under half of all
  deliveries.

## Updating the data or the site

This repo only contains the deployed frontend (`index.html`). Data refreshes happen upstream
by re-running the internal SAP → Supabase pipeline, which pushes updated records directly to
the database — nothing here needs to change for that. To change the site itself, edit and
commit `index.html` directly in this repo.
