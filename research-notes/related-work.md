# Related Work and Comparative Architecture (v0.1)

## Scope of comparison

GRAIL is best compared with systems that separate a description of available behavior from the mechanism that executes it. Its environment declares conditions, affordances, prerequisites, and effects; its runtime chooses an unmet condition (a need) and an eligible affordance (a means); its capabilities perform domain operations. The claim examined here is not that this arrangement invents goal-directed action selection. Rather, it is that an intentionally small coordinator can exercise bounded operational autonomy when environmental authorship supplies the relevant structure.

Comparisons must distinguish **expressive power**, **selection policy**, **operational responsibility**, and **authoring burden**. Two systems can use similar precondition/effect representations yet differ in how they choose actions, how they recover, and what they assume about external effects. Our experiments characterize GRAIL; they do not benchmark competing systems.

## Classical planning and action representations

Classical AI planning has long represented actions through preconditions and effects. STRIPS (Fikes and Nilsson, 1971) and later planning languages such as PDDL (McDermott et al., 1998) establish a clear precedent for declarative action models and goal-directed search. Thus, preconditions and effects are not themselves a novel GRAIL contribution.

The distinction is operational. Classical planners typically search for a plan, often a sequence or partially ordered set of actions, according to a formal transition model. GRAIL instead resolves unmet conditions and selects affordances during pursuit, invoking real capabilities as it proceeds. This avoids requiring a complete plan before execution, but it also limits what the runtime can guarantee. Its local selectors need not reason globally about dependency cycles, resource conflicts, or optimal paths. E4 demonstrates this limitation: a cyclic dependency was repeatedly explored until the external harness terminated the run. GRAIL's architecture should therefore be described as **runtime goal-directed dependency resolution**, not as a replacement for classical planning.

## Workflow and process orchestration

Workflow systems describe activities, dependencies, and coordination across services and people. Workflow patterns research (van der Aalst et al., 2003) shows that the design space is much richer than a single predetermined linear path: branching, synchronization, cancellation, and other control-flow structures are established capabilities. Accordingly, it would be inaccurate to contrast GRAIL with an allegedly universal model of rigid, static workflows.

A narrower contrast is defensible. In many process models, the author explicitly specifies control-flow relationships among activities, even when the model admits alternatives. In GRAIL, the author specifies a bounded space of affordances and their declared effects; the runtime determines a path through that space by selecting needs and means. E1 shows that an alternate affordance can be selected after failure without the environment prescribing that exact execution sequence. The benefit is a different authoring abstraction, not an absence of control flow or a claim of superior expressiveness. Whether it reduces total engineering effort depends on the task and on the cost of composing and validating the environment.

## Reactive, situated, and behavior-based systems

Situated-agent research provides a closer conceptual comparison. Maes (1990) investigated how goal-directed behavior can arise from action selection rather than complete deliberative planning. Brooks (1991) questioned the necessity of centralized, comprehensive representations for intelligent action. These works support the broader possibility of meaningful autonomy through comparatively local mechanisms. GRAIL nevertheless uses explicit symbolic conditions and declared effects, so it should not be described as representation-free or as implementing Maes's activation-network approach.

Behavior trees, widely used in robotics and games (Colledanchise and Ögren, 2018), also coordinate modular behaviors reactively. They make execution structure and fallback behavior explicit through tree nodes and ticking semantics. GRAIL's environment, by contrast, represents eligible means through affordances associated with conditions, with runtime selection between candidates. Both approaches can support modular execution; they differ in the locus of control-flow authorship and the semantics of progression. No comparative performance or maintainability result follows from this conceptual distinction alone.

## Agent architectures and delegated reasoning

Belief–desire–intention (BDI) architectures distinguish representations of the world, desired states, and committed courses of action (Rao and Georgeff, 1995). GRAIL similarly distinguishes initial worldstate, goal, and action selection, but it does not implement a general BDI deliberation cycle. Its current selectors are intentionally simple and need not maintain beliefs, intentions, or plans in the BDI sense.

This comparison helps locate GRAIL's autonomy. The runtime has delegated authority to choose *which unmet condition* to pursue and *which available affordance* to invoke. It does not have authority to invent arbitrary capabilities or redefine the environment. Domain-specific reasoning can be implemented within capabilities, including deterministic rules, human review, or language-model inference. E3 demonstrates human participation through interactive STDIO; the absence of a required LLM in the coordinator does not imply that LLMs are unnecessary for every task.

## Architectural economy and the location of complexity

GRAIL's architectural proposition concerns **where complexity resides**. The environment author supplies the possible actions and their relationships; selectors choose locally; capabilities handle operational details and domain semantics. This resembles Simon's observation that system behavior depends on the organization of the system and its environment, and his account of modular complexity (Simon, 1962). It suggests a route to economical coordination when the relevant action space is sufficiently stable and can be authored explicitly.

There is a corresponding cost transfer. A small runtime does not eliminate the need to define accurate affordances, reliable capability contracts, recovery behavior, and suitable boundaries. E5 makes this especially clear: GRAIL reached its internal goal after a capability falsely reported success, even though the external effect did not occur. E6 shows that a conservative failure-memory rule can rule out a multi-effect affordance for another responsibility. These are architectural tradeoffs, not incidental details.

Consequently, the current evidence supports a **feasibility and architectural-economy argument**, not a measured claim of lower cost. Demonstrating cost-effectiveness would require controlled comparisons of implementation effort, execution overhead, inference expenditure where applicable, and operational maintenance under comparable tasks.

## Positioning and contribution

The defensible contribution is an empirically examined composition pattern: a bounded environment of declared possibilities; eligibility and consequence modeled through conditions and effects; two local selectors that choose needs and means; and a capability boundary that delegates operational execution and domain interpretation. E1–E3 show this pattern coordinating fallback, recursive worlds, and heterogeneous human/software operations. E4–E6 establish limits involving cycles, external truth, and failure-memory scope.

This positions GRAIL **between explicit process orchestration and general-purpose planning or agent deliberation**, without asserting that it dominates either. The research contribution is a precise operational account of what this composition enables, the responsibility it shifts to environmental authors and capabilities, and the guarantees it does not supply.

## References to incorporate

- Fikes, R. E., and Nilsson, N. J. (1971). STRIPS: A new approach to the application of theorem proving to problem solving. *Artificial Intelligence*, 2(3–4), 189–208.
- McDermott, D., et al. (1998). PDDL — The Planning Domain Definition Language. AIPS-98 Planning Competition Committee technical report.
- van der Aalst, W. M. P., ter Hofstede, A. H. M., Kiepuszewski, B., and Barros, A. P. (2003). Workflow patterns. *Distributed and Parallel Databases*, 14, 5–51.
- Maes, P. (1990). Situated agents can have goals. *Robotics and Autonomous Systems*, 6(1–2), 49–70.
- Brooks, R. A. (1991). Intelligence without representation. *Artificial Intelligence*, 47(1–3), 139–159.
- Colledanchise, M., and Ögren, P. (2018). *Behavior Trees in Robotics and AI: An Introduction*. CRC Press.
- Rao, A. S., and Georgeff, M. P. (1995). BDI agents: From theory to practice. *Proceedings of the First International Conference on Multi-Agent Systems*.
- Simon, H. A. (1962). The architecture of complexity. *Proceedings of the American Philosophical Society*, 106(6), 467–482.
