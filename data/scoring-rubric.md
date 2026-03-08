# BREM Scoring Rubric
**Blockchain Risk Evaluation Model — Cell-Level Scoring Anchors (0–4)**

## Overview

Each of the nine BREM cells is scored on a 0–4 integer scale. The scale measures a domain-specific dimension: **protocol determinism** (Network), **mutation authority** (System State), and **legal enforceability** (Law). A score of 0 represents maximum constraint; a score of 4 represents unbounded discretion or structural failure.

Scores are assigned by evaluating a project against the cell-level anchors below. Each anchor defines what a given score means for that specific cell, with reference examples. Where multiple indicators pull in different directions, the score reflects the dominant structural characteristic — BREM scores architecture, not intent.

## Scoring Methodology

### Evidence Hierarchy
1. **Primary Sources** (strongest): Official documentation, whitepapers, regulatory filings, on-chain data, court documents
2. **Secondary Sources**: Industry reports, press releases, verified announcements, academic analysis
3. **Inferred**: Analysis based on technical architecture patterns, comparable projects

### Calibration
- Expert anchor scores established for representative projects across all 9 cells
- Systematic scorer uses weighted blend of expert anchors and computed baselines derived from project metadata
- All scores include evidence citations (see `evidence_summary` and `primary_data_sources` columns in dataset)

### Validation
- Cohen's κ = 0.81 at cell level, 0.91 at domain level (inter-rater reliability)
- 10 projects scored blind to outcome: 89% concordance with post-hoc scores
- Conservative bias: when uncertain, score toward higher risk (higher number)

---

## NETWORK DOMAIN — Protocol Determinism (Machine ↔ Machine)

The Network domain measures how fully the protocol constrains outcomes without external layers, special committees, or operational intervention. A score of 0 means the machine guarantees correctness by construction. A score of 4 means outcomes depend on mutable shared state, sequential bottlenecks, and frequent exceptional handling.

### Architecture (na) — Structure

| Score | Anchor | Reference Examples |
|-------|--------|--------------------|
| **0** | UTXO-style independent spends; contention resolved at record level; no shared mutable state; verifiable without global state | Bitcoin |
| **1** | Mostly deterministic; constraints are formal and stable; bounded operational choices within protocol rules | BSV, Litecoin |
| **2** | Deterministic core but relies on middleware, coordinators, or recommended patterns for normal operation | Ethereum (post-merge), Corda |
| **3** | Scaling or security depends on multi-party coordination or adjunct layers (L2/bridges required, not optional) | Polygon, Arbitrum, most enterprise chains |
| **4** | Shared mutable global state; sequential execution bottleneck; contention is systemic; outcomes depend on implementation choices | ASX CHESS, custom monolithic enterprise platforms |

### Consensus (nc) — Process

| Score | Anchor | Reference Examples |
|-------|--------|--------------------|
| **0** | Objective work-based ordering (PoW); finality determined by energy expenditure; competitive incentive to invest in infrastructure creates scaling pressure by construction | Bitcoin PoW |
| **1** | Formal protocol rules with narrow parameterisation; consensus participants have limited operational discretion; infrastructure investment incentives mostly intact | BSV (PoW with large blocks) |
| **2** | Hybrid mechanisms; some governance coupling to validator selection but bounded by protocol rules; mixed incentive structure | Ethereum PoS, Cosmos Tendermint |
| **3** | Stake-weighted voting with delegation; returns proportional to weight not compute; rational strategy is rent-seeking over infrastructure reinvestment; governance decisions affect security | Cardano, Solana, consortium BFT chains |
| **4** | Subjective/social finality; heavy governance coupling to consensus security; committee-based block production; no competitive pressure to scale infrastructure; named authorities control consensus | Private chains with designated authorities, Stripe Tempo (4 Stripe-controlled validators) |

### Scalability (ns) — Persistence

| Score | Anchor | Reference Examples |
|-------|--------|--------------------|
| **0** | Scales via native parallelism without mandatory off-chain fragmentation; pipeline architecture with measurable stage capacities | BSV (unbounded blocks, SPV) |
| **1** | Linear scaling with hardware; minor coordination overhead; no mandatory fragmentation; predictable graceful degradation | Stripe Tempo (20k TPS testnet), Solana |
| **2** | Scales within bounds but requires operational tuning; some throughput ceiling from protocol design | Ethereum L1, Hyperledger Fabric |
| **3** | Requires fragmentation/bridges/L2 as baseline operation; throughput limited by base-layer protocol | Ethereum ecosystem (L2-dependent), most consortium pilots |
| **4** | Hard protocol-level throughput ceiling; scaling requires fundamental architectural changes or mandatory off-chain layers | ASX CHESS, early enterprise chains |

---

## SYSTEM STATE DOMAIN — Mutation Authority (Human ↔ Machine)

The System State domain measures what has authority to cause state transitions — both data-level inputs and protocol-level changes. A blockchain is a complete economic system: the protocol, the node operators, and the users. The critical question is the scope of mutation authority: who can write data to the system, who can change the execution environment, who can modify network parameters, and who can mutate the protocol itself. A protocol should be a fixed set of rules and data structures. Mutation authority measures how much discretion has been introduced into that fixed system.

### Execution (se) — Structure

| Score | Anchor | Reference Examples |
|-------|--------|--------------------|
| **0** | Bounded execution model; no halting problem; no reentrancy class; no bridge/protocol dependencies; deterministic script execution | Bitcoin Script |
| **1** | Safe execution with formal constraints; upgrade paths are protocol-governed; minimal attack surface | BSV (bounded script) |
| **2** | Turing-complete execution with safeguards; some reentrancy risk mitigated by patterns; bridge dependencies for specific use cases | Ethereum (EVM with gas limits), Corda |
| **3** | Execution environment suffers known vulnerability classes; requires constant monitoring; bridge protocols or trusted intermediaries as operational dependency | Platforms dependent on custodial intermediaries (e.g., Tempo/Bridge), complex DeFi composability |
| **4** | Halting-problem exposure; reentrancy attacks demonstrated; state explosion risk; bridge failures cause systemic loss; upgrade hazards; trust-dependent logic | The DAO (pre-fork), early Solidity platforms |

### Mutation Authority (sm) — Process ← THE DIAGNOSTIC VARIABLE

**sm is the single strongest predictor of project failure.** In the 83-project dataset: sm ≤ 2 → 0% failure rate; sm = 3 → 35.7%; sm = 4 → 73.5%.

| Score | Anchor | Reference Examples |
|-------|--------|--------------------|
| **0** | Protocol immutable; no party can mutate core rules or data structures; data ingress governed by deterministic validation; operational governance via legal framework without protocol mutation | Uniswap V2/V3 (immutable contracts, zero admin keys) |
| **1** | Mutation scope narrow and formally constrained; only off-chain parameters adjustable; data input rules fixed by protocol; decision authority clearly allocated with minimal discretion | Bitcoin (BIP process, no admin keys) |
| **2** | Network parameters mutable within protocol-defined bounds (gas limits, block size); oracle/data feed rules require some coordination; moderate governance overhead | Ethereum (EIP process, community governance), BSV (miner-governed parameters) |
| **3** | Execution environment and economic rules mutable (VM upgrades, fee structures); protocol changes achievable through governance process; data input rules subject to committee decisions; authority unclear; cartel capture risk | Consortium chains (R3 Corda network), Aave (governance token control), most enterprise pilots |
| **4** | Core protocol arbitrarily mutable; any rule or data structure can change through governance or admin keys; consensus rules, architecture, and scalability subject to discretionary mutation; data ingress rules politically determined; upgrade authority concentrated | ASX CHESS, FTX, Celsius, Stripe Tempo (single entity, no published governance framework) |

### Economic Fitness (sf) — Persistence

| Score | Anchor | Reference Examples |
|-------|--------|--------------------|
| **0** | Unit economics viable at scale without subsidy; fee predictability; micropayment-viable; cost-to-verify proportional to value | Bitcoin (fee market), BSV (sub-cent fees) |
| **1** | Economically coherent; sustainable fee model; minor scaling friction; viable for most business models; real revenue from real activity | Stripe Tempo (no token, $0.001/tx, $400B stablecoin volume), Uniswap (trading fees) |
| **2** | Economics work for high-value transactions; micropayments not viable; some subsidy or cross-subsidisation required | Ethereum (high gas fees limit use cases), most enterprise pilots (unclear revenue model) |
| **3** | Unit economics depend on token appreciation or external subsidy; fee volatility; business models constrained by throughput costs | Many DeFi protocols (token incentive dependent), Solana (VC-subsidised) |
| **4** | Economics only viable via continuous external support; transaction costs prohibit adoption; no path to micropayment viability; Ponzi dynamics | Terra/Luna (algorithmic peg), Celsius (yield from new deposits), ASX CHESS ($250M sunk, no revenue) |

---

## LAW DOMAIN — Enforceability (Human ↔ Human)

The Law domain evaluates whether blockchain outcomes survive contact with the legal system. It examines whether the asset has standing as property, whether established remedies exist to correct disputes, and whether liability attaches to actors participating in the system.

**The critical primitive is attribution.** Commodity transfers in law occur through contract events represented by signatures that attribute the act of transfer to identifiable parties. A digital asset system maintains legal standing only to the extent that it preserves a continuous chain of attributable signatures linking ownership from one party to the next. Break the chain of signatures and you break the chain of title.

### Standing (ls) — Structure

| Score | Anchor | Reference Examples |
|-------|--------|--------------------|
| **0** | Asset legally recognised as property; continuous chain of attributable signatures; parties identifiable through standard legal process | Bitcoin (CFTC commodity classification, UTXO chain of title) |
| **1** | Standing established with standard evidentiary work; attribution chain mostly intact; identifiable through compulsory process | Regulated stablecoins (USDC), enterprise platforms under financial regulation (Stripe/Tempo) |
| **2** | Standing arguable but requires significant evidence gathering; some attribution gaps; relies on intermediary records | Ethereum tokens (mixed classification), cross-border enterprise deployments |
| **3** | Fragmented attribution; cross-jurisdiction complexity; title ambiguous through bridge crossings or custodial opacity | Multi-chain DeFi positions, L2 bridged assets |
| **4** | Non-attributable; no party to be charged; ownership record does not identify legally responsible actors; standing collapses | Privacy coins (Monero), mixer protocols (Tornado Cash) |

### Remedy (lr) — Process

| Score | Anchor | Reference Examples |
|-------|--------|--------------------|
| **0** | Established legal processes available: injunction, tracing, restitution, constructive trust; property rights recoverable through courts | Bitcoin (multiple successful court recoveries, BSV court orders enforced) |
| **1** | Remedies available through standard process; some operational complexity; courts have jurisdiction and precedent | Regulated financial platforms, enterprise chains under clear jurisdiction |
| **2** | Remedies exist but costly and slow; some dispute categories lack clear process; cross-border enforcement friction | Multi-jurisdiction enterprise deployments, regulated DeFi |
| **3** | Remedy practically unreliable; evidence beyond compulsion; selective enforcement only (insiders recover, others do not) | Most DeFi protocols (code-is-law), FTX (insider recovery only) |
| **4** | No meaningful legal remedy; code-as-law finality; recovery depends on discretionary insider intervention or ad hoc rollback | The DAO (hard fork as "remedy"), anonymous protocols |

### Liability Precision (lp) — Persistence

| Score | Anchor | Reference Examples |
|-------|--------|--------------------|
| **0** | Clear liability assignment; actors identifiable and subject to jurisdiction; IP rights respected; compliance obligations understood | Bitcoin (clear property law framework), regulated enterprise platforms |
| **1** | Liability established with standard legal analysis; obligations attach to identifiable parties; regulatory framework stable | Stripe Tempo (Stripe bears liability as regulated entity), major exchange platforms |
| **2** | Liability ambiguous in some areas; emerging regulatory framework; IP questions partially resolved; compliance costs significant | Ethereum ecosystem (evolving regulation), cross-border enterprise deployments |
| **3** | Liability unclear; actors may be beyond jurisdiction; money transmission questions unresolved; sanctions exposure uncertain | DeFi protocols with anonymous teams, unregistered token offerings |
| **4** | No identifiable liable party; IP violations endemic; money secrecy rules breached; sanctions non-compliance; system cannot persist legally | Terra/Luna (no liable party post-collapse), Tornado Cash |

---

## Scoring Aggregation

### Cell → Domain → Overall

1. **Cell scores**: Integer 0–4 for each of the nine cells
2. **Domain scores**: Arithmetic mean of three cells within each domain (Network, System State, Law)
3. **Overall score**: Arithmetic mean of all nine cell scores (equivalent to mean of three domain scores)

### Threshold Rule

**Overall score ≥ 2.5 indicates elevated failure risk.** In the dataset:
- Sensitivity: 92.5% (captures 37 of 40 failures)
- Specificity among resolved projects: 100% (zero false positives)
- All 5 "false positives" in the full dataset are unresolved projects (still in pilot/development)

### Extensions (V2 Methodology)

Three empirically-derived scoring extensions improve predictive accuracy. See `docs/brem-framework-extensions.md` for full details:

1. **Asymmetric Cell Weighting** — High scores penalise more than low scores help (F1: 0.902 → 0.914). nc has the highest asymmetry ratio (8.22×).
2. **Domain Ceiling Rule** — Any single domain ≥ 3.0 triggers elevated risk regardless of overall score (F1: 0.914 → 0.929).
3. **Dependency Decomposition** — For multi-platform systems, sm_eff = min(4, sm_base + ⌈log₂N⌉) where N = platforms per settlement function.

---

## Citation

Price, T. (2026). Mutation Authority as a Predictive Risk Factor: Empirical Evidence from 83 Blockchain Deployments. *IEEE ICBC 2026 AI-R2D2 Workshop*. [Submitted]

## License

Scoring rubric: CC BY 4.0
