# GRAIL research — companion artifact package

**Status: PRE-PUBLICATION REVIEW PACKAGE.** This is a proposed working tree for the private `mamund/grail-research` repository. It has not been certified as clean-room reproducible and is not a public release.

**Paper:** *Bounded Autonomy Through Environmental Composition: An Experimental Study of the GRAIL Runtime*  
**Author:** Mike Amundsen (Independent Researcher)  
**ORCID:** https://orcid.org/0000-0002-9681-8338  
**Contact:** mca@mamund.com

The paper investigates whether bounded operational autonomy can arise from authored environmental constraints and local decisions about **needs** (unmet conditions) and **means** (affordances), without a general-purpose planning agent in the coordination runtime.

## Start here

- [`paper/grail-paper.pdf`](paper/grail-paper.pdf) — manuscript v1.4 (publication candidate).
- [`docs/reproducibility.md`](docs/reproducibility.md) — experiment-by-experiment setup, commands, and known gaps.
- [`docs/artifact-manifest.md`](docs/artifact-manifest.md) — provenance and archived source mappings.
- [`evidence/evidence-register.md`](evidence/evidence-register.md) — original eleven recorded scenario executions.
- [`evidence/reproduction-reports/`](evidence/reproduction-reports/) — separate behavioral re-execution reports.
- [`research-notes/`](research-notes/) — supplementary working research documents, not independently peer-reviewed publications.

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

See [`LICENSES.md`](LICENSES.md) and [`CITATION.cff`](CITATION.cff). Licensing terms are proposed for author approval before public release. Cite the final tagged artifact release when one exists; no DOI has yet been assigned.
