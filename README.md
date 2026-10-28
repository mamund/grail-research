# GRAIL research — companion artifact package

**Status: Published research package (v1.0.0).** The repository is public, and the release is permanently archived on Zenodo. The package has not been certified as clean-room reproducible.

**Paper:** *Bounded Autonomy Through Environmental Composition: An Experimental Study of the GRAIL Runtime*  
**Author:** Mike Amundsen (Independent Researcher)  
**ORCID:** https://orcid.org/0000-0002-9681-8338  
**Contact:** mca@mamund.com

The paper investigates whether bounded operational autonomy can arise from authored environmental constraints and local decisions about **needs** (unmet conditions) and **means** (affordances), without a general-purpose planning agent in the coordination runtime.

## Publication and archive

- **Research package:** v1.0.0, published October 28, 2026.
- **Permanent archive (DOI):** [10.5281/zenodo.23740177](https://doi.org/10.5281/zenodo.23740177).
- **GitHub release:** [v1.0.0](https://github.com/mamund/grail-research/releases/tag/v1.0.0).
- **Manuscript:** v1.8, included in the archived research package.

The tagged GitHub release and Zenodo deposit preserve the published package. Subsequent repository edits do not alter those archived artifacts.

## Start here

- [`paper/GRAIL-research-paper-v1.8.pdf`](paper/GRAIL-research-paper-v1.8.pdf) — archived manuscript v1.8 ([LaTeX source](paper/GRAIL-research-paper-v1.8.tex)).
- [`docs/reproducibility.md`](docs/reproducibility.md) — experiment-by-experiment setup, commands, and known gaps.
- [`docs/artifact-manifest.md`](docs/artifact-manifest.md) — provenance and archived source mappings.
- [`evidence/scenario-index.md`](evidence/scenario-index.md) — eleven-scenario claim-to-evidence map.
- [`evidence/evidence-register.md`](evidence/evidence-register.md) — original eleven recorded scenario executions.
- [`evidence/reproduction-reports/`](evidence/reproduction-reports/) — separate behavioral re-execution reports.
- [`research-notes/`](research-notes/) — supplementary working research documents, not independently peer-reviewed publications.
- [`docs/publication-status.md`](docs/publication-status.md) — publication update and permanent archival DOI for the research artifacts.

## Experimental cases

| Case | Topic | Original recorded scenarios |
|---|---|---:|
| E1 | Alternative affordance selection after failure | 1 |
| E2 | Nested onboarding | 1 |
| E3 | HTTP / Node / interactive STDIO purchasing | 3 |
| E4 | Cyclic prerequisite boundary | 2 |
| E5 | False-success trust boundary | 2 |
| E6 | Failure-memory scope | 2 |
| **Total** | | **11** |

`experiments/` contains review-friendly extracted copies of supplied archives, **excluding installed `node_modules`, backup files and version-control metadata**. The corresponding untouched ZIPs are in `evidence/original-archives/` and are the preservation baseline. Do not treat extracted copies as byte-identical to their source ZIPs. Original observations and fresh reproduction evidence must remain distinct.

## Limitations

All six experiment families were behaviorally re-executed in the earlier verification environment, **not** cleanly installed and reproduced from pinned registry dependencies. E2's original local runtime revision remains unknown. E4's reproduction required a working-copy harness output-buffer increase. E1's randomized reproduction did not match its original failure count. No general termination, external-effect correctness, or cost superiority claim follows.

## License and citation

See [`LICENSES.md`](LICENSES.md) and [`CITATION.cff`](CITATION.cff). Licensing allocation approved: MIT for original code/configurations and CC BY 4.0 for the paper, documentation, and recorded evidence. Historical E4 machine paths are preserved unchanged with author approval. For the research argument, cite the manuscript (the preferred citation in `CITATION.cff`). For the executable worlds and evidence, cite the versioned research package: Amundsen, M. (2026). *GRAIL Research: Bounded Autonomy Through Environmental Composition* (Version 1.0.0). Zenodo. https://doi.org/10.5281/zenodo.23740177.
