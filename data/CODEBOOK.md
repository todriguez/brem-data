# BREM Dataset Codebook v3.0

## Overview

This dataset contains risk scores for 83 blockchain projects (71 enterprise, 12 DeFi) evaluated using the Blockchain Risk Evaluation Model (BREM). Each project is scored across 9 dimensions derived from the Structure–Process–Persistence (SPP) framework applied twice: vertically across domain rows and horizontally within each domain.

**Assessment period**: 2016–2026
**Scoring date**: March 2026
**Version**: 3.0

---

## Column Definitions

### Identification

| Column | Type | Description |
|--------|------|-------------|
| `project_id` | String | Unique identifier. E-prefixed for enterprise (E001–E071), D-prefixed for DeFi (D001–D012) |
| `project_name` | String | Project or protocol name |
| `organization` | String | Lead organization or consortium |
| `segment` | Categorical | `enterprise` or `defi` |

### Context

| Column | Type | Description |
|--------|------|-------------|
| `category` | String | Primary use case category (e.g., CBDC, Trade Finance, Lending Protocol) |
| `region` | String | Primary geographic region |
| `platform` | String | Underlying technology platform |
| `governance_type` | String | Governance structure (e.g., Single entity, Consortium, DAO governance, Immutable contracts) |
| `launch_year` | Integer/blank | Year the project launched or began pilots |
| `end_year` | Integer/blank | Year the project ended (blank if ongoing) |
| `investment_usd` | String | Known investment amount in USD (blank or "Undisclosed" if unavailable) |

### Outcome

| Column | Type | Description |
|--------|------|-------------|
| `status_original` | String | Original status label as recorded during data collection |
| `status_clean` | Categorical | Standardised status: `Failed`, `Distressed`, `Operational`, `Operational (limited)`, or descriptive (e.g., `Production`, `Pilot`) |
| `outcome` | Binary | `failed` or `not_failed`. Classification criteria below. |
| `resolution` | Categorical | `resolved` (outcome determined) or `unresolved` (project still in pilot/development) |

**Outcome classification criteria:**
- `failed`: Project shut down, collapsed, discontinued, abandoned, or in sustained distress with no viable path to recovery
- `not_failed`: Project operational, in production, actively piloting, or in development
- Projects labelled "Limited" or "Struggling" are classified as `failed` (distressed) unless actively serving their primary use case
- 21 projects are classified as `unresolved` (pilots, research, early development) — these have outcomes but may change

### BREM Scores (9 canonical cells)

All scores are integers on a 0–4 scale:

| Score | Meaning |
|-------|---------|
| 0 | Negligible risk — protocol-constrained, no discretionary authority |
| 1 | Low risk — minimal discretion, strong protocol constraints |
| 2 | Moderate risk — some centralised control or architectural limitations |
| 3 | High risk — significant mutation authority or structural weakness |
| 4 | Critical risk — fully centralised control or fundamental failure |

**Network Domain** (row = Structure in vertical SPP):

| Column | Cell | Description |
|--------|------|-------------|
| `na` | Architecture | Degree of architectural centralisation, node structure, protocol design |
| `nc` | Consensus | Consensus mechanism risk — validator centralisation, finality guarantees |
| `ns` | Scalability | Throughput constraints, capacity limits, performance under load |

**System State Domain** (row = Process in vertical SPP):

| Column | Cell | Description |
|--------|------|-------------|
| `se` | Execution | Smart contract / execution model risk, upgradeability, composability |
| `sm` | Mutation Authority | **Key diagnostic variable.** Scope of discretion any entity has to change protocol rules, parameters, or state without protocol-level constraints. sm=0 means immutable; sm=4 means a single entity can change anything. |
| `sf` | Economic Fitness | Economic model sustainability, token economics, fee structures, market viability |

**Law Domain** (row = Persistence in vertical SPP):

| Column | Cell | Description |
|--------|------|-------------|
| `ls` | Standing | Legal classification clarity, regulatory status, jurisdictional certainty |
| `lr` | Remedy | Availability of legal remedies if things go wrong — can users recover losses? |
| `lp` | Liability | Liability allocation clarity, insurance, who bears risk of loss |

### Domain and Overall Scores

| Column | Type | Description |
|--------|------|-------------|
| `network_domain` | Float | Mean of na, nc, ns (rounded to 2dp) |
| `system_domain` | Float | Mean of se, sm, sf (rounded to 2dp) |
| `law_domain` | Float | Mean of ls, lr, lp (rounded to 2dp) |
| `overall_score` | Float | Mean of three domain scores (rounded to 2dp). Range: 0.00–4.00. Threshold: 2.50 |

### Failure Analysis

| Column | Type | Description |
|--------|------|-------------|
| `failure_category` | String | Primary failure type (e.g., Economic Collapse, Technical Failure, Regulatory Failure). Blank for non-failed projects. |
| `failure_reasons` | String | Brief description of key failure drivers |

### Methodology

| Column | Type | Description |
|--------|------|-------------|
| `scoring_method` | Categorical | `expert-scored` (hand-scored by domain expert) or `systematic-with-review` (systematic assessment based on project documentation, subsequently reviewed) |
| `evidence_summary` | String | Brief justification for scores referencing project-specific evidence |

---

## Key Statistics

| Metric | Value |
|--------|-------|
| Total projects | 83 |
| Enterprise | 71 |
| DeFi | 12 |
| Failed | 40 |
| Not failed | 43 |
| Resolved | 62 |
| Unresolved | 21 |

### Threshold Performance (overall_score >= 2.5)

| Metric | Full Dataset (n=83) | Resolved Only (n=62) |
|--------|---------------------|----------------------|
| Sensitivity | 92.5% | 92.5% |
| Specificity | 88.4% | 100.0% |
| Precision | 88.1% | 100.0% |
| F1 Score | 0.902 | 0.961 |

Note: All 5 "false positives" in the full dataset are unresolved projects (pilots/development). Among resolved projects, no project above the 2.5 threshold has survived without distress.

### Mutation Authority (sm) Distribution

| sm | Count | Failed | Failure Rate |
|----|-------|--------|-------------|
| 0 | 1 | 0 | 0% |
| 1 | 0 | 0 | — |
| 2 | 6 | 0 | 0% |
| 3 | 42 | 15 | 35.7% |
| 4 | 34 | 25 | 73.5% |

---

## Limitations

1. **Scoring subjectivity**: Scores involve expert judgment. Inter-rater reliability testing showed Cohen's κ = 0.81 at cell level and 0.91 at domain level, but the primary dataset was scored by a single rater.

2. **Hindsight bias**: For failed projects, knowledge of failure may influence scoring. Mitigation: 10 projects were scored blind to outcome (89% concordance with post-hoc scores).

3. **Unresolved projects**: 21 projects have not yet reached a definitive outcome. Their current classification may change.

4. **DeFi sample size**: 12 DeFi projects is a small sample. DeFi findings should be treated as preliminary.

5. **Scoring method**: 67 of 71 enterprise projects were scored using a systematic methodology subsequently reviewed, rather than de novo expert scoring. This is noted in the `scoring_method` column.

6. **False negatives**: 3 resolved projects scored below 2.5 despite failing (HSBC FX Everywhere, NASDAQ Linq, SETL). These projects had sound architecture but failed commercially — the model captures systemic coordination risk better than pure market failure.

---

## Citation

Price, T. (2026). Mutation Authority as a Predictive Risk Factor: Empirical Evidence from 83 Blockchain Deployments. *IEEE ICBC 2026 AI-R2D2 Workshop*. [Submitted]

## License

Dataset: CC BY 4.0
Codebook: CC BY 4.0
