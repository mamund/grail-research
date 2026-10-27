# GRAIL manuscript v1.1 — Bibliography audit

27 October 2026

## Scope

All twelve references in `grail-paper-draft-v1.1.tex` were checked against identifiable publisher, library, institutional, or original-document records. The citation details in the manuscript are substantially correct. Some publications have no DOI; absence of a DOI is not an error. The entries below give supporting verification records, not a claim of exhaustive edition-level inspection of physical copies.

| Key | Finding | Supporting record / recommended action |
|---|---|---|
| Simon1955 | Verified: title, author, journal, volume 69(1), pages 99–118, 1955, DOI 10.2307/1884852. | Oxford Academic https://academic.oup.com/qje/article-abstract/69/1/99/1919737 |
| Simon1962 | Verified: title, author, Proceedings of the American Philosophical Society 106(6), pages 467–482, 1962. | JSTOR issue table https://www.jstor.org/stable/i240648 ; stable article https://www.jstor.org/stable/985254. Add stable article link if desired. |
| Maes1990 | Verified: title, author, Robotics and Autonomous Systems 6(1–2), pages 49–70, 1990, DOI 10.1016/S0921-8890(05)80028-4. | Elsevier https://www.sciencedirect.com/science/article/abs/pii/S0921889005800284 |
| Gibson1979 | Verified: title, author, Houghton Mifflin, 1979. | Library catalog https://catalog.libraries.vsc.edu/cgi-bin/koha/opac-detail.pl?biblionumber=26734 ; original ISBN 0395270499. |
| Norman1988 | Verified: original title, Basic Books, 1988. | Publisher https://www.hachettebookgroup.com/titles/don-norman/the-psychology-of-everyday-things/9780465067091/ ; ISBN 9780465067091. Do not confuse with later renamed edition. |
| Brooks1991 | Verified: title, author, Artificial Intelligence 47(1–3), pages 139–159, 1991, DOI 10.1016/0004-3702(91)90053-M. | Elsevier https://www.sciencedirect.com/science/article/abs/pii/000437029190053M |
| Suchman1987 | Verified: title, author, Cambridge University Press, 1987. | Cambridge publication reference https://www.cambridge.org/core/books/abs/humanmachine-reconfigurations/references/C14CEA035436211811D4EC166B079986 ; do not substitute the 2006 second edition. |
| FikesNilsson1971 | Verified: title, authors, Artificial Intelligence 2(3–4), pages 189–208, 1971, DOI 10.1016/0004-3702(71)90010-5. | Elsevier https://www.sciencedirect.com/science/article/pii/0004370271900105 |
| McDermott1998 | Verified: PDDL Version 1.2, 1998, committee authorship, Yale report CVC TR-98-003/DCS TR-1165. | Original manual https://www.cs.cmu.edu/~mmv/planning/readings/98aips-PDDL.pdf ; the manuscript's “D. McDermott et al.” is a reasonable abbreviation, but committee authorship should be clarified if expanded. |
| Aalst2003 | Verified: four authors, Distributed and Parallel Databases 14(1), pages 5–51, 2003, DOI 10.1023/A:1022883727209. | University research portal https://research.tue.nl/en/publications/workflow-patterns-2/ ; adding issue (1) would improve precision. |
| Colledanchise2018 | **Edition-year ambiguity:** book is widely cited as 2018, while some publisher/catalog metadata state 2019. DOI 10.1201/9780429489105 is associated with the electronic edition. | Author site https://btirai.github.io/ ; bibliographic record https://gpbib.cs.ucl.ac.uk/gp-html/Colledanchise_2018.html . Retain 2018 only if the cited edition is the 2018 release; otherwise identify edition explicitly. |
| RaoGeorgeff1995 | Verified: authors, title, ICMAS 1995, pp. 312–319. | Original conference paper https://cdn.aaai.org/ICMAS/1995/ICMAS95-042.pdf ; detailed record https://apice.unibo.it/bin/view/Publication/BdiIcmas95?language=en . No DOI located; original paper URL is preferable to an aggregator link. |

## Recommended manuscript bibliography edits

1. Add the issue number `(1)` to Aalst2003.
2. Replace the RaoGeorgeff1995 DBLP navigation link with the archived original conference paper.
3. Add the Simon1962 stable JSTOR article link (optional).
4. For Colledanchise2018, resolve the edition-year ambiguity before final release. Do not silently change the year to 2019 merely because a copyright page says 2019.
5. Add ISBNs for the original Gibson and Norman editions only if a consistent book-citation policy is desired.

## Outcome

All twelve works are independently identifiable and their main bibliographic details align with the manuscript. One **edition-specific year ambiguity** remains for the behavior-trees book. This audit does not validate how accurately each source is interpreted in the prose; that is a separate citation-context review.
