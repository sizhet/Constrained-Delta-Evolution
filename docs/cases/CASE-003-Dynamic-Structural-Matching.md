# CASE-003 — Dynamic Structural Matching

**Project:** Constrained Delta Evolution (CDE)  
**Series:** Cases / Research and Validation Designs  
**Status:** Proposed reference case, v1.0.0 draft  
**Date:** 2026-11-04  
**Authors:** Sizhe Tan and GPT-Obot  
**License:** Apache-2.0  
**Related Foundations:** CDE-001–012, especially CDE-002, CDE-003, CDE-004, CDE-006, CDE-007, CDE-008, CDE-009, CDE-010, and CDE-012  
**Related Specifications:** SPEC-001–006  
**Related research:** Structural Intelligence Framework (SIF); Beyond Matching: OTHERS and Directional Weak Evidence Intelligence (BM-ODWEI); Relational Evidence Navigation (REN)  
**Repository placement:** `docs/cases/CASE-003-Dynamic-Structural-Matching.md`

## Abstract

This case investigates **Dynamic Structural Matching (DSM)** as a constrained, evidence-aware exploration problem. Given two or more collections of entities, relations, attributes, and observations, a system must propose, compare, revise, and sometimes reject candidate correspondences. Unlike static similarity search, dynamic matching allows the evidence, graph structure, matching hypothesis, perspective, and evaluation metric to change during exploration. CDE provides an explicit State–Delta–Constraint–Branch framework for organizing these revisions and recording their consequences.

The proposed laboratory combines typed matching hypotheses, directional weak evidence, hard incompatibility constraints, alternative explanations, adaptive M-to-N branching, path memory, multi-agent hypothesis competition, and Structural Relay. It distinguishes **similarity**, **correspondence**, **identity**, **causality**, and **attribution**, which must not be silently conflated. The design includes a synthetic graph benchmark, privacy-preserving evaluation protocols, independent validation, uncertainty calibration, negative controls, counterexamples, and conformance tests. No empirical accuracy or runtime superiority is claimed without actual experiments.

**Core research question:** Can constrained, dynamically revised structural hypotheses improve calibrated correspondence discovery under incomplete and changing evidence, relative to strong static and iterative matching baselines, without increasing unsupported identity assertions or privacy risk?

---

## 1. Motivation: From Matching Scores to Structural Hypotheses

Conventional matching often computes a similarity score between two objects and chooses the highest-scoring correspondence. This works well in many applications, but can be inadequate when the correct interpretation depends on relationships, context, missing observations, competing explanations, or constraints across several matches.

Consider two knowledge graphs with partially overlapping concepts. An attribute-level comparison may suggest that nodes \(a\) and \(b\) match, yet their neighboring relations contradict the hypothesis. Conversely, two nodes may have different labels but highly compatible roles in corresponding subgraphs.

A dynamic matching system should therefore be able to:

1. propose multiple correspondences;
2. preserve competing hypotheses;
3. evaluate relational and directional evidence;
4. enforce one-to-one, type, or domain constraints when applicable;
5. seek additional discriminating evidence;
6. revise, split, merge, or retract hypotheses;
7. distinguish unresolved ambiguity from proven incompatibility;
8. transfer reusable methods and negative evidence across tasks.

CDE does not invent graph matching, record linkage, probabilistic inference, or constraint programming. Its proposed contribution is an **interoperable exploration and governance contract** for these processes.

## 2. Research Questions and Falsifiable Hypotheses

**RQ-1:** When does iterative structural revision outperform one-shot similarity ranking?

**RQ-2:** Does directional weak evidence help locate promising correspondences when direct attributes are incomplete?

**RQ-3:** Can explicit hard constraints reduce globally inconsistent assignments without causing excessive false exclusions?

**RQ-4:** Does a portfolio of local, regional, and structural-reconfiguration Deltas outperform a single operator family under matched evaluation cost?

**RQ-5:** Can multi-agent hypothesis competition improve uncertainty calibration and counterexample discovery?

**RQ-6:** Does Structural Relay provide positive transfer across related schemas and graph families, or does it amplify mistaken assumptions?

**RQ-7:** How should a system express `UNKNOWN` rather than forcing a match or nonmatch?

**H1 — Structural context:** Dynamic structural revision will improve correspondence F1 over static attribute-only matching on synthetic graph families with substantial attribute corruption and informative relational structure.

**H2 — Negative control:** On clean, uniquely labeled data, the dynamic method may offer little accuracy benefit and may incur unnecessary computation.

**H3 — Constraints:** Correctly specified global consistency constraints will reduce incompatible match sets; incorrectly specified constraints can cause systematic false negatives.

**H4 — Weak evidence:** Calibrated directional evidence can improve proposal efficiency, while misleading directional evidence can worsen it.

**H5 — Competition:** Independent hypothesis agents can improve detection of counterexamples, but correlated agents may create false consensus.

**H6 — Relay:** Target-validated methods may improve efficiency on related tasks; unvalidated source-specific assumptions may produce negative transfer.

All hypotheses require experimental testing. They are not findings.

## 3. Problem Definition

Let the source and target structures be attributed graphs:

\[
G_A=(V_A,E_A,\mathcal A_A),\qquad
G_B=(V_B,E_B,\mathcal A_B),
\]

where \(V\) are nodes, \(E\) typed relations, and \(\mathcal A\) node/edge attributes.

A candidate correspondence is:

\[
h=(u,v,\tau,\kappa),\quad u\in V_A,\;v\in V_B,
\]

where \(\tau\) is a hypothesis type (e.g. `same_concept`, `functional_analogue`, `possible_record_link`, `related_but_distinct`) and \(\kappa\) is a versioned interpretation context.

The goal is not always to find a bijection. Depending on the task, a valid matching can be one-to-one, one-to-many, many-to-many, partial, or explicitly unresolved.

Define a hypothesis set \(H\) and its evidence \(E_H\). A constrained optimization profile may seek:

\[
\max_H U(H;M_v,E_H)
\quad\text{subject to}\quad C_v(H)=\mathrm{PASS},
\]

where \(M_v\) is a versioned scoring model and \(C_v\) is the applicable hard-constraint bundle.

A high value of \(U\) does not prove identity, correctness, or causality. The decision policy must declare what a selected correspondence actually means.

### 3.1 Three distinct outputs

A DSM system SHOULD return:

- **Hypothesis portfolio:** candidate correspondences and alternatives;
- **Validated decisions:** accepted/rejected decisions under explicit evidence and policy;
- **Residual uncertainty:** unresolved candidates, missing evidence, counterexamples, and Leftover tasks.

A forced complete matching MUST NOT be assumed when the task permits abstention.

## 4. Matching Semantics and Claim Boundaries

The following terms have different meanings:

| Term | Appropriate claim | What it does not prove |
|---|---|---|
| Attribute similarity | Features resemble each other | Same entity |
| Structural similarity | Graph neighborhoods or roles resemble | Causal or identity relation |
| Functional analogy | Similar function under a perspective | Same mechanism |
| Correspondence | Task-defined mapping is supported | Universal equivalence |
| Record linkage | Records may refer to the same entity | Verified real-world identity without sufficient evidence |
| Attribution | Evidence supports responsibility or origin | Definitive attribution merely from technical overlap |
| Causal relation | Intervention/counterfactual evidence supports causation | Causation from correlation alone |

Every output MUST carry a `claim_type` and its decision threshold or validation basis. A structural matching system MUST NOT silently upgrade a similarity claim into an identity or attribution claim.

## 5. Scope and Application Profiles

The laboratory uses four profiles.

**P1 — Synthetic concept alignment.** Match nodes between transformed knowledge graphs with known ground truth. This is the primary evaluation profile.

**P2 — Schema and data integration.** Match field names, entities, and relational roles across generated heterogeneous datasets. Includes one-to-many mappings.

**P3 — Dynamic evidence matching.** Observations arrive in batches, and hypotheses are revised as the graph changes.

**P4 — Privacy-preserving relational inference.** Use synthetic or explicitly authorized data to evaluate uncertain associations and abstention policies. This profile must not be interpreted as a mandate for real-world surveillance or person identification.

The primary scientific conclusions SHOULD be based on P1–P3. P4 is an additional governance and uncertainty stress test.

## 6. Synthetic Benchmark Generation

A reproducible benchmark starts with a latent graph \(G^\star\) and creates two observed views.

\[
G_A=T_A(G^\star,\epsilon_A),\qquad
G_B=T_B(G^\star,\epsilon_B).
\]

Transformations may include:

1. node relabeling and alias generation;
2. attribute deletion, substitution, and noise;
3. edge deletion and insertion;
4. relation-type renaming;
5. node splitting and merging;
6. partial overlap between graphs;
7. distractor nodes and deceptive local neighborhoods;
8. delayed evidence arrival;
9. schema version drift;
10. changes in the task's perspective or matching policy.

The benchmark generator retains ground-truth mappings for evaluation. The matching algorithm MUST NOT access them during inference.

### 6.1 Example graph families

| Family | Structure | Stress factor |
|---|---|---|
| F1 | Labeled trees | Local topology and aliases |
| F2 | Directed acyclic graphs | Multiple parents and shared descendants |
| F3 | Community graphs | Similar neighborhoods and distractors |
| F4 | Bipartite graphs | Assignment consistency |
| F5 | Heterogeneous knowledge graphs | Relation types and schema drift |
| F6 | Temporal graphs | Evidence arrival and retraction |
| F7 | Split/merged entities | One-to-many correspondences |
| F8 | Adversarial lookalikes | Weak-evidence misdirection |
| F9 | Sparse overlap | Abstention and open-world matching |
| F10 | Cross-domain analogies | Perspective-dependent functional correspondence |

### 6.2 Controlled corruption parameters

Suggested benchmark dimensions:

\[
p_{\mathrm{attr\_missing}}\in\{0,0.2,0.5,0.8\},
\]

\[
p_{\mathrm{edge\_noise}}\in\{0,0.1,0.3\},
\]

with varying overlap ratios, node counts, relation types, and distractor prevalence.

These are proposed experiment settings, not measured results. A practical study SHOULD preregister a smaller factorial design to keep compute manageable.

## 7. State Container Contract

The logical DSM State Container includes:

\[
S_t=(G_A^v,G_B^v,H_t,E_t,C_v,M_v,P_v,B_t,\Gamma_t).
\]

Here:

- \(G_A^v,G_B^v\): versioned graph snapshots;
- \(H_t\): current hypothesis portfolio;
- \(E_t\): evidence ledger and uncertainty;
- \(C_v\): hard-constraint bundle;
- \(M_v\): metric/scoring version;
- \(P_v\): branch and validation policy version;
- \(B_t\): remaining budget;
- \(\Gamma_t\): path, provenance, and governance context.

The actual storage MAY use immutable references to large graph and evidence objects.

```yaml
state_id: "dsm-state-014"
schema_id: "cde-dsm-state"
schema_version: "1.0.0"
state_revision: "r14"
payload_ref_or_value:
  source_graph_ref: "graph-A-v3"
  target_graph_ref: "graph-B-v2"
  hypothesis_portfolio_ref: "portfolio-014"
  evidence_ledger_ref: "evidence-014"
  unresolved_hypotheses_ref: "leftover-014"
  remaining_validation_budget: 42
constraint_bundle_id: "dsm-constraints-v2"
context_refs:
  - "matching-metric-v4"
  - "generator-portfolio-v2"
  - "decision-policy-v3"
  - "path-context-014"
provenance_ref: "commit-014"
```

### 7.1 State identity and equivalence

Identical hypothesis sets do not imply identical future states if graph snapshots, evidence, remaining budgets, or path decisions differ.

A cache key for a score SHOULD include the graph, feature extraction, metric, and relevant evidence versions. A solver's future-equivalence key may require additional context.

## 8. Typed Hypothesis Contract

A canonical matching hypothesis SHOULD contain:

```yaml
MatchingHypothesis:
  hypothesis_id: string
  source_entity_ref: string
  target_entity_ref: string
  claim_type: same_concept | functional_analogue |
              possible_record_link | related_but_distinct |
              structural_correspondence
  cardinality_profile: one_to_one | one_to_many |
                       many_to_many | partial
  perspective_id: string
  evidence_refs: [string]
  counterevidence_refs: [string]
  score_refs: [string]
  status: PROPOSED | UNDER_REVIEW | ACCEPTED |
          REJECTED | DEFERRED | UNKNOWN | RETRACTED
  constraint_bundle_id: string
  metric_version: string
  provenance_ref: string
```

An accepted hypothesis is accepted **under a declared task policy**, not necessarily a verified real-world identity.

### 8.1 Alternative hypotheses

A system SHOULD preserve mutually exclusive alternatives until evidence or a legitimate constraint resolves them. Discarding all but the current top score can hide uncertainty and create brittle decisions.

### 8.2 Open-world handling

A node MAY have no corresponding node in the other graph. The benchmark MUST permit `UNMATCHED` or `UNKNOWN` where ground truth contains no match or evidence is insufficient.

## 9. Evidence Contract and Weak-Evidence Navigation

Evidence items should be typed:

```yaml
EvidenceRecord:
  evidence_id: string
  subject_refs: [string]
  evidence_type: attribute | relation | topology |
                 temporal | contextual | external |
                 negative | directional
  observation_or_claim: typed
  source_ref: string
  observation_context_ref: string
  reliability_model_ref: optional
  uncertainty: typed
  dependency_refs: [string]
  valid_from: optional
  valid_until: optional
  provenance_ref: string
```

The system MUST distinguish an observation from an interpretation and a source's assertion from independently validated evidence.

### 9.1 Correlated evidence

Two evidence items originating from the same underlying source are not independent confirmations. The scoring model SHOULD track dependencies to avoid double counting.

### 9.2 Directional evidence

A directional field can suggest which attributes, neighboring nodes, or relations to inspect next. It can also guide the selection of Delta operators. It MUST NOT become a hard identity rule solely because its score is high.

### 9.3 Perspective distance

Two structures may be similar under one perspective and dissimilar under another. The perspective ID and feature-selection rule MUST be explicit when a score depends on them.

### 9.4 Missingness

Absence of evidence is not always evidence of absence. The benchmark SHOULD distinguish:

- explicitly observed negative evidence;
- missing observations;
- unobserved but potentially obtainable evidence;
- structural impossibility under a hard rule.

## 10. Hard Constraints

Examples of hard constraints, when justified by the domain, include:

- incompatible entity types;
- mutually exclusive one-to-one assignments;
- impossible temporal orderings;
- inconsistent resource or cardinality assignments;
- prohibited use of restricted data;
- unauthorized identity claims;
- forbidden evidence sources;
- invalid schema versions.

A one-to-one constraint is appropriate only if the application actually requires one-to-one matching. It would be incorrect for split/merged entities or functional analogies.

### 10.1 Four-valued validation outcome

A hard check returns `PASS`, `FAIL`, `UNKNOWN`, or `ERROR`, consistent with SPEC-001. Unknown or error MUST NOT be treated as a pass for committing a decision that requires the check.

### 10.2 Soft penalties are not hard constraints

A mismatch in labels may reduce score without invalidating a match. Conversely, an actual hard incompatibility cannot be overridden by a high structural similarity score.

### 10.3 Constraint revision

A change in cardinality, authorization, or type rules requires a new constraint-bundle version and revalidation of affected decisions. Historical acceptance records MUST remain traceable.

## 11. Delta Operator Library

DSM uses typed Deltas that change hypotheses, evidence focus, or matching structure.

| Delta type | Effect | Typical scale |
|---|---|---|
| `ADD_MATCH` | Propose a correspondence | Local |
| `RETRACT_MATCH` | Withdraw a correspondence | Local |
| `SWAP_TARGET` | Replace a target candidate | Local |
| `ADD_ALTERNATIVE` | Preserve a competing hypothesis | Local |
| `EXPAND_NEIGHBORHOOD` | Inspect adjacent relations | Regional |
| `REWIRE_SUBGRAPH` | Revise a cluster of correspondences | Regional |
| `SPLIT_MATCH` | Convert one-to-one into candidate one-to-many | Regional |
| `MERGE_MATCH` | Propose a shared target or group mapping | Regional |
| `CROSS_CLUSTER_PROBE` | Explore distant graph regions | Long-range |
| `SCHEMA_BRIDGE` | Propose cross-schema feature mapping | Long-range |
| `REQUEST_EVIDENCE` | Seek discriminating observations | Directional |
| `CHALLENGE_HYPOTHESIS` | Search for counterexamples | Directional |
| `RECALIBRATE_SCORE` | Create a metric candidate | Meta-Delta |
| `UPDATE_GENERATOR` | Propose an operator/policy revision | Meta-Delta |

Metric and generator changes require the stricter SPEC-006 governance contract; they are not ordinary task-state changes.

### 11.1 Delta identity

A Delta definition is not a proposal instance. The same `SWAP_TARGET` operation may be proposed by several agents with different evidence. Both proposal histories should remain accessible even when the operation is deduplicated.

### 11.2 Delta scale

Scale is structural, not necessarily Euclidean. A cross-cluster probe can be long-range even if two nodes have similar names.

### 11.3 Candidate generation limits

A generator MUST declare whether it enumerates all admissible correspondences or samples a subset. A heuristic top-\(k\) generator cannot claim exhaustive coverage without a valid proof.

## 12. Adaptive M-to-N Branching

For each generation invocation, distinguish:

\[
M=\#\text{proposals},\quad
Q=\#\text{attempts},\quad
N=\#\text{realized edges},\quad
K=\#\text{unique successors}.
\]

An invocation might generate 20 hypotheses, select 8 for evidence checking, commit 3 valid changes, defer 3, and reject 2. These are illustrative counts only.

### 12.1 Branch selection criteria

A selector may consider:

- expected improvement in validated matching quality;
- uncertainty and information gain;
- structural diversity;
- constraint feasibility likelihood;
- validation cost;
- counterexample potential;
- coverage of unexplored graph regions;
- reliability of evidence sources.

The selector SHOULD reserve budget for alternative explanations and negative evidence. An overly aggressive exploitation policy may produce confident but incorrect matches.

### 12.2 Pruning

A low score MAY justify deprioritization but MUST NOT be called sound pruning. Certified pruning requires a valid bound or impossibility proof under the applicable solver contract.

## 13. Path Memory, NoSubtree, and Loop Back

A constructive matching profile can use path-local decision memory:

- `ACCEPT(h)` resolves a correspondence positively;
- `EXCLUDE(h)` resolves it negatively for the current path;
- `DEFER(h)` preserves the option for later investigation.

In a finite fixed-context constructive profile, the same decision SHOULD NOT be resolved twice along a path. However, DSM also supports reconfiguration: new evidence may justify retracting or revisiting a previously accepted match.

### 13.1 Version-sensitive re-evaluation

A re-evaluation under new evidence is not necessarily an illegal repeat. It MUST carry a new evidence/context version and a traceable reason for revisitation.

### 13.2 State revisitation

Two different Delta sequences can produce the same hypothesis set. A revisit policy must distinguish identical recorded state, future-equivalent state, and a mere resemblance.

### 13.3 Loop prevention

Possible policies include:

- fixed-decision NoSubtree;
- maximum revision count per hypothesis;
- tabu tenure;
- strict progress on a declared potential function;
- budget-bounded open exploration.

No-repeat Delta decisions do not guarantee the absence of state cycles. Conversely, a permitted state revisit does not automatically imply an infinite loop.

## 14. Matching Metrics and Calibration

A candidate score may combine several factors:

\[
s(h)=w_a s_{\rm attr}(h)
+w_r s_{\rm relation}(h)
+w_t s_{\rm topology}(h)
+w_c s_{\rm context}(h)
-w_x p_{\rm conflict}(h).
\]

This is an illustrative scalarization. It is not necessarily a calibrated probability.

A model claiming probabilities SHOULD be evaluated for calibration, not only ranking accuracy. A high score is not a legal or evidentiary threshold for identifying a real person.

### 14.1 Structural consistency

A global matching score may include penalties for inconsistent edge mappings. Such penalties should be distinguished from hard constraints and from independent evidence.

### 14.2 Metric versions

Comparisons across different scoring models require re-evaluation or a validated mapping. A new metric that improves its own score has not thereby demonstrated better matching quality.

### 14.3 Directional scoring

A directional score can prioritize evidence collection, for example choosing the neighbor relation most likely to discriminate between two competing hypotheses. It should be evaluated by actual information gain and downstream matching performance.

## 15. Multi-Agent Hypothesis Competition

A multi-agent profile separates roles:

1. **Proposal Agent:** generates candidate correspondences.
2. **Structural Agent:** checks graph-role and relational consistency.
3. **Counterexample Agent:** actively seeks contradictory evidence.
4. **Constraint Agent:** checks hard rules and authorization.
5. **Evidence Agent:** identifies missing observations and dependencies.
6. **Portfolio Agent:** selects exploration branches and budgets.
7. **Independent Validator:** evaluates candidate decisions against held-out or trusted evidence.

These are logical roles and need not correspond to seven physical models.

### 15.1 Independence caveat

Multiple agents sharing the same model, prompt, training data, or evidence source may make correlated errors. Agreement is not independent confirmation unless the dependence structure is examined.

### 15.2 Competitive Relay

Agents MAY publish competing hypotheses, counterexamples, and unresolved questions. The coordinator SHOULD preserve dissenting evidence rather than collapsing disagreement into a majority vote.

### 15.3 Decision authority

A proposal or scoring agent MUST NOT unilaterally bypass hard constraints or authorize a sensitive identity claim.

## 16. Structural Relay and Collective Learning

DSM supports the seven CDE Relay artifact families:

- **Result:** validated match set or abstention decision;
- **Evidence:** observations and counterexamples;
- **Constraint:** applicability or incompatibility rules;
- **Method:** successful or failed operator patterns;
- **Policy:** branch-allocation strategies;
- **Metric:** scoring hypotheses and calibration evidence;
- **Leftover:** unresolved mappings, missing observations, and deferred regions.

A Relay artifact SHOULD include:

```yaml
RelayArtifact:
  relay_id: string
  artifact_type: Result | Evidence | Constraint |
                 Method | Policy | Metric | Leftover
  source_task_ref: string
  source_versions: [string]
  applicability_conditions: [string]
  evidence_refs: [string]
  uncertainty: typed
  validation_status: PROPOSED | SOURCE_VALIDATED |
                     TARGET_VALIDATED | REJECTED
  governance_ref: string
  provenance_ref: string
```

### 16.1 Transfer validation

A source-task match or method does not automatically apply to a target graph. The receiver must check schema, relation semantics, evidence reliability, and task-specific constraints.

### 16.2 Negative evidence

A failed match may be useful in a related task, but a source-local rejection MUST NOT become a universal impossibility claim without additional support.

### 16.3 Repeated execution

Independent repeated matching runs can reveal instability and accumulate evidence. Repetition counts as meaningful temporal Relay only when its results and conditions are recorded and compared.

### 16.4 Collective learning claim

Shared artifacts are not automatically validated transfer, and validated transfer is not automatically measured collective learning. Learning claims require demonstrated target-task benefit under controlled evaluation.

## 17. Privacy, Security, and Attribution Boundaries

Dynamic structural matching may be used in low-risk ontology alignment or in high-impact decisions about people. The same technical score can have radically different consequences.

This case's primary benchmark uses **synthetic or appropriately authorized data**. The laboratory SHOULD avoid collecting personal identifiers, device fingerprints, network addresses, or other sensitive linkage features unless there is a lawful, necessary, and explicitly governed research purpose.

### 17.1 Identity and attribution caution

Shared infrastructure, a common public key, repeated behavior, or structural similarity can be evidence of a relationship but does not by itself establish that two observations come from the same individual or organization. Alternative explanations such as shared services, copied data, key reuse, compromised credentials, and correlated behavior must be considered.

### 17.2 Data minimization

The benchmark SHOULD use pseudonymous synthetic identifiers and record only features necessary to test the matching mechanism. Access, retention, and deletion policies must be documented.

### 17.3 Decision abstention

For consequential identity or attribution applications, the system SHOULD support explicit abstention, human review, independent corroboration, and appeal or correction mechanisms as appropriate.

### 17.4 Governance

Authorization to generate a hypothesis is not authorization to disclose, act on, or publicize an identity assertion. Sensitive decisions require a separate application-level governance process.

## 18. Experimental Baselines

Compare CDE-DSM with strong established approaches:

| ID | Method | Role |
|---|---|---|
| B0 | Exact key/label matching | Simple sanity baseline |
| B1 | Weighted attribute similarity | Static scoring |
| B2 | Bipartite maximum-weight matching | Global assignment under one-to-one profile |
| B3 | Graph neighborhood similarity | Structural baseline |
| B4 | Iterative collective entity resolution | Relational refinement |
| B5 | Graph embedding alignment | Learned structural representation |
| B6 | Probabilistic record linkage | Uncertainty-aware baseline |
| B7 | Constraint programming / ILP | Exact constrained profile where tractable |
| B8 | Active learning / query selection | Evidence acquisition baseline |
| C1 | CDE with local Deltas | Contract-controlled local revision |
| C2 | CDE multi-scale | Regional and cross-cluster Deltas |
| C3 | CDE multi-scale + directional evidence | Weak-evidence guidance |
| C4 | CDE multi-agent + validated Relay | Competition and transfer |

Baselines must receive comparable information, training data, query budgets, and compute accounting. If an exact solver is used on small instances, its guarantee must be preserved rather than attributed to CDE.

## 19. Evaluation Metrics

### 19.1 Precision, recall, and F1

For a declared matching relation:

\[
\mathrm{Precision}=\frac{TP}{TP+FP},\qquad
\mathrm{Recall}=\frac{TP}{TP+FN}.
\]

Define the zero-denominator policy before evaluation. Report micro and macro metrics where appropriate.

### 19.2 Abstention-aware accuracy

Report both accuracy on decided cases and coverage (fraction decided). High precision achieved by abstaining on nearly everything is not equivalent to high overall utility.

### 19.3 Calibration

For probabilistic outputs, report Brier score, log loss where valid, and calibration plots. Distinguish calibrated probability from an unnormalized matching score.

### 19.4 Global consistency

Count violations of applicable cardinality, type, temporal, and relational constraints. A method with high pairwise F1 but invalid global assignments should not be called fully successful.

### 19.5 Evidence efficiency

Measure validated correct correspondences per evidence query, objective evaluation, or unit cost.

### 19.6 Revision stability

Track hypothesis reversals, time to correction after contradictory evidence, and false-confidence persistence.

### 19.7 Discovery and diversity

Track coverage of distinct structural regions and the fraction of true correspondences considered by the generator. A generator cannot recover a true match it never proposes.

### 19.8 Cost

Report generation, scoring, validation, solver, agent coordination, and Relay overhead separately, as well as total wall time and resource use.

## 20. Experimental Protocol

### 20.1 Data partitions

Generate disjoint training, validation, and held-out test graph families. Use separate random seeds and avoid leaking latent node correspondences or graph-generation parameters that reveal answers.

### 20.2 Budgets

Suggested limits include:

- maximum proposals per source node;
- maximum evidence queries;
- maximum validation checks;
- maximum graph expansions;
- maximum wall-clock time;
- maximum model/API calls.

A fair comparison should report both a matched primary budget and total cost.

### 20.3 Repetitions

Use multiple independent graph instances and stochastic seeds. Thirty independent replicates per configuration is a reasonable initial design target, not a universal statistical guarantee.

### 20.4 Primary comparisons

Pre-register C2 versus B4/B5/B7 and C3 versus C2 as primary contrasts, subject to baseline feasibility and resource parity. Treat the remaining comparisons as secondary or exploratory.

### 20.5 Statistical analysis

Report paired differences, uncertainty intervals, and performance across corruption levels. Avoid treating thousands of node pairs from one graph as independent experimental replicates.

### 20.6 Reproducibility

Record graph generator versions, model versions, prompt templates if agents are used, seeds, constraint bundles, metric versions, budgets, and full decision provenance.

## 21. Reference Algorithm — Dynamic Structural Matching

```text
function DYNAMIC_STRUCTURAL_MATCH(task, configuration):
    state = initialize_versioned_state(task)
    hypothesis_portfolio = initialize_candidates(state)
    evidence_ledger = immutable_ledger()
    incumbent = validated_matching_or_empty()

    while budget_remaining(state) and not stopping_rule(state):
        invocation = pin_generation_context(
            state,
            generator_versions=configuration.generators,
            constraint_version=state.constraint_bundle,
            metric_version=state.metric_version,
            policy_version=state.policy_version
        )

        proposals = generate_delta_proposals(
            state,
            scales={LOCAL, REGIONAL, LONG_RANGE, DIRECTIONAL},
            budget=invocation.budget
        )

        record_proposals_and_coverage(proposals, invocation)

        decisions = select_branch_attempts(
            proposals,
            expected_information_gain=True,
            diversity_constraints=configuration.diversity_policy
        )

        for decision in decisions:
            record_branch_decision(decision)

            if decision.disposition != SELECTED:
                continue

            attempt = construct_attempt(decision, state)
            candidate = apply_delta_to_candidate_state(attempt)

            if not schema_preconditions_and_authorization_pass(candidate):
                record_noncommit(attempt)
                continue

            report = validate_hard_constraints(
                candidate,
                state.constraint_bundle,
                state.context_versions
            )

            if report.result != PASS:
                record_noncommit_with_evidence(attempt, report)
                continue

            evidence = evaluate_or_request_evidence(
                candidate,
                evidence_budget=state.remaining_budget
            )

            update_hypothesis_portfolio(candidate, evidence)
            record_evidence_dependencies(evidence_ledger, evidence)

            successor = commit_if_versions_match(
                source=state,
                candidate=candidate,
                validation_report=report,
                evidence=evidence
            )

            if successor.committed:
                state = successor.state
                incumbent = update_independently_validated_result(
                    incumbent, state
                )

        reconcile_M_Q_N_K_accounting(invocation)
        preserve_leftovers_and_counterexamples(state)

    return final_report(
        validated_decisions=incumbent,
        unresolved=hypothesis_portfolio.unresolved,
        evidence=evidence_ledger,
        costs=all_resource_costs(),
        provenance=all_versioned_records()
    )
```

This is a **reference control-flow sketch**, not a production-ready implementation. A practical implementation must avoid changing the active source revision while concurrently committing incompatible branch results, and it must not treat evidence acquisition as automatically authorizing a match decision.

## 22. Reference Algorithm — Counterexample Agent

```text
function CHALLENGE_HYPOTHESIS(hypothesis, graph_context, budget):
    alternatives = generate_alternative_explanations(hypothesis)
    dependencies = inspect_evidence_dependencies(hypothesis)

    tests = propose_discriminating_checks(
        hypothesis,
        alternatives,
        graph_context,
        budget
    )

    for test in tests:
        observation = execute_authorized_test(test)
        record_provenance(observation)

        if observation.contradicts_hard_assumption:
            return CHALLENGED_WITH_EVIDENCE

        if observation.is_inconclusive:
            preserve_as_UNKNOWN(observation)

    return UNRESOLVED_OR_SUPPORTED_UNDER_TESTED_SCOPE
```

The absence of a found counterexample does not prove the hypothesis true.

## 23. Worked Example A — Ontology Alignment

Graph A uses concepts `Student`, `School`, `Route`, and `Vehicle`. Graph B uses `Pupil`, `Campus`, `Trip`, and `Bus`.

A label-based matcher may correctly propose `Student ↔ Pupil` but struggle with `Route ↔ Trip` because the terms have different operational scopes. A structural matcher considers relations such as `attends`, `served_by`, and `operated_by`.

Candidate hypotheses include:

- `Student ↔ Pupil`: strong lexical and relational support;
- `School ↔ Campus`: plausible context-dependent correspondence;
- `Route ↔ Trip`: partial functional analogy, not necessarily identity;
- `Vehicle ↔ Bus`: a subtype relation may be more accurate than equivalence.

The appropriate claim type matters. A system that forces every pair into `same_concept` may achieve misleading scores.

## 24. Worked Example B — Dynamic Graph Revision

At time \(t_0\), node \(a\) appears to correspond to node \(b_1\) based on labels. At \(t_1\), a new relation reveals that \(b_1\) has an incompatible role, while \(b_2\) has the expected neighborhood.

The system proposes:

1. `CHALLENGE_HYPOTHESIS(a,b_1)`;
2. `RETRACT_MATCH(a,b_1)`;
3. `ADD_MATCH(a,b_2)`;
4. `REQUEST_EVIDENCE` for a discriminating relation.

The new evidence version justifies revisiting the prior decision. Historical acceptance is not deleted; it is superseded by a traceable revision.

## 25. Worked Example C — One-to-One Versus One-to-Many

Suppose a source concept `TransportationService` is represented by two target concepts, `RoutePlanning` and `VehicleDispatch`.

A one-to-one matching constraint would incorrectly force the system to choose one. The correct profile permits a composite or one-to-many mapping.

The case demonstrates why cardinality is part of the task contract, not a universal hard rule.

## 26. Worked Example D — Weak Evidence and Alternative Explanations

Two synthetic records share a rare technical attribute and similar activity timing. The evidence supports further examination but not definitive linkage.

Alternative explanations include:

- shared service or infrastructure;
- reused template or software;
- common external event;
- copied or inherited metadata;
- genuinely related but distinct entities.

A Directional Delta may propose inspecting a different relational feature. The system should preserve uncertainty, source dependencies, and the possibility of `UNKNOWN`.

This example tests evidence discipline, not real-world deanonymization.

## 27. Worked Example E — Multi-Agent Competition

Three proposal agents suggest different target matches for one source node. A counterexample agent identifies a temporal inconsistency in the highest-scoring option. The constraint agent rejects that option under a hard rule, while an evidence agent requests more information about the remaining alternatives.

The orchestrator records all candidates, the rejection reason, the deferred evidence request, and the currently best validated hypothesis. A simple majority vote would have been insufficient.

## 28. Worked Example F — Cross-Task Method Relay

A source ontology-alignment task finds that relation-neighborhood checks are more reliable than labels under high attribute corruption. It publishes a Method Relay artifact with conditions, supporting results, and failure cases.

A target task with similar corruption receives the artifact but independently validates the method on a held-out subset. If target performance improves, the method may be adopted under the target policy. If not, it is rejected or retained as a hypothesis.

## 29. Negative Controls

**NC-1 — Perfect labels:** Exact matching should be strong; dynamic search may add cost without benefit.

**NC-2 — Deceptive graph symmetry:** Several nodes have indistinguishable neighborhoods. The system should express ambiguity rather than fabricate certainty.

**NC-3 — Incorrect cardinality assumption:** One-to-one constraints are imposed on a one-to-many ground truth.

**NC-4 — Misleading directional field:** Directional scores systematically favor false matches.

**NC-5 — Correlated evidence:** Multiple observations derive from one underlying source.

**NC-6 — Delayed contradictory evidence:** A previously accepted match becomes untenable.

**NC-7 — Negative transfer:** A source-task rule fails in a different target schema.

**NC-8 — No true match:** A source node has no corresponding target node.

**NC-9 — Adversarial lookalikes:** Distractors mimic high-value structural signatures.

**NC-10 — Expensive validation:** Agent and evidence costs dominate matching gains.

Negative controls are essential for measuring failure boundaries and should not be omitted from publication.

## 30. Ablation Studies

| Ablation | Removed or changed mechanism | Research question |
|---|---|---|
| A1 | No relational evidence | Value of graph structure |
| A2 | No directional evidence | Value of guided exploration |
| A3 | No long-range Deltas | Value of cross-cluster search |
| A4 | No counterexample agent | Value of active contradiction search |
| A5 | No hard consistency constraints | Global inconsistency cost |
| A6 | No alternative hypotheses | Cost of premature commitment |
| A7 | No abstention | Effect on false-positive rate |
| A8 | No path memory | Duplicate work and cycling |
| A9 | No evidence dependency tracking | Double-counting bias |
| A10 | No Relay | Transfer benefit versus overhead |
| A11 | No independent target validation | Negative transfer risk |
| A12 | Fixed versus adaptive branching | Allocation value |
| A13 | No metric-version controls | Cross-version contamination |
| A14 | No generator evolution | Meta-learning contribution |

Ablations should preserve comparable budgets and should report when removing a feature reallocates resources to other mechanisms.

## 31. Conformance Test Matrix

| ID | Test | Expected result |
|---|---|---|
| DSM-001 | Valid graph State Container | Accepted |
| DSM-002 | Unknown graph schema | Rejected or deferred |
| DSM-003 | Stale evidence snapshot | Revalidation required |
| DSM-004 | Unsupported claim type | Rejected |
| DSM-005 | Missing provenance | Conformance failure |
| DSM-006 | Invalid Delta parameters | Rejected |
| DSM-007 | Same Delta from two agents | Distinct proposals retained |
| DSM-008 | M/Q/N/K reconciliation | Correct event counts |
| DSM-009 | Partial generation | No exhaustive claim |
| DSM-010 | High score, hard type conflict | Not committed |
| DSM-011 | Required check UNKNOWN | Not committed |
| DSM-012 | One-to-many profile | Multiple targets permitted |
| DSM-013 | One-to-one profile | Conflicting assignments rejected |
| DSM-014 | `DEFERRED` hypothesis | Not treated as excluded |
| DSM-015 | Uncertified heuristic prune | Rejected |
| DSM-016 | Path-local exclusion | Does not become global impossibility |
| DSM-017 | Evidence-version change | Legitimate revision traceable |
| DSM-018 | Duplicate state, different evidence | Not assumed future-equivalent |
| DSM-019 | Directional score only | Cannot prove identity |
| DSM-020 | Correlated evidence | Dependency recorded |
| DSM-021 | Missing evidence | Not treated as negative proof |
| DSM-022 | Counterexample found | Challenge recorded |
| DSM-023 | No counterexample found | No proof of truth claimed |
| DSM-024 | Multi-agent agreement | Not treated as independent corroboration automatically |
| DSM-025 | Relay from different schema | Applicability checked |
| DSM-026 | Failed target validation | Relay not promoted |
| DSM-027 | Metric version changed | Re-evaluation or guarded comparison |
| DSM-028 | Generator version changed | Coverage claim reconsidered |
| DSM-029 | Unauthorized identity disclosure | Blocked |
| DSM-030 | No true match | `UNMATCHED` or `UNKNOWN` possible |
| DSM-031 | Invalid global merge | Rejected |
| DSM-032 | Stale distributed worker | Marked stale or revalidated |
| DSM-033 | Retry idempotency | No duplicate committed effect |
| DSM-034 | Missing budget report | Conformance failure |
| DSM-035 | Synthetic truth hidden | No inference-time oracle access |
| DSM-036 | Clean-label negative control | Reported even if CDE underperforms |
| DSM-037 | Misleading direction | Failure measured |
| DSM-038 | Calibration claim | Calibration evidence required |
| DSM-039 | Identity claim from similarity alone | Unsupported claim blocked |
| DSM-040 | Leftover unresolved hypotheses | Preserved and typed |
| DSM-041 | Failed evidence query | Typed error and cost |
| DSM-042 | External source becomes unavailable | Context invalidation handled |
| DSM-043 | Historical decision retracted | New event; old retained |
| DSM-044 | Privacy-sensitive feature requested | Authorization enforced |
| DSM-045 | Global optimality asserted by heuristic | Claim rejected |
| DSM-046 | Repeated execution | Distinct runs and evidence retained |
| DSM-047 | Exact solver certificate | Verified under pinned context |
| DSM-048 | Benchmark leakage attempt | Rejected or flagged |

These are proposed conformance cases, not reports of tests already executed.

## 32. Formal Safety Invariants

**INV-DSM-01 — Typed hypotheses:** Every active hypothesis declares a claim type and applicable cardinality profile.

**INV-DSM-02 — Provenance:** Every committed hypothesis decision has traceable evidence and a versioned validation record.

**INV-DSM-03 — Constraint supremacy:** No score overrides an applicable hard constraint.

**INV-DSM-04 — Unknown preservation:** Missing or indeterminate evidence is not silently converted into `PASS`.

**INV-DSM-05 — Historical integrity:** Retraction or revision does not erase prior decisions.

**INV-DSM-06 — Scope fidelity:** Path-local exclusion does not imply global infeasibility.

**INV-DSM-07 — Equivalence discipline:** Similar payloads are not assumed future-equivalent without a valid contract.

**INV-DSM-08 — Evidence dependence:** Correlated sources are not represented as independent confirmations without justification.

**INV-DSM-09 — Metric coherence:** Score comparisons respect metric versions.

**INV-DSM-10 — Relay validation:** Source evidence is not automatically promoted in the target context.

**INV-DSM-11 — Claim calibration:** Similarity and identity assertions remain distinguishable.

**INV-DSM-12 — Privacy boundary:** Sensitive disclosures require separate authorization.

**INV-DSM-13 — Budget accountability:** All attempted evidence and evaluation operations are recorded.

**INV-DSM-14 — Negative knowledge scope:** Local failure is not generalized beyond its validated conditions.

**INV-DSM-15 — Agent authority:** No proposal agent can bypass independent validation and commit governance.

**INV-DSM-16 — No false guarantee:** Conformance does not imply perfect matching, completeness, or optimality.

These invariants are conditional on correct validators, schema definitions, and trust boundaries.

## 33. Conditional Correctness Arguments

### 33.1 Constraint preservation

If the initial committed state satisfies its hard constraints and every committed Delta is correctly validated under coherent versions, then committed states preserve the applicable hard constraints.

This follows by induction over committed transitions. It does not prove that the constraints capture every relevant real-world requirement.

### 33.2 Matching consistency

Under a correctly specified one-to-one profile, if every commit validates global one-to-one consistency atomically, then the committed match set remains one-to-one.

This does not establish that the matched pairs are semantically correct.

### 33.3 Finite constructive termination

If the hypothesis decision universe is finite and fixed, each constructive path resolves one previously unresolved decision, and no new decisions are introduced, then path depth is bounded by the size of that universe.

This does not apply unchanged to dynamic evidence arrival, hypothesis retraction, new operator generation, or open-ended exploration.

### 33.4 No universal correctness claim

A complete enumeration of candidate pairs does not guarantee correct decisions when evidence is ambiguous or the scoring/validation model is unsound. Conversely, a sound validator does not guarantee that the generator proposes the correct hypothesis.

## 34. Performance and Complexity Considerations

For two graphs with \(|V_A|=n\) and \(|V_B|=m\), the naive pair universe contains \(nm\) candidate correspondences before structural combinations are considered.

A top-\(k\) generator can reduce the number evaluated, but may omit true matches. Relational consistency may require global optimization with substantially higher cost. A multi-agent approach can add independent evidence but also introduce coordination overhead.

The benchmark SHOULD measure:

- candidate recall before selection;
- score computation cost;
- graph-neighborhood expansion cost;
- global consistency checking cost;
- evidence query cost;
- agent orchestration cost;
- memory footprint;
- total wall time.

No asymptotic efficiency advantage is claimed for CDE as a framework.

## 35. Validation Roadmap

**V1 — Contract review:** Define State Container, Delta, hypothesis, evidence, and Relay schemas.

**V2 — Synthetic graph fixtures:** Generate small graphs with known correspondences, distractors, and missing matches.

**V3 — Static baselines:** Establish exact-label, attribute, structural, and global-assignment baselines.

**V4 — Dynamic revisions:** Test evidence arrival, retraction, and context-version changes.

**V5 — Constraint stress tests:** Evaluate cardinality, type, and temporal constraints, including deliberately incorrect rules.

**V6 — Multi-scale generation:** Compare local-only, fixed multi-scale, and adaptive branching.

**V7 — Weak-evidence calibration:** Add calibrated, noisy, and misleading directional fields.

**V8 — Multi-agent competition:** Test counterexample discovery and correlated-agent failures.

**V9 — Relay transfer:** Validate source-to-target applicability and negative transfer.

**V10 — Distributed conformance:** Inject stale workers, duplicate retries, and inconsistent results.

**V11 — Independent replication:** Release code, manifests, held-out results, uncertainty intervals, and negative findings.

A phase MUST NOT be reported complete without the relevant evidence.

## 36. Proposed Repository Companion

```text
case-003-dynamic-structural-matching/
├── README.md
├── benchmark/
│   ├── graph_generator.py
│   ├── corruption_profiles.py
│   ├── ground_truth.py
│   └── fixtures/
├── contracts/
│   ├── state_schema.json
│   ├── hypothesis_schema.json
│   ├── evidence_schema.json
│   └── relay_schema.json
├── generators/
│   ├── local_match.py
│   ├── neighborhood_expand.py
│   ├── cross_cluster_probe.py
│   └── directional_evidence.py
├── solvers/
│   ├── baseline_matching.py
│   ├── global_assignment.py
│   └── cde_orchestrator.py
├── agents/
│   ├── proposal_agent.py
│   ├── counterexample_agent.py
│   └── independent_validator.py
├── experiments/
│   ├── preregistration.yaml
│   ├── train_manifest.yaml
│   ├── test_manifest.yaml
│   └── run.py
├── tests/
│   ├── test_conformance.py
│   ├── test_negative_controls.py
│   └── test_privacy_boundaries.py
└── reports/
    ├── results.csv
    ├── calibration.csv
    └── limitations.md
```

This is a proposed future implementation layout, not a claim that these files have been created.

## 37. Reporting Template

```yaml
run_id: string
graph_family: string
graph_seed: integer
source_nodes: integer
target_nodes: integer
ground_truth_profile: string
matching_method: string
metric_version: string
constraint_bundle_id: string
generator_version: string
proposal_count: integer
attempt_count: integer
committed_edge_count: integer
unique_successor_count: integer
candidate_recall: number_or_null
precision: number_or_null
recall: number_or_null
f1: number_or_null
abstention_rate: number
global_constraint_violations: integer
evidence_queries: integer
total_cost: number
wall_time_seconds: number
calibration_metric: number_or_null
negative_transfer_detected: boolean_or_null
conformance_status: string
provenance_ref: string
```

A reporting pipeline MUST not fill unknown values with invented zeros. Metrics with undefined denominators must be labeled appropriately.

## 38. Interpretation and Generalization Boundaries

Success on synthetic graph alignment does not establish reliable real-world identity attribution. A synthetic benchmark can verify that a system handles structural evidence, changing constraints, and abstention correctly, but real deployments require domain-specific validity studies, governance, and safeguards.

Likewise, the ability to represent a matching hypothesis does not imply that the generator will discover it, that the evidence will distinguish it, that the solver will select it, or that the result will be efficient.

\[
\boxed{
\text{Representability}
\neq
\text{Candidate Coverage}
\neq
\text{Validated Correspondence}
\neq
\text{Reliable Attribution}
}
\]

The case's strongest contribution may be a disciplined representation of uncertainty and revision, not necessarily a higher headline F1 score.

## 39. Open Research Questions

1. Which structural invariants transfer across graph families?
2. How should perspective-dependent similarity be represented without collapsing incompatible meanings?
3. Can directional weak evidence improve candidate recall at fixed cost?
4. How can candidate generators provide conditional coverage guarantees?
5. When is a global one-to-one assignment harmful?
6. How should graph splitting and merging affect hypothesis identity?
7. What is the best strategy for evidence acquisition under uncertainty?
8. Can independent counterexample search improve calibration?
9. How can correlated multi-agent errors be measured?
10. When does path memory reduce redundant work without blocking legitimate revisions?
11. How should negative evidence be transferred across schemas?
12. Can a Relay registry support collective learning without shared decision authority?
13. How can metric evolution avoid self-confirming matching scores?
14. What are the best benchmarks for abstention and open-world matching?
15. When does distributed matching outperform centralized matching after coordination costs?
16. How should privacy and evidence provenance be incorporated into the state contract?
17. Can CDE improve SIF localization for structural correspondences and method discovery?
18. Which improvements remain after comparison with established collective entity-resolution methods?

## 40. Conclusion

Dynamic Structural Matching provides a useful third CDE case because it introduces **uncertain evidence and competing explanations** into the State–Delta–Constraint exploration framework.

The proposed cycle is:

\[
\boxed{
\text{Propose Correspondence}
\rightarrow
\text{Gather Structural Evidence}
\rightarrow
\text{Challenge Alternatives}
\rightarrow
\text{Validate Constraints}
\rightarrow
\text{Revise or Commit}
\rightarrow
\text{Relay Evidence and Leftovers}
}
\]

Its value lies in making uncertainty, decision scope, revision history, and validation explicit. Strong evidence can justify a task-specific correspondence; weak evidence can justify further investigation; missing evidence should remain missing; and an unsupported identity claim should not be manufactured from a similarity score.

The research program should test whether adaptive structural exploration improves correspondence quality and efficiency **without sacrificing calibration, traceability, or governance**. Any superiority claim must be supported by reproducible comparisons against strong baselines and by transparent negative results.

---

## Appendix A — Minimal Synthetic Fixture

```yaml
fixture_id: "dsm-toy-001"
source_graph:
  nodes:
    - {id: "A:student", type: "concept", label: "Student"}
    - {id: "A:school", type: "concept", label: "School"}
    - {id: "A:route", type: "concept", label: "Route"}
  edges:
    - {from: "A:student", relation: "attends", to: "A:school"}
    - {from: "A:student", relation: "served_by", to: "A:route"}
target_graph:
  nodes:
    - {id: "B:pupil", type: "concept", label: "Pupil"}
    - {id: "B:campus", type: "concept", label: "Campus"}
    - {id: "B:trip", type: "concept", label: "Trip"}
  edges:
    - {from: "B:pupil", relation: "enrolled_at", to: "B:campus"}
    - {from: "B:pupil", relation: "rides_on", to: "B:trip"}
hypothesis_candidates:
  - {source: "A:student", target: "B:pupil", claim_type: "same_concept"}
  - {source: "A:school", target: "B:campus", claim_type: "structural_correspondence"}
  - {source: "A:route", target: "B:trip", claim_type: "functional_analogue"}
constraint_bundle_id: "dsm-toy-constraints-v1"
metric_version: "dsm-toy-metric-v1"
```

This fixture is a semantic illustration, not a validated ontology-alignment ground truth.

## Appendix B — Hypothesis Decision Record

```yaml
decision_id: "dsm-decision-019"
hypothesis_id: "hypothesis-019"
source_state_revision: "r14"
disposition: "DEFERRED"
reason_codes:
  - "MISSING_DISCRIMINATING_RELATION"
evidence_refs:
  - "evidence-003"
counterevidence_refs:
  - "evidence-011"
constraint_bundle_id: "dsm-constraints-v2"
metric_version: "matching-metric-v4"
validation_status: "UNKNOWN"
requested_followup:
  type: "REQUEST_EVIDENCE"
  target: "relation-neighborhood"
provenance_ref: "decision-event-019"
```

## Appendix C — Evidence and Claim Registry

| Claim | Required evidence | Insufficient evidence alone |
|---|---|---|
| Schema conformance | Passing SPEC-001 checks | Well-formed-looking YAML |
| Branch accounting | Proposal/attempt/edge ledger | Summary counts without trace |
| Valid match under policy | Applicable constraint and evidence validation | Highest score |
| Improved F1 | Held-out labeled evaluation | One favorable example |
| Calibrated confidence | Calibration study | A score between zero and one |
| Effective Relay | Independent target validation | Source-task success |
| Reliable identity attribution | Domain-specific corroboration and governance | Structural similarity |
| Global optimum | Valid solver certificate | Best observed candidate |
| Collective learning | Measured target improvement | Shared artifacts |

## Appendix D — Figure Guidance

Potentially relevant CDE repository figures:

- `Fig-003-State-Container-and-Constraint-Governed-Delta.png`
- `Fig-004-Adaptive-M-to-N-Delta-Branching.png`
- `Fig-005-Path-Memory-No-Subtree-and-Loop-Back.png`
- `Fig-008-Solution-and-Metric-Co-Evolution.png`
- `Fig-009-Distributed-CDE-Orchestrator-and-Structural-Relay.png`

No figures are asserted to have been generated or embedded in this manuscript.

## Appendix E — Research Status and Attribution

This is a proposed CDE research design, not a completed experimental study. It draws on established work in graph matching, ontology alignment, record linkage, probabilistic inference, active learning, graph embeddings, constraint programming, and multi-agent validation. Its proposed contribution is a versioned, contract-driven orchestration of dynamic structural hypotheses, directional evidence, validation, and Relay.

No measured matching accuracy, speedup, attribution success, or optimality certificate is claimed. Executable code, independently run benchmarks, formal citations, and external validation remain future work. No DOI is asserted until release and registration.

**Version:** v1.0.0 draft  
**License:** Apache-2.0
