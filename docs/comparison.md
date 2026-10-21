# 4. Existing Systems Comparison 

## 4.1 Selection and justification
Three systems are compared: Coverup, Pyngiun and Diffblue Cover. These three systems represent three approaches:LLM-based generation (CoverUp), search-based (Pynguin), and reinforcement learning generation (Diffblue Cover). Diffblue Cover's main product supports Java, and Python version is a recent update. 

## 4.2 Comparison Table

| Criterion | CoverUp | Pynguin | Diffblue Cover |
|---|---|---|---|
| **Python support** | Yes, native | Yes, native | Partial(newer version supports Python 3.9+, core product is Java) |
| **Automatic test generation** | Yes | Yes | Yes |
| **Generation method** | LLM-driven, iterative | Search-based (genetic algorithm), recent ver has LLM | RL,newer platform adds LLM assistance |
| **Uses coverage feedback** | Yes — SlipCover measures, loop repeats until coverage stops improving | Yes,search optimises toward coverage | Yes, RL loop optimises toward coverage |
| **Uses static analysis** | Yes, but reactive:the LLM must request definitions via a `get_info` tool | Yes,bytecode/type analysis for test construction | Yes (internal, not fully public) |
| **Open source** | Yes | Yes (MIT since v0.30.0) | No (proprietary) |
| **Primary language / maturity** | Python / research prototype (FSE 2025) | Python / research prototype | Java / commercial; Python support is new and separate |

## 4.3 Evidence for Claims

### CoverUp has reactive static analysis

CoverUp (1) uses SlipCover coverage analyzer to find lines lacking coverage, (2) prompts LLMs for test for these targeted segments,(3) executes these tests, and (4) verifies before re-prompting. However, its static analysis segment cannot provide the LLM with correct context if there are conditional imports  (e.g., imports tha resolve package differences across Python versions) in code under test (Pizzorno & Berger,2024). This paper also mentions that CoverUp exposes get_info function which LLM can additionally request (e.g., types and variables), and both call request and response are appended to previous messages. Also, its reactive approach is confirmed in the official documentation where it is stated that CoverUp has conversation-based approach (GitHub - Plasma-Umass/Coverup: Automatic AI-Powered Test Suite Generator, 2025), so static analysis is a tool that can be requested during the dialogue, not a preprocessing step before creating the prompt.

### Pynguin's core generation is search-based

Pynguin functions as a Search-Based Software Engineering (SBSE) framework, and its core generation uses five genetic algorithms (Dynamosa, MOSA, MIO, WHOLE_SUITE, RANDOM) (Lukasczyk & Fraser, 2022). But, recent versions added optional LLM-assisted algorithms ( LLM, LLDYNAMOSA, LLMOSA). For example. Pynguin 0.41 release notes include the addition of LLM capabilities which are prompting, parsing, and the LLMOSAAlgorithm. and 0.47 version added LLDynaMOSA (DynaMosa + LLM calls on stall) (Releases · Se2p/Pynguin, n.d.).

### Diffblue Cover is Java/RL-based, while Python support is a recent offering

Diffblue Cover is described in its official documentation as "a reinforcement learning AI platform that automatically writes comprehensive, human-like Java unit tests" (Discover Diffblue Cover | Diffblue Documentation, 2025). However, Python support is added through Diffblue Testing Agent to orchestrate Python test generation (Diffblue, 2026). 


## 4.4 Gap 
Overall, the three claims point to a shared limitation rather than three separate ones. Although all three systems use static information, it is not applied before generation:
1) CoverUp provides static information only when the LLM requests it through get_info. So, potential constraints do not reach theprompt. Also, CoverUp official paper reports cases where conditional imports prevent the tool from providing correct context(Pizzorno & Berger, 2024).
2) Pynguin uses conditional jumps after executing a test and calculates branch distances in order to use their values as a guidance for mutation in genetic algorithms. The search moves toward each branch target over successive iterations. Its static analysis covers type inference and object construction, such as determining a function's expected parameters, but does not extract semantic constraints from branch conditions before test generation. When a test misses a rare or compound condition,Pynguin must execute and mutate again. Therefore, it has no prior information about what the condition requires as noted before.
3) Diffblue Cover's analysis is internal and dependent on Java. Meanwhile, Python support is not delivered by Diffblue Cover: Diffblue Agents delegate code generation to external AI coding agent platforms and handles only orchestration and validation.


Therefore, constraints that are not recognized by the generator are absent in the first generation attempt, and system resolves it only through failed execution and retries. Thsi is essential to this project because coverage improvement is tightly coupled with the time taken for loop to discover these constraints, therefore, iterations are spent to recover from missing constraints instead of targeting uncovered branches. The proposed system will adress this challenge by **extracting these constraints before the first prompt**. Although it is important to acknowledge that researched existing systems apply static analysis, the limitation  occurs in terms of the timing of these analyses.


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
The choice stems from the loop architecture. In the proposed system, a single module may require multiple generation rounds, and each round executes the test suite again to measure coverage change. With Coverage.py, this repeated  measurement scales the loop time with the number of rounds and can exceed the time spent on generation. SlipCover instruments bytecode and de-instruments executed code, so repeated executions approach native speed after the first pass (Slipcover, 2024). This matters for a feedback-driven loop its value depends on running many rounds (low cost for each round), and a measurement tool that adds substantial time to each iteration limits iteration number in the same time budget. 
SlipCover therefore satisfies thi requirement , while Coverage.py and pytest-cov do not at the required iteration count.


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
Diffblue. (2026, April 10). Overview. Diffblue Agents documentation. https://docs.diffblue.com

GitHub - plasma-umass/coverup: Automatic AI-powered test suite generator. (2025). GitHub. https://github.com/plasma-umass/coverup

Lukasczyk, S., & Fraser, G. (2022). Pynguin: Automated Unit Test Generation for Python. Proceedings of the ACM/IEEE 44th International Conference on Software Engineering: Companion Proceedings, 168–172. https://doi.org/10.1145/3510454.3516829

Pizzorno, J. A., & Berger, E. D. (2024). CoverUp: Coverage-Guided LLM-Based Test Generation (arXiv:2403.16218; Version 1). arXiv. https://doi.org/10.48550/arXiv.2403.16218

Releases · se2p/pynguin. (n.d.). GitHub. Retrieved October 20, 2026, from https://github.com/se2p/pynguin/releases

Slipcover. (2024, April 24). PyPI. https://pypi.org/project/slipcover/
