# BRAP 2.0 Scoring Rubric
**Converting Descriptive Project Data to Deterministic 0-4 SPF Scores**

## Overview

This rubric provides standardized criteria for scoring blockchain projects using the BRAP 2.0 45-question SPF Matrix. Each question uses a 0-4 scale where 0 represents maximum determinism and 4 represents unbounded discretion.

## Scoring Methodology

### Evidence Hierarchy
1. **Primary Sources** (Weight: 1.0): Official documentation, whitepapers, regulatory filings
2. **Secondary Sources** (Weight: 0.8): Industry reports, press releases, verified announcements
3. **Inferred Data** (Weight: 0.6): Analysis based on technical architecture patterns

### Inter-Rater Reliability
- All projects scored independently by 2+ analysts
- Conflicts resolved through evidence review and consensus
- Scoring rationale documented with supporting evidence

---

## NETWORK DOMAIN (Machine ↔ Machine)

### Architecture (Structure)

#### **na_1** - Protocol Stability
**Question**: To what extent can any subset of participants unilaterally change core protocol rules?

**Scoring Criteria:**
- **Score 0**: Bitcoin-like immutable protocol, cryptographically locked consensus rules
  - *Evidence*: No upgrade mechanism, mathematical consensus rules
  - *Examples*: Pure Bitcoin Script, immutable smart contracts

- **Score 1**: Ethereum-like hard fork requirement for protocol changes
  - *Evidence*: Requires broad community consensus and coordination
  - *Examples*: Bitcoin improvements requiring universal adoption

- **Score 2**: Formal governance process requiring supermajority (75%+)
  - *Evidence*: Documented governance requiring high thresholds
  - *Examples*: Some DeFi protocols with high governance thresholds

- **Score 3**: Core development team or foundation can make protocol changes
  - *Evidence*: Centralized development authority, admin keys
  - *Examples*: Many enterprise blockchains, consortium chains

- **Score 4**: Single entity or individual can unilaterally change protocol
  - *Evidence*: Admin controls, single-party governance
  - *Examples*: Private chains, centralized databases

#### **na_2** - Rule Uniformity
**Question**: To what extent do all participants operate under identical protocol rules?

**Scoring Criteria:**
- **Score 0**: Mathematical consensus ensures identical execution
  - *Evidence*: Deterministic virtual machine, consensus verification
  - *Examples*: Bitcoin UTXO model, Ethereum EVM

- **Score 1**: Minor version differences with backward compatibility
  - *Evidence*: Multiple client implementations with compatibility testing
  - *Examples*: Bitcoin Core vs other Bitcoin clients

- **Score 2**: Approved implementations that pass compatibility tests
  - *Evidence*: Formal certification process for client implementations
  - *Examples*: Some enterprise chains with certified clients

- **Score 3**: Interpretive flexibility in rule implementation
  - *Evidence*: Ambiguous specifications, subjective implementation choices
  - *Examples*: Loosely specified protocols

- **Score 4**: Participants operate under different rule sets
  - *Evidence*: Fragmented implementations, incompatible versions
  - *Examples*: Failed consortium chains with inconsistent implementations

#### **na_3** - Backward Compatibility
**Question**: To what extent are previously valid transactions guaranteed to remain valid?

**Scoring Criteria:**
- **Score 0**: All previous transactions remain valid indefinitely
  - *Evidence*: Genesis block still validates, no breaking changes ever
  - *Examples*: Bitcoin's perfect backward compatibility

- **Score 1**: Long deprecation windows (2+ years) with advance notice
  - *Evidence*: Documented deprecation schedules, migration tools
  - *Examples*: Gradual protocol upgrades with long transition periods

- **Score 2**: Version-based validity with clear migration paths
  - *Evidence*: Transaction format versions, upgrade documentation
  - *Examples*: Some smart contract platforms with versioned transactions

- **Score 3**: Conditional validity based on governance decisions
  - *Evidence*: Governance votes can invalidate old transaction types
  - *Examples*: Chains where governance can deprecate features

- **Score 4**: Regular breaking changes invalidate old transactions
  - *Evidence*: History of breaking changes, abandoned transaction formats
  - *Examples*: Early-stage protocols with frequent breaking updates

#### **na_4** - Architectural Dependency
**Question**: To what extent does correct operation depend on non-standard or custom components?

**Scoring Criteria:**
- **Score 0**: Uses only established, standardized components
  - *Evidence*: Standard cryptography, proven algorithms, open specifications
  - *Examples*: Bitcoin (SHA-256, ECDSA, standard networking)

- **Score 1**: Well-tested newer technologies with strong track record
  - *Evidence*: Mature implementations, extensive testing, wide adoption
  - *Examples*: Ethereum using established but newer cryptographic primitives

- **Score 2**: Mix of standard components with some newer elements
  - *Evidence*: Combination of proven and emerging technologies
  - *Examples*: Protocols using standard crypto + newer consensus mechanisms

- **Score 3**: Heavy reliance on custom or proprietary components
  - *Evidence*: Custom consensus, proprietary algorithms, vendor lock-in
  - *Examples*: Enterprise chains with proprietary consensus mechanisms

- **Score 4**: Core operation depends on experimental technologies
  - *Evidence*: Unproven algorithms, research-stage components
  - *Examples*: Chains using experimental cryptography or consensus

#### **na_5** - Change Authority
**Question**: To what extent can architectural decisions be changed without full participant alignment?

**Scoring Criteria:**
- **Score 0**: Architectural decisions are immutable
  - *Evidence*: No mechanism to change core architecture
  - *Examples*: Bitcoin's immutable UTXO architecture

- **Score 1**: Unanimous consensus required for architectural changes
  - *Evidence*: All participants must agree to changes
  - *Examples*: Hard fork requiring universal adoption

- **Score 2**: Supermajority vote through formal governance process
  - *Evidence*: 75%+ vote required, formal proposal process
  - *Examples*: Some DeFi protocols with high governance thresholds

- **Score 3**: Core development team can make architectural changes
  - *Evidence*: Developer authority over architecture decisions
  - *Examples*: Many blockchain projects with centralized development

- **Score 4**: Single entity has administrative control over architecture
  - *Evidence*: Admin keys, centralized control mechanisms
  - *Examples*: Private blockchains, centralized systems

### Consensus (Process)

#### **nc_1** - Finality Assurance
**Question**: To what extent is transaction finality immune from later reversal?

**Scoring Criteria:**
- **Score 0**: Cryptographic finality is mathematically guaranteed
  - *Evidence*: Immediate finality, cryptographic proof of immutability
  - *Examples*: Practical Byzantine Fault Tolerance (pBFT) systems

- **Score 1**: Probabilistic finality approaches mathematical certainty over time
  - *Evidence*: Proof of Work with deep confirmations
  - *Examples*: Bitcoin after 6+ confirmations

- **Score 2**: Formal finalization process provides strong guarantees
  - *Evidence*: Explicit finality gadgets, checkpoint mechanisms
  - *Examples*: Ethereum 2.0 finality, Tendermint finality

- **Score 3**: Economic finality - reversal becomes prohibitively expensive
  - *Evidence*: High cost of attack, economic security models
  - *Examples*: Proof of Stake chains with slashing conditions

- **Score 4**: Discretionary finality dependent on validator decisions
  - *Evidence*: Validators can choose to reverse transactions
  - *Examples*: Consortium chains with manual finality decisions

#### **nc_2** - Consensus Control
**Question**: To what extent is consensus influenced by identifiable individuals or committees?

**Scoring Criteria:**
- **Score 0**: Anonymous, permissionless consensus participation
  - *Evidence*: Anyone can participate anonymously
  - *Examples*: Bitcoin mining, anonymous staking

- **Score 1**: Pseudonymous stake-based consensus without identity requirements
  - *Evidence*: Stake-based voting with pseudonymous participants
  - *Examples*: Public Proof of Stake chains

- **Score 2**: Known validators operating under objective, algorithmic rules
  - *Evidence*: Identified validators following deterministic protocols
  - *Examples*: Some Proof of Stake chains with known validator identities

- **Score 3**: Permissioned set of pre-approved consensus participants
  - *Evidence*: Whitelist of approved validators, approval process
  - *Examples*: Consortium blockchains with approved members

- **Score 4**: Identifiable authorities or committees control consensus
  - *Evidence*: Named individuals with consensus authority
  - *Examples*: Private blockchains with designated consensus authorities

#### **nc_3** - Participant Inclusion
**Question**: To what extent can participants be excluded from consensus through non-technical means?

**Scoring Criteria:**
- **Score 0**: Inclusion based purely on technical requirements (computation/stake)
  - *Evidence*: Only technical barriers to participation
  - *Examples*: Bitcoin mining, public staking

- **Score 1**: Objective, measurable criteria for participation
  - *Evidence*: Clear, quantifiable requirements for participation
  - *Examples*: Minimum stake requirements, performance metrics

- **Score 2**: Transparent registration with consistent criteria
  - *Evidence*: Public registration process with defined standards
  - *Examples*: Some consortium chains with transparent membership

- **Score 3**: Subjective approval by existing participants or governance
  - *Evidence*: Discretionary approval process, voting on new members
  - *Examples*: Consortium chains requiring member approval

- **Score 4**: Arbitrary exclusion for political, personal, or jurisdictional reasons
  - *Evidence*: Exclusions based on non-technical factors
  - *Examples*: Geographically restricted chains, politically motivated exclusions

#### **nc_4** - Override Mechanisms
**Question**: To what extent can consensus rules be altered outside automated processes?

**Scoring Criteria:**
- **Score 0**: Consensus follows fixed algorithmic rules with no override
  - *Evidence*: No mechanism to override consensus rules
  - *Examples*: Pure algorithmic consensus (Bitcoin, early Ethereum)

- **Score 1**: Predefined, algorithmic emergency procedures for specific scenarios
  - *Evidence*: Automatic emergency protocols triggered by specific conditions
  - *Examples*: Circuit breakers, automatic halting mechanisms

- **Score 2**: Formal governance process can modify consensus through voting
  - *Evidence*: Governance proposals can change consensus parameters
  - *Examples*: Many modern DeFi protocols with governance

- **Score 3**: Administrative override under defined circumstances
  - *Evidence*: Admin controls for specific emergency situations
  - *Examples*: Enterprise blockchains with admin emergency powers

- **Score 4**: Consensus can be arbitrarily overridden by authorities
  - *Evidence*: Unrestricted ability to override consensus decisions
  - *Examples*: Centralized systems with admin controls

#### **nc_5** - Dependency on Coordination
**Question**: To what extent does consensus depend on ongoing human coordination?

**Scoring Criteria:**
- **Score 0**: Fully automated consensus without human intervention
  - *Evidence*: Consensus operates autonomously without human input
  - *Examples*: Bitcoin mining, automated PoS consensus

- **Score 1**: Human monitoring only - consensus operates autonomously
  - *Evidence*: Humans observe but don't intervene in consensus
  - *Examples*: Well-functioning public blockchains with monitoring

- **Score 2**: Periodic human input for parameter updates or optimization
  - *Evidence*: Occasional human adjustments to consensus parameters
  - *Examples*: Chains with periodic governance parameter updates

- **Score 3**: Active coordination required between human participants
  - *Evidence*: Ongoing human communication needed for consensus
  - *Examples*: Consortium chains requiring coordination between members

- **Score 4**: Manual operation requiring human negotiation for each decision
  - *Evidence*: Consensus decisions require human negotiation
  - *Examples*: Manual approval processes, committee-based decisions

### Scalability (Fitness)

#### **ns_1** - Throughput Constraints
**Question**: To what extent is system capacity limited by design rather than economic incentives?

**Scoring Criteria:**
- **Score 0**: Capacity allocation determined entirely by market mechanisms
  - *Evidence*: Transaction fees determine priority, no hard limits
  - *Examples*: Bitcoin fee market, Ethereum gas auction

- **Score 1**: Elastic scaling automatically adjusts based on economic parameters
  - *Evidence*: Automatic capacity adjustments based on demand/pricing
  - *Examples*: Some Layer 2 solutions with elastic block sizes

- **Score 2**: Hybrid approach combining technical limits with economic factors
  - *Evidence*: Technical constraints modified by economic incentives
  - *Examples*: Chains with adjustable block size limits

- **Score 3**: Technical design constraints with some economic considerations
  - *Evidence*: Hard technical limits with minor economic adjustments
  - *Examples*: Fixed TPS with priority fee mechanisms

- **Score 4**: Strict technical capacity limits regardless of economic incentives
  - *Evidence*: Hard-coded capacity limits that cannot be exceeded
  - *Examples*: Enterprise chains with fixed throughput limits

#### **ns_2** - Technical Scalability
**Question**: To what extent can the system handle increased load without performance degradation?

**Scoring Criteria:**
- **Score 0**: Linear scaling with no inherent bottlenecks
  - *Evidence*: Performance scales directly with resources added
  - *Examples*: Horizontal scaling systems, parallel processing architectures

- **Score 1**: Predictable, graceful degradation as load increases
  - *Evidence*: Performance degrades predictably, maintains core functions
  - *Examples*: Well-designed systems with known scaling curves

- **Score 2**: Scaling efficiency varies significantly based on workload characteristics
  - *Evidence*: Performance depends on transaction types and patterns
  - *Examples*: Systems optimized for specific transaction patterns

- **Score 3**: System prone to congestion bottlenecks affecting all participants
  - *Evidence*: Network-wide performance issues under high load
  - *Examples*: Ethereum during high congestion periods

- **Score 4**: Fundamental architectural bottlenecks prevent effective scaling
  - *Evidence*: Core design limitations that cannot be overcome
  - *Examples*: Sequential processing architectures, single-threaded systems

---

## SYSTEM STATE DOMAIN (Human ↔ Machine)

### Execution Model (Structure)

#### **se_2** - Execution Architecture
**Question**: What type of execution model does the system use for state transitions?

**Scoring Criteria:**
- **Score 0**: Script-based predicates - stateless execution via cryptographic proofs
  - *Evidence*: UTXO model, Bitcoin Script, predicate-based validation
  - *Examples*: Bitcoin, Lightning Network

- **Score 1**: Bounded state machine - VM execution with gas limits and bounded state
  - *Evidence*: Virtual machine with resource limits, deterministic execution
  - *Examples*: Ethereum Virtual Machine (EVM)

- **Score 2**: Hybrid state model - mix of on-chain and off-chain state with defined sync
  - *Evidence*: Combination of on-chain consensus with off-chain computation
  - *Examples*: State channels, some Layer 2 solutions

- **Score 3**: Mutable database - traditional database with API-mediated state changes
  - *Evidence*: SQL-like state management, API-controlled mutations
  - *Examples*: Many enterprise blockchain solutions

- **Score 4**: Trust-dependent logic - execution depends on trusted intermediaries
  - *Evidence*: Manual approval processes, trusted third-party execution
  - *Examples*: Systems requiring manual transaction approval

---

## LAW DOMAIN (Human ↔ Human)

### Legal Form (Structure)

#### **ll_1** - Blockchain Legal Classification
**Question**: To what extent is the blockchain classified as a commodity versus security?

**Scoring Criteria:**
- **Score 0**: Definitively classified as commodity by relevant authorities
  - *Evidence*: Official regulatory determination as commodity
  - *Examples*: Bitcoin (CFTC classification), Ethereum (post-merge clarity)

- **Score 1**: Network token has established non-security status with regulatory clarity
  - *Evidence*: Utility token classification, no-action letters
  - *Examples*: Tokens with clear utility classification

- **Score 2**: Operating under regulatory safe harbor with defined parameters
  - *Evidence*: Safe harbor provisions, regulatory sandbox participation
  - *Examples*: Projects in regulatory sandbox programs

- **Score 3**: Subject to securities regulation with established compliance obligations
  - *Evidence*: Registered as security, compliance framework established
  - *Examples*: Tokenized securities with proper registration

- **Score 4**: Classification pending regulatory determination or legislative action
  - *Evidence*: Uncertain status, awaiting regulatory clarity
  - *Examples*: Many altcoins awaiting classification decisions

#### **ll_4** - Security Reclassification Risk
**Question**: To what extent could the blockchain be reclassified as a security?

**Scoring Criteria:**
- **Score 0**: Commodity classification protected by specific legislation
  - *Evidence*: Statutory protection, explicit commodity status
  - *Examples*: Bitcoin with CFTC jurisdiction clearly established

- **Score 1**: Classification established through formal regulatory precedent
  - *Evidence*: No-action letters, formal regulatory guidance
  - *Examples*: Tokens with official regulatory guidance

- **Score 2**: Current status dependent on maintaining specific technical characteristics
  - *Evidence*: Classification conditional on decentralization or technical features
  - *Examples*: Tokens classified as utility based on current use patterns

- **Score 3**: Could be reclassified as security based on usage patterns or governance
  - *Evidence*: Vulnerable to Howey test based on evolution
  - *Examples*: Tokens that could fail Howey test if governance changes

- **Score 4**: Classification entirely subject to pending legislation or enforcement
  - *Evidence*: Status depends on pending regulatory actions
  - *Examples*: Tokens awaiting CLARITY Act or similar legislation

---

## Scoring Validation Framework

### Quality Control Measures

1. **Evidence Documentation**: Each score must be supported by specific evidence
2. **Consistency Checks**: Cross-validation between related questions
3. **Temporal Validation**: Scores must reflect project status at assessment date
4. **Source Verification**: Primary sources required for critical determinations

### Statistical Validation

1. **Inter-rater Reliability**: Cohen's kappa > 0.7 required
2. **Internal Consistency**: Cronbach's alpha for related question clusters
3. **Predictive Validation**: Correlation with actual project outcomes

### Data Quality Requirements

- **Minimum 40 of 45 questions** must have deterministic scores
- **At least 3 primary sources** required for high-impact classifications
- **Documentation trail** required for all scoring decisions
- **Periodic re-scoring** for active projects (annual review)

---

## Conversion from Legacy Data

### Mapping Legacy Fields to SPF Scores

When converting existing CSV data to SPF scores, use the following mapping guidelines:

1. **Technical Platform** → Execution Architecture (se_2)
2. **Consensus Mechanism** → Multiple consensus questions (nc_1-nc_5)
3. **Status** → Multiple fitness questions based on failure/success reasons
4. **Consortium Size** → Governance questions (sg_1-sg_5)
5. **Failure Category** → Domain-specific risk patterns

### Missing Data Handling

- **Score 2 (Bounded Discretion)**: Default for missing data with moderate confidence
- **Evidence Required**: Score 0 or 4 require strong evidence
- **Conservative Bias**: When uncertain, score toward higher risk (higher number)

This rubric ensures systematic, reproducible conversion of descriptive project data into the deterministic SPF Matrix format required for statistical analysis and predictive modeling.