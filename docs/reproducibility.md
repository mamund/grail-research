# Reproduction guide (pre-release)

## Requirements and boundaries

Node.js 22.x is a reasonable baseline for the recorded verification environment (Node v22.16.0; npm 10.9.2). This is **not** an exact historical environment specification. Each experiment is a separate Node project. Inspect its `README.md`, `package.json` and lockfile before installation. Execute in a copy if its harness writes to `results/`, `config/`, or `data/`; preserve the original observations unchanged.

**Package dependency provenance:** E1/E3 declare `@mamund/grail@0.2.0`; E4–E6 declare `@mamund/grail@0.3.0-beta.1`; E2 declares `file:../../grail-release`, so its exact original source cannot be reconstructed from the supplied package alone. The inspected 0.3.0-beta.1 runtime source is archived separately but **must not be presented as E2's historical build**.

## Entry points

| Case | Procedure | Important checks / qualifications |
|---|---|---|
| E1 | In `experiments/E1-failure-recovery`, install dependencies, run `node api-run.js`. | Goal `customerOnboarded`; provider failures and selected paths vary under randomized selection. |
| E2 | Supply a compatible local `grail-release` at the relative path expected by `package.json`; run `node api-run.js` from `experiments/E2-nested-onboarding`. | Parent `onboardingProcessed`, child `customerVerified`; domain outcomes `REJECTED` / `DENIED`. **Historical runtime unknown.** |
| E3 | In `experiments/E3-purchase-management`, install 0.2.0; run `node scripts/reset.mjs`, start `node service/server.mjs` in another terminal, then run `grail run --config ./config --output=json` in a real terminal. Repeat with review selections 1/2/3 and reset between runs. | `purchaseRecorded`; persisted statuses `purchase-order-created`, `request-closed-rejected`, `follow-up-pending`. Enter review note when prompted. |
| E4 | In `experiments/E4-cyclic-dependencies`, `npm test` after installing 0.3.0-beta.1. | Control reaches goal; cycle does not terminate in 3-second watchdog window. Original harness may hit `ENOBUFS` first; reproduction used a **working-copy** increase of `maxBuffer` to 64 MiB. |
| E5 | In `experiments/E5-false-success`, `npm test`. | Both scenarios reach internal goal; only truthful success creates external domain record. |
| E6 | In `experiments/E6-failure-memory`, `npm test`. | Control uses shared producer; failure scenario excludes failed shared producer and uses alternatives. |

**E2 path caution:** Because the extracted research layout differs from the original workspace, `file:../../grail-release` is not resolvable until the tester supplies the runtime source in the expected location. Do not substitute a different runtime without recording the change.

**E4 harness caution:** The extracted archived harness is preserved as supplied. If it exits with `ENOBUFS`, make a *separate working copy* and increase `maxBuffer` for the reproduction; record the modification and distinguish watchdog timeout from output-buffer exhaustion.

## Record and compare

For every run record Node/npm versions, resolved `@mamund/grail` version and source, OS, command, initial scenario, operator input (E3), process status, pursuit goal, observations, trace, and external state where relevant. Save fresh outputs outside the preserved experiment evidence. Compare *behavioral invariants* rather than IDs, randomized paths, or millisecond timings. See `evidence/reproduction-reports/` for the earlier verification results.

## Open tasks before claiming clean-room reproducibility

1. Resolve E2 historical source revision or document it as irretrievable.
2. Test fresh installation from independently accessible pinned dependencies and confirm lockfiles.
3. Make E3 human responses machine-recordable without bypassing interactive STDIO.
4. Offer an E4 harness variant that bounds logging while preserving original archived harness and trace.
5. Validate all case-specific commands on the target OS and capture clean-install logs.
