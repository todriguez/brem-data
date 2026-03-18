# BREM Risk Assessment: Project Acacia (V3 — Platform Decomposition)
## Reserve Bank of Australia — Wholesale Tokenised Asset Settlement

**Assessment Date:** 7 March 2026
**Classification:** Enterprise/Institutional Pilot — Central-Bank-Led Research
**Status:** Phase 1 (6-month testing cycle, Q1 2026 report expected)

---

## Scoring Evolution

| Version | Approach | Overall | Key Change |
|---------|----------|---------|------------|
| V1 | Monolithic (treated as single system) | 1.89 | Baseline — too generous |
| V2 | Revised se for multi-platform execution | 2.00 | Recognised execution dependency |
| V3 | **Platform decomposition** | **2.44** | **Each dependency scored independently; composite reflects weakest link** |

---

## Methodological Findings

This assessment exposed two fundamental gaps in the current BREM framework. Both findings emerged from attempting to score Project Acacia and discovering the framework lacked the tools to do so honestly.

### Finding 1: SPP Cannot Decompose Multi-Dependency Systems

The nine BREM cells assume a single coherent subject. "What is the architecture?" presupposes there *is* one architecture. When the answer is "five different architectures that have to interoperate," the framework has no instruction for:

1. How to score each dependency independently
2. How to compose dependency scores into a project score
3. Whether composition should use mean, max, or min — and crucially, the correct composition rule **differs by cell**:
   - **Architecture (na):** worst-case platform + interoperability penalty
   - **Scalability (ns):** bottlenecked by slowest link (min throughput)
   - **Mutation Authority (sm):** union of all governance bodies (any can break the system)
   - **Execution (se):** product of dependencies (each adds failure surface)

The V1 assessment scored 1.89 by treating Acacia as monolithic. The V3 score of 2.44 emerged only by applying platform decomposition ad hoc. The 0.55-point gap between V1 and V3 is entirely methodological — the project didn't change, the analysis did.

### Finding 2: Domain Averaging Allows Non-Fungible Compensation

The current overall score is a flat average across all 9 cells: (Network + System State + Law) / 3. This allows a strong law domain to offset weak technical domains. But legal clarity and technical functionality are **not fungible**. A clear legal framework doesn't fix a 10 TPS bottleneck. Established remedies don't prevent a broken signature scheme. You can sue after the system fails, but the system still fails.

**Evidence from the dataset:** The dual-threshold rule "overall > 2.5 OR any domain > 3.0" produces F1 = 0.929, outperforming the current overall-only rule (F1 = 0.902). It catches 39/40 failures vs 37/40 with the same false positive count. HSBC FX Everywhere (Tech=3.00, Law=1.00, Overall=2.33) is the clearest case: a "technical zombie" that passed the overall threshold but failed anyway.

**System State is the most predictive domain** (Cohen's d = 2.53 between failed and not-failed), followed by Network (d = 1.28) and Law (d = 1.25). Averaging System State equally with Law dilutes the strongest failure signal in the framework.

**Project Acacia under these findings:** Network = 3.00, System State = 3.00, Law = 1.33, Overall = 2.44. The technical infrastructure scores are in the failure zone. The law domain pulls the average below threshold. Whether that constitutes a genuine "pass" depends entirely on whether you believe institutional clarity can animate technically broken infrastructure. The HSBC FX Everywhere precedent suggests it cannot.

---

## Platform-Level Analysis

Before scoring the composite system, each platform dependency was assessed on its own merits.

### Hedera Hashgraph

| Dimension | Score | Evidence |
|-----------|-------|----------|
| Architecture | 3 | Account-based model. Governing council of 31 corporate members controls codebase with supermajority. Actively developing new signature scheme (hinTS/BLS). |
| Consensus | 3 | Permissioned nodes run exclusively by approved council members. Ordinary users cannot participate. Centralization concerns documented publicly. |
| Scalability | 3 | **Claims 10,000 TPS but real-world smart contract throughput throttled to ~10 TPS.** Peak recorded: 3,302 TPS (simple transfers only). Account-based model creates hot-spot contention on single-asset operations — exactly the CBDC use case. |
| Mutation Authority | 3 | Council approves platform changes. Currently developing hinTS threshold signature scheme — a breaking change for existing cryptographic integrations. |

**Independent verification: WEAK.** The 10,000 TPS figure has not been independently reproduced for smart contract operations. No verification under single-asset hot-spot conditions.

### R3 Corda

| Dimension | Score | Evidence |
|-----------|-------|----------|
| Architecture | 3 | Proprietary platform. R3 controls development roadmap. Enterprise licensing. |
| Consensus | 3 | Notary-based consensus. Permissioned. Notary is a documented bottleneck. |
| Scalability | 3 | **~600 TPS (R3's own benchmarks).** Notary is documented bottleneck. No horizontal scaling as of v4.5. |
| Mutation Authority | 3 | R3 controls roadmap unilaterally. Breaking changes between major versions documented. |

**Independent verification: MODERATE.** R3 publishes own benchmarks with caveats. No independent third-party scalability audit found.

### Canvas Connect

| Dimension | Score | Evidence |
|-----------|-------|----------|
| Architecture | 2 | ZK Layer 2 on StarkEx/Ethereum. Constrained by L1 security model. |
| Consensus | 2 | Inherits Ethereum security. ZK proofs algorithmically verified. |
| Scalability | 2 | ZK rollup batching provides scaling. Dependent on L1 for finality. |
| Mutation Authority | 3 | Canvas team controls ZK rollup operator. Startup with less governance maturity. |

**Independent verification: WEAK.** No independent benchmarks found. Relatively new platform.

### Redbelly Network

| Dimension | Score | Evidence |
|-----------|-------|----------|
| Architecture | 2 | Academic origin (UNSW). Democratic BFT formally verified at DISC 2022. |
| Consensus | 2 | Democratic BFT — first blockchain consensus fully formally verified. |
| Scalability | 2 | 30,000 TPS in peer-reviewed paper (1,000 VMs). Chainspect independent benchmark confirmed. |
| Mutation Authority | 3 | Australian startup controls development. Small team. |

**Independent verification: STRONG.** Peer-reviewed at DISC 2022 (top distributed computing venue). Chainspect independent benchmark. Best evidence base of any platform in the stack.

### EVM-Compatible Chains

| Dimension | Score | Evidence |
|-----------|-------|----------|
| Architecture | 1 | Open standard, multiple implementations, well-understood. |
| Consensus | 2 | Various — public PoS or permissioned depending on chain. |
| Scalability | 3 | Base layer congestion well-documented. Account-based = contention on single-asset ops. |
| Mutation Authority | 2 | Hard fork governance via EIPs. Community-driven. |

**Independent verification: STRONG.** Most battle-tested blockchain infrastructure. Limitations well-documented.

---

## The Account-Based Hot-Spot Problem

All account-based platforms (Hedera, EVM chains) create hot-spot contention when a single high-frequency asset dominates transactions. In a wholesale CBDC system, the CBDC *is* the single dominant asset. Every DvP settlement touches the same contract or account state.

On account-based chains, this means sequential state updates — all transactions touching the CBDC must be serialised. Throughput degrades non-linearly as participant count grows because contention on the single asset grows quadratically, not linearly.

Benchmark TPS numbers measure throughput across many independent accounts. A single-asset wholesale CBDC is the **worst case** for account-based architectures. **None of the platform scalability claims have been verified under single-asset hot-spot conditions representative of wholesale CBDC settlement.**

By contrast, UTXO-based models allow parallel processing of independent transaction outputs. The BREM framework captures this in na: shared mutable state (account-based) scores higher risk than independent state models (UTXO).

---

## Revised Cell-by-Cell Scoring (V3)

| Cell | V1 | V2 | V3 | V3 Justification |
|------|----|----|-----|-------------------|
| **na** | 2 | 2 | **3** | Multi-platform interop across fundamentally different paradigms: account-based (Hedera, EVM) vs bilateral (Corda) vs ZK rollup (Canvas). Different data models, transaction formats, state management. The interoperability layer itself is an unverified architectural risk. |
| **nc** | 3 | 3 | **3** | Permissioned across all platforms. Unchanged. |
| **ns** | 2 | 2 | **3** | Bottlenecked by slowest link: Hedera SC throttled to ~10 TPS, Corda notary ~600 TPS. CBDC as single asset creates hot-spot contention. No verification of cross-chain throughput under realistic conditions. |
| **se** | 2 | 3 | **3** | Multi-platform execution dependency on 5+ providers, each with independent upgrade cycles. |
| **sm** | 3 | 3 | **4** | **7 uncoordinated mutation authorities.** RBA Steering Committee, Hedera Council, R3, Canvas, Redbelly, Ethereum core devs, Fireblocks. Any can independently break the system. Worse than single-authority sm=4 because mutations are uncoordinated. |
| **sf** | 2 | 2 | **2** | Institutional mandate, no token dependency. Unchanged. |
| **ls** | 1 | 1 | **1** | Strong Australian legal framework. Unchanged. |
| **lr** | 1 | 1 | **1** | Established Australian remedies. Unchanged. |
| **lp** | 1 | 1 | **2** | Cross-platform partial settlement creates liability ambiguity. Who bears the loss when DvP executes on one chain but fails on another? |

---

## Aggregate Scores (V3)

| Domain | V1 | V2 | V3 |
|--------|----|----|-----|
| **Network** | 2.33 | 2.33 | **3.00** |
| **System State** | 2.33 | 2.67 | **3.00** |
| **Law** | 1.00 | 1.00 | **1.33** |
| **Overall** | 1.89 | 2.00 | **2.44** |

---

## Threshold Analysis

### Current Rule (overall > 2.5): PASSES at 2.44

### Proposed Dual Rule (overall > 2.5 OR any domain > 3.0): **BORDERLINE**

Network and System State both score exactly 3.00 — precisely at the domain ceiling boundary. This is not a comfortable pass. One additional risk factor in any technical cell would push a domain over 3.0.

### The Real Question

Network = 3.00. System State = 3.00. These are failure-zone scores. In the dataset, the mean System State for failed projects is 3.58; for not-failed projects, 2.40. Acacia's System State (3.00) is closer to the failed mean.

The overall score of 2.44 is an artefact of averaging across non-fungible domains. The law domain (1.33) is absorbing risk that it cannot actually mitigate. Legal clarity doesn't fix a 10 TPS bottleneck. Established remedies don't prevent a broken signature scheme.

**Assessment: Project Acacia's technical infrastructure has a risk profile consistent with projects that have failed. Its viability depends entirely on institutional backing compensating for technical risk — a strategy that worked for BNY Mellon and Citi (single-entity banking products) but failed for HSBC FX Everywhere (multi-party settlement).**

---

## Comparable Projects (V3)

| Project | Overall | Network | System | Law | Outcome | Pattern |
|---------|---------|---------|--------|-----|---------|---------|
| ANZ A$DC | 1.56 | 2.00 | 1.67 | 1.00 | not_failed | Single institution, single platform |
| Singapore Ubin+ | 1.56 | 1.67 | 1.67 | 1.33 | not_failed | Single platform, bounded scope |
| Project Dunbar | 2.22 | 2.33 | 2.33 | 2.00 | not_failed | Multi-jurisdiction but fewer platforms |
| **Project Acacia (V3)** | **2.44** | **3.00** | **3.00** | **1.33** | **Pilot** | **Multi-platform, single-asset CBDC** |
| ECB Digital Euro | 2.44 | 2.33 | 2.67 | 2.33 | not_failed | Complex but fewer platform dependencies |
| Fnality | 2.89 | 3.00 | 3.00 | 2.67 | not_failed | Similar tech profile, weaker legal |
| HSBC FX Everywhere | 2.33 | 2.67 | 3.33 | 1.00 | **failed** | **Same pattern: strong law, weak tech** |
| ASX CHESS | 3.78 | 4.00 | 3.67 | 3.67 | **failed** | Australian enterprise blockchain |

**HSBC FX Everywhere is the closest precedent.** Strong institutional backing (HSBC), clear legal framework (ls=1, lr=1, lp=1), but technically compromised (se=3, sm=4, sf=3). Overall=2.33 — below threshold. Failed anyway. Legal clarity did not save it.

---

## Propagation Chains

### Primary: Distributed Platform Mutation → Execution Failure → Liability Ambiguity (CRITICAL)

7 uncoordinated mutation authorities. Hedera deploys hinTS signature scheme → Fireblocks DvP contracts break → asset delivered on Corda, CBDC stuck on Hedera → who bears the loss? Currently undefined.

### Secondary: Hot-Spot Contention → Scalability Failure → Economic Non-Viability (HIGH)

Single CBDC asset on account-based chains → serialised bottleneck → effective throughput orders of magnitude below marketed TPS → if settlement can't handle volume, participants revert to existing RBA ESA → project becomes redundant.

### Tertiary: Standard sm → se → ls (MODERATE)

Steering Committee's own discretion compounding platform-layer mutation risk.

---

## De-Risking Recommendations

### 1. Independently Verify Cross-Platform Throughput Under CBDC Conditions — CRITICAL
Commission independent stress test (not vendor benchmarks) measuring cross-chain DvP throughput with a single high-frequency asset under concurrent settlement load. If effective throughput under realistic conditions is below existing RBA ESA capacity, the project has no production path.

### 2. Contractually Bind Platform Providers to Breaking-Change Governance — CRITICAL
90-day notice, backward compatibility, mandatory cross-platform integration testing. Hedera's hinTS development makes this urgent.

### 3. Elevate Mutation Authority to Coordinated Governance (sm=4 → sm=2) — CRITICAL
Steering Committee must have contractual authority over platform-layer changes affecting settlement. Platform providers cannot deploy breaking changes without approval.

### 4. Address Account-Based Architecture Risk — HIGH
Either favour UTXO/bilateral platforms (Redbelly, Corda) for high-frequency CBDC settlement, or implement application-level parallelisation (sharding the CBDC across multiple accounts) to mitigate hot-spot contention.

### 5. Define Cross-Platform Liability Allocation — HIGH
Who bears the loss when DvP partially executes due to a platform provider's breaking change?

### 6. Commission Economic Sustainability Analysis — MEDIUM
Model economics under realistic throughput constraints (not marketed TPS).

---

## ASX CHESS Comparison (V3)

| Dimension | Acacia (V3) | ASX CHESS | Gap |
|-----------|-------------|-----------|-----|
| na | 3 | 4 | -1 |
| nc | 3 | 4 | -1 |
| ns | 3 | 4 | -1 |
| se | 3 | 3 | **0** |
| sm | 4 | 4 | **0** |
| sf | 2 | 4 | -2 |
| ls | 1 | 3 | -2 |
| lr | 1 | 4 | -3 |
| lp | 2 | 4 | -2 |
| **Overall** | **2.44** | **3.78** | -1.34 |

Execution risk and mutation authority are **equal** between the two projects. Acacia's advantage is entirely in law domain and economic fitness. The gap (1.34) is almost entirely institutional, not technical.

---

*Assessment generated using BREM framework (Price, 2026) with platform-level decomposition. This assessment also identifies two proposed framework extensions: (1) dependency decomposition layer for systems-of-systems, and (2) dual-threshold rule with domain ceiling to prevent non-fungible compensation across domains.*
