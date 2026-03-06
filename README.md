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

Each cell is scored 0–4 (increasing risk). See [CODEBOOK.md](data/CODEBOOK.md) for full scoring definitions.

## Repository Structure

```
├── data/
│   ├── brem-dataset-v3.csv          # Full 83-project scored dataset
│   ├── CODEBOOK.md                  # Dataset documentation and methodology
│   └── scoring-rubric.md            # Detailed scoring rubric (0-4 scale)
├── paper/
│   ├── src/
│   │   ├── brem-ieee-paper.tex      # LaTeX source (IEEEtran format)
│   │   └── IEEEtran.cls            # IEEE class file
│   └── BREM-IEEE-Paper-Draft.pdf    # Compiled PDF
├── docs/
│   └── (supporting documents)
└── README.md
```

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

## Citation

```
Price, T. (2026). Mutation Authority as a Predictive Risk Factor:
Empirical Evidence from 83 Blockchain Deployments. IEEE ICBC 2026
AI-R2D2 Workshop. [Submitted]
```

## Author

**Todd Price** — Real Blockchain Solutions

## License

Dataset and scoring rubric: CC BY 4.0
Paper: All rights reserved (pending IEEE publication)
