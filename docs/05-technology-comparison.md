# 5. Technology Comparison

This section turns the gap into three technological decisions based on limitations from Section 4. For each decision, alternatives are compared and the choice is justified .

## 5.1 Static analysis timing

**Requirement:** 
the generator must know the constraints requires to reach the required branch before writing the first test.
| Alternative | When analysis runs | Context available at first prompt | Iteration cost |
|---|---|---|---|
| **No static analysis** | Never | None (LLM relies on source text alone) | High(many failed attempts before constraints are discovered) |
| **Reactive** (CoverUp-style) | During generation, on LLM request through `get_info` | Partial(only what the LLM thinks to ask for) | Moderate(wasted iterations when the LLM does not recognise a constraint)  |
| **Proactive** (chosen) | Before the first LLM call | Full constraint set injected into initial prompt | Low — fewer correction cycles |

**Justification:**
Section 4 established that CoverUp's reactive approach fails when the LLM does not request the information needed, and its official paper documents cases where conditional imports prevent correct context from reaching the prompt (Pizzorno & Berger,2024). Pynguin's branch conditions are used only after execution, as fitness values for mutation, not as constraints for initial construction. Proactive analysis removes the LLM's burden of knowing what it lacks: branch predicates, type restrictions, and object-construction paths are extracted from the source and placed in the initial prompt. This directly addresses the timing gap identified in Section 4


## 5.2Coverage Measurement 
**Requirement:**
Coverage measurement must be accurate and not add time overhead for each test execution, because agentic tool re-executes the test after everyr generation round. So, coverage measuring time might exceed test generation time.
| Alternative | Mechanism | Overhead relative to native execution | Accuracy |
|---|---|---|---|
| **Coverage.py** | Python tracing hooks | Significant (programs can run x2 slow)  | standard reference |
| **pytest-cov** | Wrapper around Coverage.py | Same as Coverage.py | Same as Coverage.py |
| **SlipCover** (chosen) | JIT bytecode instrumentation | Near-zero (1.1-3.4× faster than Coverage.py on CPython 3.10)  | Verified against Coverage.py, in some cases more accurate  | 

**Justification:**
The choice stems from the loop architecture. In the proposed system, a single module may require multiple generation rounds, and each round executes the test suite again to measure coverage change. With Coverage.py, this repeated  measurement scales the loop time with the number of rounds and can exceed the time spent on generation. SlipCover instruments bytecode and de-instruments executed code, so repeated executions approach native speed after the first pass (Pizzorno & Berger, 2023). This matters for a feedback-driven loop its value depends on running many rounds (low cost for each round), and a measurement tool that adds substantial time to each iteration limits iteration number in the same time budget. 
SlipCover therefore satisfies thi requirement, while Coverage.py and pytest-cov do not at the required iteration count.


## 5.3 Generation model
**Requirement:**
The generator must produce tests satisfying semantic preconditions, not only syntactically valid tests, because reaching a target branch often requires constructing objects and inputs that meet conditions not stated explicitly in the source code.

| Alternative | How it works | Semantic reasoning | Self-correction |
|---|---|---|---|
| **Search-based** (Pynguin-style) | Genetic algorithms optimise toward coverage using branch distance | None(no understanding of why a branch exists)| Yes, but only mutation and re-execution |
| **Single-shot LLM** | One prompt per module,no feedback loop | Yes, but unverified | None (failed tests are not corrected) |
| **Agentic LLM loop** (chosen) | Iterative generate -> execute -> measure -> re-prompt until coverage plateaus | Yes, with execution-grounded correction | Yes(failures are fed back into the next prompt) |

**Justification:**
Search-based generation cannot reason about semantic preconditions or object-construction sequences, which limits it on code that has inter-class dependencies. Next,single-shot LLM generation lacks the feedback needed to correct failing tests. The agentic loop combines semantic generation with execution-grounded verification. However, the loopa lone  does not guarantee that the first iteration will target the right constraints. The proposed system therefore places proactive static analysis before the loop (5.1),so the first prompt already contains the branch conditions and type constraints that generator would not discover unless  from failed attempts. The agentic loop is chosen because onlt yhis alternative can act on proactively given context.

## 5.4Summary

| Gap (Section 4) | Choice (Section 5) | Effect |
|---|---|---|
| Static analysis applied too late | Proactive analysis before first prompt (5.1) | Constraints enter the initial prompt |
| Measurement overhead accumulates through rounds | SlipCover (5.2) | Many rounds within the time budget |
| Generation must reason semantically and self-correct | Agentic LLM loop (5.3) | Semantic generation with execution feedback |


These choices depend on each other because proactive analysis(5.1) only works if loop can iterate quickly(5.3), adn that requires low overhead coverage measurement(5.2). Together these form a technical solutions for this system.

## References 

Pizzorno, J. A., & Berger, E. D. (2023). SlipCover: Near Zero-Overhead Code Coverage for Python. Proceedings of the 32nd ACM SIGSOFT International Symposium on Software Testing and Analysis (ISSTA '23), 1195–1206. https://doi.org/10.1145/3597926.3598128

Pizzorno, J. A., & Berger, E. D. (2024). CoverUp: Coverage-Guided LLM-Based Test Generation (arXiv:2403.16218; Version 1). arXiv. https://doi.org/10.48550/arXiv.2403.16218

Releases · se2p/pynguin. (n.d.). GitHub. Retrieved October 20, 2026, from https://github.com/se2p/pynguin/releases
