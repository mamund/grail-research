# Publication checklist — release candidate

## Verified in staged package

- [x] Approved manuscript v1.8 (LaTeX and PDF).
- [x] Eleven original scenario executions indexed across E1–E6.
- [x] Original evidence archives retained separately from reproduction reports.
- [x] Repository file checksums verified in previous staged audit.
- [x] MIT and CC BY 4.0 allocation approved by author.
- [x] Historical E4 machine paths approved for unchanged preservation.
- [x] E2 historical runtime provenance limitation disclosed.
- [x] E4 timeout-harness limitation disclosed.
- [x] Behavioral reproduction distinguished from clean-room reproduction.
- [x] Local checkout compared with reviewed package; two superseded v1.4 manuscript files removed and committed/pushed (user-confirmed).

## Before changing repository visibility

- [ ] Confirm latest Git checkout has a clean working tree after metadata changes.
- [ ] Verify no credentials, personal information, or unintended files in latest Git checkout; earlier automated scan is not a substitute for final manual review.
- [ ] Validate `CITATION.cff`, README, licenses, and all relative links against latest checkout.
- [ ] Check whether any original archive embeds materials that cannot be relicensed by the author.
- [ ] Confirm the manuscript's artifact-availability statement remains truthful for the planned release stage.
- [ ] Decide public release tag (`v1.0.0` proposed) and whether to use GitHub release assets.

## Release and archive (do in order)

- [ ] Commit and push reviewed metadata updates.
- [ ] Change repository visibility to public after explicit author approval.
- [ ] Create annotated Git tag and GitHub release (`v1.0.0` proposed); attach v1.8 PDF.
- [ ] Verify public clone, links, download, and archive hashes.
- [ ] If using Zenodo, connect repository and archive the tagged release; capture DOI.
- [ ] After DOI exists, update citation metadata and any appropriate manuscript availability wording in a subsequent documented revision (do not retroactively alter tagged artifacts).

## Known limitations (not blockers if disclosed)

- Historical E2 runtime revision remains unidentified.
- E4 cycle was observed to exceed the specified timeout, not proven nonterminating.
- Clean-room reproduction has not been demonstrated.
