# GRAIL Property Audit — v0.3

27 October 2026 · Source-level examination and E4–E5 execution evidence

## Scope and method

This audit compares the six proposed properties in `grail-operational-semantics-v0.2.md` with the supplied runtime source (`server.js`, `client.js`, `grail.js`, `worldState.js`, `observationStore.js`). The initial property assessments derive from **static code inspection**; this revision additionally evaluates the user-supplied E4 and E5 execution archives. Neither method constitutes model checking or a universal proof. The previously supplied regression log reports 37 passing suites, but those passes do not establish universal correctness.

## Concrete control-flow sketch

1. `Grail.pursue()` resets runtime state, allocates a pursuit ID, and calls `Client.pursue(goal)`.
2. With an empty stack, `Client` asks `Server.resolveGoal(goal)` to check the goal or choose an affordance whose declared effects include the goal.
3. With a nonempty stack, `Server.attempt(top, inputs)` checks all declared prerequisites. If any are unmet, it selects **one** unmet prerequisite, chooses a producer affordance, and returns `BLOCKED` with that producer. The client pushes the producer.
4. Once prerequisites hold, `attempt()` resolves inputs. An unresolved reference returns `BLOCKED` without an offered producer; the client stops.
5. If a binding is enabled, the server invokes it and records `SUCCESS` or `FAIL`. A failed invocation returns `FAIL` without applying effects; the client pops it. A successful invocation applies all declared effects; the client pops it.
6. After a pop, the loop reevaluates the remaining stack or the goal. A previously failed affordance is filtered by `selectAffordance()` using `observationStore.getByAffordance(action)`.

The **need selector** (`selectUnmetCondition`) and **means selector** (`selectAffordance`) are separate methods. Both use `Math.random()` among candidates. There is no evidence here of learned or optimized selector policies.

## Property assessment

| Property | Assessment | Source-grounded reasoning | What remains |
|---|---|---|---|
| **P1 Internal goal soundness** | **Strong implementation support, conditional on worldstate API** | `resolveGoal()` returns `SUCCESS` only when `isPreconditionMet(goal)`; `Client.pursue()` emits `pursuit.completed` on that path; `Grail.pursue()` computes `reached` from the same worldstate predicate. | Formalize what `isPreconditionMet()` means for missing or malformed goal keys; add explicit assertions. |
| **P2 Declared prerequisite discipline** | **Strong implementation support for binding invocation** | `Server.attempt()` checks every `affordance.preconditions` entry and returns `BLOCKED` before input resolution or `executeBinding()` when any are unmet. | Scope claim to declared prerequisites and binding invocations; it cannot ensure the prerequisites correspond to actual external facts. Validate malformed affordances separately. |
| **P3 Local failure exclusion** | **Supported, with important scope qualification** | `selectAffordance()` filters candidates if `getByAffordance(action)` contains any observation with `result === 'FAIL'`. This prevents reselection of that **action name** within the store's effective observation history. | This is broader than “same producer for same condition”: exclusion is by affordance action, not condition. Determine how persisted observations and per-pursuit resets affect scope; test identical action names and multiple goals. |
| **P4 Conditional termination** | **E4 counterexample to a general termination guarantee; bounded termination not observed for this cycle** | `Client.pursue()` has an unbounded `while (true)` loop without an explicit visited-configuration guard or general step budget. In E4, the cyclic scenario exceeded the external 3,000 ms timeout (3,077 ms elapsed) and was killed; the positive control completed in 227 ms and reached its goal. | Define sufficient termination conditions (e.g., acyclic dependencies), and consider future cycle detection or step budgets. The timeout demonstrates no termination **within the observation window**, not an infinite-run proof. |
| **P5 External soundness does not follow** | **Experimentally demonstrated in E5 (controlled false-success scenario)** | `attempt()` applies declared effects after `interaction.ok`; there is no independent inspection of external domain state. A binding that reports success without performing the intended side effect would still advance internal worldstate. | E5 supplies the deliberately false-success counterexample; this does not establish that real-world capabilities routinely misreport success. |
| **P6 Composition boundary** | **Demonstrated in one world, not proven in general** | E2 records a parent capability calling a child pursuit, with distinct pursuit identifiers and child results consumed by the parent. The runtime itself invokes bindings without interpreting domain-specific result values. | Verify behavior under child `BLOCKED`, exception, and partial side effects; establish explicit parent/child outcome mapping assumptions. |

## E4 — Cyclic dependencies: execution evidence (new in v0.2)

**Artifact:** `cyclic-dependencies.zip`, user-supplied execution collection (27 October 2026). The experiment compares the same goal (`conditionA`) and affordances under two initial worldstates:

- `establishA`: requires `conditionB`, declares effect `conditionA`.
- `establishB`: requires `conditionA`, declares effect `conditionB`.
- **Cycle scenario:** both conditions initially false.
- **Positive control:** `conditionB` initially true.

| Measurement | Control | Cycle |
|---|---:|---:|
| Harness completion | `true` | `false` |
| Harness elapsed | 227 ms | 3,077 ms |
| External timeout | 3,000 ms | 3,000 ms |
| Timed out | `false` | `true` |
| Process exit | 0 | SIGKILL (`ETIMEDOUT`) |
| Goal reached | `true` | Not reported (worker terminated) |
| Capability observations | 1 SUCCESS | No completed output/observation artifact |
| Pursuit duration (reported by runtime) | 6.186 ms | Not available |

The control trace records one affordance selection, one stack push, and one stack pop. The cycle worker's captured stdout contains repeated dependency-resolution attempts, with no completed result artifact. This supports the interpretation that the resolver revisited the dependency cycle without making observable progress. Because the harness killed the worker, the cycle's eventual behavior **without** a timeout remains unobserved.

**Interpretation:** E4 contradicts an unqualified assertion that the current GRAIL runtime terminates on every finite declared environment. More narrowly, it demonstrates a specific dependency cycle for which GRAIL did not terminate within a three-second observation window. It does **not** establish nontermination mathematically, characterize all cycles, or show that an acyclic environment always terminates.

**Research disposition:** Preserve E4 as a negative boundary result. Do not modify the runtime as part of this audit. Cite E4 in the manuscript's limitations and revise any P4 statement to conditional language. The next planned boundary test is E5 (false-success trust boundary).

## E5 — False success: execution evidence (new in v0.3)

**Artifact:** `false-success-e5(1).zip`, user-supplied execution collection (27 October 2026). Both scenarios use goal `recordCreated`, initially false, and invoke the same `recordCreation` affordance. The capability returns `SUCCESS` in both cases; its mode controls whether it writes the external `domain-record.json` file.

| Measurement | Truthful-success control | False-success scenario |
|---|---:|---:|
| Harness completed / timed out | true / false | true / false |
| Harness elapsed | 238 ms | 224 ms |
| Pursuit duration | 6.691 ms | 6.317 ms |
| Reported goal reached | true | true |
| Final internal `recordCreated` | true | true |
| Capability observations | 1 SUCCESS | 1 SUCCESS |
| Trace events | 3 | 3 |
| External `domain-record.json` exists | yes | **no** |
| Harness assessment | `controlPassed: true` | `demonstratesFalseSuccess: true` |

The standalone assessment reports agree with the pursuit output and file presence. The truthful control contains a domain record with `recordId: E5-001` and `created: true`; the false-success results directory contains no such record. The false-success observation nevertheless reports `result: SUCCESS` and `reportedStatus: CREATED`. Both pursuits finish with `reached: true` and `worldstate.recordCreated: true`.

**Interpretation:** E5 establishes a controlled counterexample to the implication **internal goal reached ⇒ external effect occurred**. The runtime advances its declared Boolean worldstate based on the successful capability report, without independently verifying the file-system side effect. This demonstrates a *deliberate semantic trust boundary*, not a defect proven to occur in production, and does not imply that every externally observable effect must be checked by the GRAIL runtime.

**Research disposition:** Accept E5 as completed evidence for P5. Preserve the scenario-specific output, observations, trace, assessment, harness status, and external file presence/absence as the reproducibility record. No runtime changes are proposed during this research phase. Next boundary experiment: E6, action-scoped failure memory with multi-effect affordances.


## E6 — Failure-memory scope: execution evidence (new in v0.4)

**Artifact:** `failure-memory-e6(1).zip`, user-supplied execution collection (27 October 2026). The two scenarios use the same goal `workCompleted`; a shared producer declares effects on `conditionA` and `conditionB`. The controlled selection sequence exercises both a successful shared producer and a failed shared producer with alternatives.

| Measurement | Control | Failure scenario |
|---|---:|---:|
| Harness completed / timed out | true / false | true / false |
| Harness elapsed (ms) | 237 | 252 |
| Runtime pursuit duration (ms) | 7.054 | 12.290 |
| Goal reached | true | true |
| SUCCESS / FAIL observations | 2 / 0 | 3 / 1 |
| Trace events | 7 | 15 |
| Shared producer invocations | 1 | 1 |
| Harness assessment passed | true | true |

The failure scenario observation sequence is `sharedProducer` (FAIL), `alternativeA` (SUCCESS), `alternativeB` (SUCCESS), `completeWork` (SUCCESS). After the failed invocation while satisfying `conditionA`, the selection event for `conditionB` lists only `alternativeB`, not `sharedProducer`. The shared producer is therefore excluded across its declared effects **in the observed pursuit**, not merely for the condition at which failure occurred. The control invokes `sharedProducer` (SUCCESS) and `completeWork` (SUCCESS), satisfying the goal in two invocations. In both scenarios the standalone observations match those embedded in `output.json`; trace sequence lengths and harness assessments are consistent.

**Interpretation:** E6 supplies controlled execution evidence for **P3 (failure exclusion)** at affordance-action scope, including a multi-effect affordance. This is a conservative exclusion strategy: a failure while fulfilling one condition removes that action as a candidate for another condition. The experiment does not establish the lifetime of failure memory across separate pursuits, its behavior under concurrent pursuits, or a general reliability improvement.

**Research disposition:** Accept E6 as completed. All three boundary experiments (E4–E6) are now recorded. Retain the runtime unchanged during this characterization phase; incorporate this qualified result in the operational semantics and manuscript.

## Important refinements to the formal model

**R1 — Distinguish three different failure situations.** Binding `FAIL` is recorded in observations and can trigger affordance exclusion and fallback. Unmet prerequisites produce `BLOCKED` and cause a dependency to be pushed if a producer exists. Missing required inputs produce `BLOCKED` without an offered producer and stop this pursuit. These must not be collapsed into a single “failure” transition.

**R2 — Make failure memory action-scoped.** The concrete selector checks past `FAIL` observations by `affordance.action`; it does **not** index exclusions by condition or invocation. Any formal `F` must capture this behavior (or clearly state that it is an abstraction).

**R3 — Make candidate selection nondeterministic.** `Math.random()` means a single trace witnesses one path, not a deterministic execution policy. For reproducible property tests, inject or control randomness; do not claim faithful replay from a trace alone.

**R4 — Separate terminal result from returned `reached`.** The public API returns a Boolean `reached` derived from final worldstate; `Client` also emits distinct progress events for completed, unresolvable and stopped. A paper model should preserve these distinctions instead of inventing a single exhaustive status enum.

**R5 — Bound claims about success.** An affordance can have its binding disabled and still reach `applyEffects()` in the inspected `attempt()` path. Accordingly, P2 refers to invocation safety and P1 to **internal** state, not a guarantee that every true effect resulted from an executed capability.

**R6 — Specify observation lifetime.** `Grail.pursue()` calls `resetPursuit()`, which creates a new observation store. Whether a configured observation path loads earlier records determines whether failure exclusions persist beyond the current pursuit. Avoid stating that memory is strictly per-pursuit until that code path and tests are examined.

## Targeted executable tests to add

1. **P1 / positive and negative:** reachable goal returns `reached: true`; unreachable goal returns `reached: false`, with matching terminal progress events.
2. **P2 / prerequisite gate:** instrument a capability and assert it is never invoked while any declared prerequisite is false.
3. **P3 / multi-effect failure — COMPLETED (E6):** failed shared producer was not reselected for its second effect; alternative producers completed the pursuit. Future work: observation lifetime and cross-pursuit scope.
4. **P4 / cycle — COMPLETED (E4):** Two affordances depend on each other; a 3,000 ms external timeout terminated the cyclic scenario, while a control with one prerequisite initially true reached the goal. Further work: acyclic graph variants, larger cycles, and explicit termination bounds.
5. **P5 / false success — COMPLETED (E5):** both scenarios reached the internal goal; only the truthful control created the external record.
6. **P6 / nested failure:** child returns unreached/blocked; verify the parent capability's mapping and resulting parent pursuit status, without assuming automatic propagation.

## Publication-safe conclusion

The supplied source strongly supports the **structural separation of need and means selection**, **prerequisite checks before binding invocation**, and **failure-based exclusion of affordance actions**. It also supports a limited internal goal-soundness statement. E4 provides an observed **counterexample to any unconditional termination claim** for the tested implementation under the specified execution conditions: the cyclic pursuit did not terminate within three seconds and required external process termination. E5 directly demonstrates that internal goal attainment **does not entail external effect soundness** under the deliberately false-success capability. E6 demonstrates affordance-action-wide failure exclusion for the tested multi-effect scenario. The evidence does not establish globally correct recovery or general compositional correctness. Those distinctions should appear explicitly in the manuscript.

## Sources

- `grail-release(20261027-160642).zip`: `server.js` (`attempt`, `resolveGoal`, `selectUnmetCondition`, `selectAffordance`, `findAffordancesForCondition`); `client.js` (`pursue`); `grail.js` (`resetPursuit`, `pursue`).
- `grail-operational-semantics-v0.2.md`, properties P1–P6.
- `grail-evidence-register-v0.5.md` and the E1–E3 execution archives.
- `cyclic-dependencies.zip`: `results/control/harness-status.json`, `results/control/output.json`, `results/control/trace.json`, `results/cycle/harness-status.json`, and `results/cycle/worker-stdout.log`.

- `false-success-e5(1).zip`: `results/{truthful-success,false-success}/` assessment, output, observations, trace, harness status; truthful control `domain-record.json`.

- `failure-memory-e6(1).zip`: `results/{control,failure}/` output, observations, trace, assessment, harness status. SHA-256 `a6ccb09710942a87f3f21be00bc3a5277a8a95860bfdc0a44f81cd2f8c0555bd`.
