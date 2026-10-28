# Velerity Sales OS — CLAUDE.md

## Project Overview
**App:** Velerity Sales OS — GPU Server Sales Enablement + CRM
**Deploy file:** `C:\Users\15859\Desktop\VelerityDeploy\index.html`
**GitHub:** https://github.com/jstanton-21mbtc/velerity-sales-os (main branch)
**Netlify:** https://velerity-sales-os.netlify.app (auto-deploys from GitHub main)
**Architecture:** Single-file HTML app — all HTML/CSS/JS in `index.html`
**Based on:** Velerity Compute (https://www.veleritycompute.com), a Sprout company

---

## Business Context
Velerity sells certified-refurbished NVIDIA AI servers (A100, H100, H200, DGX systems).
Core sales differentiation:
- Lead time: ~5 weeks vs. 6–9 month OEM lead times
- Cost: significant discount vs. new; cheaper than cloud for sustained workloads
- Trust: NVIDIA DGX health workflow, 30-min stress test, serialized health report, warranty
- Compliance: formal export-control program; buyer screening before order closes

**Four competitive paths reps must navigate:**
1. New from OEM
2. Cloud GPU rental (on-demand or reserved)
3. Uncertified gray-market resale
4. Certified refurbished (Velerity)

---

## Tech Conventions

### Architecture
- Single `index.html` file — all HTML, CSS, and JavaScript inline
- No build step, no npm, no frameworks — vanilla JS + CSS
- Data stored in `localStorage` only
- Claude API called via `fetch` with header `anthropic-dangerous-direct-browser-access: true`
- No backend, no server, no database

### Storage Keys
- Data: `velerity_data_v1`
- Settings: `velerity_settings_v1`
- Anthropic API key: `velerity_anthropic_key_v1`

### Auth
- No authentication in current build (deferred)

### AI Models
- Rep Copilot, email drafts, account research: `claude-sonnet-4-6`
- Quick lookups, flashcard generation, spec queries: `claude-haiku-4-5-20251001`

### API Proxy
- Direct browser calls use header `anthropic-dangerous-direct-browser-access: true`
- Endpoint: `https://api.anthropic.com/v1/messages`

---

## Branding
- **Background:** `#000922` (dark navy)
- **Card/surface:** `#000513` (deep navy)
- **Accent/primary:** `#4AD0FA` (electric cyan)
- **Text:** `#FFFFFF` (primary), `#94A3B8` (muted)
- **Success:** `#22C55E` | **Warning:** `#F59E0B` | **Danger:** `#EF4444`
- **Font:** System sans-serif stack (`-apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif`)
- **Style:** Dark, modern, enterprise — high contrast, clean cards, generous whitespace
- **Logo text:** VELERITY (all caps, cyan accent)

---

## Domain Glossary

| Term | Definition |
|------|-----------|
| GPU generation | A100, H100, H200 — the three primary generations sold |
| SXM / PCIe | Form factors for GPU modules; SXM uses NVLink, higher BW; PCIe is standard slot |
| NVL | NVLink rack-scale interconnect (e.g., H100 NVL, H200 NVL) |
| HGX | NVIDIA's GPU baseboard standard used by OEM partners (Dell, HPE, etc.) |
| DGX | NVIDIA's own branded AI server (DGX A100, DGX H100, DGX H200) |
| MIG | Multi-Instance GPU — A100/H100 feature to partition one GPU into up to 7 isolated instances |
| NVLink | High-bandwidth GPU-to-GPU interconnect (600 GB/s on H100 SXM) |
| NVSwitch | Switch chip enabling all-to-all NVLink connectivity in 8-GPU nodes |
| TDP | Thermal Design Power — GPU power envelope (e.g., 400W A100, 700W H100 SXM) |
| Transformer Engine | H100/H200 hardware for FP8 mixed-precision training |
| ConnectX-6/7 | Mellanox/NVIDIA NICs; ConnectX-7 supports 400GbE/NDR InfiniBand |
| InfiniBand | Low-latency fabric for GPU cluster interconnect (HDR=200Gb, NDR=400Gb) |
| BlueField DPU | Data Processing Unit — offloads networking/storage from host CPU |
| Health report | Serialized diagnostic output from NVIDIA DGX health workflow + stress test |
| Certification status | Incoming / Testing / Certified / Reserved / Sold / Shipped |
| Condition grade | Grade A/B/C or equivalent scale for refurbished hardware condition |
| Export control | US BIS EAR regulations on advanced GPU exports; reps must escalate red flags |
| EAR / BIS | Export Administration Regulations / Bureau of Industry and Security |
| ICP | Ideal Customer Profile — industry segment definition for prospecting |
| Buying signal | Observable event indicating GPU procurement intent (funding, job posts, etc.) |
| Compliance screening | Mandatory deal step: buyer country + end-use verification before order closes |
| TCO | Total Cost of Ownership — own vs. cloud vs. OEM cost comparison |
| VRAM | Video RAM on GPU — primary constraint for model size and batch size |
| FP8/FP16/BF16/TF32/FP32/FP64 | Numerical precisions; lower = faster/less memory, higher = more accurate |
| Neocloud | Cloud provider built specifically on GPU infrastructure (CoreWeave, Lambda, etc.) |

---

## Pipeline Stages (CRM)
1. Prospect
2. Discovery
3. Workload Qualified
4. Buyer Compliance Screening ← **gated: must mark complete with reviewer + date**
5. Configuration & Quote
6. Documentation Review
7. PO / Contract
8. Fulfillment
9. Deployed / Expansion

**Rule:** A deal cannot advance from stage 4 to stage 5 without compliance screening marked complete.

---

## Data Model (localStorage)

### `velerity_data_v1` structure
```json
{
  "gpus": [],          // GPU reference specs
  "platforms": [],     // Server/platform catalog
  "accounts": [],
  "contacts": [],
  "deals": [],
  "activities": [],
  "tasks": [],
  "inventory": [],
  "suppliers": [],
  "quotes": [],
  "auditLog": [],
  "objections": [],
  "quizProgress": {}
}
```

### Key field patterns
- All records: `id` (uuid), `createdAt`, `updatedAt`
- Spec records: `sourceUrl`, `verified` (bool), `verifiedBy`, `verifiedAt`
- Deals: `complianceComplete` (bool), `complianceReviewer`, `complianceDate`
- Inventory: `serial`, `conditionGrade`, `certificationStatus`, `healthReportUrl`, `reservedDealId`

---

## Module Map

| Module | Nav Label | Phase |
|--------|-----------|-------|
| Server Academy | Academy | 1 |
| GPU Reference | Academy → GPUs | 1 |
| Platform Catalog | Academy → Platforms | 1 |
| Comparison Tool | Academy → Compare | 1 |
| Refurb Explainer | Academy → Certification | 1 |
| Export Control Primer | Academy → Compliance | 1 |
| CRM – Accounts | CRM → Accounts | 2 |
| CRM – Contacts | CRM → Contacts | 2 |
| CRM – Deals / Pipeline | CRM → Pipeline | 2 |
| CRM – Activities | CRM → Activities | 2 |
| Manager Dashboard | CRM → Dashboard | 2 |
| Inventory | Inventory | 3 |
| Quote Builder | Quotes | 3 |
| Workload-Fit Calculator | Tools → Workload | 4 |
| TCO Calculator | Tools → TCO | 4 |
| DC Readiness Checklist | Tools → DC Check | 4 |
| Objection Library | Tools → Objections | 4 |
| Training / Quizzes | Training | 4 |

---

## Build Phases

### Phase 1 — Academy + Shell
App shell (sidebar nav, topbar, content router), GPU reference catalog (A100/H100/H200 seeded specs), Platform catalog (DGX + OEM HGX systems seeded), Side-by-side comparison tool, Refurbishment/certification explainer, Export-control primer, Admin spec editor with verified flags.

### Phase 2 — CRM
Accounts, Contacts, Deals with all deal fields, 9-stage pipeline Kanban, Compliance gating logic + audit log, Activities (calls/emails/meetings/notes), Tasks/reminders, Manager dashboard (pipeline by stage/GPU type, forecast, win/loss).

### Phase 3 — Inventory + Quotes
Serialized inventory records, Certification workflow (status transitions), Reserve inventory against deals, Supplier records with qualification status, Quote builder (line items: servers + GPUs + warranty + shipping), Print/PDF export.

### Phase 4 — Tools + Training
Workload-fit + VRAM calculator, TCO calculator (own vs. cloud vs. OEM) with shareable summary, Data center readiness checklist, Objection-handling library (four-paths frame), Rep Copilot (Claude-powered Q&A + call prep + email drafts), Flashcards + quizzes + certification path with progress tracking.

---

## Seed Data
- Demo flag: `_demo: true` on all seeded records
- Label: "Sample Data" badge visible in UI when demo records are shown
- Seed: 3 accounts, 6 contacts, 5 deals (spread across pipeline stages), 8 inventory units, 2 suppliers, 3 quotes

## Non-Functional Requirements
- Responsive: works on mobile (field reps)
- Fast: no unnecessary re-renders; DOM manipulation only where needed
- Accessible: sufficient color contrast on dark theme
- Tests: VRAM calculator and TCO calculator logic in `<script>` with `console.assert` self-tests on load
- Audit log: every deal stage change and compliance step logged to `auditLog[]`
