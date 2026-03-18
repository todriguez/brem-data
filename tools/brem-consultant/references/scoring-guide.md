# BREM Scoring Guide — 9-Cell Framework

## The Matrix

|           | Structure | Process | Persistence |
|-----------|-----------|---------|-------------|
| **Network** (M2M) | na: Architecture | nc: Consensus | ns: Scalability |
| **System State** (H2M) | se: Execution | sm: Mutation Authority | sf: Economic Fitness |
| **Law** (H2H) | ls: Standing | lr: Remedy | lp: Liability Precision |

## Scale: 0-4

- **0** = Negligible risk. Protocol-constrained, no discretionary authority.
- **1** = Low risk. Narrow constraints, minimal discretion.
- **2** = Moderate risk. Bounded governance, formal processes required.
- **3** = High risk. Broad discretion retained by identifiable parties.
- **4** = Critical risk. Full centralized control or fundamental design failure.

## Scoring Each Cell

### na — Architecture (Network Structure)

Measures how much the protocol's core design constrains versus enables discretionary change.

**Key questions to ask:**
1. Can any subset of participants unilaterally change core protocol rules?
2. Do all participants operate under identical protocol rules?
3. Are previously valid transactions guaranteed to remain valid?
4. Does correct operation depend on custom or proprietary components?
5. Can architectural decisions be changed without full participant alignment?

**Anchors:**
- na=0: Bitcoin-like immutable UTXO architecture. No upgrade mechanism.
- na=1: Ethereum-like design requiring hard fork for changes.
- na=2: Formal governance with supermajority thresholds for changes.
- na=3: Core team can make architectural decisions. Admin keys present.
- na=4: Single entity controls architecture. Shared mutable global state. (ASX CHESS = 4)

### nc — Consensus (Network Process)

Measures how much consensus depends on identifiable human authority versus algorithmic rules.

**Key questions to ask:**
1. Is transaction finality cryptographic, probabilistic, or discretionary?
2. Is consensus influenced by identifiable individuals or committees?
3. Can participants be excluded through non-technical means?
4. Can consensus rules be altered outside automated processes?
5. Does consensus depend on ongoing human coordination?

**Anchors:**
- nc=0: Anonymous, permissionless participation. Bitcoin mining.
- nc=1: Pseudonymous stake-based. Public PoS chains.
- nc=2: Known validators operating under algorithmic rules.
- nc=3: Permissioned set of pre-approved participants. Most enterprise chains.
- nc=4: Named authorities control consensus. Manual approval processes.

### ns — Scalability (Network Persistence)

Measures whether the system can survive real-world load demands.

**Key questions to ask:**
1. Is capacity limited by design or by economic incentives?
2. Can the system handle increased load without performance degradation?
3. Are there fundamental architectural bottlenecks?
4. What happens under 10x current load?
5. Is scaling dependent on off-chain systems?

**Anchors:**
- ns=0: Linear scaling, no inherent bottlenecks. UTXO parallelism.
- ns=1: Predictable graceful degradation.
- ns=2: Scaling varies by workload type.
- ns=3: Prone to congestion bottlenecks. (Ethereum during high demand = ~3)
- ns=4: Fundamental architectural bottlenecks. Sequential processing. (ASX CHESS = 4)

### se — Execution (System State Structure)

Measures how much the execution environment depends on trusted intermediaries.

**Key questions to ask:**
1. What execution model? Script-based predicates, VM, hybrid, or database?
2. Is execution deterministic or dependent on external inputs?
3. Can execution outcomes be altered after the fact?
4. Are there trusted intermediaries in the execution path?
5. Can smart contracts be upgraded or paused?

**Anchors:**
- se=0: Script-based predicates. Stateless validation (Bitcoin Script).
- se=1: Bounded VM with gas limits (EVM).
- se=2: Hybrid on-chain/off-chain with defined sync.
- se=3: Mutable database with API-mediated state changes.
- se=4: Execution depends on trusted intermediaries. Manual approval.

### sm — Mutation Authority (System State Process) — THE DIAGNOSTIC VARIABLE

This is the single most predictive variable. It measures whether anyone can change the rules after deployment.

**Key questions to ask:**
1. **Admin keys**: Do admin keys exist? Who holds them? Can they be used to alter protocol rules?
2. **Contract upgradeability**: Can smart contracts be upgraded? Through what mechanism? (Proxy patterns, UUPS, etc.)
3. **Parameter changes**: Can governance or admin change fee structures, interest rates, collateral ratios, or other economic parameters?
4. **Pause/freeze**: Is there a pause function? Emergency shutdown? Who controls it?
5. **Data mutability**: Can transaction history be altered? Can state be rolled back?

**Anchors:**
- sm=0: Protocol is immutable. No admin keys, no upgrades, no pause. (Uniswap V2 = 0)
- sm=1: Time-locked changes with community veto. Narrow, constrained governance.
- sm=2: Formal governance with high thresholds. Bounded discretion. (Aave ≈ 2)
- sm=3: Core team or foundation can alter protocol. Admin keys present. Most enterprise. (Terra = 3)
- sm=4: Single entity has database-admin-level control. Can change anything. (ASX CHESS = 4)

**Critical thresholds:**
- sm ≤ 2: Zero failures in 83-project dataset (n=7)
- sm = 3: 35.7% failure rate (15/42)
- sm = 4: 73.5% failure rate (25/34)

### sf — Economic Fitness (System State Persistence)

Measures whether the economic model is sustainable or dependent on unsustainable incentives.

**Key questions to ask:**
1. Does the protocol generate sustainable revenue from real economic activity?
2. Are incentives dependent on new capital inflows (Ponzi dynamics)?
3. Is the token/economic model tested across market cycles?
4. What happens to the system if token price drops 90%?
5. Are there unsustainable yield promises or algorithmic pegs?

**Anchors:**
- sf=0: Revenue from real economic activity. Sustainable fee model.
- sf=1: Strong economic model with minor dependencies on token price.
- sf=2: Viable but untested across full market cycles.
- sf=3: Heavy dependence on token incentives or new capital. (Many enterprise = 3)
- sf=4: Fundamentally unsustainable. Ponzi dynamics, algorithmic peg without reserves. (Terra sf = 4)

**This cell has the largest effect size (Cohen's d = 2.77) between failed and not-failed projects.** It's the cell where failures most clearly diverge from survivors.

### ls — Standing (Law Structure)

Measures whether the system operates within a clear legal framework with identifiable participants.

**Key questions to ask:**
1. Is there a legal entity responsible for the protocol?
2. Do participants have clear legal standing to bring claims?
3. Is the system operating in a recognized regulatory framework?
4. Can participants be identified for legal purposes?
5. Is there jurisdictional clarity?

**Anchors:**
- ls=0: Clear legal entity, regulated, participants identifiable.
- ls=1: Legal entity exists, operating under known regulatory framework.
- ls=2: Legal structure exists but some ambiguity in standing.
- ls=3: Ambiguous legal entity, unclear regulatory status. (Many DeFi protocols)
- ls=4: No identifiable legal entity, no jurisdiction, anonymous participants.

### lr — Remedy (Law Process)

Measures whether participants have meaningful recourse when things go wrong.

**Key questions to ask:**
1. Are there established dispute resolution mechanisms?
2. Can participants recover assets through legal channels?
3. Is there insurance or compensation mechanisms?
4. Are there precedents for enforcement?
5. Can smart contract outcomes be challenged legally?

**Anchors:**
- lr=0: Established legal remedies, insurance, clear enforcement precedents.
- lr=1: Legal remedies exist, some enforcement precedents.
- lr=2: Dispute resolution mechanisms exist but untested.
- lr=3: Limited or theoretical remedies. Unclear enforcement. (Most DeFi = 3)
- lr=4: No meaningful remedy. "Code is law" with no legal fallback.

### lp — Liability Precision (Law Persistence)

Measures whether liability is clearly defined and attributable.

**Key questions to ask:**
1. If the system fails, who is liable?
2. Are liability boundaries clearly defined in agreements?
3. Can liability be traced to specific parties?
4. Is there proportional liability (vs. binary all-or-nothing)?
5. Are liability obligations enforceable across jurisdictions?

**Anchors:**
- lp=0: Clear liability attribution, enforceable agreements, identified parties.
- lp=1: Liability defined, mostly enforceable.
- lp=2: Some liability clarity but gaps in attribution or enforcement.
- lp=3: Vague liability, hard to attribute, enforcement uncertain.
- lp=4: No liability attribution possible. Anonymous participants, no agreements. (ASX CHESS = 4 due to multi-party blame)

## Computing the Overall Score

1. Each cell produces a single 0–4 integer score through the branching decision logic (gating → refinement/severity → override).
2. Average cells within each row for domain scores:
   - Network domain = (na + nc + ns) / 3
   - System State domain = (se + sm + sf) / 3
   - Law domain = (ls + lr + lp) / 3
3. Overall BREM score = mean of all 9 cell scores.

## Interpreting the Score

- **Below 1.5**: Very low risk. Protocol-constrained design. (Example: Uniswap V2 = 1.22)
- **1.5 - 2.0**: Low risk. Bounded governance with clear constraints. (Example: Aave = 1.33)
- **2.0 - 2.5**: Moderate risk. Below threshold but watch for elevated cells. Mixed design.
- **2.5 - 3.0**: Above threshold. 92.5% of projects in this range that failed. Active risk.
- **Above 3.0**: High risk. Strong failure pattern. Most projects here have failed. (Failed mean = 3.04)

## The Three Propagation Chains

1. **Architecture → Scaling → Economics** (na→ns→sf, ρ=0.606 and 0.566)
   Enterprise pattern: poor architecture constrains scalability, which kills economic viability.

2. **Mutation → Execution → Standing** (sm→se→ls, ρ=0.462 and 0.489)
   Governance pattern: broad discretion degrades execution integrity and legal standing.

3. **Economics → Mutation** (sf→sm, DeFi-specific reversal)
   DeFi pattern: economic stress exposes governance discretion, which is then exercised destructively.

## De-risking Recommendations by Cell

| Cell | If elevated, recommend... |
|------|--------------------------|
| na | Reduce shared mutable state. Consider UTXO or independent state models. |
| nc | Move toward algorithmic consensus. Reduce permissioned validator sets. |
| ns | Address architectural bottlenecks before scaling needs arise. |
| se | Reduce reliance on trusted intermediaries. Favor deterministic execution. |
| sm | **Most impactful.** Remove admin keys, make contracts immutable, use time-locks with community veto. |
| sf | Ensure sustainable economics. Stress-test under 90% token price drop. Remove Ponzi dynamics. |
| ls | Establish clear legal entity. Operate in recognized jurisdiction. |
| lr | Build dispute resolution mechanisms. Consider insurance coverage. |
| lp | Define liability clearly in participant agreements. Identify responsible parties. |

---

## Scoring Extensions (V2 Methodology)

These three extensions improve predictive accuracy and address analytical gaps in multi-platform architectures. They are compatible with the base framework — apply them **after** computing base scores.

### Extension 1: Asymmetric Cell Weighting

**Problem:** The flat average treats each cell's contribution as symmetric — a low score offsets a high one equally. Empirically, **every cell is asymmetric in the same direction**: being bad hurts more than being good helps.

**Asymmetry ratios** (fail-pull / surv-pull, all > 1.0):

| Cell | Asymmetry | Interpretation |
|------|-----------|----------------|
| nc | **8.22×** | Being bad at consensus is 8.2× more impactful than being good |
| se | **4.53×** | Execution failure is 4.5× more impactful than execution safety |
| na | **2.07×** | Architectural risk is 2.1× more impactful than architectural safety |
| ls | **1.77×** | Legal risk hurts 1.8× more than legal clarity helps |
| ns | 1.52× | Moderate asymmetry |
| lr | 1.52× | Moderate asymmetry |
| sm | 1.44× | Mild asymmetry |
| sf | 1.37× | Mild asymmetry |
| lp | 1.37× | Mild asymmetry |

**How to apply:** For each cell, assign w_up (proportional to fail-pull) when score > 2, and w_down (proportional to surv-pull) when score ≤ 2. Asymmetric weighted overall = Σ(score_i × w_i) / Σ(w_i). Optimal threshold shifts to 2.7.

**Impact:** F1 improves from 0.902 to **0.914** (sensitivity 92.5%, specificity 90.7%).

**Key insight for consulting:** A strong law domain (ls=1, lr=1, lp=1) does NOT fully offset weak technical cells. Legal clarity "cannot polish a turd" — government CAN kill a project (high w_up) but legal clarity alone CANNOT save one (discounted w_down). Always flag this when you see mixed profiles.

### Extension 2: Domain Ceiling Rule

**Problem:** Flat averaging allows a strong domain to mask a failing domain. Legal clarity and technical functionality are not fungible — a clear legal framework doesn't fix a 10 TPS bottleneck.

**Rule:** Flag if overall ≥ 2.5 **OR** any domain > 3.0.

When a domain exceeds 3.0 but overall is below 2.5, report as **"DOMAIN-MASKED RISK"** — a "technical zombie" kept alive on paper by strong legal or economic scores.

**Impact:** F1 improves to **0.929** (sensitivity **97.5%**, specificity 88.4%). Catches 39/40 failures vs 37/40 with the same false positive count.

**Supporting evidence:**
- System State is most predictive domain (Cohen's d = **2.53**)
- Network is second (d = 1.28)
- Law is weakest (d = 1.25)
- Averaging all three equally dilutes the strongest signal with the weakest

**Key example:** HSBC FX Everywhere (overall=2.33) has Network and System State domains averaging 3.0 while Law averages 1.0. Flat average places it below threshold. It failed anyway. The domain ceiling catches it.

### Extension 3: Dependency Decomposition

**Problem:** The SPP framework assumes a single coherent subject per cell. When a project depends on N independent platforms with independent governance, each cell question has N different answers.

**When to apply:** Any project that depends on multiple independent blockchain platforms, each with its own governance body. Common in CBDC pilots, enterprise interoperability projects, and cross-chain DeFi.

**Step 1: Identify platform dependencies.** List every independent platform the project depends on and its governance body.

**Step 2: Score each platform independently** on at least na, nc, ns, sm.

**Step 3: Compose platform scores into project scores using cell-specific rules:**

| Cell | Composition Rule | Rationale |
|------|-----------------|-----------|
| na | max across platforms + interoperability penalty | Architecture constrained by worst platform plus paradigm mismatch |
| nc | max across platforms | Consensus weakened by weakest link |
| ns | bottlenecked by slowest link (min throughput) | Cross-chain settlement serialised at slowest chain |
| se | worst case + count penalty | Each dependency adds execution failure surface |
| sm | union of all governance bodies + log₂(N) penalty | Any can independently break the system |

**Mutation authority dependency penalty:**

    sm_eff = min(4, sm_base + ⌈log₂(N)⌉)

where N = number of independent governance bodies with uncoordinated mutation authority.

**Rationale:** If each platform has probability p of a breaking change per year:
- N=1: P(break) = p = 10%
- N=3: P(break) = 1-(1-p)³ = 27%
- N=5: P(break) = 1-(1-p)⁵ = 41%
- N=7: P(break) = 1-(1-p)⁷ = **52%**

**Critical nuance — per-function N, not total N:** The penalty should scale with the number of platforms that must interoperate *for a single settlement function*, not the total platform count in the ecosystem. A modular project with 7 platforms but only 2-3 per function scores better than a systemic project with 5 platforms all required for one function.

**Cross-validation (three central bank pilots):**

| Project Type | Platforms | Per-function N | sm_eff | Overall |
|-------------|----------|----------------|--------|---------|
| Single-platform (Fnality) | 1 | 1 | 3 | **2.00** |
| Modular multi (Guardian) | 7+ | 2-3 | 3-4 | **2.33** |
| Systemic multi (Acacia) | 5 | 5 | 4 | **2.44** |

All three have identical Law domains (~1.33) and similar institutional backing. The entire risk differential is architectural.

**Account-based hot-spot contention:** When scoring ns for platforms using account-based models (Ethereum, Hedera, etc.) where a single high-frequency asset (like a CBDC) dominates transactions, all state updates must be serialised. Benchmark TPS figures (measured across independent accounts) do NOT reflect single-asset contention performance. Score ns based on realistic single-asset throughput, not marketed TPS.

**Platform liability gap:** Check whether platform providers disclaim liability for third-party integrations. If they do (common — e.g., Hedera caps total liability at $100 USD), the project's governance is a facade: it controls rules but not the infrastructure those rules depend on. Flag this explicitly in your assessment.

### Applying All Three Extensions Together

After computing base scores:

1. **Check for multi-platform dependencies.** If N > 1, apply dependency decomposition to get adjusted cell scores (especially sm_eff).
2. **Compute domain scores** using adjusted cells.
3. **Apply domain ceiling rule.** Flag if any domain > 3.0, even if overall is below 2.5.
4. **Apply asymmetric weighting** for the most accurate overall risk assessment.
5. **Report all three results:** base score, domain ceiling status, asymmetric-weighted score.

### Threshold Comparison Summary

| Method | Sensitivity | Specificity | F1 |
|--------|-------------|-------------|-----|
| Base (overall ≥ 2.5) | 92.5% | 88.4% | 0.902 |
| Asymmetric weighted (≥ 2.7) | 92.5% | 90.7% | 0.914 |
| Domain ceiling (≥ 2.5 or domain > 3.0) | **97.5%** | 88.4% | **0.929** |
