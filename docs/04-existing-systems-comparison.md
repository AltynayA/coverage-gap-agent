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

### 4.3.1 CoverUp has reactive static analysis

CoverUp (1) uses SlipCover coverage analyzer to find lines lacking coverage, (2) prompts LLMs for test for these targeted segments,(3) executes these tests, and (4) verifies before re-prompting. However, its static analysis segment cannot provide the LLM with correct context if there are conditional imports  (e.g., imports tha resolve package differences across Python versions) in code under test (Pizzorno & Berger,2024). This paper also mentions that CoverUp exposes get_info function which LLM can additionally request (e.g., types and variables), and both call request and response are appended to previous messages. Also, its reactive approach is confirmed in the official documentation where it is stated that CoverUp has conversation-based approach (GitHub - Plasma-Umass/Coverup: Automatic AI-Powered Test Suite Generator, 2025), so static analysis is a tool that can be requested during the dialogue, not a preprocessing step before creating the prompt.

### 4.3.2 Pynguin's core generation is search-based

Pynguin functions as a Search-Based Software Engineering (SBSE) framework, and its core generation uses five genetic algorithms (Dynamosa, MOSA, MIO, WHOLE_SUITE, RANDOM) (Lukasczyk & Fraser, 2022). But, recent versions added optional LLM-assisted algorithms ( LLM, LLDYNAMOSA, LLMOSA). For example. Pynguin 0.41 release notes include the addition of LLM capabilities which are prompting, parsing, and the LLMOSAAlgorithm. and 0.47 version added LLDynaMOSA (DynaMosa + LLM calls on stall) (Releases · Se2p/Pynguin, n.d.).

### 4.3.3 Diffblue Cover is Java/RL-based, while Python support is a recent offering

Diffblue Cover is described in its official documentation as "a reinforcement learning AI platform that automatically writes comprehensive, human-like Java unit tests" (Discover Diffblue Cover | Diffblue Documentation, 2025). However, Python support is added through Diffblue Testing Agent to orchestrate Python test generation (Diffblue, 2026). 


## 4.4 Gap 
Overall, the three claims point to a shared limitation rather than three separate ones. Although all three systems use static information, it is not applied before generation:
1) CoverUp provides static information only when the LLM requests it through get_info. So, potential constraints do not reach theprompt. Also, CoverUp official paper reports cases where conditional imports prevent the tool from providing correct context(Pizzorno & Berger, 2024).
2) Pynguin uses conditional jumps after executing a test and calculates branch distances in order to use their values as a guidance for mutation in genetic algorithms. The search moves toward each branch target over successive iterations. Its static analysis covers type inference and object construction, such as determining a function's expected parameters, but does not extract semantic constraints from branch conditions before test generation. When a test misses a rare or compound condition,Pynguin must execute and mutate again. Therefore, it has no prior information about what the condition requires as noted before.
3) Diffblue Cover's analysis is internal and dependent on Java. Meanwhile, Python support is not delivered by Diffblue Cover: Diffblue Agents delegate code generation to external AI coding agent platforms and handles only orchestration and validation.


Therefore, constraints that are not recognized by the generator are absent in the first generation attempt, and system resolves it only through failed execution and retries. Thsi is essential to this project because coverage improvement is tightly coupled with the time taken for loop to discover these constraints, therefore, iterations are spent to recover from missing constraints instead of targeting uncovered branches. The proposed system will adress this challenge by **extracting these constraints before the first prompt**. Although it is important to acknowledge that researched existing systems apply static analysis, the limitation  occurs in terms of the timing of these analyses.



## References 
Diffblue. (2025). What is Diffblue Cover? Diffblue Documentation. https://docs-proxy.diffblue.io/get-started/what-is-diffblue-cover

Diffblue. (2026, April 10). Overview. Diffblue Agents documentation. https://docs.diffblue.com

GitHub - plasma-umass/coverup: Automatic AI-powered test suite generator. (2025). GitHub. https://github.com/plasma-umass/coverup

Lukasczyk, S., & Fraser, G. (2022). Pynguin: Automated Unit Test Generation for Python. Proceedings of the ACM/IEEE 44th International Conference on Software Engineering: Companion Proceedings, 168–172. https://doi.org/10.1145/3510454.3516829

Pizzorno, J. A., & Berger, E. D. (2024). CoverUp: Coverage-Guided LLM-Based Test Generation (arXiv:2403.16218; Version 1). arXiv. https://doi.org/10.48550/arXiv.2403.16218

Releases · se2p/pynguin. (n.d.). GitHub. Retrieved October 20, 2026, from https://github.com/se2p/pynguin/releases

