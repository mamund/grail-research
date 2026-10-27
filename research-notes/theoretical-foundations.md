# Theoretical foundations for bounded autonomy through environmental composition

Version 0.1 — 27 October 2026

**Purpose.** This note develops a scholarly framing for the GRAIL manuscript. Historical/theoretical connections are interpretive comparisons, not claims that the cited authors designed or endorsed GRAIL. Empirical statements are bounded by Evidence Register v0.5.

## 1. The question: how much reasoning must reside in an agent?

GRAIL examines a constrained form of autonomy: a runtime can pursue a declared goal by selecting unresolved conditions and affordances from a finite environment. Its apparent flexibility comes partly from the structure supplied by the world author. The research question is therefore not whether planning disappears, but **where the work of specifying possibilities, dependencies and consequences occurs**. In GRAIL, much of that work is performed before execution through environmental composition; at runtime, two local selection mechanisms choose a need and a means.

This arrangement should not be described as planning-free in an absolute sense. A world registry can embody substantial prior design knowledge, and the runtime resolves dependencies toward a goal. The defensible contrast is with systems that construct an unrestricted global action plan during each pursuit.

## 2. Bounded rationality and the designed choice set

Herbert Simon's account of bounded rationality challenges the assumption of unconstrained optimization by decision-makers. GRAIL offers an engineering analogue: the runtime does not search the universe of possible actions. It chooses among explicitly published affordances under declared preconditions, effects and observed failures. Its bound is both computational and semantic: it can select only what the environment represents.

This connection explains tractability but not correctness. An incomplete or misleading registry may make an attainable external goal unreachable internally, or make an internally reached goal externally false. Moreover, randomized selection in the inspected implementation does not amount to Simonian satisficing as a formal decision rule; the comparison concerns bounded choice, not an identical algorithm.

## 3. Affordances and the authored virtual environment

James Gibson's ecological account treats affordances as action possibilities relative to an organism and its environment. GRAIL borrows the useful vocabulary of possibilities, but operationalizes it differently: an affordance is a declared software construct with effects, prerequisites and a capability binding. These are **engineered declarations**, not automatically perceived properties of a physical environment.

Donald Norman's design-oriented treatment of affordances helps illuminate the role of discoverability and representation, but should not be conflated with Gibson's ecological claim. A GRAIL registry makes actionable possibilities legible to the runtime. The virtual-world premise also removes many constraints associated with physical distance: a Node call, HTTP request or STDIO process can be modeled as an available means without modeling physical traversal. Latency, authorization, cost and failure remain real constraints, even if distance is absent from the formal world model.

## 4. Situated action and reactive coordination

Rodney Brooks challenged the necessity of centralized, complete world representations for intelligent behavior; Lucy Suchman emphasized that action unfolds in situated circumstances rather than simply executing plans. GRAIL shares a limited family resemblance with reactive approaches: it responds to current worldstate and invocation feedback, and may choose a different affordance when one fails.

The difference is consequential. GRAIL has an explicit symbolic registry, a declared goal and a dependency-resolution mechanism. It is not a subsumption architecture, and its recorded feedback does not by itself establish online learning. Local selection should be understood as a constrained execution policy over an authored symbolic environment.

## 5. Relation to classical planning and workflow coordination

Classical planning formalisms such as STRIPS also model actions through preconditions and effects. GRAIL's vocabulary therefore has a clear antecedent. The distinguishing empirical focus is the combination of (i) responsibility-oriented Boolean conditions, (ii) selection among declared alternative producers, (iii) externally bound capabilities, (iv) runtime observations and failure exclusion, and (v) a loose semantic boundary between reported invocation success and domain outcome. These are differences in architecture and experimental emphasis, not proof of a new general planning paradigm.

Workflow engines likewise coordinate predefined work. Without controlled comparisons, we cannot claim GRAIL is cheaper, more reliable or more autonomous than a workflow system. A productive future comparison would hold tasks, capability implementations and failure distributions constant while varying the orchestration model.

## 6. A bounded definition of autonomy

For this paper, **bounded autonomy** means the runtime's ability, within an explicitly composed environment, to select intermediate needs and available means, react to invocation results, and continue or stop pursuit without requiring a human to prescribe every invocation sequence. It does **not** imply authority to create new capabilities, independently verify external effects, optimize globally, or guarantee attainment.

A useful distinction follows:

- **World:** published possibilities and their declared relationships.
- **Scenario:** the intent, inputs and initial worldstate for one pursuit.
- **Observations:** recorded experience of invocations and outcomes.

The world bounds possible behavior; the scenario supplies a direction; observations can affect later choice. In the inspected runtime, the selectors are randomized within eligible sets and use failure exclusion. Any assertion that stronger selectors *increase* autonomy is a research hypothesis until tested under a defined measure.

## 7. Empirical connection and scope

E1 shows two local fallback episodes within one successful pursuit. E2 shows one parent–child composition and unfavorable business outcomes alongside successful responsibility completion. E3a–c show HTTP, Node and interactive STDIO coordinated to record approved, rejected and deferred purchasing decisions. These observations support **existence claims about these bounded executions**, not statistical performance claims or universal termination and correctness.

The most important conceptual limit is the capability trust boundary. The runtime can mark a declared condition satisfied when a capability reports success; it cannot infer, from that report alone, that the corresponding external fact is true. That is not an incidental implementation flaw but a property of delegated execution that must be specified and tested.

## References to verify and format for publication

- Simon, H. A. (1955). “A Behavioral Model of Rational Choice.” *Quarterly Journal of Economics* 69(1), 99–118.
- Gibson, J. J. (1979). *The Ecological Approach to Visual Perception*. Houghton Mifflin.
- Norman, D. A. (1988). *The Psychology of Everyday Things*. Basic Books.
- Brooks, R. A. (1991). “Intelligence without Representation.” *Artificial Intelligence* 47(1–3), 139–159.
- Suchman, L. A. (1987). *Plans and Situated Actions: The Problem of Human-Machine Communication*. Cambridge University Press.
- Fikes, R. E., and Nilsson, N. J. (1971). “STRIPS: A New Approach to the Application of Theorem Proving to Problem Solving.” *Artificial Intelligence* 2(3–4), 189–208.

**Bibliographic status:** These are established works identified from general scholarly knowledge; bibliographic details and the precise relevance of individual passages should be checked against authoritative editions before submission. No quotations are attributed to these works in this draft.
