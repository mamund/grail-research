# GRAIL bounded autonomy — evidence register

**Version 0.6 — 27 October 2026**

## Scope and provenance

This register extends v0.4 with five user-supplied world execution archives. Source, recorded observations, traces and configuration were inspected; the executions were **not rerun independently**. The regression log remains a separate user-run verification: 37 passing suites for the runtime source package declaring `0.3.0-beta.1`. World archives declare dependencies separately; do not infer that the traces were produced by an unmodified published runtime.

## World-level execution records

| ID | Archive | Goal reached | SUCCESS / FAIL | Trace events | Duration (ms) | Observations agree | Declared dependency |
|---|---|---|---:|---:|---:|---|---|
| E1 | `failure-recovery(1).zip` | true (`customerOnboarded`) | 7 / 2 | 34 | 7.54 | yes | `0.2.0` |
| E2 | `nested-customer-onboarding.zip` | true (`onboardingProcessed`) | 4 / 0 | 15 | 43.15 | yes | `file:../../grail-release` |
| E3a | `purchase-management(1).zip` | true (`purchaseRecorded`) | 5 / 0 | 19 | 7465.47 | yes | `0.2.0` |
| E3b | `purchase-management-reject.zip` | true (`purchaseRecorded`) | 5 / 0 | 19 | 5193.75 | yes | `0.2.0` |
| E3c | `purchase-management-defer.zip` | true (`purchaseRecorded`) | 5 / 0 | 19 | 3482.50 | yes | `0.2.0` |

**Archive SHA-256 (fixity):**

- E1: `926db9f119aef57de4b796cee668af15de6dccaf715d85404456740039b06dca`
- E2: `493b6d8aa44a2795d737b640757f6c9148e60e8b072dc8dbe6db6b5835312df9`
- E3a: `bf9cb75216a45c524c776b2eab4a7da4db92f8b147f51d4a33ee4f3b2515e8a2`
- E3b: `8cf39c89c8a5a1d2005e24a04eec4ddc027dffe69e22beb55b84afa92406def8`
- E3c: `56eb754be448ee44df279a6950fed533183c0ccce101454f02868b44ea1dba13`

## Experimental interpretation

**E1 — alternative affordance recovery.** The pursuit reached `customerOnboarded` despite two recorded failed invocations. The trace records selector events and subsequent selections. This is a concrete demonstration of local fallback, not compensating recovery or a quantified reliability improvement.

**E2 — recursive composition.** The parent pursuit reached `onboardingProcessed` through a capability invoking a child verification world. Domain outcomes `REJECTED` and `DENIED` coexist with successful responsibility completion. The standalone child observations may come from a different execution; the child evidence embedded in the parent output is the contemporaneous record. The parent trace does not constitute a full child decision trace.

**E3a–c — heterogeneous execution and domain outcomes.** Three runs of the same purchasing world reached `purchaseRecorded` after human decisions of approval, rejection and deferral. Node, HTTP and interactive STDIO mechanisms are used. The resulting business statuses differ while the goal and success status remain stable. Durations are single-run descriptive measurements dominated by human interaction; they are not performance estimates.

## Trace and output integrity

- **E1**: 34 trace events (affordance.selected: 9, condition.selected: 7, stack.pop: 9, stack.push: 9); standalone and embedded observations match; no selected domain-status fields in top-level observation outputs.
- **E2**: 15 trace events (affordance.selected: 4, condition.selected: 3, stack.pop: 4, stack.push: 4); standalone and embedded observations match; verificationStatus=REJECTED, activationStatus=DENIED.
- **E3a**: 19 trace events (affordance.selected: 5, condition.selected: 4, stack.pop: 5, stack.push: 5); standalone and embedded observations match; reviewStatus=approved, processingStatus=purchase-order-created.
- **E3b**: 19 trace events (affordance.selected: 5, condition.selected: 4, stack.pop: 5, stack.push: 5); standalone and embedded observations match; reviewStatus=rejected, processingStatus=request-closed-rejected.
- **E3c**: 19 trace events (affordance.selected: 5, condition.selected: 4, stack.pop: 5, stack.push: 5); standalone and embedded observations match; reviewStatus=deferred, processingStatus=follow-up-pending.

## Claim disposition (C1–C10)

| Claim | v0.5 disposition | Bound on interpretation |
|---|---|---|
| C1 Goal pursuit within bounded environment | Runtime source, regression, E1–E3 | Demonstrations only; not guaranteed attainment |
| C2 LLM-independent pursuit | Runtime source and regression; selected worlds use deterministic capabilities | Does not exclude LLM-bearing capabilities |
| C3 Separate need/means selectors | Runtime source, regression, selection traces | Does not demonstrate improved policy |
| C4 Alternative affordance fallback | Runtime source, regression, E1 | Not equivalent to compensating side-effect recovery |
| C5 Recursive composition | Runtime source, regression, E2 | One parent–child nesting pattern, not arbitrary depth |
| C6 Domain outcomes distinct from completion | E2 and E3a–c | Runtime trusts capability-reported completion and effects |
| C7 Heterogeneous bindings | Runtime source, regression, E3a–c | Not evidence of schema-level interchangeability |
| C8 Adaptive selector benefit | **Untested hypothesis** | No controlled selector comparison |
| C9 Faithful replay | **Not established**; traces recorded | Capture does not reproduce nondeterministic effects |
| C10 Explicit I/O contracts | **Untested hypothesis** | Contracts not implemented |

## Reproducibility caveats and follow-up

1. World `package.json` files declare `@mamund/grail` `0.2.0`, whereas the separately supplied runtime source and regression log declare `0.3.0-beta.1`. Determine exact executing binary/commit for each world trace before asserting version equivalence.
2. Record execution command, Node version, host OS, environment variables and timestamps in a manifest for each archive.
3. E1 includes `node_modules`; retain the original evidence archive, but distribute a cleaned source-only derivative with lockfile for reproducibility.
4. E2 contains editor backup files and an older standalone child observation file; label or exclude these from a cleaned derivative.
5. For purchase management, preserve isolated store snapshots for each decision and record reset procedure.
6. These five records are selected demonstrations, not a randomized, repeated-measures study.

## Next paper deliverables

- A small-step operational model with an explicit assumption that reported effects may diverge from external reality.
- A manuscript presenting verified observations separately from proposed semantics and future hypotheses.

## Boundary experiments E4–E6 (added in v0.6)

These experiments test limitations and selection semantics rather than adding runtime functionality. They are controlled, single-execution demonstrations, not statistical estimates or formal proofs.

| Experiment | Scenario | Goal reached | SUCCESS / FAIL | Trace events | Harness result | Principal finding |
|---|---|---|---:|---:|---|---|
| E4 | Cycle control | yes | 1 / 0 | 3 | completed, 227 ms | Satisfied prerequisite permits progress |
| E4 | Unsatisfied cycle | not established | no invocations observed | no completed trace | timed out at 3,077 ms | No termination within observation window |
| E5 | Truthful success | yes | 1 / 0 | 3 | completed | Internal and external states agree |
| E5 | False success | yes | 1 / 0 | 3 | completed | Internal goal reached despite absent external record |
| E6 | Shared producer control | yes | 2 / 0 | 7 | completed, 237 ms | Multi-effect producer satisfies both conditions |
| E6 | Shared producer failure | yes | 3 / 1 | 15 | completed, 252 ms | Failed affordance excluded for second effect |

**E6 details:** Both recorded executions have goal `workCompleted`. Control pursuit duration is 7.054 ms, failure pursuit duration 12.290 ms. Failure observation sequence: `sharedProducer` FAIL → `alternativeA` SUCCESS → `alternativeB` SUCCESS → `completeWork` SUCCESS. The trace excludes `sharedProducer` from the later candidate list for `conditionB`; its invocation count remains one. Standalone and embedded observations agree for both runs.

**E6 archive SHA-256:** `a6ccb09710942a87f3f21be00bc3a5277a8a95860bfdc0a44f81cd2f8c0555bd` (`failure-memory-e6(1).zip`).

**Claim impact:** E4 qualifies P4 (termination); E5 directly supports P5 (external soundness limitation); E6 supports P3 (action-scoped failure exclusion within the tested pursuit). Neither E4 nor E5 should be framed as a universally quantified theorem. E6 does not establish cross-pursuit memory lifetime.

**Provenance note:** E4 and E5 results were reviewed from their separately supplied execution archives in the prior audit; E6 was checked directly against the newly uploaded archive. Runtime version provenance should continue to be reported per archive and must not be inferred from the version of an unrelated regression suite.
