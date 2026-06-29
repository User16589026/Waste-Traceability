# Waste Traceability Dashboard

A web dashboard for tracking industrial waste disposal through INSEE Ecocycle's end-to-end traceability process — from customer discharge through lab certification to final kiln consumption.

## Live Site

[https://user16589026.github.io/Waste-Traceability](https://user16589026.github.io/Waste-Traceability)

## Features

- **Ops Dashboard** — Full delivery register with traceability flow visualization, status tracking, and step-by-step progress timeline
- **Customer View** — Company-based login to view delivery reports filtered by customer, with PDF export
- **Traceability Flow** — 7-step funnel (Weight In → Waste Analysis → Load In → Weight Out → Pre-process → Finish Goods → Kiln Consumption)
- **Batch Detail Modal** — Vertical timeline, key metrics, and step-level detail popup with lazy-loaded SAP data
- **Filters** — Search by delivery, batch, PO, or customer; filter by status, waste type, and date range
- **Export** — PDF report generation for ops and customer views

## Tech Stack

- Vanilla HTML / CSS / JavaScript (no framework, no build step)
- Static hosting via GitHub Pages
- Backend: Supabase (PostgreSQL) — ~30,010 delivery records
- Font: Sukhumvit Set

## Data Pipeline (SAP → Dashboard)

| Script | Description |
|---|---|
| `build_stepbystep.py` | Joins SAP reports 01+02+03+04+05 into SAP_StepByStep.xlsx |
| `generate_sap_data.py` | Converts Excel → sap_data.js (local mode) |
| `upload_to_supabase.py` | Uploads delivery records to Supabase (requires Service Role Key) |

### SAP Report Join Order
1. 01 ZSDLER040 + 02 ZQMXXR018 → join on Delivery
2. step1 + 03 MB51 (agg by Delivery) → join on Delivery
3. 04 ZQMXXE004 + 05 MB51 Kiln (agg by BlendBatch) → join on BlendBatch → groupby OrigBatch
4. step2 + step3_agg → join on Batch = OrigBatch

### 7-Step Logic
| Step | Name | Source |
|---|---|---|
| 1 | Weight In | 01 — Weight In Date |
| 2 | Waste Analysis | 02 — Judgement |
| 3 | Load In | 03 — ReceivedQty > 0 |
| 4 | Weight Out | 01 — Weight Out Date |
| 5 | Pre-process | 04 — IsBlended == True |
| 6 | Finish Goods | 04 — BlendBatches has value |
| 7 | Kiln Consumption | 04+05 — IsConsumed == True |

## Supabase Setup

- **Project ID:** `rscwnlfndzdzpqnhoxmy` (ap-northeast-1)
- **Table:** `public.deliveries`
- **RLS:** SELECT-only for anon key (safe to embed in frontend)
- **Writes:** Require Service Role Key (Supabase Dashboard → Settings → API)
- Detail columns (`wi`, `wa`, `li`, `wo`) are JSONB, lazy-loaded per delivery on modal open

## Known Limitations

- **Step dates (Pre-process, Finish Goods, Kiln)** are batch-level, not delivery-level. Dates earlier than the delivery's Weight-in date are hidden (`—`) because they belong to earlier trips in the same batch.
- **Cycle Time** = first blend date ≥ WI date minus WI date. Only ~44% of deliveries have a calculable cycle time.
- **Last Updated** was removed from customer report (was batch-level kiln date, misleading for later trips).

## Deployment

1. Edit `Main/index.html`
2. Copy to `deploy/index.html`
3. `git add index.html && git commit && git push` (git repo is in `deploy/`)
