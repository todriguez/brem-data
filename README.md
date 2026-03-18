# BREM — Blockchain Risk Evaluation Model

A quantitative risk scoring framework for blockchain implementations, evaluated across 83 projects spanning enterprise consortia, central bank pilots, and DeFi protocols.

## Key Findings

- **83 projects scored** (71 enterprise, 12 DeFi) across nine canonical risk dimensions using the SPP × SPP framework
- **92.5% sensitivity** at the ≥2.5 overall risk threshold (F1 = 0.902)
- **100% specificity among resolved projects** — every project above threshold has failed or is in sustained distress
- **Mutation authority (sm)** is the strongest single predictor: zero projects with sm ≤ 2 have failed
- **Three failure cascades** account for the majority of project failures

## The SPP × SPP Framework

BREM organises blockchain risk into a 3×3 matrix using the Structure–Process–Persistence (SPP) pattern applied twice:

**Vertically** (rows = SPP):

| Domain | SPP Role | Description |
|--------|----------|-------------|
| Network | Structure | The architectural substrate |
| System State | Process | State transitions within the network |
| Law | Persistence | Legal enforceability in the real world |

**Horizontally** (columns within each row):

| | Structure | Process | Persistence |
|---|-----------|---------|-------------|
| **Network** | Architecture (na) | Consensus (nc) | Scalability (ns) |
| **System State** | Execution (se) | Mutation Authority (sm) | Economic Fitness (sf) |
| **Law** | Standing (ls) | Remedy (lr) | Liability (lp) |

Each cell is scored 0–4 (increasing risk). See [`scoring-rubric.md`](data/scoring-rubric.md) for cell-level anchor definitions and [`CODEBOOK.md`](data/CODEBOOK.md) for dataset documentation.

## Repository Structure

```
├── data/
│   ├── brem-dataset-v3.csv              # Full 83-project scored dataset
│   ├── CODEBOOK.md                      # Dataset documentation and methodology
│   ├── scoring-rubric.md               # Cell-level scoring anchors (0-4 scale)
│   └── scoring-instrument.md           # Branching decision logic for cell-level scoring
├── assessments/                         # Worked BREM assessments
│   ├── brem-assessment-project-acacia.md    # BIS/RBA — CBDC settlement (2.44)
│   ├── brem-assessment-project-guardian.md  # MAS — multi-platform DeFi (2.33)
│   ├── brem-assessment-fnality-international.md  # BoE-supervised — wholesale settlement (2.00)
│   ├── brem-assessment-stripe-tempo.md      # Stripe/Paradigm — payments L1 (2.11)
│   └── brem-assessment-arc-network.md       # Circle — stablecoin finance L1 (2.00)
├── docs/
│   └── brem-framework-extensions.md     # Three scoring extensions (V2 methodology)
├── tools/
│   └── brem-consultant/                 # AI advisor skill (scoring guide + dataset summary)
└── README.md
```

### Reviewer Guide

**To verify claims in the paper:**

1. **Scoring methodology**: [`data/scoring-rubric.md`](data/scoring-rubric.md) — cell-level anchor definitions (what each 0–4 score means for each cell, with reference examples)
2. **Scoring instrument**: [`data/scoring-instrument.md`](data/scoring-instrument.md) — branching decision logic for cell-level scoring (4–5 diagnostic questions per cell with gating, refinement, and override logic)
3. **Dataset**: [`data/brem-dataset-v3.csv`](data/brem-dataset-v3.csv) — all 83 projects with scores, outcomes, evidence summaries, and data sources
4. **Dataset documentation**: [`data/CODEBOOK.md`](data/CODEBOOK.md) — column definitions, outcome classification criteria, key statistics, limitations
5. **Scoring extensions**: [`docs/brem-framework-extensions.md`](docs/brem-framework-extensions.md) — asymmetric weighting, domain ceiling, dependency decomposition
6. **Worked assessments**: [`assessments/`](assessments/) — five full BREM assessments demonstrating the scoring process cell-by-cell with evidence citations

## Threshold Performance

| Metric | Full Dataset (n=83) | Resolved Only (n=62) |
|--------|---------------------|----------------------|
| Sensitivity | 92.5% | 92.5% |
| Specificity | 88.4% | 100.0% |
| Precision | 88.1% | 100.0% |
| F1 Score | 0.902 | 0.961 |

The 5 "false positives" in the full dataset are all unresolved projects (pilots, development phase) — these are predictions, not errors. The 3 false negatives among resolved projects (HSBC FX Everywhere, NASDAQ Linq, SETL) had sound architecture but failed commercially; the model captures systemic coordination risk better than pure market failure.

## Mutation Authority Distribution

| sm Score | Projects | Failed | Failure Rate |
|----------|----------|--------|-------------|
| 0 | 1 | 0 | 0% |
| 2 | 6 | 0 | 0% |
| 3 | 42 | 15 | 35.7% |
| 4 | 34 | 25 | 73.5% |

The only project with sm = 0 is Uniswap V2/V3 — protocol-immutable with zero admin keys.