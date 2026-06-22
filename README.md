# Waste Traceability Dashboard

A static web dashboard for tracking industrial waste disposal through INSEE Ecocycle's end-to-end traceability process — from customer discharge through lab certification to final kiln consumption.

## Features

- **Ops Dashboard** — Full batch register with traceability flow visualization, status tracking, and step-by-step progress timeline
- **Customer View** — Secure company-based login to view batch reports and export PDF
- **Traceability Flow** — 8-step funnel showing batch counts at each stage (Discharge → Weight In → Waste Analysis → Load In → Weight Out → Pre-process → Finish Goods → Kiln Consumption)
- **Batch Detail** — Expandable row with vertical timeline, key metrics, and step-level detail popup
- **Filters** — Search by batch code, name, PO, or customer; filter by status and date range
- **Export** — PDF report generation for ops and customer views

## Tech Stack

- Vanilla HTML / CSS / JavaScript (no framework, no build step)
- Static hosting via GitHub Pages
- Font: Sukhumvit Set

## Data

- `sap_data.js` — SAP batch data (LOTS)
- `traceability_data.js` — Full traceability records (~30,000 batches)

## Live Site

[https://user16589026.github.io/Waste-Traceability](https://user16589026.github.io/Waste-Traceability)
