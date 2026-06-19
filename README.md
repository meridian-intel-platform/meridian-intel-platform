# Meridian Intel

> Where business, tech, and community converge.

**Meridian Intel** is a commercial-grade Retrieval-Augmented Generation (RAG) platform built for organizations that need zero-hallucination document intelligence across pharma, legal, finance, research, and beyond.

This repository contains the **frontend** of the Meridian Intel platform. The backend is proprietary and deployed separately.

---

## Live Platform

🔗 **[meridianintel.app](https://meridianintel.app)** *(coming soon)*

---

## What It Does

Meridian Intel lets you upload any document — a pharmaceutical Certificate of Quality, a legal contract, a financial SEC filing, an IT runbook — and query it with natural language. The platform retrieves only what's in your document, never hallucinating beyond it.

```
Upload document → Chunk → Embed → Store
Query → Retrieve → Generate → Audit
```

Every response comes with:
- Source chunk citations (page, section, character position)
- Faithfulness score (how grounded the answer is)
- Context recall score (did retrieval surface the right content)
- PII scrubbing confirmation

---

## Five Intelligence Tiers

| Tier | Price | Chunk Size | Results | Key Features |
|---|---|---|---|---|
| **Ephemeral** | Free | 600 tokens | 3 | General field, text responses |
| **Segment** | $29/mo | 512 tokens | 5 | 5 domains, markdown responses |
| **Lattice** | $59/mo | 384 tokens | 6 | 10 domains, semantic cache, cross-doc |
| **Vector** | $89/mo | 256 tokens | 8 | All 14+ domains, PDF highlights, eval scores |
| **Enclave** | Enterprise | 200 tokens | 10 | Per-user isolation, audit trail, SLA |

---

## Supported Domains

Meridian Intel handles 14+ field types with automatic normalization. Unknown fields fall back to `general` without failing.

**Science & Research**
`Pharmaceuticals & Biotech` · `Legal & Compliance` · `Finance & Accounting` · `Medical & Clinical` · `Engineering & Manufacturing`

**Technology**
`Software & DevOps` · `Cybersecurity` · `Data Science & ML`

**Business**
`HR & People Ops` · `Marketing & Brand` · `Operations & Supply Chain`

**Academic**
`Academic Research` · `Public Policy & Gov`

---

## Platform Features

### Zero-Hallucination Core
If retrieval returns no relevant chunks, the LLM call is blocked entirely. The platform never generates an answer beyond what your document contains.

### Hybrid Retrieval
Dual retrieval using ChromaDB (semantic vector search) and SQLite FTS5 (keyword search), fused via Reciprocal Rank Fusion (RRF) for maximum recall precision.

### PII Scrubbing
Nine-category PII scrubbing runs before any data reaches the LLM. Field-aware defaults protect pharmaceutical identifiers (lot numbers, catalog IDs) from incorrect redaction.

| Category | Default |
|---|---|
| Email | ON |
| SSN | ON |
| Date of Birth | ON |
| Address | ON |
| Names | ON |
| Phone | ON (OFF for pharma fields) |
| Credit Card | OFF |
| IP Address | OFF |
| Passport | OFF |

### Dynamic Route Shield (AUTO Shield)
Tries local Mistral-7B inference first. Automatically switches to Gemini 2.5 Flash cloud fallback if local inference is rate-limited or times out. Fully transparent to the user.

### Tier-Aware Chunking
Recursive boundary-aware chunking with tier-specific parameters. Splits on natural boundaries (paragraph → sentence → word → character) before falling back to smaller units.

### Eval Framework
25-sample evaluation suite across 14 fields and all 5 tiers. Measures Faithfulness, Answer Relevance, and Context Recall. Regression testing catches quality drops before deployment.

### Document & Query History
- **Segment+** — File history, re-query previous documents
- **Lattice+** — 7-day query history
- **Vector+** — Full history with faithfulness scores
- **Enclave** — Audit log export, compliance trail

---

## Frontend Architecture

```
frontend/
├── index.html                    ← Entry point, auto-redirects
├── PAGE_1_LANDING_HUB.html       ← Landing, tier selection, auth, FAQ chatbot
├── PAGE_2_CORE_WORKSPACE.html    ← Command center, document viewer, audit panel
└── 404.html                      ← On-brand error page
```

### Tech Stack (Frontend)
- **HTML5 / CSS3** — no framework, no build step
- **Vanilla JavaScript** — zero dependencies
- **Space Grotesk** — display typography
- **DM Mono** — monospace labels, telemetry, data
- **Fully responsive** — desktop, tablet, mobile (320px to 4K)

### Design System
- Dark command center + light document viewer (hybrid layout)
- Five tier accent colors that shift the entire UI
- Tier-aware color palette (1 color on Ephemeral → 14 colors on Enclave)
- Light / Dark mode toggle with localStorage persistence
- Font size control (Small / Medium / Large)

### Key UI Components
- Typewriter intro cycling tier names, use cases, and platform stats
- Five-tier interactive selection cards with live accent switching
- Grouped field dropdown (14+ domains across 5 categories + custom field)
- Tabbed audit panel (Source Chunks + Eval Scores)
- Live telemetry bar (latency, tokens, chunks, cache, engine, tier)
- History sidebar (slides in from left, tier-gated)
- Platform settings panel (slides in from right)
- FAQ chatbot with auto-escalation and ticket creation
- Owner test mode with secure backend verification

---

## Backend Overview

The backend is proprietary and not included in this repository. Architecture summary for technical evaluation:

```
OCaml Gateway (port 8000)
  └── FastAPI ML Worker (port 8001)
        ├── core/
        │   ├── Recursive chunker (tier-aware)
        │   ├── ChromaDB + SQLite FTS5 hybrid retrieval
        │   ├── Mistral-7B local (llama-cpp-python, CPU)
        │   ├── Gemini 2.5 Flash cloud fallback
        │   └── PII scrubber (nine categories)
        └── routers/
            ├── /ingest    — PDF upload pipeline
            ├── /query     — RAG query pipeline
            └── /owner     — Secure owner verification (JWT)
```

**Backend tech:** OCaml (Dream, Lwt), Python (FastAPI, ChromaDB, pdfplumber, llama-cpp-python), SQLite FTS5, sentence-transformers

**Security:** Owner code never stored in source. Verified server-side via JWT with session lockout after failed attempts.

---

## Screenshots

*Coming soon — platform screenshots will be added here.*

---

## Roadmap

**v2.1**
- [ ] Streaming token responses
- [ ] PDF.js canvas highlights wired to real chunk positions
- [ ] OCR layer for scanned documents
- [ ] Clerk auth integration
- [ ] Stripe Revenue Perimeter wiring

**v2.2**
- [ ] HNSW vector indexing upgrade
- [ ] VLM (Vision-Language Model) for diagram ingestion
- [ ] Docker Compose deployment
- [ ] Render production deployment

**v3**
- [ ] GraphRAG for cross-document reasoning
- [ ] HyDE query expansion
- [ ] SOC 2 Type II compliance
- [ ] White-label enterprise offering
- [ ] Meridian Community launch

---

## About Meridian

Meridian is a technology company building at the intersection of business, tech, and community. Meridian Intel is our first product — a zero-hallucination document intelligence platform built for the domains that matter most.

**Contact:** merdianintel@gmail.com
**LinkedIn:** [linkedin.com/in/faith-odhe](https://www.linkedin.com/in/faith-odhe/) *(personal — company page coming soon)*

---

## License

Frontend code is provided for portfolio and evaluation purposes.
Backend, models, and proprietary logic are not included in this repository.

© 2026 Mazal Arc · All rights reserved.
