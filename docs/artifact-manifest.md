# Artifact manifest and provenance

This package was assembled from the experiment archives and manuscript available in the research conversation on 2026-10-27. No claim is made that the original archived runtime and all historical operating environments can be reconstructed exactly.

| Case | Original ZIP | Extracted working tree | Declared GRAIL dependency |
|---|---|---|---|
| E1 | `failure-recovery(1).zip` | `experiments/E1-failure-recovery/` | 0.2.0 |
| E2 | `nested-customer-onboarding.zip` | `experiments/E2-nested-onboarding/` | local `file:../../grail-release` |
| E3 | `purchase-management(1).zip` | `experiments/E3-purchase-management/` | 0.2.0 |
| E4 | `cyclic-dependencies.zip` | `experiments/E4-cyclic-dependencies/` | 0.3.0-beta.1 |
| E5 | `false-success-e5(1).zip` | `experiments/E5-false-success/` | 0.3.0-beta.1 |
| E6 | `failure-memory-e6(1).zip` | `experiments/E6-failure-memory/` | 0.3.0-beta.1 |

Additional original archives include E3 rejected/deferred snapshots and an inspected runtime source archive. E3 snapshots are kept as distinct ZIPs because they may contain scenario-specific observations and mutable domain state. Source ZIP checksums appear in `SHA256SUMS`.

The extracted trees omit `node_modules`, editor backup files, and Git metadata. **They are not historical exact directory images.** Where a package includes original `results/` or `config/observations.json`, those files are retained in the extracted tree; do not overwrite them when rerunning.
