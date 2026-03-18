# BREM Risk Assessment: Project Guardian
## Monetary Authority of Singapore — Tokenised Asset & Wholesale Settlement

**Assessment Date:** 7 March 2026
**Classification:** Enterprise/Institutional — Sovereign Regulator-Led Multi-Platform Initiative
**Status:** Phase 3 (Commercialisation). SGD Testnet live. $500M+ in tokenised FX/securities trades executed since Jan 2025.

---

## Project Summary

Project Guardian is a multi-platform tokenised asset initiative led by the Monetary Authority of Singapore (MAS), co-architected with the IMF. It spans 40+ institutions across 7+ jurisdictions testing tokenised fixed income, FX, and asset management. Key participants include JPMorgan (Kinexys), DBS Bank, HSBC, UBS, Standard Chartered, Citi, BNY Mellon, and Fidelity International.

The project operates across 7+ independent blockchain platforms: Polygon, Ethereum, Stellar, Aptos, Arbitrum, Avalanche, and Base. Interoperability is provided by Chainlink CCIP, Swift integration, and HTLCs. Token standards include ERC-3643, CMTAT, and Stellar SAC.

---

## Platform-Level Analysis

### Polygon / Ethereum / EVM Chains
| Dimension | Score | Evidence |
|-----------|-------|----------|
| Architecture | 2 | Account-based. Well-understood EVM model. Open standard with multiple implementations. |
| Consensus | 2 | Public PoS. Permissioned access layer (Aave Arc fork, verifiable credentials). |
| Scalability | 3 | Base layer congestion well-documented. Account-based hot-spot contention on high-frequency single-asset operations. |
| Mutation Authority | 2 | Hard fork governance via EIPs. Community-driven. No single admin. |

**Independent verification: STRONG.** Most battle-tested blockchain infrastructure.

### Stellar
| Dimension | Score | Evidence |
|-----------|-------|----------|
| Architecture | 2 | Federated Byzantine Agreement. SAC token standard (SEP-41 interface). |
| Consensus | 2 | SCP (Stellar Consensus Protocol). Federated model with quorum slices. |
| Scalability | 2 | ~1,000 TPS documented. Adequate for institutional settlement volumes. |
| Mutation Authority | 3 | Stellar Development Foundation controls roadmap. Core team governance. |

**Independent verification: MODERATE.** Public network with documented performance history.

### Aptos
| Dimension | Score | Evidence |
|-----------|-------|----------|
| Architecture | 2 | Move language. Fungible Asset (FA) standard. Novel but not battle-tested. |
| Consensus | 2 | AptosBFT. High-throughput consensus. |
| Scalability | 2 | Claims 160,000+ TPS (theoretical). Limited independent verification for complex DeFi operations. |
| Mutation Authority | 3 | Aptos Labs controls development. Relatively new chain with VC-funded team. |

**Independent verification: WEAK.** New chain, TPS claims not independently verified under realistic conditions.

### Chainlink CCIP (Interoperability Layer)
| Dimension | Score | Evidence |
|-----------|-------|----------|
| Architecture | 2 | Cross-chain protocol. Private Transactions feature for settlement data. |
| Consensus | 2 | Decentralised oracle network. Risk Management Network monitors cross-chain transactions. |
| Scalability | 2 | Designed for cross-chain messaging, not high-frequency settlement. |
| Mutation Authority | 3 | Chainlink Labs controls development. Multisig governance with undisclosed thresholds. |

**Independent verification: MODERATE.** Well-established oracle infrastructure, CCIP newer.

---

## The Multi-Platform Dependency Problem

Guardian has **MORE platforms than Acacia** (7+ vs 5) but with a critical structural difference: the platforms serve different asset classes rather than all supporting a single settlement flow. JPMorgan's Kinexys DeFi trials run on Polygon, UBS tokenised funds on Ethereum+Chainlink, DBS FX on separate infrastructure. This means the dependency decomposition applies **per use case** rather than systemically.

However, the Global Layer One (GL1) initiative aims to create a common cross-border settlement layer — at which point all platforms must interoperate for atomic settlement. When GL1 matures, the dependency decomposition problem becomes identical to Acacia's.

**Current assessment: modular per use case, converging toward systemic.**

### Mutation Authority Count
At project level: MAS + JPMorgan Kinexys + Ethereum core devs + Polygon governance + Stellar Development Foundation + Aptos Labs + Chainlink Labs + Swift = **8+ mutation authorities** (worst case under GL1).

Per use case: typically 2-3 (MAS + 1-2 platform governance bodies).

---

## Account-Based Hot-Spot Analysis

Same structural issue as Acacia for the SGD Testnet CBDC use case. When wholesale CBDC is the single dominant settlement asset on EVM/Polygon, all state updates to that asset must be serialised. Benchmark TPS (measured across independent accounts) does not reflect single-asset contention performance.

However, Guardian's modular structure means different asset classes settle on different chains. This distributes the hot-spot risk rather than concentrating it. The risk concentrates specifically on the SGD Testnet / wholesale CBDC settlement function.

---

## Cell-by-Cell Scoring

Scored at the overall project level with dependency decomposition applied where relevant.

| Cell | Score | Justification |
|------|-------|---------------|
| **na** | 3 | 7+ platforms, different paradigms (EVM account-based, Stellar FBA, Aptos Move, Corda bilateral). Interoperability via CCIP + Swift + HTLCs introduces additional architectural surface. No unified settlement layer yet (SGD Testnet is partial). |
| **nc** | 3 | Permissioned access across all platforms. Institutional whitelisting (ERC-3643). Validators controlled by platform operators. |
| **ns** | 3 | Account-based hot-spot contention for CBDC settlement. 3,000 TPS benchmark target not independently verified. No published stress test reports. Cross-chain settlement throughput unmeasured. |
| **se** | 3 | Multi-platform execution dependency on 7+ providers with independent upgrade cycles. Chainlink CCIP and Swift bridges add execution surface. Smart contract modifications required (Aave Arc fork, Uniswap OTC pricing). |
| **sm** | 3 (base) / 4 (eff) | MAS has stronger central authority than Acacia's steering committee — sovereign regulator vs coordinating body. But cannot control Ethereum core devs, Polygon governance, Aptos Labs, or Stellar Foundation. **sm_eff = min(4, 3 + ⌈log₂(7)⌉) = 4** under full GL1 convergence. Currently modular: ~3 per use case. |
| **sf** | 2 | Institutional mandate. $500M+ in live trades. No token dependency. But sandbox exit requirements unclear. Commercial viability beyond pilot unproven. |
| **ls** | 1 | MAS is sovereign regulator. Clear sandbox framework. Multiple jurisdictions cooperating (FSA, FINMA, FCA, IMF). |
| **lr** | 1 | Established remedies in Singapore. Sandbox safeguards include defined exposure limits. International regulatory cooperation. |
| **lp** | 2 | Multi-jurisdictional liability complex across 7 jurisdictions. 40+ institutions. Cross-platform partial settlement liability unclear. Sandbox exit liability framework undefined. |

---

## Aggregate Scores

### Using sm_base = 3 (current modular state)

| Domain | Score |
|--------|-------|
| **Network** | (3+3+3)/3 = **3.00** |
| **System State** | (3+3+2)/3 = **2.67** |
| **Law** | (1+1+2)/3 = **1.33** |
| **Overall** | 21/9 = **2.33** |

### Using sm_eff = 4 (under GL1 convergence)

| Domain | Score |
|--------|-------|
| **Network** | **3.00** |
| **System State** | (3+4+2)/3 = **3.00** |
| **Law** | **1.33** |
| **Overall** | 22/9 = **2.44** |

---

## Threshold Analysis

### Current Rule (overall ≥ 2.5): PASSES at 2.33/2.44

### Domain Ceiling Rule (overall ≥ 2.5 OR domain > 3.0): **BORDERLINE / FLAGGED**
- Current modular state: Network = 3.00 (at boundary). Borderline.
- GL1 convergence: Network = 3.00 AND System State = 3.00 (at boundary). Borderline.

### Asymmetric Weighting
Under asymmetric weights, the elevated Network scores (nc=3, na=3, ns=3) pull disproportionately upward while the Law domain protective scores are discounted. Effective risk is higher than the flat average suggests.

---

## Comparison with Project Acacia and Fnality

| Feature | Guardian | Acacia | Fnality |
|---------|----------|--------|---------|
| **Platforms** | 7+ | 5 | 1 (Autonity) |
| **Central Authority** | MAS (sovereign) | RBA (steering) | BoE (oversight) + consortium |
| **Mutation Authorities** | 8+ (project) / 2-3 (per use case) | 7 | 2 (consortium + Clearmatics) |
| **Settlement Model** | DvP/DvPvP (chain-specific) | Cross-chain DvP | Atomic PvP (single chain) |
| **Legal Framework** | Sandbox (exit unclear) | Pilot (research phase) | Settlement finality designated |
| **Overall Score** | 2.33 / 2.44 | 2.44 | ~2.00 |

### Key Insight: Platform Count Drives Risk Differentiation

All three projects have:
- Strong legal frameworks (Law domain ≈ 1.33)
- Institutional backing from major banks
- Central bank involvement
- Similar governance models

The variation in overall scores is driven almost entirely by platform dependency count:
- **Fnality (N=1)**: 2.00 — single platform, single settlement paradigm
- **Guardian (N=7+, modular)**: 2.33 — many platforms but currently siloed per use case
- **Acacia (N=5, systemic)**: 2.44 — all platforms must interoperate for core settlement
- **Guardian (N=7+, GL1 converged)**: 2.44 — matches Acacia when platforms must interoperate

This gradient directly validates the dependency decomposition extension. Same governance quality, same legal quality, dramatically different risk profiles based on platform count.

---

## Structural Advantages Over Acacia

1. **Modular architecture**: Different use cases can fail independently without systemic cascade.
2. **MAS sovereign authority**: Stronger than a steering committee; can mandate compliance.
3. **Live commercial execution**: $500M+ proves the concept works at scale for current use cases.
4. **Open standard preference**: EVM/Ethereum focus means broader developer ecosystem and battle-testing.

## Structural Risks Unique to Guardian

1. **GL1 convergence risk**: If GL1 succeeds, Guardian inherits the full multi-platform dependency decomposition problem at 7+ platforms.
2. **Multi-jurisdictional liability**: 7 jurisdictions × 40+ institutions = liability attribution nightmare if cross-border atomic settlement fails.
3. **Sandbox exit uncertainty**: Current success is within sandbox exemptions. Production viability requires full regulatory compliance.
4. **Smart contract fork risk**: Modified Aave Arc, modified Uniswap — forked protocols diverge from upstream security patches.

---

## Propagation Chains

### Primary: Platform Mutation → Cross-Chain Settlement Failure → Jurisdictional Liability Chaos (HIGH)
8+ uncoordinated governance bodies. Polygon upgrade breaks Kinexys DeFi contracts → JPMorgan FX settlement fails → which jurisdiction's remedies apply? Unclear.

### Secondary: Hot-Spot Contention → SGD Testnet Scalability Failure → CBDC Use Case Abandoned (MODERATE)
Same as Acacia for the CBDC-specific function. Less severe because other use cases aren't CBDC-dependent.

### Tertiary: Sandbox Exit → Regulatory Reclassification → Economic Non-Viability (MODERATE)
If full compliance requirements post-sandbox significantly increase costs, commercial viability of tokenised settlement may not survive.

---

## De-Risking Recommendations

### 1. Maintain Modular Architecture — Don't Rush GL1 Convergence — CRITICAL
The modular per-use-case architecture is Guardian's greatest structural advantage. Each use case's risk is bounded. Premature convergence to a unified GL1 settlement layer imports the full dependency decomposition problem.

### 2. Independently Verify Cross-Chain Settlement Under CBDC Conditions — HIGH
Same recommendation as Acacia for the SGD Testnet CBDC function specifically.

### 3. Define Cross-Jurisdictional Liability Framework Before Production — HIGH
7 jurisdictions must agree on liability allocation for partial atomic settlement failures. This must be resolved before sandbox exit.

### 4. Audit Forked Smart Contract Security — HIGH
Modified Aave Arc and Uniswap have diverged from upstream. Security patches to the original protocols may not be applied to the forks.

### 5. Secure Platform-Layer Change Notification Agreements — MEDIUM
Same as Acacia: 90-day notice, backward compatibility requirements from each platform governance body.

---

*Assessment generated using BREM framework (Price, 2026) with platform-level decomposition and dependency decomposition extensions.*
