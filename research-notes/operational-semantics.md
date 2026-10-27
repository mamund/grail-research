# GRAIL: preliminary operational semantics

**Version 0.3 — 27 October 2026**  
**Status:** Implementation-informed abstract transition system, revised against Evidence Register v0.6 and Property Audit v0.4. This is **not** a verified executable specification or a formal proof of runtime correctness.

## 1. Scope and provenance

The model describes the GRAIL runtime's coordination of Boolean responsibility conditions and externally implemented capabilities. It is informed by inspection of `server.js`, `client.js`, and related modules in the supplied runtime archive, the separately supplied 37-suite regression log, and user-executed E1–E6 archives. Exact runtime revisions used for all archived world runs are not uniformly established: E1/E3 declare a 0.2.0 dependency, E2 a local `file:` dependency, and E4–E6 were packaged against local runtime sources. Recorded traces are not proofs of all possible executions.

## 2. World, scenario, and domain state

Let a finite world be **W = (C, A, pre, eff, bind)**, where C is a set of named Boolean conditions, A a set of affordance actions, pre(a) ⊆ C declared prerequisites, eff(a) ⊆ C declared effects, and bind(a) an implementation reference. A scenario is **S = (W, g, s₀, i₀)**: a goal g ∈ C, initial internal worldstate s₀: C → {false,true}, and input environment i₀. An external domain state d ∈ D is included in the *research model* but is not generally observable to GRAIL. In particular, s(c)=true does **not** imply that the corresponding proposition about d holds.

An affordance denotes a declared responsibility and its eligible means; a binding executes an operational capability. Domain outcomes (APPROVED, REJECTED, DEFERRED, etc.) remain values returned by capabilities rather than the runtime's Boolean occurrence conditions.

## 3. Pursuit state and selectors

A schematic pursuit configuration is **q = ⟨W, g, s, d, κ, I, O, T, F, m⟩**. κ is the resolution/invocation stack; I holds inputs and reported outputs; O is the observation history; T is a trace of selector/stack events; F represents failed affordance actions visible in the effective observation history; m is a control mode. The external component d is a modeling device, not a runtime field.

For condition c, **producer(c) = {a ∈ A | c ∈ eff(a)}**. GRAIL separates two choices:

- **Need selection:** σ_need(q) chooses an unmet declared prerequisite of the currently resolving action.
- **Means selection:** σ_means(c,q) chooses an eligible producer for c, excluding affordances with applicable recorded FAIL observations.

In the inspected implementation these choices are randomized over available candidates; they are not learned, globally optimizing, or assumed fair. An affordance can be *selected* before its prerequisites are satisfied. Selection does not imply immediate invocation.

**Important correction (E6):** F is indexed by **affordance action**, not by `(condition, affordance)` pair. If a multi-effect action fails while attempting one responsibility, that action can be excluded when subsequently resolving a different responsibility in the same effective observation context. Cross-pursuit persistence depends on observation-store lifetime and is not established by E6.

## 4. Abstract transition rules

The following rules describe observable control-flow obligations, not every JavaScript stack operation.

**(G) Goal:** If s(g)=true, the pursuit may terminate with reached=true. P1 concerns this internal predicate only.

**(N) Need:** If an action at the top of κ has unmet declared prerequisites, choose one unmet condition c via σ_need, select a producer via σ_means, and push the selected producer for resolution. Append the corresponding selection/stack events to T.

**(M) Means:** When resolving an unsatisfied goal or prerequisite c, choose an eligible producer a ∈ producer(c) that is not excluded by the applicable failure observations. If none exists, the implementation may report a blocked or failed path; exact status depends on the concrete call path.

**(I) Invocation discipline:** Invoke bind(a) only after all declared pre(a) evaluate true in internal worldstate and required inputs can be resolved. This is a declaration-level check, not verification of external domain facts.

**(S) Reported success:** When a binding reports SUCCESS, append an observation, incorporate reported outputs into I, and apply declared eff(a) to internal state s according to the runtime update rule. Pop the completed obligation. The capability may independently change d→d′, but no rule infers the truth of external effects from SUCCESS.

**(F) Reported failure:** On FAIL, append a failure observation; do not apply the failed action's declared effects; pop the failed obligation and permit another eligible producer. The effective exclusion set is action-scoped: a recorded FAIL for action a can prevent future selection of a for *any* effect it declares. Forward fallback is distinct from compensating recovery.

**(B) No eligible means / unresolved inputs:** If no suitable producer exists or required inputs cannot be resolved, the runtime can terminate without reaching the goal. The abstract model does not conflate BLOCKED with FAIL or claim every unresolved configuration is detected.

**(R) Optional compensation:** Separately enabled recovery may invoke capability-defined compensators for previously successful forward invocations in reverse order. Neither exact restoration of d nor guaranteed compensator success follows.

## 5. Cycles and progress

**No general termination theorem follows from finite C and A.** A selected affordance can require another affordance whose prerequisites eventually require the original condition, causing repeated stack expansion without capability invocation or state progress. E4 exhibits this behavior for `conditionA` ↔ `conditionB`: the positive control reached its goal, whereas the unsatisfied cycle was externally killed after approximately 3.077 seconds. E4 establishes failure to terminate *within the observed window*, not a proof of an infinite execution. The abstract semantics therefore allows unresolved repeating resolution configurations; it does not assume cycle detection, a visited-state guard, or a step budget.

Sufficient conditions for termination, if any, require further proof and explicit assumptions about dependency structure, selector behavior, capability termination, and input resolution.

## 6. Internal completion and external truth

Write `report(a)=SUCCESS` for a binding report, `outcome(a)` for its domain value, and `Ext(c,d)` for an external proposition that an internal condition is intended to represent. The runtime can establish **s(c)=true** following reported SUCCESS without establishing **Ext(c,d)**. E5 is a controlled witness: both truthful and false-success scenarios reached `recordCreated`, but only the truthful one produced the external record.

Likewise `report(a)=SUCCESS ∧ outcome(a)=REJECTED` is consistent. E2 and E3 demonstrate that successful responsibility completion need not imply approval, acceptance, or another favorable business outcome.

## 7. Property dispositions after E4–E6

| Property | Status | Evidence and precise scope |
|---|---|---|
| **P1 Internal goal soundness** | Implementation-supported candidate invariant | Goal reached is computed from internal goal condition; malformed goals and all runtime paths are not formally verified. |
| **P2 Prerequisite discipline** | Implementation-supported candidate safety property | Binding invocation is guarded by declared prerequisites; does not validate external truth. |
| **P3 Failure exclusion** | Source-supported and E6-demonstrated | Recorded FAIL excludes an affordance action across declared effects within the tested pursuit; no general cross-pursuit lifetime claim. |
| **P4 Conditional termination** | General termination unsupported; E4 negative boundary result | A cyclic world did not terminate within a 3-second external limit; no universal nontermination proof. |
| **P5 External soundness limitation** | E5 counterexample demonstrated | `reached` does not entail the corresponding external effect. |
| **P6 Recursive composition** | E2 demonstrated; not universally proven | Parent capability invokes child pursuit and consumes outputs; arbitrary-depth correctness unestablished. |

## 8. Research implications and open proof obligations

1. Formalize precise stack operations and the resolution path for cyclic configurations; state and prove any restricted termination theorem before asserting it.
2. Characterize failure-memory lifetime and the behavior of multi-effect affordances across distinct pursuits.
3. Test P1/P2 on malformed declarations and unresolved inputs; do not infer full safety from successful regressions.
4. Separate reporting correctness, domain correctness, and external effect verification in future contract work.
5. Model recursive composition with explicit parent/child outcome mapping and side-effect assumptions.
6. Preserve experiment provenance and exact executable revision before attempting deterministic replay claims.

**Sources:** `grail-operational-semantics-v0.2.md`; `grail-property-audit-v0.4.md`; `GRAIL-Evidence-Register-v0.6-regenerated.md`; user-supplied E1–E6 archives and runtime source. Mathematical definitions are analytical abstractions, not verbatim runtime contracts.
