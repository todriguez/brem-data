# BREM Scoring Instrument
**Branching Decision Logic for Cell-Level Scoring (0–4)**

## How This Instrument Works

Each of the nine BREM cells is scored through a sequence of 4–5 diagnostic questions with **branching logic**. The first question (Q1) is a **gating question** that establishes a score range. Subsequent questions either refine within that range or act as **overrides** that can raise the score if they discover structural weaknesses not captured by earlier questions.

**Key design principles:**

1. **Asymmetric by design.** Override questions (Q4) can only raise scores, never lower them. A single structural weakness dominates, consistent with the empirical asymmetry ratios (nc: 8.22×, se: 4.53×).

2. **Branching eliminates redundancy.** If Q1 establishes that a protocol is immutable, questions about governance thresholds are skipped. This is why the effective question count per project is fewer than 45 — the branching logic routes around questions that cannot change the score.

3. **Overrides cancel favorable answers.** A project may answer Q1–Q3 favorably, but Q4 can discover that admin keys exist regardless of governance claims, raising the floor. This is the "look more closely" check.

4. **Context capture is separate from scoring.** The final question (Q5) in each cell captures qualitative context for the assessment report without changing the score.

---

## NETWORK DOMAIN — Protocol Determinism

### na — Architecture

**Q1 (GATING):** How is state organised in the system?

| Answer | Effect |
|--------|--------|
| Independent record-level state (UTXO or equivalent); no shared mutable state; verifiable without global knowledge | Ceiling = 1, go to Q3 |
| Shared mutable global state; account-based model; sequential execution over common state | Floor = 2, go to Q2 |

**Q2 (SEVERITY — only if Q1 = shared state):** Does the architecture require off-chain fragmentation (L2s, bridges, rollups, sharding) for its target workload?

| Answer | Effect |
|--------|--------|
| No — single-layer operation handles target throughput | Score = 2 |
| Yes, fragmentation is optional but available | Score = 3 |
| Yes, fragmentation is mandatory for baseline operation | Score = 4 |

**Q3 (REFINEMENT — only if Q1 = independent state):** Does correct operation depend on non-standard, proprietary, or experimental components?

| Answer | Effect |
|--------|--------|
| Uses only established, standardised components (standard cryptography, proven algorithms) | Score = 0 |
| Well-tested components; bounded operational choices within protocol rules | Score = 1 |

**Q4 (OVERRIDE — always asked):** Does correct protocol operation depend on middleware, coordinators, or adjunct layers (bridges, oracles, relayers) at the *architectural* level — not just the execution path?

| Answer | Effect |
|--------|--------|
| No — protocol is architecturally self-contained | No change |
| Yes — protocol requires middleware/coordinators for consensus or state validity | Floor = max(current, 2) |
| Yes — protocol requires cross-chain bridges or L2s for base-layer state coherence | Floor = max(current, 3) |

*Note: Custodial or execution-path dependencies (e.g., a trusted intermediary issuing wrapped assets) are captured in se (Execution), not na. na measures architectural structure, not the trust model of assets running on it.*

**Q5 (CONTEXT):** Which architectural pattern best describes the system? *(for reporting, does not change score)*

- [ ] UTXO / independent spends
- [ ] Account-based / shared state (EVM, Solidity)
- [ ] Hybrid (state channels, payment channels)
- [ ] Custom / proprietary architecture
- [ ] Multi-chain / cross-chain by design

---

### nc — Consensus

**Q1 (GATING):** Who can participate in consensus, and how is participation determined?

| Answer | Effect |
|--------|--------|
| Permissionless — anyone meeting objective technical requirements (computation, stake) can participate without approval | Ceiling = 1, go to Q3 |
| Permissioned — participation requires approval from existing members or a controlling entity | Floor = 3, go to Q2 |

**Q2 (SEVERITY — only if Q1 = permissioned):** Are the consensus participants independently controlled entities with divergent interests?

| Answer | Effect |
|--------|--------|
| Yes — multiple independent organisations each control their own validators | Score = 3 |
| No — all validators are controlled by a single entity or tightly affiliated group | Score = 4 |

**Q3 (REFINEMENT — only if Q1 = permissionless):** What determines consensus ordering and finality?

| Answer | Effect |
|--------|--------|
| Objective work-based ordering (PoW); finality by energy expenditure; competitive infrastructure incentive | Score = 0 |
| Protocol rules with narrow parameterisation; limited operational discretion; mostly objective | Score = 1 |

**Q4 (OVERRIDE — always asked):** Can consensus rules be overridden, paused, or altered outside the automated protocol by any party?

| Answer | Effect |
|--------|--------|
| No — consensus follows fixed algorithmic rules with no manual override | No change |
| Yes — defined emergency procedures exist (circuit breakers, admin pause) | Floor = max(current, 2) |
| Yes — authorities can arbitrarily override consensus decisions | Floor = max(current, 4) |

**Q5 (CONTEXT):** *(for reporting, does not change score)*

- [ ] PoW (competitive, infrastructure-scaling incentive)
- [ ] PoS (stake-weighted, returns proportional to weight not compute)
- [ ] BFT variant (Tendermint, PBFT, HotStuff, Simplex)
- [ ] Consortium / permissioned round-robin
- [ ] Single-operator / no meaningful consensus

---

### ns — Scalability

**Q1 (GATING):** Does the system's throughput degrade gracefully under load, with capacity well above its target use case requirements?

| Answer | Effect |
|--------|--------|
| Yes — native parallelism or demonstrated throughput orders of magnitude above requirements; predictable degradation | Ceiling = 1, go to Q3 |
| Moderate — throughput adequate for current use case but limited headroom; requires tuning under load | Score = 2, go to Q4 |
| No — throughput ceiling below or near target requirements; scaling requires architectural changes or mandatory off-chain layers | Floor = 3, go to Q2 |

**Q2 (SEVERITY — only if Q1 = hard ceiling):** Is the throughput ceiling below the system's target or stated use case requirements?

| Answer | Effect |
|--------|--------|
| No — current throughput exceeds requirements despite ceiling | Score = 3 |
| Yes — system cannot handle target workload at base layer | Score = 4 |

**Q3 (REFINEMENT — only if Q1 = native parallelism):** Has the scaling been independently verified under realistic workloads?

| Answer | Effect |
|--------|--------|
| Yes — independent verification under production-like conditions | Score = 0 |
| No — self-reported or testnet-only metrics; predictable degradation demonstrated | Score = 1 |

**Q4 (OVERRIDE — always asked):** Does the system's stated use case require batching, netting, or deferred settlement to handle expected volumes?

| Answer | Effect |
|--------|--------|
| No — individual transactions settle at protocol speed | No change |
| Yes — batching/netting required for economic viability at target scale | Floor = max(current, 2) |

**Q5 (OVERRIDE — always asked):** Does the system suffer from single-asset hot-spot contention (e.g., single CBDC token, single liquidity pool) where all transactions compete for the same state?

| Answer | Effect |
|--------|--------|
| No — contention distributed across multiple independent state objects | No change |
| Yes — systemic hot-spot; all transactions contend for shared state | Floor = max(current, 3) |

---

## SYSTEM STATE DOMAIN — Mutation Authority

### se — Execution

**Q1 (GATING):** What is the execution model for state transitions?

| Answer | Effect |
|--------|--------|
| Script-based predicates — stateless, bounded, no halting problem, deterministic | Ceiling = 1, go to Q3 |
| Turing-complete VM with resource limits (gas, compute bounds) | Score = 2, go to Q4 |
| Execution depends on off-chain processes, trusted intermediaries, or manual approval | Floor = 3, go to Q2 |

**Q2 (SEVERITY — only if Q1 = off-chain dependent):** Is the trusted intermediary's solvency or operational status a prerequisite for settlement finality?

| Answer | Effect |
|--------|--------|
| No — off-chain dependency is bounded; on-chain settlement is independently final | Score = 3 |
| Yes — settlement outcome depends on intermediary solvency (custodial representations, IOUs) | Score = 4 |

**Q3 (REFINEMENT — only if Q1 = script-based):** Are there any known vulnerability classes in the execution model?

| Answer | Effect |
|--------|--------|
| No — bounded execution, no reentrancy class, no halting problem | Score = 0 |
| Minor — safe execution with formal constraints; minimal attack surface | Score = 1 |

**Q4 (OVERRIDE — always asked):** Does the system depend on bridge protocols, custodial intermediaries, or wrapped/synthetic asset representations for its core function?

| Answer | Effect |
|--------|--------|
| No — assets are native to the execution environment | No change |
| Yes — core assets are wrapped, bridged, or custodially issued representations | Floor = max(current, 3) |

**Q5 (CONTEXT):** *(for reporting, does not change score)*

- [ ] Bitcoin Script / predicate-based
- [ ] EVM / Solidity
- [ ] WASM / Move / custom VM
- [ ] Smart legal contracts (Corda, DAML)
- [ ] Custodial / API-mediated

---

### sm — Mutation Authority ← THE DIAGNOSTIC VARIABLE

**Q1 (GATING):** Can any single entity change protocol rules, consensus parameters, or contract state without protocol-level constraints?

| Answer | Effect |
|--------|--------|
| Yes — one entity (or tightly affiliated group) has unilateral change authority | Floor = 3, go to Q2 |
| No — changes require multi-party process or are protocol-constrained | Ceiling = 2, go to Q3 |

**Q2 (SEVERITY — only if Q1 = yes):** Are there ANY formal, published constraints on that authority? (time-locks, multisig requiring independent parties, community veto, published governance framework)

| Answer | Effect |
|--------|--------|
| Yes — formal constraints exist and are documented/verifiable | Score = 3 |
| No — constraints are undisclosed, informal, or do not exist | Score = 4 |

**Q3 (REFINEMENT — only if Q1 = no):** What is the governance process for protocol changes?

| Answer | Effect |
|--------|--------|
| Protocol is immutable — no upgrade mechanism exists; no admin keys | Score = 0 |
| Changes require broad community consensus (hard fork, BIP-style process) | Score = 1 |
| Formal governance with defined thresholds; bounded parameter adjustment | Score = 2 |

**Q4 (OVERRIDE — always asked):** Do admin keys, proxy/upgradeable contracts, or undocumented upgrade mechanisms exist — regardless of what governance documentation claims?

| Answer | Effect |
|--------|--------|
| No — verified on-chain; no admin functions, no proxy patterns, no upgrade paths | No change |
| Unknown — upgrade mechanisms not publicly documented or verified | Floor = max(current, 3) |
| Yes — admin keys or upgrade mechanisms exist | Floor = max(current, 3) |

**Q5 (CONTEXT):** Which mutation surfaces exist? *(multi-select, for reporting, does not change score)*

- [ ] Protocol rules / consensus parameters
- [ ] Smart contract upgradeability (proxy patterns, UUPS)
- [ ] Fee structures / economic parameters
- [ ] Validator set / access control
- [ ] Data ingress / oracle rules
- [ ] Emergency pause / kill switch

---

### sf — Economic Fitness

**Q1 (GATING):** What is the primary revenue/sustainability model?

| Answer | Effect |
|--------|--------|
| Transaction fees from real economic activity; no native token dependency; unit economics viable | Ceiling = 1, go to Q3 |
| Token-based incentives, staking rewards, or subsidy-dependent | Floor = 2, go to Q2 |

**Q2 (SEVERITY — only if Q1 = token/subsidy):** Can the economic model sustain itself without token price appreciation or continuous new capital inflow?

| Answer | Effect |
|--------|--------|
| Yes — fee revenue covers operating costs independent of token price | Score = 2 |
| Partially — viable for high-value transactions; micropayments not feasible | Score = 3 |
| No — economics depend on token appreciation, yield from new deposits, or external subsidy | Score = 4 |

**Q3 (REFINEMENT — only if Q1 = real revenue):** Are micropayments economically viable on this system?

| Answer | Effect |
|--------|--------|
| Yes — sub-cent transaction costs; micropayment business models feasible | Score = 0 |
| No — transaction costs viable for standard payments but not micropayments | Score = 1 |

**Q4 (OVERRIDE — always asked):** Does the architecture score (na) impose a structural ceiling on economic viability? (i.e., does na ≥ 3 make the target business model impossible at scale?)

| Answer | Effect |
|--------|--------|
| No — architecture supports the target economic model | No change |
| Yes — architectural constraints prevent the business model from scaling | Floor = max(current, 3) |

**Q5 (OVERRIDE — always asked):** Is the system pre-production with no mainnet revenue?

| Answer | Effect |
|--------|--------|
| No — production system with demonstrated revenue | No change |
| Yes — pre-production; economics are projected, not proven | Floor = max(current, 1) |

---

## LAW DOMAIN — Enforceability

### ls — Standing

**Q1 (GATING):** Does the system maintain a continuous chain of attributable signatures linking ownership from one party to the next?

| Answer | Effect |
|--------|--------|
| Yes — UTXO chain of title; parties identifiable through standard legal process | Ceiling = 1, go to Q3 |
| Partially — attribution chain exists but has gaps (intermediaries, mixers, bridge crossings) | Score = 2, go to Q4 |
| No — non-attributable transactions; ownership record does not identify legally responsible actors | Floor = 3, go to Q2 |

**Q2 (SEVERITY — only if Q1 = non-attributable):** Can parties be identified through any compulsory legal process (subpoena, court order)?

| Answer | Effect |
|--------|--------|
| Yes — identifiable through regulated intermediaries (exchanges, custodians) | Score = 3 |
| No — system designed to prevent identification; no intermediary to compel | Score = 4 |

**Q3 (REFINEMENT — only if Q1 = full attribution):** Is the asset definitively classified by relevant regulatory authorities?

| Answer | Effect |
|--------|--------|
| Yes — commodity classification or equivalent established (CFTC, equivalent authority) | Score = 0 |
| Mostly — standing established with standard evidentiary work; regulatory framework clear | Score = 1 |

**Q4 (OVERRIDE — always asked):** Does the system involve cross-jurisdiction operation where legal standing is ambiguous or conflicting?

| Answer | Effect |
|--------|--------|
| No — operates within a single clear jurisdiction or harmonised regulatory framework | No change |
| Yes — multi-jurisdiction with emerging or conflicting frameworks | Floor = max(current, 2) |

**Q5 (CONTEXT):** *(for reporting, does not change score)*

- [ ] Commodity-classified (CFTC or equivalent)
- [ ] Regulated financial product (securities, e-money)
- [ ] Utility token / no clear classification
- [ ] Privacy-preserving / attribution gaps
- [ ] Unregistered / classification pending

---

### lr — Remedy

**Q1 (GATING):** If something goes wrong (fraud, error, theft), can affected parties recover losses through established legal processes?

| Answer | Effect |
|--------|--------|
| Yes — courts have jurisdiction, precedent exists, property rights enforceable (injunction, tracing, restitution) | Ceiling = 1, go to Q3 |
| Partially — some remedies exist but costly, slow, or jurisdictionally limited | Score = 2, go to Q4 |
| No — "code is law"; no meaningful legal remedy; recovery depends on insider discretion | Floor = 3, go to Q2 |

**Q2 (SEVERITY — only if Q1 = no remedy):** Have losses actually occurred that went unrecovered?

| Answer | Effect |
|--------|--------|
| No — no loss events yet, or losses recovered through ad hoc mechanisms | Score = 3 |
| Yes — demonstrated losses with no recovery (hacks, exploits, rug pulls) | Score = 4 |

**Q3 (REFINEMENT — only if Q1 = full remedy):** Are remedies available to all participants equally, or only to insiders/large holders?

| Answer | Effect |
|--------|--------|
| All participants — standard legal process available regardless of size or status | Score = 0 or 1 |
| Selective — insiders or large holders have practical access to remedy; retail does not | Floor = max(current, 3) |

**Q4 (OVERRIDE — always asked):** Does the system's design explicitly remove chargeback, reversal, or dispute resolution mechanisms?

| Answer | Effect |
|--------|--------|
| No — dispute resolution mechanisms exist (chargebacks, arbitration, etc.) | No change |
| Yes — finality is absolute; no reversal mechanism by design | Floor = max(current, 2) |

**Q5 (CONTEXT):** *(for reporting, does not change score)*

- [ ] Court-enforceable property rights
- [ ] Regulated dispute resolution (chargebacks, arbitration)
- [ ] Protocol-level dispute mechanism (optimistic rollback, governance vote)
- [ ] No formal remedy — code is law
- [ ] Ad hoc recovery only (hard fork, admin intervention)

---

### lp — Liability Precision

**Q1 (GATING):** Can liability for system failures be attributed to identifiable, jurisdictionally reachable parties?

| Answer | Effect |
|--------|--------|
| Yes — clear legal entity, identifiable operators, established liability framework | Ceiling = 1, go to Q3 |
| Partially — some parties identifiable, but liability allocation ambiguous in novel areas | Score = 2, go to Q4 |
| No — anonymous or pseudonymous operators; no identifiable liable party | Floor = 3, go to Q2 |

**Q2 (SEVERITY — only if Q1 = no identifiable party):** Is the system designed to evade liability (anonymity, jurisdiction shopping, decentralisation theatre)?

| Answer | Effect |
|--------|--------|
| No — genuinely decentralised with no single point of liability by design | Score = 3 |
| Yes — liability evasion appears intentional (anonymous team, offshore, no legal entity) | Score = 4 |

**Q3 (REFINEMENT — only if Q1 = clear liability):** Are liability boundaries well-defined for blockchain-specific failure modes (consensus failure, smart contract bugs, bridge exploits)?

| Answer | Effect |
|--------|--------|
| Yes — liability framework addresses blockchain-specific risks; SLAs or equivalent exist | Score = 0 |
| Mostly — general liability framework applies; blockchain-specific gaps exist but manageable | Score = 1 |

**Q4 (OVERRIDE — always asked):** Does the system involve cross-border liability chains where a failure in one jurisdiction cannot be remedied in another?

| Answer | Effect |
|--------|--------|
| No — single jurisdiction or bilateral liability agreements in place | No change |
| Yes — cross-border liability gaps exist; no harmonised framework | Floor = max(current, 2) |

**Q5 (OVERRIDE — always asked):** Are there unresolved money transmission, sanctions, or compliance obligations that could create existential liability?

| Answer | Effect |
|--------|--------|
| No — compliance obligations understood and met | No change |
| Yes — money transmission or sanctions questions unresolved | Floor = max(current, 3) |

---

## Scoring Aggregation

After completing all questions for a cell, the **final cell score** is determined by:

1. Start with the score from Q1 → Q2/Q3 (gating + refinement)
2. Apply each override: final = max(current_score, override_floor)
3. The highest applicable override wins

Cell scores aggregate as:
- **Domain score** = mean of three cells in that domain
- **Overall score** = mean of all nine cells (= mean of three domain scores)
- **Threshold**: overall ≥ 2.5 indicates elevated failure risk

## Relationship to Cell-Level Anchors

This instrument operationalises the cell-level anchors published in `scoring-rubric.md`. Each Q1–Q4 answer maps to a specific anchor level. The branching logic ensures that the instrument reaches the same score an expert would assign by reading the anchor definitions directly.

The instrument is a **forward-looking scoring tool** for new assessments. The existing 83-project dataset was scored through expert assessment against the cell-level anchors, not through this branching instrument. The instrument is calibrated to reproduce expert scores when applied to projects with known profiles.

---

*BREM Scoring Instrument v1.0 — March 2026*
*Price, T. — Blockchain Risk Evaluation Model*
