# BREM Risk Assessment: Fnality International
## Consortium-Backed Wholesale Payment System — Tokenised Central Bank Money

**Assessment Date:** 7 March 2026
**Classification:** Enterprise/Institutional — Consortium-Led, Regulator-Supervised
**Status:** Live (Sterling FnPS commenced controlled payments Dec 2023). Settlement finality designation from Bank of England (Dec 2024). Series C ($136M, Sep 2025).

---

## Project Summary

Fnality International operates a consortium-backed wholesale payment system using tokenised central bank money (originally "Utility Settlement Coin" / USC). The Sterling Fnality Payment System (FnPS) is the world's first regulated DLT-based wholesale payment system, backed by funds held in an omnibus account at the Bank of England.

**24 consortium shareholders** including Banco Santander, Bank of America, BNY Mellon, Barclays, BNP Paribas, Citi, Goldman Sachs, HSBC, UBS, and others.

**Core technology:** Private Ethereum fork called Autonity, developed by Clearmatics (Fnality's technical partner). Operates on a permissioned network with IBFT 2.0 Proof of Authority consensus.

**Settlement model:** True peer-to-peer atomic settlement. Payment-versus-Payment (PvP) for FX, Delivery-versus-Payment (DvP) for securities. No intermediary required. 100% backed by central bank money at 1:1 parity.

---

## Architecture: Single-Platform Advantage

Unlike Acacia (5 platforms) and Guardian (7+), Fnality operates on a **single blockchain platform** (Autonity). This is the defining structural difference:

- **One consensus mechanism** (IBFT 2.0)
- **One execution environment** (EVM-compatible)
- **One governance body** for the platform (consortium, with Clearmatics as technical partner)
- **No cross-chain bridges** required for core settlement
- **Atomic settlement on-chain** — both legs of PvP/DvP settle on the same ledger

Cross-chain interoperability with external platforms (e.g., Finteum on Corda for FX swaps) exists but is **optional extension**, not core architecture.

### Multi-Currency Model
Each currency (GBP, USD, EUR, JPY, CAD) operates as a **separate FnPS** with its own instance, supervised by the respective central bank. This is geographic distribution, not architectural decomposition — each FnPS is architecturally identical.

---

## Platform Analysis: Autonity (Clearmatics)

| Dimension | Score | Evidence |
|-----------|-------|----------|
| Architecture | 2 | Private Ethereum fork. Account-based. Well-understood EVM model but proprietary Autonity layer. Single paradigm, no interop complexity. |
| Consensus | 3 | IBFT 2.0 Proof of Authority. Named validators (consortium members). 2/3 threshold. Permissioned. |
| Scalability | 2 | 800-1,200 TPS claimed. Adequate for wholesale (high-value, low-volume). Single currency per FnPS reduces hot-spot. No independent verification but wholesale volumes manageable. |
| Mutation Authority | 3 | Consortium governs via shareholder agreement. Clearmatics controls Autonity codebase. Governance thresholds not publicly disclosed. Changes require consortium approval. |

**Independent verification: WEAK.** TPS claims not independently audited. But wholesale use case requires lower throughput than retail.

### Clearmatics Dependency
Clearmatics develops and maintains Autonity. This creates a **single technical partner dependency** — not as severe as multi-platform governance but still a concentration risk. If Clearmatics fails or pivots, the consortium must either:
1. Fork and maintain Autonity internally, or
2. Migrate to a different platform (extremely disruptive)

**Mitigation:** 24-shareholder consortium with $136M+ raised provides strong incentive alignment. BoE oversight adds regulatory pressure for continuity.

---

## Cell-by-Cell Scoring

| Cell | Score | Justification |
|------|-------|---------------|
| **na** | 2 | Single platform (Autonity/EVM). No cross-chain interop required for core settlement. Account-based but single-currency per FnPS mitigates hot-spot. Proprietary fork is a moderate risk. |
| **nc** | 3 | IBFT 2.0 PoA. Named validators. Permissioned. Standard enterprise consensus = 3. |
| **ns** | 2 | 800-1,200 TPS for wholesale settlement. Adequate for use case (high-value, low-volume). Single-currency per FnPS reduces contention. Still in controlled phase with BoE-imposed limits. |
| **se** | 2 | Single platform execution. Atomic PvP/DvP on-chain. No trusted intermediary for core settlement. Clearmatics dependency is moderate. |
| **sm** | 3 | Consortium governance with 24 shareholders. Clearmatics controls underlying codebase. Governance thresholds undisclosed. Standard enterprise = 3. |
| **sf** | 2 | Institutional backing ($136M Series C). Real wholesale use case. No token dependency. Controlled phase — commercial viability at scale unproven. |
| **ls** | 1 | **HM Treasury designated as systemically important payment system (Aug 2022)**. Bank of England oversight. First DLT system to receive settlement finality designation (Dec 2024). Strongest legal standing of any DLT payment system globally. |
| **lr** | 1 | UK legal remedies fully available. BoE supervision provides ultimate backstop. Settlement finality protection means payments cannot be unwound in insolvency. |
| **lp** | 2 | Liability frameworks not publicly disclosed. No published SLAs. Liability caps not specified. Gaps in attribution if Clearmatics introduces breaking change. BoE oversight provides some floor. |

---

## Aggregate Scores

| Domain | Score |
|--------|-------|
| **Network** | (2+3+2)/3 = **2.33** |
| **System State** | (2+3+2)/3 = **2.33** |
| **Law** | (1+1+2)/3 = **1.33** |
| **Overall** | 18/9 = **2.00** |

---

## Threshold Analysis

### Current Rule (overall ≥ 2.5): **PASSES** at 2.00 — well below threshold.

### Domain Ceiling Rule (overall ≥ 2.5 OR domain > 3.0): **PASSES** — no domain exceeds 3.0.

### Asymmetric Weighting: Still below adjusted threshold.

### Dependency Decomposition
Only 2 governance bodies (consortium + Clearmatics). **sm_eff = min(4, 3 + ⌈log₂(2)⌉) = min(4, 4) = 4.**

However, the consortium is designed to contractually control Clearmatics — unlike Acacia where the RBA Steering Committee has no authority over Hedera or R3. If the contractual relationship is robust, sm_eff should remain at sm_base = 3.

**Assessment:** Dependency decomposition penalty is marginal for single-platform architectures with contractual authority over the technical partner. This confirms the extension works correctly — it penalises uncoordinated multi-platform governance, not all platform dependencies.

---

## Comparison with Acacia and Guardian

| Dimension | Fnality | Guardian | Acacia |
|-----------|---------|----------|--------|
| na | 2 | 3 | 3 |
| nc | 3 | 3 | 3 |
| ns | 2 | 3 | 3 |
| se | 2 | 3 | 3 |
| sm | 3 | 3/4(eff) | 4(eff) |
| sf | 2 | 2 | 2 |
| ls | 1 | 1 | 1 |
| lr | 1 | 1 | 1 |
| lp | 2 | 2 | 2 |
| **Overall** | **2.00** | **2.33/2.44** | **2.44** |

### The Single-Platform Advantage
Fnality scores lower in **4 of 9 cells** (na, ns, se, sm) — all driven by architectural simplicity:
- **na**: One paradigm, no interop complexity
- **ns**: Single-currency per FnPS, adequate throughput for wholesale
- **se**: No cross-chain execution dependencies
- **sm**: Two governance bodies (coordinated) vs 7-8 (uncoordinated)

The Law and Economic Fitness cells are identical across all three projects. **The entire risk differential is architectural.**

---

## What Fnality Gets Right (From BREM Perspective)

1. **Single platform**: Eliminates dependency decomposition problem entirely for core settlement.
2. **Atomic on-chain settlement**: Both legs of PvP/DvP on same ledger. No cross-chain bridge risk.
3. **Central bank money backing**: 100% backed at 1:1 parity. No algorithmic peg risk (sf stays low).
4. **Settlement finality designation**: Legal certainty that no other DLT payment system has achieved.
5. **Per-currency isolation**: Each FnPS is independent. GBP failure doesn't cascade to USD.
6. **Regulatory alignment**: BoE oversight from inception, not retrofitted.

## Residual Risks

1. **Clearmatics concentration**: Single technical partner dependency. Proprietary codebase.
2. **Unverified throughput**: 800-1,200 TPS not independently audited.
3. **Controlled phase limitations**: BoE-imposed limits indicate confidence isn't full. Only 3 initial participants.
4. **Undisclosed governance thresholds**: Consortium voting weights and veto provisions not public.
5. **USD regulatory uncertainty**: US expansion pending regulatory clearance. Timeline unknown.
6. **Liability gaps**: No published SLAs or liability caps despite being live.

---

## Propagation Chains

### Primary: Clearmatics Mutation → Platform-Wide Execution Disruption → Liability Ambiguity (MODERATE)
Clearmatics deploys Autonity upgrade → consensus mechanism changes → settlement interruption. Who bears the loss? Consortium governance should prevent this, but governance thresholds are undisclosed.

### Secondary: BoE Constraint → Scalability Cap → Adoption Stall → Economic Non-Viability (LOW-MODERATE)
BoE-imposed limits constrain volumes → insufficient transaction flow to justify consortium investment → participants withdraw.

### Tertiary: Standard sm → se → ls (LOW)
Consortium discretion is bounded by BoE oversight. Settlement finality designation provides ultimate legal backstop.

---

## De-Risking Recommendations

### 1. Publish Governance Thresholds and Liability Framework — HIGH
The absence of public governance documentation is the primary gap. Consortium voting weights, veto provisions, and Clearmatics contractual terms should be transparent to participants.

### 2. Commission Independent Throughput Verification — MEDIUM
800-1,200 TPS claims should be independently audited under realistic wholesale settlement conditions.

### 3. Define Clearmatics Succession Plan — MEDIUM
What happens if Clearmatics fails, is acquired, or pivots away from Autonity? The consortium should have source code escrow and maintenance contingency.

### 4. Publish SLAs — MEDIUM
For a systemically important payment system with settlement finality designation, the absence of published SLAs is notable.

---

## Note on Dataset Score Reconciliation

The existing BREM 83-project dataset lists Fnality at 2.89 (Network=3.00, System=3.00, Law=2.67). The discrepancy with this assessment (2.00) is attributable to:

1. **BoE settlement finality designation (Dec 2024)**: ls improved from ~2 to 1, lr from ~2 to 1.
2. **Live operational status (Dec 2023)**: Reduced uncertainty across multiple cells.
3. **Series C funding ($136M, Sep 2025)**: Confirmed institutional commitment.
4. **Single-platform re-assessment**: The original dataset may have scored Fnality with planned cross-chain capabilities weighted more heavily.

This temporal improvement demonstrates an important BREM characteristic: scores are point-in-time assessments. Regulatory milestones (like settlement finality designation) can materially improve the legal domain, while the technical domain remains relatively static. The 0.89-point improvement is almost entirely from legal risk reduction.

---

*Assessment generated using BREM framework (Price, 2026) with platform-level decomposition and dependency decomposition extensions.*
