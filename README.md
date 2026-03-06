# BREM — Blockchain Risk Evaluation Model

A quantitative risk scoring framework for blockchain implementations, tested across 86 projects spanning enterprise consortia, central bank pilots, and DeFi protocols.

## Key Findings

- **86 projects scored** across nine canonical risk dimensions using the SPP × SPP framework
- **100% failure prediction** at the ≥2.5 overall risk threshold (sensitivity = 1.0, 95% CI [0.89, 1.0])
- **Three failure cascades** account for every project failure in the dataset
- **85 of 86 projects** retain mutation authority sufficient to override protocol constraints
- **$60B+** in tracked value destroyed across failed projects

## The Simulation Doctrine

Enterprise computing operates under what BREM identifies as the *Simulation Doctrine*: digital systems simulate events (purchase orders, settlement instructions, trade confirmations) that humans later verify and reconcile. Blockchain inverts this — a transaction *is* the state change, not a message about one.

When institutions adopt blockchain, they preserve the simulation doctrine. The result is architecturally indistinguishable from a database with distributed witnesses. The dataset proves this empirically.

## Repository Structure

```
├── data/                          # Dataset and scoring materials
│   ├── brap-v2-spf-auto-scored.csv          # Full 86-project scored dataset
│   ├── brap-v2-unified-dataset-cleaned.csv  # Cleaned unified dataset
│   ├── blockchain-implementations-canonical-scores.csv
│   ├── brap-v2-scoring-rubric.md            # Scoring rubric (0-4 scale, 9 dimensions)
│   └── dataset-metadata.json
├── paper/                         # IEEE ICBC 2026 workshop paper
│   ├── src/
│   │   ├── brem-ieee-paper.tex              # LaTeX source
│   │   └── IEEEtran.cls                     # IEEE class file
│   └── BREM-IEEE-Paper-Draft.pdf            # Compiled PDF
├── docs/                          # Supporting documents
│   ├── BREM-SPP-Framework.pdf               # Full whitepaper (22 pages)
│   └── BREM-Simulation-Doctrine-One-Pager.pdf  # One-page thesis
└── README.md
```

## BREM Framework: SPP × SPP

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

Each cell is scored 0–4 (increasing risk). The **mutation authority (sm)** variable is the single strongest predictor of project failure.

## Scoring Scale

| Score | Meaning |
|-------|---------|
| 0 | Negligible risk — protocol-constrained |
| 1 | Low risk — minimal discretion |
| 2 | Moderate risk — some centralised control |
| 3 | High risk — significant mutation authority |
| 4 | Critical risk — fully centralised control |

## Three Failure Cascades

1. **Architecture Death Spiral** (na→ns→sf): Network design flaws compound through scalability into economic collapse. 75% of failures.
2. **Mutation Authority Collapse** (sm→se→ls): Centralised control enables execution manipulation, destroying legal standing. 12.5% of failures.
3. **Regulatory Kill** (ls/lp): External legal action terminates the project. 6.25% of failures.

## Citation

If using this dataset or framework, please cite:

```
Price, T. (2026). Mutation Authority as a Predictive Risk Factor:
Empirical Evidence from 86 Blockchain Deployments. IEEE ICBC 2026
AI-R2D2 Workshop. [Submitted]
```

## Author

**Todd Price** — Real Blockchain Solutions
- Email: todd.price.aus@gmail.com

## License

Dataset and scoring rubric: CC BY 4.0
Paper: All rights reserved (pending IEEE publication)
