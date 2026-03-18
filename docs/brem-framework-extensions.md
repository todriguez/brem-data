# BREM Framework Extensions
## Findings from Project Acacia Assessment

Three proposed extensions to the BREM framework, each discovered through attempting to honestly score a real-world multi-platform infrastructure project (Project Acacia, RBA wholesale tokenised asset settlement). All three are empirically validated against the 83-project dataset.

---

## Extension 1: Dependency Decomposition and Mutation Surface Scaling

### Problem

The SPP framework assumes a single coherent subject per cell. When a project depends on N independent platforms with independent governance, the framework has no instruction for:
- How to score each dependency independently
- How to compose dependency scores into a project score
- Whether composition uses mean, max, or min

### Proposed Rule

**Mutation Authority Dependency Penalty:**

    sm_effective = min(4, sm_base + ceil(log2(N)))

where N = number of independent governance bodies with uncoordinated mutation authority over the system.

**Rationale:** If each platform has independent probability p of a breaking change per year, the probability of at least one break is 1-(1-p)^N. With 7 authorities at 10% each, P(break) = 52% vs 10% for a single authority. The risk scales faster than linearly.

**Composition rules differ by cell:**
- **na (Architecture):** max across platforms + interoperability penalty
- **ns (Scalability):** bottlenecked by slowest link (min throughput)
- **sm (Mutation Authority):** union of all governance bodies + log2(N) penalty
- **se (Execution):** each dependency adds failure surface

### Impact on Acacia

N=7 governance bodies (Steering Committee, Hedera Council, R3, Canvas, Redbelly, Ethereum devs, Fireblocks). sm_base=3 → sm_effective = min(4, 3+3) = 4.

### Critical Finding: Platform Liability Gap

Hedera's Terms of Service cap total aggregate liability at **$100 USD** and explicitly disclaim all responsibility for third-party integrations. The Steering Committee has zero contractual recourse if Hedera deploys a breaking change. This means the dependency penalty cannot be mitigated through governance — the platform providers operate under take-it-or-leave-it terms.

---

## Extension 2: Asymmetric Cell Weighting

### Problem

The current overall score is a flat average across all 9 cells. This treats each cell's contribution as symmetric — a low score (protective) offsets a high score (damaging) by the same amount. But empirically, cells are asymmetric: being bad at some cells hurts far more than being good at them helps.

### Empirical Finding

Each cell was split at its median. "Fail-pull" measures excess failure rate when the cell is above median. "Surv-pull" measures excess survival rate when below median. The asymmetry ratio = fail-pull / surv-pull.

| Cell | Fail-Pull | Surv-Pull | Asymmetry | Interpretation |
|------|-----------|-----------|-----------|----------------|
| nc | +0.518 | +0.063 | **8.22** | Being bad at consensus is 8.2x more impactful than being good |
| se | +0.518 | +0.114 | **4.53** | Execution failure is 4.5x more impactful than execution safety |
| na | +0.333 | +0.160 | **2.07** | Architectural risk is 2.1x more impactful than architectural safety |
| ls | +0.351 | +0.199 | **1.77** | Legal risk hurts 1.8x more than legal clarity helps |
| ns | +0.427 | +0.282 | 1.52 | Moderate asymmetry |
| lr | +0.306 | +0.202 | 1.52 | Moderate asymmetry |
| sm | +0.253 | +0.176 | 1.44 | Mild asymmetry |
| sf | +0.490 | +0.357 | 1.37 | Mild asymmetry |
| lp | +0.261 | +0.190 | 1.37 | Mild asymmetry |

### Key Insight

**Every cell is asymmetric in the same direction:** being bad hurts more than being good helps. No cell has an asymmetry ratio below 1. This means the flat average systematically underestimates risk for projects with mixed profiles (some very bad cells, some very good cells).

**The legal domain confirms Todd Price's observation:** ls has 1.77 asymmetry — government CAN kill a project (w_up=0.678) but government clarity alone CANNOT save one (w_down=0.557). Legal clarity should not animate a technical zombie.

### Proposed Rule

For each cell, assign:
- **w_up** (weight when score > 2): proportional to fail-pull
- **w_down** (weight when score ≤ 2): proportional to surv-pull

Asymmetric weighted overall = Σ(score_i × w_i) / Σ(w_i)

### Prediction Improvement

| Method | Threshold | F1 | Sensitivity | Specificity |
|--------|-----------|-----|-------------|-------------|
| Current (flat average) | 2.5 | 0.902 | 0.925 | 0.884 |
| Asymmetric weighted | 2.7 | **0.914** | 0.925 | 0.907 |

### Impact on Acacia

All 5 technical cells (na=3, nc=3, ns=3, se=3, sm=4) use w_up weights (full penalty). All 3 law cells (ls=1, lr=1, lp=2) use w_down weights (discounted protection). Asymmetric score: 2.50 vs 2.44 symmetric. The law domain's protective effect is empirically discounted.

---

## Extension 3: Domain Ceiling Rule

### Problem

The flat average allows one domain to subsidise another when they are not fungible. A project with Network=3.0, System=3.0, Law=1.0 scores 2.33 overall — "below threshold." But the technical infrastructure is in the failure zone regardless of legal clarity.

### Proposed Rule

**Dual threshold:** Flag if overall > 2.5 **OR** any domain > 3.0.

When a domain exceeds 3.0 but overall is below 2.5, report as "DOMAIN-MASKED RISK."

### Prediction Improvement

| Method | F1 | Sensitivity | Specificity |
|--------|-----|-------------|-------------|
| Overall > 2.5 (current) | 0.902 | 0.925 | 0.884 |
| Overall > 2.5 OR any domain > 3.0 | **0.929** | **0.975** | 0.884 |

The dual threshold catches 39/40 failures vs 37/40, with the same false positive count. The 2 additional catches include HSBC FX Everywhere (the closest precedent to Project Acacia — strong legal framework, technically compromised, failed anyway).

### Supporting Evidence

System State is the most predictive domain (Cohen's d = 2.53 between failed and not-failed). Network is second (d = 1.28). Law is weakest (d = 1.25). Averaging all three equally dilutes the strongest signal with the weakest.

### Impact on Acacia

Network = 3.00 and System State = 3.00 — precisely at the domain ceiling. Not a comfortable position. One additional risk factor in any technical cell would trigger the domain ceiling.

---

## Combined Effect on Project Acacia

| Scoring Method | Score | Threshold | Result |
|---------------|-------|-----------|--------|
| V1 Monolithic | 1.89 | 2.5 | PASSES (comfortable) |
| V2 se revised | 2.00 | 2.5 | PASSES (moderate) |
| V3 Platform decomposition | 2.44 | 2.5 | PASSES (barely) |
| V3 + Asymmetric weighting | 2.50 | 2.7 | PASSES (borderline) |
| V3 + Domain ceiling check | — | domain > 3.0 | Network=3.0, System=3.0 — AT boundary |

Under every extension, Acacia moves closer to — or onto — the threshold. The law domain is progressively less able to compensate for technical risk as the methodology becomes more honest about asymmetric contributions.

---

## Implications for the Paper

These three extensions were discovered through practical application of the framework to a real project. They represent the kind of iterative refinement that characterises an evolving assessment methodology. Each improves predictive performance on the existing dataset while addressing a genuine analytical gap.

The extensions are compatible with the current framework — they modify the scoring interpretation, not the SPP structure itself. They could be presented as a "V2 scoring methodology" that maintains backward compatibility with the 83-project dataset while improving accuracy for complex modern infrastructure.
