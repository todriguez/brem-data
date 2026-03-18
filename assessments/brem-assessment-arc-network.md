# BREM Risk Assessment: Arc Network
## Circle — Institutional Stablecoin Finance L1

**Assessment Date:** 9 March 2026
**Classification:** Enterprise/Institutional — Permissioned L1 for Stablecoin Finance
**Status:** Public testnet. Mainnet TBD. Pre-production.
**Methodology:** Dual-track scoring — expert judgment (cell anchors) and branching instrument (decision tree) applied independently, then compared.

---

## Project Summary

Arc is a purpose-built Layer-1 blockchain developed by Circle Internet Group (NYSE: CRCL) for institutional stablecoin finance — cross-border payments, FX settlement, and multi-currency stablecoin operations. It uses Malachite BFT consensus (Tendermint-derived, developed by Informal Systems under Circle contract), Reth-based EVM execution, and USDC as the native gas token.

**Key design choices:**
- **USDC-native** — gas fees paid in USDC; no volatile native token (exploring governance token, not committed)
- **Permissioned validators** — 20 validators on testnet, regulated institutions across 10+ jurisdictions (targeting Goldman Sachs, BlackRock, HSBC, etc.)
- **EVM-compatible** — standard Solidity tooling; proprietary extensions for USDC blocklist enforcement and modified fee mechanism
- **Refund Protocol** — application-layer dispute resolution via escrow and authorised arbiters; does not violate base-layer finality
- **~3,000 TPS** with 350ms finality (20 validators); up to 10,000 TPS in optimised configs

**Interoperability:** Chainlink CCIP (oracle/cross-chain), LayerZero and Wormhole (bridging), but these are for interop, not core settlement.

---

## Cell-by-Cell Scoring

### na — Architecture: **2**

**Track 1 (Expert):** Account-based EVM (Reth execution layer). Shared mutable global state. Single coherent platform, no cross-chain requirement for core function. Modular separation of consensus (Malachite) and execution (Reth), both controlled by Circle. EVM-compatible means well-understood execution model. USDC blocklist enforcement and modified fee mechanism are proprietary extensions but the core is recognisable EVM.

**Track 2 (Instrument):**
- Q1: Shared mutable global state → Floor = 2
- Q2: No off-chain fragmentation required for target workload; monolithic architecture → Score = 2
- Q4: USDC blocklist enforcement baked into consensus, not middleware; oracle/bridge integrations are for interop, not core settlement → No change

**Score: 2** — Deterministic core with middleware-level customisations. Standard EVM with proprietary compliance extensions. Not as constrained as standard Ethereum, not as fragmented as L2-dependent chains.

**Tracks agree: ✅**

---

### nc — Consensus: **3**

**Track 1 (Expert):** Malachite BFT (Tendermint-derived). 20 permissioned validators on testnet. Validators are regulated institutions — independently controlled entities with their own compliance obligations across 10+ jurisdictions. Unlike Tempo where all 4 validators are Stripe-controlled, Arc's validators are independent entities with genuinely divergent interests. BFT is meaningful because the Byzantine threat model applies — these entities could actually be adversarial. However, Circle controls validator admission/removal and upgrades. The validator set is permissioned, not permissionless.

**Track 2 (Instrument):**
- Q1: Permissioned — requires approval from Circle → Floor = 3
- Q2: Yes — independently controlled entities with divergent interests (regulated institutions across 10+ regions) → Score = 3
- Q4: Circle coordinates upgrades; validators can collectively refuse (2/3 majority) but Circle controls the upgrade proposal process → Floor = max(3, 2) = 3

**Score: 3** — Permissioned set of pre-approved consensus participants. Textbook nc=3. Multiple independent institutions, but admission controlled by Circle. This is the key structural difference from Tempo (nc=4): Arc's validators are genuinely independent, making BFT consensus meaningful rather than ceremonial.

**Tracks agree: ✅**

---

### ns — Scalability: **2**

**Track 1 (Expert):** ~3,000 TPS with 20 validators. Up to 10,000 TPS in optimised configs. 350ms finality. For institutional FX settlement and cross-border payments, 3,000 TPS is adequate — these are high-value, lower-frequency transactions. Not positioned for AI micropayments. Monolithic architecture, not reliant on rollups or sharding. Multi-currency stablecoin support distributes contention. Key tension: adding more validators (roadmap to 100+) actually reduces throughput (780ms at 100 validators) — fundamental tradeoff between validator count and performance.

**Track 2 (Instrument):**
- Q1: Moderate — 3,000 TPS adequate for institutional FX but limited headroom; performance degrades with more validators → Score = 2
- Q4: Individual settlement at protocol speed, no batching/netting required → No change
- Q5: USDC is native gas and primary settlement asset, but multi-currency support distributes load → No change

**Score: 2** — Scales within bounds but protocol-level ceiling from BFT consensus with 20 validators. Adequate for target use case with limited headroom.

**Tracks agree: ✅**

---

### se — Execution: **2**

**Track 1 (Expert):** Full EVM compatibility (Reth). Turing-complete with gas limits. Standard execution model. USDC blocklist enforcement at three tiers (pre-mempool, post-mempool, runtime) is non-standard execution behaviour — transactions can be censored or reverted based on Circle's blocklist. The Refund Protocol provides application-layer dispute resolution that doesn't violate base-layer finality (escrow + arbitration, not chain rollback).

USDC is native to the chain — not a wrapped asset or custodial representation. USDC is Circle's liability by design (fully backed by reserves), but it's the native unit, not a derivative. The blocklist enforcement is a censorship capability (captured in sm), not an execution model risk.

**Track 2 (Instrument):**
- Q1: Turing-complete VM with resource limits (EVM/gas) → Score = 2
- Q4: USDC is native to the chain, not wrapped/bridged. Circle backs USDC with reserves, but USDC is the native unit, not a derivative → No change

**Score: 2** — Standard EVM with gas limits. USDC is native, not custodial. The blocklist enforcement is a mutation authority concern (sm), not an execution model risk. Compare with Tempo se=3 where TIP-20 tokens are custodial IOUs — Arc's USDC is the real asset, not a representation.

**Tracks agree: ✅**

*Note: The instrument does not explicitly capture execution-level censorship (blocklist enforcement). This is by design — censorship reflects who controls the system (sm), not how state transitions work (se). Recorded in se Q5 context for reporting.*

---

### sm — Mutation Authority: **4** ← THE DIAGNOSTIC VARIABLE

**Track 1 (Expert):** Circle controls validator set (admission/removal), protocol upgrades and parameters, fee structure modifications, USDC blocklist administration. No published governance framework beyond "permissioned PoA". No native governance token (exploring, not committed). No time-locks, community veto, or formal constraint mechanisms documented. Near-total control.

Counterpoint: validators are independent institutions that can collectively refuse upgrades (2/3 BFT). But Circle controls validator admission — if a validator refuses, Circle could in principle remove them. This is an institutional check, not a protocol-level check.

**Track 2 (Instrument):**
- Q1: Yes — Circle has unilateral authority over validator set, upgrades, parameters → Floor = 3
- Q2: Validators can collectively refuse (2/3 BFT), but Circle controls admission. No published governance framework, no time-locks, no community veto → Score = 4
- Q4: Upgrade mechanisms not publicly documented → Floor = max(4, 3) = 4

**Score: 4** — Core protocol arbitrarily mutable. Circle can change anything. No published constraints. BREM scores architecture, not institutional quality. Circle's status as a publicly traded, multi-jurisdictionally regulated company creates external constraints, but protocol-level, Circle has database-admin-level discretion.

**Tracks agree: ✅**

---

### sf — Economic Fitness: **1**

**Track 1 (Expert):** USDC as native gas — no volatile token. $0.01/tx fee target. Circle is a publicly traded company with real revenue (USDC reserve yield, $1.7B revenue in 2024). Stablecoin-native economy avoids token appreciation dependency. Pre-production (testnet only), no mainnet revenue yet. Exploring native Arc token for governance/incentives — if launched, this would introduce token dynamics, but currently no token.

**Track 2 (Instrument):**
- Q1: Transaction fees from real economic activity; no native token dependency → Ceiling = 1
- Q3: At $0.01/tx, standard payments viable but micropayments (sub-cent) not → Score = 1
- Q4: na = 2, so no architectural ceiling on economics → No change
- Q5: Pre-production (testnet only) → Floor = max(1, 1) = 1

**Score: 1** — Economically coherent. Real revenue parent company, no token dependency, sustainable fee model. Not sf=0 because pre-production and micropayments not viable at $0.01/tx. Compare with Tempo sf=1 at $0.001/tx — Tempo's 10x lower fees give it micropayment optionality that Arc lacks, though both land at sf=1.

**Tracks agree: ✅**

---

### ls — Standing: **1**

**Track 1 (Expert):** Circle is regulated in multiple jurisdictions (UK, Singapore, Bermuda, ADGM, NYDFS, MiCA-compliant, OCC conditional approval). USDC is the most regulated stablecoin globally. Clear legal entity (Circle Internet Group, Inc., NYSE-listed). All participants identifiable through regulated infrastructure (KYC enforcement, permissioned validator access). Standing is strong through institutional framework, not through cryptographic attribution.

**Track 2 (Instrument):**
- Q1: Yes — continuous institutional attribution. All participants identified through regulated infrastructure (KYC/AML enforcement, permissioned access, NYSE-listed regulated entity operates the system) → Ceiling = 1
- Q3: Mostly — standing established with standard evidentiary work; USDC regulatory framework clear across multiple jurisdictions → Score = 1
- Q4: Multi-jurisdiction operation, but Circle holds licenses in each jurisdiction (harmonised through Circle's licensing framework) → No change

**Score: 1** — Standing established through institutional attribution. Circle's regulatory infrastructure provides standing equivalent to cryptographic attribution through institutional means.

**Tracks agree: ✅** (after ls Q1 instrument revision to recognize institutional attribution as equally valid path)

*Note: The initial instrument version gated ls Q1 exclusively on UTXO chain-of-title, which would have scored ls=2. Arc's dual-track comparison revealed this bias and prompted a revision to recognise institutional attribution (regulated entity with KYC enforcement) as an equally valid path to ls=0-1. See instrument v1.1 revision notes.*

---

### lr — Remedy: **1**

**Track 1 (Expert):** The Refund Protocol is genuinely differentiated — application-layer dispute resolution providing escrow, authorised arbiters, and counter-payments. This preserves base-layer finality (consensus blocks are immutable) while offering structured remedy through smart contracts. Circle's regulatory licenses provide court-enforceable remedies. Validators are regulated institutions subject to legal process.

**Track 2 (Instrument):**
- Q1: Yes — Circle regulated, courts have jurisdiction, Refund Protocol provides structured remedy → Ceiling = 1
- Q3: Available to all participants — Refund Protocol is protocol-level, not selective → Score = 1
- Q4: No — the Refund Protocol explicitly provides dispute resolution → No change

**Score: 1** — Remedies available through standard process. The Refund Protocol is a notable innovation — blockchain-native remedy without sacrificing finality. Compare with Tempo lr=1 which relies on Stripe's existing chargeback infrastructure.

**Tracks agree: ✅**

---

### lp — Liability Precision: **2**

**Track 1 (Expert):** Circle Internet Group is the identifiable liable party (publicly traded, regulated). Validators are regulated institutions with compliance obligations. But: CTS (Circle Technology Services) disclaims liability in testnet terms. Mainnet liability framework TBD. Blockchain-specific liability (consensus failure, smart contract bugs) is novel territory. Cross-border operations across 10+ jurisdictions add complexity, though Circle holds licenses in each.

**Track 2 (Instrument):**
- Q1: Yes — Circle Inc., NYSE-listed, multi-jurisdictionally regulated → Ceiling = 1
- Q3: Mostly — general framework applies, blockchain-specific gaps manageable → Score = 1
- Q4: Yes — 10+ jurisdictions, no harmonised framework → Floor = max(1, 2) = 2
- Q5: No — Circle holds licenses in each jurisdiction → No change

**Score: 2** — Liability ambiguous in some areas. Clear liable party (Circle), but cross-border and blockchain-specific liability gaps exist. The Q4 cross-jurisdiction override correctly raises this from 1 to 2.

**Tracks agree: ✅**

---

## Aggregate Scores

| Domain | Cells | Domain Score |
|--------|-------|-------------|
| **Network** | na=2, nc=3, ns=2 | **2.33** |
| **System State** | se=2, sm=4, sf=1 | **2.33** |
| **Law** | ls=1, lr=1, lp=2 | **1.33** |
| **Overall** | | **2.00** |

---

## Extension Analysis

### Dependency Decomposition

**N = 1** (single platform — Arc). No multi-platform dependency. sm_eff = sm_base = 4.

Chainlink, LayerZero, and Wormhole integrations are for interoperability, not core settlement. Settlement occurs natively on Arc. Not equivalent to the multi-platform governance dependencies in Guardian or Acacia.

### Domain Ceiling Rule

- Network: 2.33 — below 3.0 ✓
- System State: 2.33 — below 3.0 ✓
- Law: 1.33 — below 3.0 ✓
- Overall: 2.00 — below 2.5 ✓

**PASSES all thresholds.** Comfortable margin on all rules.

### Asymmetric Weighting

sm=4 receives full w_up penalty. nc=3 is at the nc=3 threshold (35.7% failure rate — elevated but not maximal). sf=1 and ls=1 receive discounted w_down protection.

Asymmetric weighted overall: ~2.15 (below 2.7 threshold).

**PASSES asymmetric threshold.**

---

## Threshold Analysis

| Method | Score | Threshold | Status |
|--------|-------|-----------|--------|
| Base | 2.00 | 2.5 | **PASSES** (comfortable margin) |
| Domain ceiling | max 2.33 | 3.0 | **PASSES** (comfortable margin) |
| Asymmetric weighted | ~2.15 | 2.7 | **PASSES** (comfortable margin) |

---

## Dual-Track Comparison Summary

| Cell | Expert | Instrument | Match? |
|------|--------|------------|--------|
| na | 2 | 2 | ✅ |
| nc | 3 | 3 | ✅ |
| ns | 2 | 2 | ✅ |
| se | 2 | 2 | ✅ (instrument misses blocklist censorship — by design, captured in sm) |
| sm | 4 | 4 | ✅ |
| sf | 1 | 1 | ✅ |
| ls | 1 | 1 | ✅ (after instrument Q1 revision to recognise institutional attribution) |
| lr | 1 | 1 | ✅ |
| lp | 2 | 2 | ✅ |

**Expert overall: 2.00 | Instrument overall: 2.00 | Divergence: 0.00**

All 9 cells agree after the ls Q1 revision. The single pre-revision divergence (ls: Expert 1, Instrument 2) revealed and corrected a systematic bias in the instrument's gating question — the original Q1 privileged UTXO chain-of-title as the only path to ls=0-1, penalising account-based systems regardless of institutional standing.

---

## Comparison with Cross-Validation Projects

| Project | N | sm | nc | ns | sf | Overall | Key Difference |
|---------|---|----|----|----|----|---------|----------------|
| **Arc Network** | 1 | **4** | **3** | **2** | **1** | **2.00** | Independent validators, USDC native, no token, institutional FX |
| Fnality | 1 | 3 | 3 | 2 | 2 | 2.00 | Consortium (24 shareholders), BoE settlement finality |
| Stripe Tempo (V2) | 1 | 4 | 4 | 1 | 1 | 2.11 | Single entity controls everything, extreme scalability |
| Guardian (modular) | 7+ | 3-4 | 3 | 3 | 2 | 2.33 | Multi-platform, sovereign regulator |
| Acacia (systemic) | 5 | 4 | 3 | 3 | 2 | 2.44 | Multi-platform, CBDC, systemic dependency |

**Arc vs Tempo:** Same sm=4, but Arc's nc=3 (independent validators) vs Tempo's nc=4 (all Stripe-controlled) is the critical structural difference. Arc's validators being independent institutions makes BFT consensus meaningful — the Byzantine threat model applies. Tempo's ns=1 (20,000 TPS) vs Arc's ns=2 (3,000 TPS) reflects Tempo's scalability advantage, but both are adequate for their target use cases. Arc's se=2 (USDC native) vs Tempo's se=3 (Bridge custodial IOUs) is the second major differentiator — Arc doesn't depend on a custodial intermediary for its core asset.

**Arc vs Fnality:** Same overall (2.00), same nc=3, same ns=2. The structural difference is sm: Arc's sm=4 (Circle controls everything) vs Fnality's sm=3 (bounded consortium governance with BoE oversight). Fnality's regulatory structure provides protocol-level constraints that Arc lacks. Arc compensates with sf=1 (real revenue, no token) vs Fnality's sf=2.

---

## Instrument Refinement Findings

This dual-track assessment prompted two instrument refinements:

### 1. ls Q1 Revision: Institutional Attribution Path (v1.1)

**Problem:** Original Q1 gated exclusively on "continuous chain of attributable signatures" — a UTXO-specific property. Account-based systems with strong institutional attribution (regulated entity, KYC enforcement, permissioned access) were forced into ls=2 regardless of their standing.

**Fix:** Q1 now recognises two equally valid paths to ls=0-1: (a) cryptographic attribution (UTXO chain-of-title) and (b) institutional attribution (regulated entity with KYC enforcement). The question now asks whether transaction parties can be reliably attributed to identifiable legal persons, regardless of mechanism.

**Impact:** Arc ls moves from 2 to 1 (instrument now matches expert judgment). This also correctly handles other account-based regulated platforms (e.g., Tempo ls=1, enterprise banking chains).

### 2. se Q5 Context: Execution-Level Censorship

**Problem:** Arc's USDC blocklist enforcement at the consensus level is a non-standard execution behaviour not captured by the instrument's se questions.

**Resolution:** This is correctly a mutation authority concern (sm), not an execution model risk (se). Circle's ability to censor addresses reflects who controls the system, not how state transitions work. Added as a Q5 context option in se for reporting purposes, with explicit note that censorship is scored through sm.

---

## De-Risking Recommendations

### 1. Publish Governance Framework — CRITICAL (sm: 4→3)
Circle should publish formal governance constraints: upgrade procedures, time-locks, validator veto mechanisms, parameter change processes. Even retaining significant control, formalising constraints moves sm from 4 (unconstrained) to 3 (bounded discretion). This is particularly important given that independent validators provide a natural check if governance rules are published.

### 2. Commit to Validator Expansion Timeline — HIGH (nc: 3→2)
Roadmap to 100+ validators is stated but uncommitted. Define specific milestones with dates. The validator-throughput tradeoff (780ms at 100 validators) should be addressed through engineering, not by capping validator count.

### 3. Address Validator-Performance Tradeoff — HIGH (ns: 2→1)
The fundamental tension between validator count and throughput (3,000 TPS at 20 validators, degrading with more) must be resolved before mainnet. If the roadmap calls for 100+ validators, the consensus mechanism needs optimisation to maintain target throughput.

### 4. Define Mainnet Liability Framework — HIGH (lp: 2→1)
CTS testnet terms disclaim liability. Mainnet requires clear SLAs, validator liability allocation, and blockchain-specific failure mode coverage. Circle's regulatory licenses provide the institutional foundation; they need protocol-specific liability terms.

### 5. Governance Token Risk — MEDIUM (sf: 1→2 if mishandled)
If the explored native Arc token is launched, careful design is essential to avoid introducing token appreciation dependency or governance capture dynamics. The current no-token model is a strength (sf=1). A governance token that introduces speculative dynamics would raise sf and potentially sm.

---

## Summary

Arc Network presents a balanced BREM profile: **strong legal standing and economics (ls=1, lr=1, sf=1) paired with maximum mutation authority (sm=4) but meaningful consensus independence (nc=3)**. The overall score of **2.00** passes all three threshold rules with comfortable margins.

The critical structural difference from Tempo (2.11) is the validator model: Arc's independently controlled institutional validators make BFT consensus meaningful (nc=3), while Tempo's single-entity validators make it ceremonial (nc=4). Arc's native USDC avoids the custodial intermediary dependency that raised Tempo's se from 2 to 3.

The primary risk is sm=4 — Circle controls everything at the protocol level. But unlike projects where sm=4 is combined with weak economics (sf=3-4) or fragmented architecture (ns=3-4), Arc's strong economic fundamentals and adequate scalability prevent the failure cascades that typically accompany maximum mutation authority.

**De-risking path:** Publishing governance constraints (sm: 4→3) and resolving the validator-throughput tradeoff (ns: 2→1) would move the overall score from 2.00 toward ~1.67. Both are achievable and within Circle's control.

This assessment also served as a calibration exercise for the branching scoring instrument. The dual-track comparison identified and corrected a systematic bias in the ls gating question (UTXO-only attribution path) and confirmed that execution-level censorship is correctly captured through sm rather than se.

---

*Assessment generated using BREM framework (Price, 2026) with dual-track scoring methodology (expert judgment + branching instrument). Cross-validated against 83-project dataset and four prior assessments.*
