# GRAIL Research Contributions — v0.1

## Central claim

This paper presents and empirically characterizes **bounded operational autonomy through environmental composition**: a coordination architecture in which an authored environment specifies possible actions, eligibility constraints, and credited effects, while a runtime makes local choices about **which unmet condition to address (need)** and **which affordance to invoke (means)**. Operational capabilities remain responsible for domain-specific behavior and external consequences.

This is a **feasibility and boundary-characterization claim**, not a claim of novel planning primitives, general intelligence, optimality, scalability, or measured cost superiority.

## Contribution 1 — Architectural formulation

We formulate environmental composition as the separation of (i) a world of declared possibilities, (ii) a scenario specifying intent and initial state, and (iii) observations of execution. The runtime's two selection points delegate consequential choices without prescribing a complete execution sequence. Unlike claims that any choice is intelligent, the proposed definition concerns **decision authority within a bounded choice set**. The architecture draws on existing affordance, planning, reactive-action, and modularity traditions; its contribution is their specific operational arrangement and explicit trust boundary.

**Support:** GRAIL source inspection and regression record; E1 (fallback), E2 (nested pursuit), E3 (heterogeneous bindings). **Limit:** no comparative evidence establishes uniqueness relative to other declarative orchestration systems.

## Contribution 2 — Executable feasibility evidence

We report recorded pursuits demonstrating alternative selection after failure (E1), recursive world composition and separation of execution status from domain outcomes (E2), and the same purchasing goal reached under APPROVED, REJECTED, and DEFERRED decisions using Node, HTTP, and interactive STDIO bindings (E3). The runtime does not require an LLM for these coordination decisions; capabilities can nevertheless employ sophisticated reasoning or human judgment.

**Support:** archived configurations, implementations, results, observations, and traces. **Limit:** deliberately constructed scenarios, not controlled comparative trials or evidence of lower total costs.

## Contribution 3 — Empirical characterization of boundaries

We pair the feasibility demonstrations with three controlled boundary experiments. E4 exhibits repeated prerequisite resolution and a harness timeout under a dependency cycle (with a successful control); E5 demonstrates internal goal attainment without an external record when a capability falsely reports success (with a truthful control); E6 demonstrates affordance-wide exclusion after failure in a multi-effect action (with a successful control). Together these identify important constraints on termination, external soundness, and failure-memory scope.

**Support:** E4–E6 recorded artifacts and property audit. **Limit:** a finite timeout does not prove infinite execution; one false-success example does not establish how often external effects fail; action exclusion is established for the tested pursuit.

## Architectural economy: motivation, not result

A lightweight coordination runtime that does not require LLM inference is a plausible way to limit **coordination cost**. But authored worlds and capabilities incur **composition cost**. The experiments do not quantify either cost, so the paper should say *potentially economical* rather than *cost-effective in practice*. A cost claim would require matched tasks, engineering time, runtime resources, inference expenditure, and maintenance measures.

## Reviewer-facing response

**What is new if preconditions, effects, and action selection already exist?** Not those primitives individually. The paper contributes a precise architectural formulation of bounded decision authority, an executable case study across different bindings and recursive worlds, and an unusually explicit account of its failure and trust boundaries. It studies what this composition can achieve without requiring a general-purpose planner or LLM in the coordinator, and what it cannot guarantee.

## Placement in the manuscript

- **Introduction:** state the central claim and three contributions, with explicit non-claims.
- **Theory and related work:** attribute antecedents; avoid categorical workflow-versus-agent dichotomies.
- **Architecture and operational model:** formalize world/scenario/observations and two selectors.
- **Experiments:** map E1–E3 to feasibility, E4–E6 to boundaries.
- **Discussion:** distinguish coordination from composition cost and explain limitations.
- **Conclusion:** restate feasibility plus boundaries, without upgrading them to universal guarantees.
