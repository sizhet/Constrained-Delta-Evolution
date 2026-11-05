# CDE-005 — Search, Dynamic Programming, and Branch-and-Bound Integration

**Series:** Constrained Delta Evolution (CDE) — Foundations  
**Status:** Foundational research proposal, v1.0.0 draft  
**Date:** 2026-11-04  
**Authors:** Sizhe Tan and GPT-Obot  
**License:** Apache-2.0  
**Companions:** [CDE-001 — From BTP and D-BTP to Constrained Delta Evolution](CDE-001-From-BTP-and-D-BTP-to-Constrained-Delta-Evolution.md); [CDE-002 — State Containers and Constraint-Governed Transitions](CDE-002-State-Containers-and-Constraint-Governed-Transitions.md); [CDE-003 — Delta Semantics and Adaptive M-to-N Branching](CDE-003-Delta-Semantics-and-Adaptive-M-to-N-Branching.md); [CDE-004 — Path Memory, No-Subtree Semantics, and Loop-Back Control](CDE-004-Path-Memory-No-Subtree-Semantics-and-Loop-Back-Control.md)  
**Related specification:** `../specifications/SPEC-004-Solver-and-Optimization-Contract.md` (planned)

## Abstract

Constrained Delta Evolution (CDE) separates a general model of constraint-governed state transformations from the algorithms that traverse, reuse, evaluate, or prune the resulting exploration space. This separation permits depth-first and best-first search, dynamic programming (DP), branch-and-bound (B&B), constraint propagation, and heuristic optimization to participate in a common exploration architecture without pretending that they are interchangeable or that their guarantees transfer automatically. This paper defines a solver-neutral transition contract, a future-equivalence condition for DP state merging, admissible-bound conditions for B&B pruning, a disciplined distinction between sound constraint propagation and heuristic filtering, and a provenance-preserving interface for heterogeneous solver workers. It addresses adaptive M-to-N branching, NoSubtree decision history, loop-back states, changing metrics, distributed incumbents, and the role of CDE in exploratory localization within the Structural Intelligence Framework (SIF). Formal propositions and counterexamples identify when termination, completeness, and optimality can be preserved. A benchmark and conformance plan is provided for testing whether the CDE integration layer adds measurable value beyond existing standalone solvers.

**Keywords:** Constrained Delta Evolution; CDE; state-space search; dynamic programming; memoization; branch-and-bound; admissible bounds; constraint propagation; solver interoperability; adaptive branching; structural relay; exploratory localization

## 1. Motivation: A Framework Is Not a Solver

The transition from Bucket Tree of Permutation (BTP) to Dynamic BTP (D-BTP) and then CDE enlarges the family of possible states, Deltas, and exploration topologies. A constructive assignment may naturally produce a binary Yes/No tree. A dynamic routing problem may use multiple reassignment operations and revisit earlier states. A metric-discovery problem may evolve both a solution and the metric used to assess it. None of these facts determines *how* the search should be conducted.

Three established families provide complementary mechanisms:

1. **Search** specifies how candidate states or paths are explored.
2. **Dynamic programming** exploits reusable subproblem structure, provided that the states being merged have equivalent future consequences.
3. **Branch-and-bound** maintains feasible incumbents and uses mathematically valid bounds to eliminate regions that cannot improve them.

CDE does not replace these methods. Its proposed contribution is a shared **State–Delta–Constraint–Evaluation** contract that allows them to operate over compatible problem representations, exchange validated results, and adapt exploration policies without erasing the assumptions required by each method.

**Central thesis:** CDE is a solver-neutral exploration and coordination abstraction; algorithmic guarantees belong to particular solver configurations under explicit conditions.

## 2. The Common CDE Problem Model

Let a problem instance be represented as

\[
\mathcal{P}=(Q,\Delta,G,T,C,F,J,\mathcal{O}),
\]

where:

- \(Q\) is the set of **exploration configurations**, not merely domain states;
- \(\Delta\) is a typed universe of candidate transformations;
- \(G(q)\subseteq\Delta\) generates candidates applicable to configuration \(q\);
- \(T(q,\delta,o)\) yields successor configurations for a Delta and outcome \(o\);
- \(C\) specifies feasibility, transition, path, and global constraints;
- \(F(q)\) is a terminal/goal predicate or a terminal-outcome relation;
- \(J\) evaluates complete solutions or paths under a declared metric version;
- \(\mathcal{O}\) is an observation/outcome model, including nondeterministic outcomes when needed.

A configuration may be written

\[
q=(s,c,e,h,m,g,b),
\]

with domain state \(s\), constraint context \(c\), evidence \(e\), relevant path history \(h\), metric identity \(m\), generator/policy context \(g\), and remaining resources \(b\). Not every application needs every field. A field may be omitted from an equivalence key **only if it is provably irrelevant to future feasibility, reachable outcomes, and objective values**.

### 2.1 Edge and decision records

A realized transition edge is

\[
e=(q_i,\delta,a,o,q_j,\pi),
\]

where \(a\) is a disposition such as `Apply` or `Exclude`, \(o\) identifies the realized outcome, and \(\pi\) contains provenance. `Exclude` can change decision history without changing the domain state. A deferred candidate is ordinarily a scheduler record rather than a committed transition.

Let \(M(q)=|G(q)|\) be the number of generated candidate Deltas and let \(N(q)\) be the number of realized outgoing transition edges. In general,

\[
M(q)\neq N(q)\neq |\operatorname{UniqueSuccessors}(q)|.
\]

A single Delta can have several stochastic outcomes, and distinct Deltas can lead to the same successor. A solver adapter must not infer one-to-one correspondence from a branch count.

### 2.2 Optimization direction and path cost

For clarity, most propositions below use **minimization**. A complete solution \(z\) has cost \(J_m(z)\), evaluated under metric \(m\). Maximization requires reversing the appropriate inequalities. Path-dependent costs, resource limits, or histories must be retained in the search configuration unless a valid sufficient statistic exists.

## 3. Solver-Neutral Exploration Interface

A CDE-compliant solver need not use the same internal data structure as another solver. It must preserve the meaning of the problem instance and return verifiable records.

| Interface operation | Required semantics | Examples of implementations |
|---|---|---|
| `canonicalize(q)` | Stable, versioned identity for the chosen equivalence purpose | Assignment encoding, normalized schedule |
| `generate(q, policy)` | Candidate Delta stream, with provenance and budget | Exhaustive, heuristic, learned generator |
| `check(q, delta)` | Distinguish hard infeasibility from policy rejection | Constraint checker, CP propagation |
| `transition(q, delta, outcome)` | Produce typed successor and event record | Deterministic move, stochastic transition |
| `is_goal(q)` | State/path goal semantics | Complete assignment, terminal policy |
| `evaluate(solution, metric)` | Comparable value with metric identity | Route cost, prediction loss |
| `bound(region, metric)` | Certified lower/upper bound with scope | LP relaxation, combinatorial bound |
| `equivalent(q1, q2)` | Sound future-equivalence relation for a declared task | DP key, bisimulation-like quotient |
| `emit(record)` | Auditable result/evidence/method record | Worker result, counterexample, proof |

The adapter may return `UNKNOWN` rather than claiming an unsupported proof. In particular, an estimated score is not a certified bound, and a syntactic state hash is not a proof of future equivalence.

### 3.1 Separate four kinds of scores

CDE should distinguish:

- **Objective value:** evaluated cost of a feasible complete solution.
- **Certified bound:** proven optimistic limit for a defined search region.
- **Priority score:** heuristic ordering of frontier entries.
- **Metric quality score:** evidence about the usefulness of an evaluation metric itself.

These quantities may be correlated but are not interchangeable. Only a bound with the correct direction and assumptions may justify B&B pruning.

## 4. Search as a CDE Solver Family

A generic search solver maintains a frontier \(\mathcal{F}\) of configurations and repeatedly selects an entry, generates Deltas, checks constraints, and expands accepted transitions. Depth-first search (DFS), breadth-first search (BFS), uniform-cost search, and best-first search differ primarily in frontier discipline and assumptions about edge costs and heuristics.

```text
procedure CDE_SEARCH(initial, policy, budget):
    frontier <- initialize_frontier(initial)
    while frontier not empty and budget permits:
        entry <- frontier.pop(policy.priority)
        if terminal(entry.configuration):
            emit verified solution candidate
            continue or stop according to solver contract
        for delta in generate(entry.configuration, policy):
            decision <- check_constraints_and_path_policy(entry, delta)
            if decision is HARD_INFEASIBLE:
                emit optional certified exclusion evidence
                continue
            if decision is DEFER:
                queue for possible later exploration
                continue
            for outcome in applicable_outcomes(entry, delta):
                child <- transition(entry.configuration, delta, outcome)
                if valid(child):
                    frontier.push(child, provenance=(entry, delta, outcome))
    return results, frontier_status, coverage_statement
```

This is a **schema**, not an executable reference implementation. A constructive Yes/No search may create an explicit exclusion child; another solver may store exclusion in a decision record. The choice must preserve NoSubtree path semantics (CDE-004).

### 4.1 Completeness depends on the generator

If the Delta generator never proposes a necessary transition, no frontier policy can recover the missing solution. Let \(R_G(q_0)\) be the states reachable under generated and retained transitions. An optimal solution \(q^*\notin R_G(q_0)\) is inaccessible to that solver configuration.

**Proposition S1 (finite exhaustive search, conditional).** If the reachable exploration graph is finite, all relevant successors are enumerated, the solver uses a fair frontier policy, no valid path is unsoundly pruned, and termination checks are correct, exhaustive search can find every reachable goal (or certify none exists). A finite domain state space alone is insufficient when path histories or metric versions generate infinitely many distinct configurations.

### 4.2 Exploration is not always optimization

CDE also supports hypothesis discovery, evidence acquisition, and exploratory SIF Localization. In such settings the result may be a set of plausible candidates or an improved region map rather than a single global optimum. The solver contract must state whether its claim concerns feasibility, coverage, best-known quality, or proven optimality.

## 5. Dynamic Programming: Reuse Requires Future Equivalence

DP is not simply “search with a cache.” Its defining opportunity is that distinct histories can lead to the *same future subproblem*. A safe reuse key must preserve all information relevant to future feasibility and value.

For configurations \(q_1,q_2\), define a task-relative relation

\[
q_1\equiv_{\mathcal P,m}q_2
\]

when their admissible continuations and continuation costs are equivalent for the declared objective \(m\), after accounting for any accumulated cost represented outside the key. This is a semantic requirement; an implementation may use a more conservative equivalence test.

For an acyclic, additive-cost problem, a Bellman-style recurrence is

\[
V(q)=\min_{\delta\in A(q)}\left[c(q,\delta)+V(T(q,\delta))\right],
\]

with terminal boundary condition \(V(q)=J_{\mathrm{terminal}}(q)\). Here \(A(q)\) includes only legal actions, and the recurrence assumes deterministic successors. For stochastic transitions, an expectation, risk operator, or other declared objective replaces the simple successor value.

### 5.1 DP state-key obligations

A reusable key may need:

1. canonical domain state;
2. remaining resources and obligations;
3. constraint version and relevant evidence;
4. path decision exclusions affecting future candidates;
5. objective/metric version;
6. other sufficient statistics affecting the future.

A cache entry must also identify whether it stores an exact value, an admissible bound, a heuristic estimate, or an incomplete search result. A timeout does not create an exact DP value.

### 5.2 Counterexample: identical visible state, different No history

Suppose two constructive paths reach the same partial assignment \(s\). Path A has permanently rejected `Assign(Alice, Route1)`; Path B has not. If this exclusion remains binding, their future feasible continuation sets differ. Merging them under a key that contains only \(s\) may incorrectly remove a feasible solution or permit a forbidden decision.

Thus

\[
\operatorname{Hash}(s_A)=\operatorname{Hash}(s_B)
\not\Rightarrow q_A\equiv_{\mathcal P,m}q_B.
\]

The NoSubtree rule is not a universal mandate to keep full histories forever; rather, a DP compression must preserve the **future-relevant consequences** of those histories.

### 5.3 Dominance is weaker than equivalence

A state \(q_1\) may dominate \(q_2\) when every continuation feasible from \(q_2\) can be matched or improved from \(q_1\) at no greater total cost. If proven, \(q_2\) may be discarded for an optimization objective even though the states are not equivalent. Dominance certificates should be separately typed and scoped.

### 5.4 Cyclic graphs

For cyclic state spaces, naive recursive memoization may loop indefinitely. Valid approaches include value iteration under suitable convergence assumptions, graph algorithms, strongly connected component handling, fixed-point methods, or explicit cycle controls. CDE's Loop Back permission does not imply that an arbitrary Bellman recurrence converges.

## 6. Branch-and-Bound: Correctness Comes from Valid Bounds

For minimization, let \(U\) be the cost of the best verified feasible complete solution (the incumbent). For a search region \(R\), let \(L(R)\) be a **certified lower bound** satisfying

\[
L(R)\le \inf_{z\in\operatorname{Feasible}(R)}J_m(z).
\]

If \(L(R)\ge U\), the region cannot improve the incumbent and may be pruned for the purpose of finding a strictly better solution. Equality handling must be adjusted if the task requires enumerating all optima or preserving tied alternatives.

**Proposition B1 (sound incumbent pruning).** Assume a fixed objective \(m\), a feasible incumbent of value \(U\), and a valid lower bound \(L(R)\) over every feasible completion in region \(R\). Then pruning \(R\) when \(L(R)\ge U\) cannot remove a solution with objective value strictly less than \(U\).

*Proof sketch.* For every feasible \(z\in R\), \(J_m(z)\ge L(R)\ge U\). Hence no such \(z\) improves the incumbent. QED.

### 6.1 Bound certificates

A bound record should carry:

```yaml
bound_record:
  region_id: "..."
  objective_id: "route-cost-v3"
  constraint_version: "c17"
  bound_type: "certified_lower"
  value: 42.0
  assumptions: ["nonnegative-route-costs", "capacity-model-v2"]
  method_id: "relaxation-lp-v1"
  verification_status: "verified"
```

This is an illustrative contract sketch, not a finalized schema. If the objective, constraints, or admissible region change, the bound may no longer be valid.

### 6.2 Directional scoring is not a lower bound

A learned directional score \(D(q,\delta)\) can prioritize promising regions, but it cannot prune a region unless a sound mathematical relationship to the true objective has been established. An impressive historical correlation is not a proof of admissibility.

### 6.3 Constraint propagation versus heuristic exclusion

**Sound propagation** removes choices inconsistent with the declared hard constraints. **Heuristic filtering** merely chooses not to explore some candidates under current budget. The first may preserve completeness when the propagation rule is correct; the second generally sacrifices it unless deferred work remains available to a fair exhaustive process.

### 6.4 Shared incumbents in distributed CDE

A worker may receive a newer incumbent asynchronously. It may safely use a verified feasible incumbent evaluated under the same objective and constraint semantics. A claimed score from an unverified worker must not be used as a pruning threshold. An older but valid incumbent may reduce efficiency without causing unsound pruning; an invalid or mismatched incumbent can destroy correctness.

## 7. A Unifying Solver-Portfolio View

CDE may coordinate a portfolio of specialized solvers, for example:

| Worker | Primary role | Returns | Guarantee conditions |
|---|---|---|---|
| DFS/backtracking | Enumerate constructive assignments | Feasible paths, exclusions | Complete only under exhaustive/fair coverage |
| Best-first search | Prioritize promising branches | Best-known solutions, frontier | Optimality requires appropriate search assumptions |
| DP solver | Reuse equivalent subproblems | Exact/partial values, memo entries | Sound future equivalence and recurrence |
| B&B solver | Prune with certified bounds | Incumbents, bound certificates | Valid bounds and exhaustive residual coverage |
| Constraint solver | Propagate hard constraints | Infeasibility certificates, domains | Sound model and propagation rules |
| Heuristic/local search | Improve solutions quickly | Candidate solutions, empirical evidence | No global guarantee by default |
| Metric learner | Propose scoring changes | Candidate metrics, validation records | Independent validation; no self-certification |

The CDE Orchestrator can route work based on estimated utility, solver capability, budget, and evidence. It must **not** translate “solver finished” into “problem solved optimally” without a valid proof or coverage certificate.

### 7.1 Result status taxonomy

Recommended statuses:

- `FEASIBLE_VERIFIED`: a valid complete solution was checked.
- `BEST_KNOWN`: the strongest verified candidate currently available.
- `OPTIMAL_PROVEN`: a valid optimality certificate or exhaustive argument exists.
- `INFEASIBLE_PROVEN`: the specified search region has a sound infeasibility proof.
- `BOUND_CERTIFIED`: a bound is verified for a declared scope.
- `PARTIAL`: search was interrupted or budget-limited.
- `HEURISTIC`: empirical or unverified assessment only.
- `UNKNOWN`: no justified conclusion.

## 8. Adaptive M-to-N Branching Meets Solver Guarantees

CDE may generate \(M\) candidates and realize \(N\) branches based on constraint checks, deduplication, solver policy, and resources. The critical question is whether the missing candidates are:

- **proved infeasible**;
- **proved dominated**;
- **safely bounded away**;
- **deferred for later exploration**; or
- **discarded heuristically**.

Only the first three categories can justify irreversible removal under their stated assumptions. Deferred candidates require explicit persistence if later completeness is claimed. Heuristically discarded candidates create a coverage gap.

### 8.1 Budgeted branching

Let \(B_t\) be an expansion budget and \(\rho_t\) the selection policy. Then

\[
E_t=\operatorname{Select}_{\rho_t}(G(q_t),B_t,C_t)
\]

is the selected expansion set. The existence of \(E_t\) does not imply that unselected Deltas are invalid. An orchestrator should record the **reason** for nonexpansion, not merely the absence of an edge.

### 8.2 A useful dual ledger

Maintain separate ledgers for:

1. **Explored/proven regions** (with scope and evidence), and
2. **Unexplored/deferred regions** (with priority and budget).

This enables honest reporting of search coverage and later Structural Relay of unresolved work.

## 9. Search Topology: Tree, DAG, and Cyclic Graph

A tree represents unique exploration histories naturally. A DAG can reuse equivalent subproblems. A cyclic graph can model reconfiguration and revisitation. The same CDE problem may admit multiple solver-specific topologies.

| Topology | Typical strength | Main obligation |
|---|---|---|
| Tree | Transparent branch history | Prevent redundant decision reintroduction where required |
| DAG | Reuse through state merging | Prove future equivalence or dominance |
| Cyclic graph | Reconfiguration, repeated actions | Specify loop and termination policy |
| Distributed graph | Parallel coverage and relay | Preserve scope, version, and provenance |

**Important distinction:** two paths that reach the same domain state are not necessarily mergeable if they differ in remaining budget, path restrictions, accumulated cost, evidence, or metric version.

### 9.1 Revisit policies

A revisit may be:

- forbidden by a constructive path contract;
- allowed if the new path has a better cost label;
- allowed after a meaningful context/evidence change;
- allowed within a finite revisit budget;
- processed by a fixed-point algorithm.

A generic global visited set is insufficient to justify correctness across all five cases.

## 10. Objective and Metric Evolution: Preventing Invalid Reuse

CDE-007 will examine solution–metric co-evolution in depth. For solver integration, one rule is already essential: **every score, bound, DP value, and incumbent is scoped to its objective and constraint semantics**.

Suppose a routing solution was evaluated under

\[
J_{m_1}=\text{number of vehicles},
\]

and a revised objective is

\[
J_{m_2}=\alpha\,\text{vehicles}+\beta\,\text{student travel time}.
\]

A bound valid for \(m_1\) is not automatically a bound for \(m_2\). An incumbent under \(m_1\) cannot be compared numerically to an incumbent under \(m_2\) without re-evaluation or a proven transformation.

### 10.1 Metric-aware cache keys

At minimum, a reusable artifact should identify:

```text
(problem_id, domain_version, constraint_version,
 objective_or_metric_version, region_or_state_key,
 algorithm_assumptions, result_type, verification_status)
```

The objective version may be omitted only when an artifact is genuinely objective-independent, such as a valid feasibility proof under unchanged constraints.

### 10.2 Avoiding metric gaming

A metric generator that optimizes its own training score may produce an apparently improved metric without improving external task performance. CDE should maintain independent evaluation criteria, held-out evidence, or explicitly declared stakeholder objectives. “Better metric” is a claim requiring evidence, not an internal score label.

## 11. Constraint-Guided Localization within SIF

The Structural Intelligence Framework (SIF) includes Localization as a functional concern: locating relevant problems, capabilities, structural components, or actionable regions. CDE can be investigated as an **exploratory localization method** when the target is not already indexed or directly addressable.

A CDE Localization workflow can be expressed as:

\[
\text{Target description}
\rightarrow\text{candidate state region}
\rightarrow\text{constraint-guided Delta expansion}
\rightarrow\text{evidence update}
\rightarrow\text{refined localization}.
\]

This is distinct from deterministic lookup. It may combine retrieval, constraint satisfaction, directional search, DP reuse, or B&B elimination depending on the problem. A negative result should carry a coverage qualification: “not found in explored region” is not equivalent to “does not exist.”

**Research hypothesis:** CDE's common Delta and evidence contracts may allow heterogeneous Localization methods to exchange partial regions, exclusion proofs, and transferable search policies. This is not yet an empirical claim of superior localization accuracy or efficiency.

## 12. Distributed Solver Coordination and Structural Relay

A distributed CDE worker receives an exploration contract

\[
W=(q_0,\Delta_{\mathrm{scope}},C,m,P,B,V),
\]

where \(P\) specifies a solver/policy, \(B\) a resource budget, and \(V\) a validation protocol. It returns typed artifacts rather than an unqualified scalar score.

### 12.1 Relay artifact classes

- **Result Relay:** feasible candidates, incumbents, objective values.
- **Bound Relay:** certified bounds with regions and assumptions.
- **Evidence Relay:** counterexamples, observed failures, uncertainty.
- **Constraint Relay:** globally valid restrictions or locally scoped conflicts.
- **Method Relay:** reusable solver configurations, generators, propagation rules.
- **Unresolved-Work Relay:** deferred regions and search frontiers.

Method Relay is especially important for Collective Learning: a worker may contribute *how* it discovered a useful region, not only the final solution.

### 12.2 Distributed consistency hazards

A coordinator must handle:

1. duplicate work and overlapping regions;
2. stale or inconsistent constraint versions;
3. incumbent values under different metric versions;
4. unsound extrapolation from local infeasibility to global infeasibility;
5. interrupted workers and unreported frontier regions;
6. heterogeneous proof and verification capabilities.

A globally shared result should preserve local assumptions. For example, an exclusion derived from a path-specific No decision is not automatically valid for every worker.

### 12.3 Merge semantics depend on the problem

For independent best-candidate search, combining verified candidates by minimum cost is straightforward. For coupled scheduling or resource allocation, locally feasible partial solutions may conflict. CDE should therefore distinguish **candidate comparison**, **evidence aggregation**, and **joint-solution reconciliation**; only the first is generally reducible to `min` or `max`.

## 13. Worked Example A: Assignment with NoSubtree Memory

Consider two workers \(A,B\) and two jobs \(1,2\), with each job assigned exactly once and each worker used at most once. Deltas are `Assign(worker, job)`.

A constructive solver may branch on `Assign(A,1)`:

- **Yes:** commit the assignment; remove incompatible candidates.
- **No:** permanently exclude `Assign(A,1)` on this path; keep other assignments available.

A DFS solver can traverse both branches. A DP solver may merge partial states only when their remaining assignment possibilities, objective contributions, and relevant exclusions are future-equivalent. A B&B solver can prune a partial assignment if a certified minimum remaining cost plus accumulated cost cannot improve the incumbent.

**Failure mode:** if the No branch returns to the same visible assignment set but the DP key omits the exclusion, a generator may reintroduce `Assign(A,1)` and create duplicate or invalid exploration. CDE-004's decision memory is therefore a solver-integration concern, not merely a tree-implementation detail.

## 14. Worked Example B: School-Bus Routing and Multiobjective Scoring

Let \(s\) represent bus routes and assignments, with capacity, pickup windows, and student ride-time constraints. Deltas include:

- `MoveStudent(student, routeA, routeB)`;
- `SwapStops(routeA, routeB)`;
- `MergeRoutes(routeA, routeB)`;
- `SplitRoute(route, partition)`;
- `AddVehicle(route)` or `RemoveVehicle(route)`.

A local-search worker may rapidly improve feasible routes. A B&B worker may optimize a restricted subproblem with certified lower bounds. A DP worker may solve a bounded route-partition subproblem where sound subproblem equivalence exists. A metric worker may evaluate alternative trade-offs between vehicle count, mean ride time, worst-case ride time, and fairness.

**Coupling challenge:** two local worker results cannot necessarily be merged into a feasible global schedule because they may reuse a vehicle, stop, driver, or time resource. The orchestrator must validate global feasibility and may need to solve a new reconciliation problem.

**Objective challenge:** a route optimized under “fewest vehicles” cannot be declared superior under a new fairness-sensitive metric without re-evaluation.

## 15. Worked Example C: Multi-Scale Peak Search

Let \(x\) denote a location in a bounded terrain and \(f(x)\) its elevation. Candidate Deltas include short local steps, regional jumps, and long-range relocation. Independent workers explore different areas.

- A best-first worker orders candidate locations by a heuristic score.
- A local-search worker climbs from a starting point.
- A B&B worker can certify global optimality **only if** it has valid upper bounds on elevation over unexplored regions (for maximization).
- A DP cache is useful only where repeated subproblems and valid future-equivalence exist; merely memoizing sampled elevations is not automatically DP.

Taking the highest verified observed point is an easy aggregation step. Proving it is the global maximum is a different problem. This example makes the distinction between **best found** and **optimal proven** visible to non-specialists.

## 16. Correctness Claims and Proof Obligations

The following table states claims that may be made only after checking their assumptions.

| Claim | Minimum obligations | What does **not** suffice |
|---|---|---|
| Termination | Finite reachable configurations or a well-founded progress/limit policy | Finite Delta *types* alone |
| Search completeness | Exhaustive/fair coverage, valid transitions, no unsound pruning | High heuristic scores |
| DP reuse correctness | Future equivalence or proven dominance; correct value semantics | Equal visible state hashes |
| B&B pruning correctness | Valid bound, feasible incumbent, matched metric and scope | Learned directional estimate |
| Global optimality | Complete coverage of residual regions plus sound pruning/certificates | Best candidate among completed workers |
| Distributed soundness | Versioned contracts, verified results, scoped evidence | Trust in worker self-report |
| Metric improvement | Independent and appropriate validation | Increased score under the same learned metric |

### 16.1 A conditional composition proposition

**Proposition C1 (sound portfolio pruning, conditional).** Suppose a fixed finite optimization problem is partitioned into regions whose union covers all feasible solutions. Each worker may discard a region only upon producing a verified infeasibility proof, a verified dominance proof, or a valid B&B bound relative to a verified feasible incumbent under the same objective and constraints. If all remaining regions are exhaustively processed, the best verified feasible solution is globally optimal.

*Proof sketch.* Coverage ensures every feasible solution lies in some region. The allowed pruning rules remove only regions with no feasible solutions or no solution better than the incumbent. Exhaustive processing of all unpruned regions rules out an undiscovered better feasible solution. Therefore the final incumbent is optimal. QED.

**Scope note:** the proposition does not guarantee that a practical distributed CDE system will meet these conditions, terminate, or be faster than a conventional solver.

### 16.2 Counterexample: unsound heuristic pruning

Assume two candidate branches \(\delta_a,\delta_b\) have heuristic scores 0.9 and 0.1. The policy keeps only \(\delta_a\). Suppose the true optimal solution is reachable only through \(\delta_b\). Then the solver can report a feasible solution but cannot claim completeness or optimality. Increasing the number of workers exploring \(\delta_a\) does not repair the missing branch.

### 16.3 Counterexample: objective version mismatch

A worker finds a solution of cost 10 under metric \(m_1\). Another worker finds cost 8 under metric \(m_2\). Selecting 8 as “better” is invalid unless both results are re-evaluated under a common criterion or a valid cross-metric ordering is established.

## 17. Reference Integration Algorithm

The following high-level pseudocode illustrates a conservative orchestration strategy. It intentionally does not prescribe the internal algorithm of each solver.

```text
procedure CDE_SOLVER_PORTFOLIO(problem, initial, workers, budget):
    contract <- freeze_problem_constraint_and_metric_versions(problem)
    frontier <- seed_regions(initial, contract)
    incumbent <- NONE
    evidence_store <- empty_verified_store()

    while frontier has eligible work and budget remains:
        tasks <- dispatch_regions(frontier, workers, contract, budget)
        for report in receive_worker_reports(tasks):
            if not validate_scope_versions_and_provenance(report, contract):
                quarantine(report)
                continue

            for candidate in report.feasible_candidates:
                if independently_verify_feasibility(candidate, contract):
                    value <- evaluate(candidate, contract.metric)
                    incumbent <- better_verified(incumbent, candidate, value)

            for certificate in report.certificates:
                if verify_certificate(certificate, contract):
                    evidence_store.add(certificate)
                    apply_only_sound_pruning(frontier, certificate, incumbent)

            for unresolved_region in report.unresolved_regions:
                frontier.add_with_provenance(unresolved_region)

            store_nonproof_method_and_heuristic_evidence(report)
            optionally_update_future_dispatch_policy(report)

    status <- certify_coverage_or_mark_partial(frontier, evidence_store)
    return incumbent, status, evidence_store, unresolved_frontier
```

The contract is frozen for the proof-relevant run. A metric or constraint change starts a new evaluation context or invokes a separately justified incremental-update protocol. Heuristic policy changes may occur without invalidating proof claims if they alter only ordering and not sound coverage/pruning.

## 18. Conformance Test Suite Proposal

The following tests are candidates for `SPEC-004` and a future executable harness.

| ID | Test | Expected behavior |
|---|---|---|
| CDE-SOL-001 | Exhaustive finite two-job assignment | Finds all feasible outcomes or proves none |
| CDE-SOL-002 | NoSubtree exclusion omitted from naive DP key | Rejects unsound merge or restores exclusion in key |
| CDE-SOL-003 | Two paths with proven future-equivalence | Permits safe DP reuse |
| CDE-SOL-004 | Equal state hash, different remaining resources | Forbids unsound cache reuse |
| CDE-SOL-005 | Valid lower bound exceeds incumbent | Permits B&B prune with certificate |
| CDE-SOL-006 | Heuristic score masquerading as bound | Rejects proof-level pruning |
| CDE-SOL-007 | Maximization bound direction | Uses upper bound, not minimization lower bound |
| CDE-SOL-008 | Metric version changes mid-run | Invalidates/re-evaluates scoped scores and bounds |
| CDE-SOL-009 | Constraint version mismatch | Quarantines stale proof or revalidates it |
| CDE-SOL-010 | Worker timeout with unexplored frontier | Reports `PARTIAL`, not `OPTIMAL_PROVEN` |
| CDE-SOL-011 | Overlapping distributed subregions | Deduplicates safely without losing coverage |
| CDE-SOL-012 | Cyclic graph with no termination policy | Does not falsely guarantee termination |
| CDE-SOL-013 | Feasible incumbent from untrusted worker | Requires independent verification before pruning |
| CDE-SOL-014 | Multiple Delta outcomes converge to one state | Preserves edge provenance despite state merge |
| CDE-SOL-015 | Locally feasible, globally conflicting routes | Requires reconciliation or rejects combined plan |
| CDE-SOL-016 | Heuristic-filtered necessary Delta | Reports incomplete coverage and no optimality proof |

## 19. Testable Hypotheses and Benchmark Design

These are hypotheses, not established performance results.

**H1 — Interoperability:** a common CDE contract reduces the effort required to substitute or combine heterogeneous solvers across several problem domains. Measure adapter effort, semantic failures, and reuse rates against bespoke integration.

**H2 — Solver portfolio:** under matched compute budgets, a heterogeneous portfolio may improve time-to-first-feasible or best-known solution quality on some mixed problem families. Compare against the strongest individual solver and standard portfolio baselines, not only against naive DFS.

**H3 — Evidence Relay:** sharing verified bounds, infeasibility certificates, and incumbents can reduce duplicated exploration. Measure wall time, total CPU time, communication overhead, and proof verification cost.

**H4 — Adaptive M-to-N:** budget-aware branching may improve anytime performance on some landscapes but may reduce coverage or optimality guarantees when candidates are discarded heuristically. Report both quality and coverage.

**H5 — DP contract safety:** explicit future-equivalence checks reduce incorrect memoization in history-dependent problems. Measure false merges and runtime overhead.

**H6 — Metric version discipline:** versioned evaluation artifacts reduce cross-metric ranking errors in co-evolution experiments. Test deliberate metric changes and stale-cache attacks.

**H7 — Exploratory Localization:** CDE-guided search can locate relevant structures or capabilities more effectively than selected retrieval-only or search-only baselines on suitable tasks. Specify target recall, cost, and uncertainty reporting before evaluation.

### 19.1 Benchmark families

1. Small exhaustive assignment instances with known optima and controlled NoSubtree histories.
2. Routing and scheduling instances with capacity and fairness constraints.
3. Finite graph shortest-path and cyclic-reconfiguration problems.
4. Synthetic multimodal landscapes with tunable local/global structure.
5. Metric-selection tasks with held-out validation and distribution shift.
6. Distributed task sets with controlled communication latency and worker failures.

### 19.2 Required measurements

- verified objective quality and optimality gap where a valid bound exists;
- time-to-first-feasible, time-to-best-known, wall time, CPU/GPU usage;
- generated Deltas, expanded edges, unique configurations, cache hits;
- valid vs invalid pruning events and unresolved coverage;
- memory usage, communication volume, proof-checking overhead;
- sensitivity to objective/constraint changes;
- reproducibility across random seeds and worker scheduling;
- negative results and failure cases.

Use ablations: remove DP sharing, remove B&B bounds, remove directional policy, remove Relay, and remove generator adaptation individually. Otherwise portfolio gains cannot be attributed to specific mechanisms.

## 20. Relationship to the CDE Research Program

- **CDE-001** introduces the generalization from BTP/D-BTP and positions CDE as a research framework.
- **CDE-002** defines logical State Containers and constraint-governed transitions.
- **CDE-003** defines Delta semantics and adaptive M-to-N branching.
- **CDE-004** supplies path decision memory, NoSubtree, and Loop Back contracts required for safe reuse.
- **CDE-005 (this paper)** defines the interface and proof obligations for heterogeneous search/optimization solvers.
- **CDE-006** will explore directional fields and multiscale Delta generation.
- **CDE-007** will address solution-space and metric-space co-evolution.
- **CDE-008–011** will address orchestration, distributed execution, Structural Relay, and controlled recursive evolution.
- **CDE-012** will consolidate generalization boundaries, guarantees, and open research questions.

The companion `SPEC-004-Solver-and-Optimization-Contract.md` is planned to translate these conceptual obligations into machine-checkable schemas and conformance criteria.

## 21. Open Questions

1. What is the smallest solver-neutral state representation that still preserves future-equivalence obligations?
2. Can proof-carrying DP equivalence or dominance certificates be made practical across heterogeneous solvers?
3. Which bound certificates are cheaply transferable between B&B, constraint programming, and learned-guidance systems?
4. How should the orchestrator allocate budget between heuristic improvement and proof-oriented exhaustive coverage?
5. When does the cost of serialization, verification, and Relay exceed the benefit of distributed solver cooperation?
6. Can objective-independent feasibility evidence be safely reused after metric evolution?
7. What is the correct notion of fairness for adaptive generators with effectively unbounded candidate spaces?
8. How can probabilistic confidence and deterministic proof statuses coexist without being confused?
9. Under what conditions can exploratory Localization claims be certified rather than merely ranked?
10. What are the minimal interoperability tests that a third-party CDE solver plugin must pass?

## 22. Conclusion

CDE is most useful as a **shared computational language and coordination layer**, not as a renaming of search, DP, or B&B. Search supplies traversal; DP supplies principled subproblem reuse; B&B supplies proof-based pruning; constraint propagation supplies sound reductions; heuristics supply priorities and candidate discovery. CDE proposes to connect these capabilities through explicit State, Delta, Constraint, Metric, Branch, Evidence, and Result contracts.

The unifying principle is simple but demanding: **reuse only when future semantics are preserved; prune only when a valid argument supports pruning; compare only under compatible metrics; and report only the guarantees actually earned**. With these safeguards, adaptive M-to-N branching and distributed Structural Relay can be explored without silently sacrificing correctness. Whether this integration yields better engineering outcomes remains a question for systematic experiments, open benchmarks, and collective validation.

---

## References and Contextual Research Directions

The following are foundational research traditions and pointers for later bibliography expansion; this document does not claim that CDE originated their techniques.

- Richard Bellman, *Dynamic Programming* (1957): Bellman recurrences and optimal substructure.
- A. H. Land and A. G. Doig, “An Automatic Method of Solving Discrete Programming Problems” (1960): branch-and-bound foundations.
- R. E. Korf and related heuristic-search literature: state-space search, admissibility, and search complexity.
- Constraint programming literature: consistency, propagation, global constraints, and solver integration.
- Distributed optimization and algorithm-portfolio literature: parallel search, incumbent sharing, work stealing, and heterogeneous solvers.
- CDE-001–004 in this repository: proposed CDE definitions, transition semantics, branching, and memory contracts.

**Research integrity note:** CDE is presented as a proposed unifying framework and set of interoperable contracts. Its broad applicability, implementation cost, and empirical advantages are hypotheses to be evaluated, not established theorems.
