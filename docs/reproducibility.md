# GRAIL research — reproducibility guide (v2)

**Status:** Publication-package candidate, 28 October 2026. This guide documents how to attempt behavioral reproduction of the six experiment families (eleven historical scenarios). It does **not** certify clean-room reproduction. Consult `evidence/scenario-index.md` for original evidence, which must not be overwritten.

## 1. What can be reproduced

The experiments test bounded coordination behavior, not statistically stable timings or deterministic execution paths. A reproduction should compare the **behavioral invariant** for each scenario, preserving any differences in selected affordances, generated IDs, durations, and human responses. Original records live in `evidence/original-archives/` and the supplied extracted `experiments/` trees. Previous independent attempts are described in `evidence/reproduction-reports/`.

### Environment and dependency provenance

- The prior verification environment used Node.js **v22.16.0** and npm **10.9.2**. Node 22 is a reasonable starting point, not proof of the historical environment.
- E1 and E3 declare `@mamund/grail@0.2.0`; E4–E6 declare `@mamund/grail@0.3.0-beta.1`.
- **E2 declares `@mamund/grail: file:../../grail-release`.** Its exact historical local revision is not established. The archived inspected runtime source is **not** evidence of the revision used in the original E2 run.
- Earlier behavioral checks used locally available runtime dependencies because npm registry access was unavailable. These checks **do not** establish fresh installation from the published package.

## 2. Preserve original evidence before running

1. Work from a separate copy of the repository or experiment directory. Do not execute tests against the archival source of record: harnesses may rewrite `results/`, `config/`, or `data/`.
2. Verify the original ZIP checksums listed in `evidence/scenario-index.md` or repository `SHA256SUMS`.
3. For each new execution, record Node/npm/OS, the resolved GRAIL version **and source**, the exact command, operator inputs, process exit status, output, observations, trace, and any external state. Save outputs under a separately named reproduction directory, not inside the historical evidence archives.
4. Distinguish **historical evidence**, **previous reproduction**, and **your new reproduction** in any report.

Suggested environment capture in a working copy:

```sh
node --version
npm --version
node -p 'process.platform + " " + process.arch'
npm ls @mamund/grail
```

For published-version cases, use `npm ci` from the experiment directory **only after reviewing its lockfile and installation scripts**. If installation fails, record the failure; do not silently replace a package or lockfile.

## 3. Execution matrix

Commands below run from the indicated experiment directory inside a **working copy**. Paths in the evidence index are relative to repository root.

| Case | Run | Expected behavioral observation | Qualification |
|---|---|---|---|
| E1 | `cd experiments/E1-failure-recovery && npm ci && node api-run.js` | `customerOnboarded` reached, failed provider(s) followed by eligible alternative(s). | Randomized choice can change which provider fails and how many invocations occur; do not require exact original counts. |
| E2 | See §4; `node api-run.js` after supplying a compatible local runtime | Parent `onboardingProcessed` and child `customerVerified` reached; `REJECTED` child verification, `DENIED` activation in the supplied fixture. | Historical runtime commit unknown; child evidence embedded in parent output is the appropriate same-run comparison. |
| E3 | See §5; interactive terminal plus HTTP service | Three separate outcomes: APPROVED, REJECTED, DEFERRED; each reaches `purchaseRecorded` while persisted status differs. | Reset state between runs; do not assume extracted primary snapshot represents all three original outcomes. |
| E4 | `cd experiments/E4-cyclic-dependencies && npm ci && npm test` | Control completes; cycle does not finish inside 3-second observation window **if** the watchdog is reached. | Original harness may hit `ENOBUFS` before timeout; see §6. |
| E5 | `cd experiments/E5-false-success && npm ci && npm test` | Both reach internal `recordCreated`; only truthful case creates `domain-record.json`. | Inspect external file, not just GRAIL's SUCCESS. |
| E6 | `cd experiments/E6-failure-memory && npm ci && npm test` | Control uses shared producer; failure case excludes failed shared producer for another effect within the same pursuit and uses alternatives. | No cross-pursuit failure-memory claim. |

**Do not combine all cases into one shared install or worldstate.** Package versions and external state differ.

## 4. E2 — nested onboarding and unresolved historical runtime

The original `experiments/E2-nested-onboarding/package.json` specifies `file:../../grail-release`. In the research repository layout, this resolves to a sibling directory of `experiments/`, i.e. `grail-research/grail-release`, which is **not included as a guaranteed matching historical checkout**. The original world README also mentions `runGrailWorld()` and describes a local development runtime. The dependency cannot be treated as a published, pinned historical build.

**Procedure:**

1. In a disposable working copy, obtain a GRAIL runtime that exports `runGrailWorld()` and record its source archive/commit, version, and any modifications.
2. Place that runtime at the relative location required by the existing `file:` dependency, **or** explicitly change the dependency in the working copy and document the change.
3. Run `npm install` from `experiments/E2-nested-onboarding` and verify the API export:

   ```sh
   node --input-type=module -e "import('@mamund/grail').then(m => console.log(typeof m.runGrailWorld))"
   ```

   Expected output: `function`.
4. Run `node api-run.js` and inspect the parent `verifyCustomer` observation for the embedded child worldstate and child observations.
5. Compare the parent and embedded child evidence to the original archive. **Do not** equate separately saved child observations with the embedded child record unless matching execution identifiers establish provenance.

**Publication status:** E2's architecture and behavioral result can be tested with a documented compatible runtime; exact historical binary reproduction remains **unverified** unless the original source revision is recovered.

## 5. E3 — HTTP + interactive STDIO

Run from `experiments/E3-purchase-management` in a working copy. This case requires a real interactive terminal; piping answers may not exercise the intended prompts.

Terminal A:

```sh
npm ci
node scripts/reset.mjs
node service/server.mjs
```

Terminal B, while the service is running:

```sh
grail run --config ./config --output=json
```

For each of **APPROVED**, **REJECTED**, and **DEFERRED**, reset the local service state using `node scripts/reset.mjs` **before** starting the next scenario, choose the corresponding review option at the prompt, and enter the requested note. Record the exact selections, output, and resulting `data/state.json`. Verify respectively `purchase-order-created`, `request-closed-rejected`, and `follow-up-pending` in persisted state. Consult the experiment README for prompt labels; do not assume option ordering if the working copy has changed. The three original scenario ZIPs are separately archived.

## 6. E4 — watchdog timeout versus output-buffer exhaustion

The archived `api-run.js` launches `api-worker.js` using `spawnSync` with a **3,000 ms** timeout, `SIGKILL`, and `maxBuffer: 1024*1024`. In a prior reproduction the cycle produced enough output to cause **`ENOBUFS` after approximately 763 ms**, before the watchdog. This is not the same observation as a 3-second timeout.

In a **disposable working copy only**, replace the single `maxBuffer:1024*1024` value in `experiments/E4-cyclic-dependencies/api-run.js` with `maxBuffer:64*1024*1024`, documenting the one-line harness accommodation. Then run `npm test`. The previous verification recorded control completion and cycle termination with `ETIMEDOUT` after approximately 3,051 ms. The archived original recorded a timeout after approximately 3,077 ms.

Inspect `results/{control,cycle}/harness-status.json` and worker logs. For the cycle, `timedOut: true` with `error` indicating timeout supports **failure to complete within the observation window**. An `ENOBUFS` error supports only **output-buffer exhaustion**. There is no completed cycle trace. Neither result proves mathematical nontermination.

**Do not edit the archived original ZIP or present a modified harness as original evidence.** If a future bounded-logging harness is added, identify it as a reproduction aid and compare its behavior separately.

## 7. E5 and E6 — interpretation checks

- **E5:** inspect `results/truthful-success/domain-record.json` and verify that the corresponding file is absent for `false-success`. Internal `reached: true` alone does not establish the external side effect.
- **E6:** compare control and failure assessments, traces, and observations. The relevant scope is a **single pursuit**: a failed affordance is not retried for the other effect in that pursuit. No evidence establishes a persistent failure cache across runs.

## 8. Reporting template

For each scenario, record:

```text
Scenario ID:
Date/time (timezone):
Operating system / architecture:
Node / npm:
GRAIL version, resolved source, and commit or package integrity if known:
Working-copy changes (including E4 harness changes):
Install command and outcome:
Execution command:
Operator inputs (E3):
Exit code / timeout / signal:
Goal reached (as reported):
Observed affordance outcomes and relevant domain outcomes:
External-state verification (E3/E5):
Observation / trace / log paths:
Differences from original evidence:
Classification: reproduced | partially reproduced | not reproduced | inconclusive
Limitations:
```

## 9. Remaining release gates

1. Recover E2's historical local runtime revision if possible; otherwise retain the explicit provenance limitation.
2. Execute and log clean installations using accessible pinned dependencies for E1/E3 and E4–E6; separately document any E2 substitute.
3. Confirm E3 operator interactions and persistent-state snapshots for all three outcomes in a fresh working copy.
4. Supply a safe, bounded-output E4 reproduction harness or document the working-copy buffer accommodation and its implications.
5. Verify that commands and expected files match the **final public repository checkout**, then publish the run logs and environment records.

Until those checks are completed, the supported claim is **behavioral reproduction in previously available local environments**, not independent clean-room reproducibility.
