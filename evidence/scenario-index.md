# Scenario evidence index — GRAIL research

**Status:** Publication-package working draft, 28 October 2026.  
**Scope:** Eleven original scenario executions across six experimental families. Paths are relative to the `grail-research/` repository root unless explicitly marked as *inside an archive*. This index describes the supplied evidence; it does not assert clean-room reproduction or an exact historical runtime commit.

## Reading the evidence

- **Original archive** means a preserved historical ZIP under `evidence/original-archives/`. Where separate original snapshots exist, use the named ZIP for that scenario rather than assuming the extracted working tree represents every execution.
- **Extracted path** identifies the source/configuration or recorded result in `experiments/`. Extracted trees omit some incidental historical files and may represent only one snapshot of a multi-run experiment.
- **Observations** record what the GRAIL runtime reported. A goal marked reached is **not** independent proof that an external side effect occurred.
- **Trace** records the available coordinator events. The E4 cycle timeout has **no completed trace**; use its harness logs and status instead.
- **Reproduction** is documented separately under `evidence/reproduction-reports/` and `docs/reproducibility.md`. Do not substitute those runs for original evidence.

## Eleven-scenario map

| ID | Scenario | Goal and observed result | Original evidence ZIP | Configuration / scenario input | Recorded observation and trace | External evidence or important qualification |
|---|---|---|---|---|---|---|
| **E1** | Failed providers with alternatives | `customerOnboarded` reached; 7 SUCCESS, 2 FAIL; 34 trace events | `failure-recovery(1).zip` | `experiments/E1-failure-recovery/config/{goal,worldstate,inputs,registry}.json` | `experiments/E1-failure-recovery/config/observations.json`; `config/failure-recovery-trace.json` and `config/failure-recovery-output.json` (same experiment root) | Demonstrates local fallback after failed invocations, **not** compensating recovery. |
| **E2** | Nested verification and onboarding | Parent `onboardingProcessed` reached; parent 4 SUCCESS, 15 events; child `customerVerified`; domain `REJECTED` / `DENIED` | `nested-customer-onboarding.zip` | `experiments/E2-nested-onboarding/config/{goal,worldstate,inputs,registry}.json`; child world at `child-world/customer-verification/config/` | Parent `config/observations.json`, `config/nested-customer-onboarding-trace.json`, `config/nested-customer-onboarding-output.json` | **Contemporaneous child evidence is embedded in parent output**; standalone child observations may come from a different execution. Historical dependency is `file:../../grail-release`; exact runtime revision unverified. |
| **E3a** | Purchase **approved** | `purchaseRecorded` reached; 5 SUCCESS, 19 events; `purchase-order-created` | `purchase-management(1).zip` | **Inside that ZIP:** `config/{goal,worldstate,inputs,registry}.json` | **Inside that ZIP:** `config/observations.json`, `config/purchase-management-trace.json`, `config/purchase-management-output.json` | **Inside that ZIP:** `data/state.json`; interactive STDIO and HTTP service required. Extracted `experiments/E3-purchase-management/` corresponds to this primary snapshot, not all three. |
| **E3b** | Purchase **rejected** | `purchaseRecorded` reached; 5 SUCCESS, 19 events; `request-closed-rejected` | `purchase-management-reject.zip` | **Inside that ZIP:** `config/{goal,worldstate,inputs,registry}.json` | **Inside that ZIP:** `config/observations.json`, `config/purchase-management-trace.json`, `config/purchase-management-output.json` | **Inside that ZIP:** `data/state.json`; preserve rejected snapshot separately. |
| **E3c** | Purchase **deferred** | `purchaseRecorded` reached; 5 SUCCESS, 19 events; `follow-up-pending` | `purchase-management-defer.zip` | **Inside that ZIP:** `config/{goal,worldstate,inputs,registry}.json` | **Inside that ZIP:** `config/observations.json`, `config/purchase-management-trace.json`, `config/purchase-management-output.json` | **Inside that ZIP:** `data/state.json`; preserve deferred snapshot separately. |
| **E4a** | Satisfied-prerequisite **control** | `conditionA` reached; 1 SUCCESS, 3 events | `cyclic-dependencies.zip` | `experiments/E4-cyclic-dependencies/config/{goal,inputs,registry}.json`; `scenarios/control/worldstate.json` | `experiments/E4-cyclic-dependencies/results/control/{observations,trace,output,harness-status}.json` | `results/control/worker-stdout.log` and `worker-stderr.log`; control completed. |
| **E4b** | Unsatisfied **cycle** | `conditionA` not established; timed out after ~3,077 ms (3,000 ms watchdog) | `cyclic-dependencies.zip` | `experiments/E4-cyclic-dependencies/config/{goal,inputs,registry}.json`; `scenarios/cycle/worldstate.json` | **No completed observations/trace**; `experiments/E4-cyclic-dependencies/results/cycle/harness-status.json` and `worker-stdout.log`, `worker-stderr.log` | 1,382 repeated attempt messages reported in original analysis; timeout establishes nontermination **within the observed window**, not an infinite-loop proof. |
| **E5a** | **Truthful** capability success | `recordCreated` reached; 1 SUCCESS, 3 events | `false-success-e5(1).zip` | `experiments/E5-false-success/config/goal.json`; `scenarios/truthful-success/inputs.json` | `experiments/E5-false-success/results/truthful-success/{observations,trace,output,assessment,harness-status}.json` | `results/truthful-success/domain-record.json` exists; external record agrees with reported success. |
| **E5b** | **False** capability success | `recordCreated` reached internally; 1 SUCCESS, 3 events | `false-success-e5(1).zip` | `experiments/E5-false-success/config/goal.json`; `scenarios/false-success/inputs.json` | `experiments/E5-false-success/results/false-success/{observations,trace,output,assessment,harness-status}.json` | **No `domain-record.json` in false-success results**; the internal claim does not establish external effect. |
| **E6a** | Shared producer **control** | `workCompleted` reached; 2 SUCCESS, 7 events | `failure-memory-e6(1).zip` | `experiments/E6-failure-memory/config/goal.json`; `scenarios/control/inputs.json` | `experiments/E6-failure-memory/results/control/{observations,trace,output,assessment,harness-status}.json` | Shared producer supplies two effects in the control. |
| **E6b** | Shared producer **failure** | `workCompleted` reached; 3 SUCCESS, 1 FAIL, 15 events | `failure-memory-e6(1).zip` | `experiments/E6-failure-memory/config/goal.json`; `scenarios/failure/inputs.json` | `experiments/E6-failure-memory/results/failure/{observations,trace,output,assessment,harness-status}.json` | Failed `sharedProducer` excluded for the second condition in the **same pursuit**; no claim about cross-pursuit memory. |

## Original archive fixity

The following SHA-256 values identify the ZIP files preserved in `evidence/original-archives/`. Use `SHA256SUMS` for the repository's broader checksum coverage.

| Experiment | Archive | SHA-256 |
|---|---|---|
| failure | `failure-recovery(1).zip` | `926db9f119aef57de4b796cee668af15de6dccaf715d85404456740039b06dca` |
| nested | `nested-customer-onboarding.zip` | `493b6d8aa44a2795d737b640757f6c9148e60e8b072dc8dbe6db6b5835312df9` |
| purchase | `purchase-management(1).zip` | `bf9cb75216a45c524c776b2eab4a7da4db92f8b147f51d4a33ee4f3b2515e8a2` |
| purchase | `purchase-management-reject.zip` | `8cf39c89c8a5a1d2005e24a04eec4ddc027dffe69e22beb55b84afa92406def8` |
| purchase | `purchase-management-defer.zip` | `56eb754be448ee44df279a6950fed533183c0ccce101454f02868b44ea1dba13` |
| cyclic | `cyclic-dependencies.zip` | `f7cbc3f9603ce408cd29433b3543260796a9ec1cc3f1a5ffc4254e778d06598b` |
| false | `false-success-e5(1).zip` | `c397a76ff2a235cd6aa376c8e8b5908ad06859400ba4016115cb052eb81e899b` |
| failure | `failure-memory-e6(1).zip` | `a6ccb09710942a87f3f21be00bc3a5277a8a95860bfdc0a44f81cd2f8c0555bd` |

## Claim-to-evidence guide

| Paper finding | Principal scenarios | Evidentiary boundary |
|---|---|---|
| Bounded pursuit and local fallback | E1 | One historical run; does not establish reliability gain or compensation. |
| Recursive environmental composition | E2 | One parent–child case; parent and child pursuit IDs are distinct. |
| Heterogeneous execution; outcome/completion separation | E3a–E3c; E2 | Three interactive outcomes; no statistical timing claim or guarantee of binding interchangeability. |
| Cyclic dependency can prevent timely completion | E4a–E4b | Timeout observed; no universal termination result. |
| Internal SUCCESS can diverge from external state | E5a–E5b | Controlled demonstration of a trust-boundary limitation. |
| Failure memory excludes an affordance across effects within a pursuit | E6a–E6b | Does not demonstrate memory across separate pursuits. |

## Outstanding provenance and publication checks

1. **Exact runtime revisions:** E1/E3 declare `@mamund/grail` 0.2.0; E4–E6 declare 0.3.0-beta.1; E2 references a local source tree. Package declarations are not proof of the binary that produced historical observations. Record exact commit/runtime where recoverable.
2. **E2 child provenance:** Use the child evidence embedded in the parent output for the nested historical run; do not silently equate it with the separately stored child `observations.json`.
3. **E3 scenario isolation:** Keep all three original ZIPs and their respective `data/state.json`; do not overwrite the extracted primary snapshot or merge state across outcomes.
4. **E4 watchdog:** Preserve the original timeout evidence and avoid claiming a completed GRAIL trace for the cycle case. Historical worker logs include a workstation-specific Node path; review display copies before publication without modifying archival ZIPs.
5. **Execution metadata:** Original Node versions, OS, commands, timestamps and environment variables are not fully established for every case. Mark unknowns rather than inferring them from later reproductions.
6. **Publication verification:** Check each file against the final checked-out Git repository and verify SHA-256 values after copying this index; the present index was constructed against the previously assembled package, not a live GitHub clone.
