# SPEC-005 — Distributed Exploration and Relay Contract

**Project:** Constrained Delta Evolution (CDE)  
**Series:** Specifications  
**Status:** Proposed normative contract, v1.0.0 draft  
**Date:** 2026-11-04  
**Authors:** Sizhe Tan and GPT-Obot  
**License:** Apache-2.0  
**Normative dependencies:** SPEC-001, SPEC-002, SPEC-003, SPEC-004  
**Companion:** SPEC-006 — Metric and Recursive Evolution Contract  
**Foundations:** CDE-008, CDE-009, CDE-010, CDE-011, CDE-012

---

## Abstract

This proposed specification defines a versioned, auditable, solver-neutral **Distributed Exploration and Structural Relay Contract** for Constrained Delta Evolution (CDE). It covers task envelopes, immutable context snapshots, task partitioning and coverage, leases and retries, at-least-once delivery and idempotent effects, worker trust, result verification, globally coupled constraints, shared incumbents and certified bounds, partial results, checkpoint/restart, failure recovery, termination detection, and typed Structural Relay artifacts. It distinguishes receipt of information from validation, applicability, assimilation, and measured collective learning. It offers required and optional conformance profiles, formal conditional invariants, a 60-case test suite, algorithms, and concrete case studies. It does not claim that distributed execution necessarily accelerates a task, that local solutions always compose, or that shared evidence is inherently correct.

## 1. Normative language, dependencies, and purpose

**MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and **MAY** denote normative requirements for the declared profile. Baseline: **CDE-Distributed-Relay-Core-v1**. Normative dependencies: SPEC-001 (state/constraint/commit), SPEC-002 (Delta/proposal/branch accounting), SPEC-003 (path memory/Loop Back), SPEC-004 (solver/optimization/verification). The present contract defines inter-agent orchestration and evidence exchange, not a network transport, programming language, consensus protocol, cryptographic scheme, or universal solver. SPEC-006 governs promotion of metrics and recursive policies. A conforming system MUST declare its actual consistency boundary and trust model. Distributed work MAY be centrally orchestrated, federated, or hierarchical; the control plane is logical, not necessarily a single physical server.

## 2. Core distinctions

A **task** is an authorized, version-pinned unit of exploration; a **lease** is a time/epoch-bounded right to work; a **result** is a worker report; a **verified result** has passed required checks; an **accepted global update** has passed applicable reconciliation; a **relay artifact** is transferable information with provenance and assumptions; **assimilation** is a validated use of a relay artifact in another context. These are different events. **Worker completion ≠ task acceptance ≠ global feasibility ≠ proof of optimality.** A shared result is not necessarily an independently reproducible result. A negative finding is scoped to its assumptions, search coverage, versions, and time. A partial task is not a completed search partition.

## 3. Logical architecture and authority boundaries

The architecture has: (a) a **Task Registry** holding task identities and immutable definitions; (b) a **Scheduler/Orchestrator** assigning work and budgets; (c) **Workers/Solvers** executing authorized tasks; (d) an **Independent Verifier** checking evidence; (e) a **Global Reconciler** checking coupled constraints; (f) an **Incumbent/Bound Registry** holding versioned claims; (g) a **Relay Registry** storing typed artifacts and lifecycle states; and (h) an **Audit/Policy Service** recording authorization, events, and updates. Roles MAY be combined physically but MUST remain logically separable. A generator's proposal authority MUST NOT implicitly confer global commit authority, metric governance authority, or permission to revise hard constraints. Trust decisions must be explicit.

## 4. Distributed Exploration Unit (DEU)

A DEU is the immutable logical work specification

\[
W=(id,S_0,\Delta_{scope},C_v,M_v,P_v,B,V,\Gamma,\pi),
\]

where `id` is a unique task identity; `S0` an immutable source state/snapshot; `Delta_scope` the permitted exploration scope; `Cv` hard-constraint bundle/version; `Mv` objective/metric version; `Pv` generator/solver/control policy versions; `B` resource budget; `V` verification requirements; `Γ` governance/authorization context; and `π` provenance/lineage. Implementations MUST additionally bind relevant schema, external context, path-memory, and equivalence versions when they affect validity. An unversioned pointer to mutable data is insufficient for a reproducible task. A task MAY reference immutable artifacts instead of copying large payloads.

## 5. Canonical task envelope

```yaml
DistributedExplorationTask:
  task_id: string
  task_version: string
  parent_task_id: optional
  lineage_refs: [string]
  source_state_ref: {state_id: string, revision: string, digest: string}
  schema_ref: string
  delta_scope: typed
  path_context_ref: optional
  constraint_bundle_ref: string
  metric_ref: string
  generator_ref: string
  solver_policy_ref: string
  context_snapshot_refs: [string]
  equivalence_contract_ref: optional
  resource_budget: typed
  verification_contract_ref: string
  authorization_ref: string
  expected_output_contract_ref: string
  idempotency_scope: string
  provenance_ref: string
```

The task MUST identify the precise domain and versions under which a result is to be interpreted. A change in any future-relevant version MUST create a new task version or an explicit revalidation event. Task identity MUST NOT be silently reused to represent materially different work.

## 6. Worker capability and trust manifest

```yaml
WorkerManifest:
  worker_id: string
  worker_version: string
  supported_state_schemas: [string]
  supported_delta_types: [string]
  supported_solver_profiles: [string]
  verification_capabilities: [string]
  resource_limits: typed
  trust_domain: string
  security_capabilities: [string]
  software_artifact_digests: [string]
  provenance_ref: string
```

The scheduler MUST match required task capabilities and authorization to the worker manifest. Worker self-declarations are claims, not proof of competence. Deployments SHOULD use independent validation, attestation, or sandboxing appropriate to their threat model. A worker MUST NOT receive credentials or data beyond the task's authorized scope. An untrusted worker MAY propose results, but cannot unilaterally authorize global acceptance.

## 7. Task lifecycle and durable event log

Canonical states: `PROPOSED → VALIDATED → QUEUED → LEASED → RUNNING → REPORTING → VERIFIED → ACCEPTED`, with terminal alternatives `REJECTED`, `FAILED`, `CANCELLED`, `EXPIRED`, `SUPERSEDED`, and `PARTIAL`. A task may be retried or resumed through new attempt records. Every state transition MUST have an immutable event ID, timestamp or logical ordering, actor, prior state, new state, reason, and version context. A system MUST NOT equate `RUNNING`, `REPORTED`, or `VERIFIED` with `ACCEPTED`. `PARTIAL` is a first-class status; remaining work MUST be accounted for. `SUPERSEDED` preserves old results for audit while preventing stale acceptance.

## 8. Lease, epoch, and fencing semantics

```yaml
TaskLease:
  task_id: string
  attempt_id: string
  worker_id: string
  lease_id: string
  epoch: integer
  granted_at: timestamp
  expires_at: timestamp
  allowed_actions: [string]
  budget_cap: typed
  issuer_ref: string
```

A lease MAY expire while a worker continues computing. The coordinator MUST fence outdated lease epochs from mutating shared authoritative state. Clock skew, delayed messages, and network partitions must be handled under documented assumptions; a wall-clock expiry alone is not a global synchronization proof. A worker result from an expired lease MAY be retained as evidence but MUST undergo current-version checks before acceptance. A new lease MUST not silently inherit the authority of an older attempt.

## 9. Delivery semantics and idempotency

Messages may be lost, duplicated, delayed, or reordered. A baseline implementation SHOULD assume **at-least-once delivery** and implement idempotent authoritative effects. `task_id`, `attempt_id`, `message_id`, and `idempotency_key` MUST have declared scopes. Duplicate messages MUST NOT create duplicate committed state transitions, double-consume exclusive resources, or double-promote an incumbent. Exactly-once *effects* require explicit storage/transaction assumptions and are not established by a message broker's delivery label. A repeated computation MAY be useful for independent validation or temporal evidence accumulation, but it MUST be distinguished from unintended duplicate side effects.

## 10. Partitioning and coverage contract

A parent exploration scope \(\Omega\) may be divided into child scopes \(\Omega_1,\dots,\Omega_k\). The partition report MUST state whether it claims: (a) complete coverage, (b) partial coverage, or (c) heuristic sampling. Complete coverage requires a justified condition such as

\[
\Omega\subseteq\bigcup_{i=1}^{k}\Omega_i.
\]

Disjointness is a separate property: \(\Omega_i\cap\Omega_j=\varnothing\) for \(i\ne j\). Overlap MAY be intentional for verification or robustness. The scheduler MUST NOT infer coverage from task count. A child task MUST inherit the relevant parent constraints and path context, or declare a sound transformation. Every unresolved region MUST remain in a typed **Leftover** record.

## 11. Split and merge records

```yaml
PartitionRecord:
  parent_task_id: string
  child_task_ids: [string]
  parent_scope_digest: string
  child_scope_digests: [string]
  coverage_claim: complete | partial | heuristic
  overlap_policy: disjoint | permitted | intentional_redundancy
  coverage_evidence_refs: [string]
  inherited_context_refs: [string]
  unresolved_scope_refs: [string]
  verifier_ref: optional
```

A merge MUST NOT assume that individually valid child solutions can be combined. Merge requirements depend on the domain: union, best-of, Pareto aggregation, shared incumbent update, or globally constrained composition. A failed merge SHOULD create a negative-evidence or Leftover Relay artifact. If a child is cancelled or lost, its scope MUST remain visible unless independently covered or safely pruned.

## 12. Path memory and distributed state equivalence

SPEC-003 path-local Accepted/Excluded decisions MUST be transmitted with tasks whenever they affect admissible continuations. A global state cache MUST NOT replace path context merely because two workers report the same payload hash. Cross-worker deduplication MAY use a declared future-equivalence key, but the key MUST include all future-relevant constraints, resources, objective semantics, and path restrictions. State revisitation and Loop Back are controlled by the chosen path policy; deduplication alone does not prove termination. If a worker's context omits required path information, its result MUST be rejected or revalidated.

## 13. Worker execution report

```yaml
WorkerResult:
  result_id: string
  task_id: string
  task_version: string
  attempt_id: string
  lease_epoch: integer
  worker_id: string
  status: complete | partial | failed | cancelled
  source_context_digest: string
  candidate_state_refs: [string]
  committed_transition_refs: [string]
  feasibility_evidence_refs: [string]
  objective_evaluation_refs: [string]
  bound_certificate_refs: [string]
  coverage_certificate_refs: [string]
  method_artifact_refs: [string]
  negative_evidence_refs: [string]
  leftover_scope_refs: [string]
  resource_usage: typed
  execution_trace_ref: string
  result_digest: string
```

A worker MUST distinguish observed outcomes from modeled possibilities and committed transitions, per SPEC-002. The report MUST not claim verified global feasibility merely because the worker passed local checks. Missing required evidence MUST produce a typed incomplete or rejected status.

## 14. Verification and acceptance gates

The receiving authority MUST check: (1) schema/type integrity, (2) task and attempt identity, (3) lease epoch and authority, (4) immutable context and version compatibility, (5) hard-constraint feasibility under SPEC-001, (6) objective and bound semantics under SPEC-004, (7) partition coverage claims if used, and (8) globally coupled constraints before combined acceptance. Verification outcomes are `PASS`, `FAIL`, `UNKNOWN`, or `ERROR`; unknown/error MUST NOT be treated as pass. `VERIFIED_LOCAL` and `ACCEPTED_GLOBAL` SHOULD be separate statuses. Evidence MAY be reused only under a documented dependency-validity contract.

## 15. Global coupling and reconciliation

Local feasibility is not closed under arbitrary composition. Two school-bus subproblems may assign the same vehicle or exceed a district-wide fleet cap; two agents may reserve the same resource; two partial schedules may create timing conflicts. A **Global Reconciliation Contract** MUST define shared resources, compatibility predicates, conflict resolution, and authoritative commit. If joint feasibility cannot be established, the combined result MUST NOT be published as globally feasible. The coordinator MAY accept a single verified child result without accepting an incompatible aggregate. Conflict evidence SHOULD be relayed to the affected workers.

## 16. Shared incumbent contract

```yaml
IncumbentUpdate:
  incumbent_id: string
  candidate_state_ref: string
  objective_value: typed
  objective_sense: min | max
  metric_version: string
  constraint_bundle_ref: string
  verification_report_ref: string
  global_reconciliation_ref: optional
  source_task_id: string
  source_result_id: string
  registry_revision: string
```

An incumbent MUST be a verified feasible solution under the registry's declared objective/constraint context. A lower numeric value is not universally better; objective sense and lexicographic/Pareto semantics MUST be explicit. Concurrent updates MUST be serialized or resolved with a correct compare-and-swap/merge rule. A worker's claimed improvement MUST NOT replace the incumbent until validation succeeds. Incompatible metric versions require re-evaluation or separate registries.

## 17. Bounds and pruning across workers

A bound report MUST declare objective sense, valid search scope, constraint and metric versions, bound direction, proof assumptions, and verification procedure. For minimization, a valid lower bound on a remaining region can support pruning when it is no better than a verified incumbent under compatible conditions; maximization reverses the ordering. A heuristic estimate MUST NOT be treated as an admissible bound. Bound sharing MUST include the source region and scope: a bound for one partition is not automatically global. Invalidated bounds MUST be withdrawn or marked stale, not silently retained as current proof.

## 18. Completion and termination detection

A worker reporting `COMPLETE` proves only that its assigned work ended according to its contract. Global completion requires: all required scopes accounted for; no unprocessed in-flight tasks or messages under the chosen protocol; all deferred/Leftover regions either resolved or explicitly excluded by sound rules; and any optimality/completeness certificate verified under SPEC-004. A coordinator MUST NOT infer global termination merely because its queue is temporarily empty. For asynchronous systems, termination detection requires a documented protocol and failure model. A timeout MAY end a budgeted run but MUST be labeled `BUDGET_TERMINATED`, not exhaustive completion.

## 19. Checkpoint, restart, and crash recovery

```yaml
CheckpointRecord:
  checkpoint_id: string
  task_id: string
  attempt_id: string
  source_context_digest: string
  solver_state_ref: string
  path_memory_ref: optional
  pending_scope_refs: [string]
  completed_scope_refs: [string]
  incumbent_snapshot_ref: optional
  consumed_budget: typed
  software_version_ref: string
  created_at: timestamp
  checkpoint_digest: string
```

A restored task MUST revalidate version compatibility, lease authority, and any cached proof dependencies. Checkpoint restoration MUST NOT silently reset spent budget or erase completed/remaining scope accounting. Stochastic solver replay MUST distinguish reproducible simulation from preservation of an actually observed outcome. Incomplete checkpoints MAY be used as evidence but cannot imply coverage.

## 20. Failure taxonomy and recovery

Failures include worker crash, coordinator crash, network partition, lost message, duplicate result, corrupt artifact, stale lease, stale constraint/metric version, malicious report, invalid bound, partial checkpoint, inconsistent merge, and resource exhaustion. Every failure MUST produce a typed disposition and durable recovery decision. Retries SHOULD use exponential backoff or bounded policies appropriate to the environment, but no particular backoff is normative. The system MUST distinguish *retryable transport failure* from *infeasible task*, and *unverified evidence* from *false evidence*. A recovered task MUST preserve provenance and leftover scope.

## 21. Structural Relay taxonomy

CDE defines seven baseline Relay artifact types: **Result** (candidate/solution), **Evidence** (support, counterexample, trace), **Constraint** (constraint insight or proposed revision), **Method** (solver/operator/procedure), **Policy** (resource/selection strategy), **Metric** (evaluation proposal), and **Leftover** (unfinished or deferred scope). Each type has different admissibility and promotion rules. A Relay artifact MAY carry negative evidence or a counterexample. Constraint, Metric, and Policy artifacts MUST NOT automatically modify authoritative governance merely because they were received. The registry is shared memory, not shared authority.

## 22. Canonical Relay artifact envelope

```yaml
StructuralRelayArtifact:
  relay_id: string
  relay_type: Result | Evidence | Constraint | Method | Policy | Metric | Leftover
  artifact_schema_ref: string
  artifact_version: string
  payload_ref: string
  payload_digest: string
  origin_task_id: string
  origin_worker_id: string
  source_domain_ref: string
  source_context_versions: [string]
  assumptions: [string]
  applicability_claim: typed
  evidence_refs: [string]
  validation_status: proposed | source_verified | target_verified | rejected | unknown
  access_policy_ref: string
  provenance_ref: string
  created_at: timestamp
```

The envelope MUST preserve origin and source assumptions. A relay artifact MAY be distributed as a content-addressed object. Missing assumptions or version dependencies MUST be surfaced as `UNKNOWN` applicability rather than silently inferred.

## 23. Relay lifecycle and receiving-side validation

Recommended lifecycle: `CREATED → REGISTERED → DISCOVERED → APPLICABILITY_CHECKED → TARGET_VALIDATED → ASSIMILATED → MONITORED`, with `REJECTED`, `DEFERRED`, `SUPERSEDED`, or `REVOKED` alternatives. Discovery is not validation. Source verification is not target verification. Assimilation MUST record the receiving context, validation evidence, decision authority, and resulting change or use. A target system MUST NOT treat an artifact as universally transferable merely because it has succeeded in a different domain. A rejected transfer SHOULD retain a counterexample and the failure's scope.

## 24. Applicability decision contract

```yaml
RelayApplicabilityDecision:
  relay_id: string
  target_context_ref: string
  decision: YES | NO | UNKNOWN
  assumption_match_results: [typed]
  version_compatibility: typed
  required_target_tests: [string]
  evidence_refs: [string]
  evaluator_id: string
  decision_version: string
```

`YES` means the declared prerequisites are met, not that adoption improves performance. `NO` identifies a documented incompatibility. `UNKNOWN` requires more evidence or restricted experimentation. The target MUST independently validate claims that matter to hard constraints or safety. Applicability decisions MUST be revisited when relevant assumptions change.

## 25. Result and Evidence Relay

A **Result Relay** transfers a candidate state, verified solution, partial solution, bound, or Pareto member with exact source context. It MUST label its verification level and must not disguise a local result as global. An **Evidence Relay** transfers observations, counterexamples, traces, certificates, and negative findings. Evidence strength, source, sampling process, and uncertainty SHOULD be documented. Repeated observations MAY strengthen a hypothesis but are not automatically independent; duplicated copies of one observation MUST NOT be counted as multiple independent confirmations.

## 26. Method Relay and transfer contracts

A **Method Relay** may contain an operator, solver configuration, search heuristic, proof technique, or executable component. It MUST specify required input schema, assumptions, capabilities, expected effects, external dependencies, and verification tests. Executable methods MUST be sandboxed or otherwise controlled under the deployment trust model. Receiving workers SHOULD run a shadow evaluation or controlled pilot before promotion. Method reuse is a hypothesis about transfer; performance improvement must be measured on independent tasks, not inferred from the method's origin.

## 27. Constraint, Metric, and Policy Relay

A **Constraint Relay** may report a discovered invariant or conflict, but a hard-constraint bundle changes only through authorized migration under SPEC-001. A **Metric Relay** may propose a new evaluation dimension or calibration, but approval requires independent validity tests and SPEC-006 governance. A **Policy Relay** may propose allocation, splitting, stopping, or solver-portfolio rules; it MUST NOT silently change the authority boundary. These artifact types SHOULD include rollback plans, impacted dependencies, and invalidation requirements for cached scores, bounds, and coverage certificates.

## 28. Explicit Leftover Relay

A Leftover artifact MUST distinguish unresolved scope from infeasible or certified-pruned scope. It SHOULD include: source task, remaining Delta/state region, path context, constraint/metric versions, reason for suspension, partial evidence, cost already spent, estimated remaining work, dependencies, and suggested continuation strategies. Leftovers MUST survive task cancellation, budget exhaustion, crash recovery, and unsuccessful global merges unless independently resolved. A scheduler MAY prioritize leftovers by expected value, urgency, or coverage obligations. It MUST NOT erase them merely because the parent worker reported partial completion.

## 29. Repeated execution as temporal Relay

Repeating a task can generate evidence over time: stability estimates, changed-environment observations, reproducibility tests, or independent solver confirmations. This is a **temporal Relay** when the execution creates identifiable new evidence and the lineage is preserved. The system SHOULD distinguish `RETRY_FOR_RECOVERY`, `INDEPENDENT_REPLICATION`, `TEMPORAL_RESAMPLING`, and `EXPLORATORY_REDUNDANCY`. Identical retries with the same random seed and dependencies may add little independent evidence. Repeated execution MUST NOT create repeated authoritative side effects. Data accumulation is not automatically learning; a receiving policy must use validated information and demonstrate an outcome change.

## 30. Collective Learning measurement

**Shared artifacts ≠ validated transfer ≠ collective learning.** A learning claim SHOULD specify a baseline, target task distribution, independent evaluation, metric version, uncertainty, resource cost, and adverse effects. Useful measures include time-to-first-feasible, best verified objective, certified gap, coverage, failed-transfer rate, negative-knowledge reuse, method portability, total compute, coordination overhead, and fairness across task families. An apparent gain caused by benchmark leakage, easier task selection, or metric drift MUST NOT be reported as robust collective improvement. Claims about generalization require held-out or shifted domains.

## 31. Governance, privacy, and security

The trust boundary covers task authorization, worker identity, result integrity, artifact signing/digests, code execution, privacy, and malicious or accidental poisoning. Implementations MUST enforce capability-scoped access to sensitive states and relay payloads. Untrusted Relay artifacts MUST be treated as data until validated. A shared IP address, machine signature, or public key may be relevant evidence in some domains but does not alone establish a person's identity; inference systems require independent evidentiary and privacy safeguards. The system MUST record who may publish, verify, assimilate, revoke, and promote each artifact type. Privacy-preserving redaction MUST retain sufficient authorized auditability.

## 32. Budgeting, scheduling, and efficiency

Scheduling MAY use static partitioning, work stealing, adaptive budgets, solver portfolios, priority queues, and exploratory redundancy. Every task MUST have a budget contract; the coordinator SHOULD account for compute, network, storage, verification, serialization, synchronization, and failed attempts. Work stealing MUST preserve task scope, path memory, and version identity. Adaptive allocation MUST be versioned and auditable. A distributed speedup claim SHOULD compare against a strong local baseline at comparable total resources. Coordination overhead and stragglers can make distributed exploration slower, even when worker utilization is high.

## 33. Observability and audit records

An audit record SHOULD expose task DAG lineage, current and historical leases, proposal/attempt/result identities, verification reports, global reconciliation decisions, incumbent changes, bound withdrawals, relay publication and assimilation events, and outstanding Leftovers. Metrics SHOULD distinguish: tasks queued/running/partial/accepted; unique versus duplicate work; verified versus unverified results; source- versus target-validated Relay; and claimed versus proven coverage. Logs MUST NOT silently rewrite historical decisions. Sensitive payloads MAY be restricted, but authorized auditors must be able to resolve their integrity references.

## 34. Version change and invalidation matrix

| Changed dependency | Potentially invalid artifacts | Required action |
|---|---|---|
| State revision | Candidate transitions, cached feasibility | Rebase/revalidate |
| Hard constraints | Feasibility, accepted aggregates, pruning | Revalidate/migrate |
| Metric/objective | Scores, incumbents, bounds, comparisons | Re-score/reprove or segregate |
| Generator coverage | Partition completeness assumptions | Reassess coverage |
| Path policy | NoSubtree, revisit legality, cache keys | Recheck path semantics |
| Solver version | Certificates, reproducibility | Verify assumptions |
| External context snapshot | Travel times, resource availability | Revalidate dependent checks |
| Relay method version | Target applicability and benchmarks | Re-run target tests |

A version update MUST NOT retroactively relabel an old proof as valid for a new context. Old artifacts remain historical evidence.

## 35. Formal safety invariants

**DR-INV-01:** Every task has a stable identity and pinned future-relevant context. **02:** A lease does not imply global commit authority. **03:** Stale lease epochs cannot mutate authoritative state. **04:** Duplicate deliveries cannot cause duplicate committed effects within the declared idempotency scope. **05:** Every accepted result has required validation evidence. **06:** Local feasibility does not imply global composition. **07:** Every shared incumbent is verified under compatible metric/constraint versions. **08:** A pruning claim has a valid, scoped certificate. **09:** Coverage claims account for all required child scopes and Leftovers. **10:** Global completion does not ignore in-flight or unresolved work. **11:** Path context is preserved where it affects admissibility. **12:** A Relay's source validation is not target validation. **13:** Governance artifacts do not self-promote. **14:** Repeated execution preserves provenance and idempotent effects. **15:** Failed transfers remain distinguishable from globally invalid methods. **16:** Every authoritative update has a durable audit trail. These invariants are conditional on correct identity, validation, authorization, and storage mechanisms.

## 36. Conditional propositions and non-guarantees

**Feasibility preservation.** If each authoritative state transition is validated under SPEC-001, and each aggregate passes all applicable global constraints before acceptance, then accepted global states satisfy those constraints under their recorded versions. Proof: induction on accepted authoritative updates.

**Coverage preservation under splitting.** If a parent scope is covered by child scopes, every child is completely explored or safely discharged, and no required result is lost in merging, then the parent scope has been accounted for. This does not prove a goal exists or a solver is optimal.

**At-least-once delivery with idempotent effects.** Under durable deduplication and atomic authoritative updates, duplicate messages need not produce duplicate committed effects. This does not imply exactly-once computation or reliable delivery without liveness assumptions.

**No universal speedup.** Coordination cost, serialization, contention, uneven tasks, and limited parallelism can dominate. Distributed CDE does not guarantee faster completion, improved solution quality, consensus, or monotonic collective learning.

## 37. Reference algorithm: task dispatch and verification

```text
function DISPATCH_AND_ACCEPT(task):
    assert task_is_version_pinned(task)
    assert authorized_to_schedule(task)
    worker = choose_compatible_worker(task)
    lease = issue_fenced_lease(task, worker)
    append_event(task, LEASED, lease)
    report = receive_report_or_timeout(task, lease)
    if report is missing:
        mark_attempt_expired(lease)
        preserve_remaining_scope(task)
        return RETRY_OR_LEFTOVER
    append_immutable_report(report)
    if report.lease_epoch != current_fencing_epoch(task):
        return STALE_EVIDENCE_ONLY
    if not verify_task_identity_and_versions(report, task):
        return REJECTED_STALE_CONTEXT
    local = independent_verify(report, task.verification_contract)
    if local != PASS:
        preserve_negative_evidence_and_leftovers(report)
        return REJECTED_OR_DEFERRED
    if report.requests_global_composition:
        global_check = reconcile_global_constraints(report, task)
        if global_check != PASS:
            preserve_conflict_evidence(report)
            return VERIFIED_LOCAL_NOT_ACCEPTED_GLOBAL
    atomic_accept_with_idempotency(task, report)
    publish_eligible_relay_artifacts(report)
    preserve_remaining_scope(report)
    return ACCEPTED
```

A deployment MUST supply its own reliable transaction, fencing, and verifier implementations. This pseudocode is not a consensus protocol.

## 38. Reference algorithm: Relay receiving and assimilation

```text
function CONSIDER_RELAY(relay, target):
    if not integrity_and_access_check(relay, target):
        return REJECTED
    match = evaluate_applicability(relay.assumptions,
                                   relay.source_context_versions,
                                   target.context)
    record_applicability(match)
    if match == NO:
        return REJECTED_INCOMPATIBLE
    if match == UNKNOWN:
        return DEFERRED_FOR_EVIDENCE
    candidate = sandbox_or_shadow_evaluate(relay, target)
    evidence = independently_validate(candidate, target)
    if evidence != PASS:
        publish_scoped_negative_transfer_evidence(relay, target)
        return REJECTED_OR_DEFERRED
    if relay.type in [Constraint, Metric, Policy]:
        return REQUEST_SEPARATE_GOVERNANCE_PROMOTION
    if not authorized_for_assimilation(relay, target):
        return UNAUTHORIZED
    record_assimilation_and_monitoring(relay, target, evidence)
    return ASSIMILATED
```

Assimilation may be reversible; the contract SHOULD retain a rollback reference and post-adoption monitoring plan.

## 39. Worked case A — Constrained school-bus assignment

A district partitions school-bus route design by geographic cluster. Each worker proposes vehicle assignments and stop sequences subject to local capacity, maximum ride-time, service coverage, and time-window constraints. The global reconciler checks fleet count, shared buses, depot limits, overlapping drivers, and intercluster route conflicts. A worker's locally feasible assignment is not automatically globally feasible. A successful merge can update a verified incumbent for the same objective version. A worker's unsearched route combinations become Leftover artifacts. A method that reduces fleet count in one cluster may be relayed to another only after checking its travel-time, demand, and topology assumptions. Repeat runs can estimate schedule robustness, but only if changing traffic samples and shared assumptions are recorded.

## 40. Worked case B — Chess strategy laboratory

Workers explore disjoint or overlapping move subtrees from a versioned position. A task binds side to move, position history, repetition rules, evaluation version, and depth/budget. Search workers MAY return candidate moves, evaluation estimates, and bounds. The orchestrator MUST NOT treat an unverified heuristic score as a game-theoretic proof. Shared transposition entries require appropriate position and history equivalence. Overlapping subtrees can be intentional for independent verification. A principal-variation method may be relayed as a Method artifact, but adoption requires target-position evaluation and does not imply universal strategic superiority.

## 41. Worked case C — Autonomous scientific questioning

Workers generate candidate research questions, test plans, counterexamples, and evidence maps under a common domain context. Relay artifacts include promising questions, failed hypotheses, negative evidence, proposed metrics, and unfinished investigations. Independent validation checks whether a question is well specified, whether evidence supports the stated claim, and whether a proposed metric captures the intended scientific objective. A high novelty score does not validate scientific correctness. Collective learning is measured through downstream verified discoveries, question quality, transfer robustness, and research cost, not the volume of shared questions.

## 42. Worked case D — Distributed structural matching

Workers examine separate evidence partitions and propose candidate structural correspondences. Shared observations may be duplicated, correlated, or privacy-sensitive. The reconciler MUST distinguish matching scores from hard identity conclusions, prevent double-counting common evidence, and enforce authorization for cross-domain data transfer. A candidate association can be relayed as weak evidence with uncertainty and provenance; it is not proof of a real-world person's identity. Conflicting hypotheses SHOULD remain visible for later review instead of being silently collapsed by majority vote.

## 43. Worked case E — Negative transfer and recovery

A route-optimization method learned in a dense urban grid is sent to a rural route task. The target applicability check returns `UNKNOWN` because distance distributions and time windows differ. A shadow test shows that the method produces longer travel times. The receiver rejects assimilation, records a scoped negative-transfer artifact, and preserves the method as potentially useful in its original domain. The failed attempt consumes budget but adds reusable evidence about the method's applicability boundary. A later improved method version requires a new validation event rather than reusing the old rejection or approval.

## 44. Conformance profiles

**Core — CDE-Distributed-Relay-Core-v1 (required):** immutable task identity/context; worker manifests; durable lifecycle; attempts/leases; idempotent authoritative effects; typed results; independent verification; source-vs-global acceptance; versioned Relay artifacts; Leftover accounting; auditability.

**CDE-Distributed-Partition-v1 (optional):** coverage certificates, parent-child scope reconciliation, termination evidence.

**CDE-Distributed-Optimization-v1 (optional):** global incumbent and bound registries, verified optimization claims under SPEC-004.

**CDE-Distributed-Federated-v1 (optional):** cross-domain trust, authorization, privacy-preserving artifact transfer, federated coordination.

**CDE-Distributed-CollectiveLearning-v1 (optional):** target validation, assimilation lifecycle, controlled evaluation of learning gains.

**CDE-Distributed-Resilient-v1 (optional):** durable checkpoint/restart, epoch fencing, partition-aware recovery. Implementations MUST declare supported profiles and MUST NOT infer optional guarantees from core conformance.

## 45. Conformance test suite

The following tests are normative candidates for an executable conformance harness. Each test MUST have a fixture, expected disposition, and auditable evidence. Negative tests are essential.

## 46. Validation methodology and benchmarks

A validation program SHOULD use small exhaustive reference instances before large benchmarks. Compare single-worker and distributed executions under identical objective/constraint versions and comparable total budgets. Include synthetic graph partitioning, constrained school-bus scheduling, chess search, stochastic peak search, multi-objective optimization, and scientific-question generation. Measure correctness, missed scopes, invalid acceptance, stale-update rejection, idempotency, verified incumbent quality, certified gaps, task throughput, network/verification overhead, and failure recovery. Inject crashes, partitions, stale versions, malicious reports, and incomplete Leftovers. Independent verifiers SHOULD check proof claims. Publish both successes and counterexamples; do not generalize speedups from a single workload.

## 47. Open research questions

1. Which partition representations support useful machine-checkable coverage proofs? 2. What is the least expensive global reconciliation method for strongly coupled optimization? 3. When does adaptive work stealing outperform static partitions after verification cost? 4. How should uncertain bounds be shared without unsafe pruning? 5. Can cross-worker future-equivalence be certified for path-sensitive states? 6. What is a robust termination detector under worker churn and partial failure? 7. Which Relay artifacts transfer most reliably across domains? 8. How should correlated evidence be de-duplicated? 9. Can negative-transfer registries improve generalization without excessive conservative bias? 10. How can collective learning be distinguished from benchmark leakage and selection bias? 11. How should federated systems share evidence under privacy constraints? 12. Which governance contracts permit recursive policy evolution without weakening safety? 13. When is redundant exploration more valuable than new search coverage? 14. What performance limits follow from synchronization and verification overhead? 15. How should long-lived Leftovers be prioritized or retired without silently losing coverage?

## 48. Implementation checklist

- [ ] Task definitions pin all future-relevant versions and contexts.
- [ ] Worker capabilities and authorization are checked before dispatch.
- [ ] Leases use epochs or equivalent fencing against stale authoritative writes.
- [ ] Duplicate delivery cannot duplicate committed effects.
- [ ] Task lifecycle, attempts, and results have immutable event records.
- [ ] Child partitions have explicit coverage and overlap declarations.
- [ ] Path memory is preserved where future admissibility depends on it.
- [ ] Local feasibility is distinct from global reconciliation.
- [ ] Shared incumbents are independently verified under compatible objectives.
- [ ] Bounds have valid scope, direction, and certificates.
- [ ] Partial work and Leftovers survive cancellation and recovery.
- [ ] Global completion accounts for in-flight tasks and unresolved scopes.
- [ ] Relay artifacts preserve source assumptions, versions, and provenance.
- [ ] Applicability YES/NO/UNKNOWN is distinct from target validation.
- [ ] Constraint/Metric/Policy artifacts require separate governance approval.
- [ ] Failed transfer and negative evidence remain accessible.
- [ ] Repeated execution is labeled and does not duplicate side effects.
- [ ] Privacy, access control, and untrusted code policies are enforced.
- [ ] Failure-injection and negative conformance tests are run.
- [ ] Performance comparisons include coordination and verification costs.

## 49. Conclusion

Distributed CDE is not merely the parallel execution of Delta proposals. It is a versioned system for **delegating bounded exploration, validating returned evidence, reconciling globally coupled constraints, preserving unfinished work, and transferring knowledge without surrendering authority**. The key progression is **Task → Lease → Execute → Report → Verify → Reconcile → Accept → Relay → Target-Validate → Assimilate**. Every step has a distinct contract and failure mode. The resulting architecture can host heterogeneous solvers and controlled collective learning while remaining explicit about non-guarantees: local feasibility does not imply global feasibility, sharing does not imply learning, and more workers do not imply better outcomes. SPEC-006 will define how validated evidence may drive governed metric and recursive policy evolution.

## Appendix A — Conformance Test Matrix (60 cases)

| ID | Scenario | Expected outcome |
|---|---|---|
| DR-001 | Task schema valid | Accept |
| DR-002 | Missing task version | Reject |
| DR-003 | Mutable unversioned state reference | Reject or defer |
| DR-004 | Constraint bundle mismatch | Stale |
| DR-005 | Metric version mismatch | Segregate/revalidate |
| DR-006 | Generator version mismatch | Stale |
| DR-007 | Missing path context | Reject or revalidate |
| DR-008 | Unauthorized task dispatch | Reject |
| DR-009 | Incompatible worker capability | Do not dispatch |
| DR-010 | Unknown worker trust domain | Apply restrictive policy |
| DR-011 | Lease issued | Record epoch |
| DR-012 | Expired lease result | Evidence only until revalidated |
| DR-013 | Old epoch attempts global write | Fence |
| DR-014 | Two concurrent leases same task | Resolve per policy |
| DR-015 | Duplicate message ID | No duplicate authoritative effect |
| DR-016 | Duplicate result submission | No duplicate incumbent update |
| DR-017 | Retry after worker crash | Preserve lineage |
| DR-018 | Retry changes payload under same idempotency key | Conflict |
| DR-019 | Lost acknowledgement | Safe retry |
| DR-020 | Out-of-order lifecycle message | Reject or reconcile |
| DR-021 | Complete partition with proof | Coverage accepted |
| DR-022 | Partial partition | Unresolved scope retained |
| DR-023 | Overlap among child scopes | Report overlap |
| DR-024 | Missing child scope | Do not claim complete coverage |
| DR-025 | Cancelled child | Create Leftover |
| DR-026 | Worker reports complete without evidence | Not accepted |
| DR-027 | Worker reports local feasibility | Not global acceptance |
| DR-028 | Global resource conflict | Reject combined result |
| DR-029 | Global merge passes | Accept with reconciliation |
| DR-030 | Invalid hard-constraint report | Reject |
| DR-031 | Validation UNKNOWN | Do not accept |
| DR-032 | Validation ERROR | Do not accept |
| DR-033 | Heuristic bound claimed as proof | Reject bound |
| DR-034 | Bound with wrong objective direction | Reject |
| DR-035 | Bound from stale metric | Invalidate |
| DR-036 | Verified incumbent improves | Update atomically |
| DR-037 | Unverified incumbent improves numerically | Do not update |
| DR-038 | Concurrent incumbent writes | Serialize or compare-and-swap |
| DR-039 | Equal payload different path context | No unsafe merge |
| DR-040 | Missing future-equivalence proof | No DP coalescence claim |
| DR-041 | Queue empty but task in flight | Do not terminate |
| DR-042 | Budget timeout | Label budget termination |
| DR-043 | Checkpoint with wrong software version | Revalidate or reject |
| DR-044 | Checkpoint loses Leftovers | Conformance failure |
| DR-045 | Recovered budget resets silently | Conformance failure |
| DR-046 | Relay without provenance | Reject or quarantine |
| DR-047 | Relay without assumptions | Applicability UNKNOWN |
| DR-048 | Source-verified method target untested | Do not assimilate |
| DR-049 | Target applicability NO | Reject transfer |
| DR-050 | Target applicability UNKNOWN | Defer |
| DR-051 | Target-validated method | Assimilate if authorized |
| DR-052 | Metric Relay requests self-promotion | Governance gate |
| DR-053 | Constraint Relay requests relaxation | Governance gate |
| DR-054 | Policy Relay requests privilege escalation | Reject |
| DR-055 | Negative transfer recorded | Scoped evidence retained |
| DR-056 | Duplicate evidence counted independently | Reject independence claim |
| DR-057 | Temporal rerun with changed environment | Version observations |
| DR-058 | Repeated execution creates duplicate external effect | Conformance failure |
| DR-059 | Sensitive Relay shared without authorization | Reject |
| DR-060 | Malicious worker forged result | Quarantine/reverify |

## Appendix B — Minimal End-to-End Example

```yaml
DistributedExplorationTask:
  task_id: bus-region-west-001
  task_version: '1'
  parent_task_id: district-plan-001
  source_state_ref: {state_id: district-snapshot, revision: r12, digest: sha256:example}
  delta_scope: {region: west, operations: [route-swap, route-merge]}
  constraint_bundle_ref: district-hard-v3
  metric_ref: fleet-and-ride-time-v2
  generator_ref: route-deltas-v1
  solver_policy_ref: solver-portfolio-v4
  context_snapshot_refs: [travel-time-v7, fleet-v3]
  resource_budget: {attempts: 500, cpu_seconds: 3600}
  verification_contract_ref: route-verifier-v2
  authorization_ref: dispatch-policy-v1
  expected_output_contract_ref: worker-result-v1
  idempotency_scope: bus-region-west-001
  provenance_ref: parent-split-001
```

A worker may return a feasible western route plan and a `PARTIAL` report with unresolved route exchanges. The global reconciler must check that the western plan does not use buses already allocated to the eastern region. If it conflicts, the report remains useful local evidence but is not an accepted district solution. The unresolved exchanges become Leftover Relay artifacts for later exploration.

## Appendix C — Suggested Repository Placement

`docs/specifications/SPEC-005-Distributed-Exploration-and-Relay-Contract.md`

## Appendix D — Traceability and Research Status

| Topic | Principal source |
|---|---|
| State and constraint commits | SPEC-001 / CDE-002 |
| Delta proposals and M-to-N accounting | SPEC-002 / CDE-003 |
| Path memory and Loop Back | SPEC-003 / CDE-004 |
| Solver proofs, bounds, and incumbents | SPEC-004 / CDE-005 |
| Orchestration | CDE-008 |
| Distributed task contracts | CDE-009 |
| Structural Relay and Collective Learning | CDE-010 |
| Controlled recursive evolution | CDE-011 / forthcoming SPEC-006 |
| Guarantees and non-guarantees | CDE-012 |

This is a proposed research contract, not an adopted standard or a verified production protocol. It draws on established distributed systems, transaction processing, search, event sourcing, provenance, and knowledge-transfer practices. The contribution sought is a coherent CDE-specific interface and validation discipline, not a claim to have invented those underlying fields. Executable reference implementations, formal protocol verification, independent benchmarks, and literature references remain future work. No DOI is asserted before release and registration.

**Version:** v1.0.0 draft  
**License:** Apache-2.0
