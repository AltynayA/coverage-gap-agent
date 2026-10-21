# Literature Review

## 1. Introduction
This section provides an overview on research on regarding unit test generation with the use of large language models (LLMs) and prioritizes feedback mechanisma that guide iterations.  A systematic search was conducted using the SCopus daatabse, and relevant work between 2023 and 2026 was selected. This reviews provides a baseline for existing systems comparison in Section 4. 
## 2. Research Context

Software testing plays a critical role in the software development cycle and maintenance of its quality and functionality (Escalante-Viteri & Mauricio, 2025). According to Wang et al. (2024), automated unit test case generation remains an ongoing challenge within this scope. To address this, previous research has established the possibility of Machine Learning-based code synthesis techniques, specifically the prospect of Large Language Models (LLMs) due to their advancement in recent years. The capacity for solving complex practical tasks in code generation is illustrated by advanced models such as ChatGPT and LLaMA (Wang et al., 2024). Considering that a number of studies have begun to examine the application of LLM in generating unit tests (Dakhel et al., 2024; Pan et al., 2025), it is important to establish that their evaluation is handled through code coverage. Andrzejewski et al. (2025) define code coverage as a metric showing the proportion of source code exercised during test runs.


Two observations frame the rest of this review. First, prompting strategy affects code coverage and correctness of generated tests, but the effect is inconsistent across different models, datasets, and code difficulty. Second, the strongest recent systems do not rely on prompting alone, instead they measure the result of a generation round and use that measurement to guide the next round. The review therefore treats prompting as a background finding and focuses on feedback mechanisms.


### 2.1  Prompting Strategy Background
Research over the last two years confirms that prompting strategy affects code coverage of LLM-generated unit tests (Walczak et al., 2026; Yang et al., 2024; Ouédraogo et al., 2026). Several studies have successfully compared prompting strategies among each other (Andrzejewski et al., 2025; Lops et al., 2025) and compared them with external baselines such as manually written tests or traditional tools like Pynguin and EvoSuite (Rodríguez et al., 2026; Bhatia et al., 2024; Ouédraogo et al., 2026). These studies are consistent in measuring prompting strategy using standard metrics such as code coverage, compilation success rate, and mutation score. However, these studies take different variables into account such as model family and code context size (Walczak et al., 2026), reinforcement learning and model fine-tuning (Zhang et al., 2026), integrating a retrieval-augmented generation (RAG) framework (Zhang et al., 2025), or model type and case difficulty (Andrzejewski et al., 2025).
Consequently, the question of how isolating prompting strategy as a single controlled variable affects these metrics remains unresolved. Although prompting strategy is a legitimate variable, its effect is not consistent . This variation justifies the systems reviewed in Section 3, which measure the result of generation and feed it back rather than relying on prompt design solely.

Consequently, it is important to highlight the prompting strategy factor when building this system.  First, prompting strategy is a leigitimate variable with a  effect measurable on coverage. Second, the extent of that effect depends on the model, the dataset, and the code under test difficulty. Thus, prompting is operationalized as a background condition within the scope of this project. The future system assumes **a fixed prompting strategy** and focuses on how measured results are fed back into the next generation iteration. The systems reviewed in Section 3 are those that close this feedback loop rather than relying on prompt design alone.


## 3. Feedback-Driven and Coverage-Guided Generation
### 3.1 Coverage feedback

Coverage feedback closes the loop by measuring which lines or branches remain uncovered after a generation round and using this gap as the target for the next prompt. CoverUp implements this loop for Python: it measures coverage with SlipCover, selects uncovered lines, prompts the LLM for tests targeting those lines, executes the tests, and re-prompts on failure (Pizzorno & Berger, 2024). 

TestWeaver, on the other hand, augments this apporach with execution traces and backward slicing, reporting 68% line coverage and 54% branch coverage on the CodaMosa benchmark, against 61% and 47% for CoverUp (Le et al., 2026).
The CodaMosa benchmark referred to here is a test suite benchmark introduced by Lemieux et al. (2023) which consists of 486 Python modules on which search-based generation performs poorly. A fully automated test generation tool called Panta (Program ANalysis guided Test Automation) applies a similar loop to Java by combining dynamic coverage with static control-flow analysis and reporting 70.18% line coverage against 43.91% for SymPrompt (Gu et al., 2026).

Meanwhile, the CodaMosa algorithm  itself takes a hybrid approach. Specifically, it runs a search-based algorithm until coverage stalls, deserializes that test case into the internal representation of the search (Lemieux et al., 2023). This method outperfomed MOSA (state-of-the-art algorithm) in terms of coverage metrics on 173 out of 486 benchmarks. 

### 3.2Execution and Repair Feedback

Several studies have investigated the use of execution results to correct the next attempt. ChatUniTest implements a three-stage pipeline of preprocessing, generation, and post-processing, in which generated tests pass through syntactic, compile-time, and runtime validation, and failures trigger rule-based repair followed by LLM-based repair that feeds the error message back to the model (Chen et al., 2024). ChaCo applies a similar repair loop to pull-request-level test augmentation, executing candidate tests and using runtime feedback to repair them before integration (Zhou et al., 2026).

In these systems, execution feedback is used to make generated tests compile and pass. As noted by the ChatUniTest authors, the pipeline contains no coverage measurement, so execution feedback there addresses correctness rather than coverage (Chen et al., 2024). By contrast, ChaCo combines execution feedback with patch coverage (Zhou et al., 2026). Systems that pair execution feedback with coverage targets (CoverUp, TestWeaver) use the execution result to repair tests within a coverage-driven loop (Pizzorno & Berger, 2024; Le et al., 2026).

### 3.3 Mutation Feedback

More recent attention has focused on mutation feedback, which uses surviving mutants as the signal for the next generation round. A recent study by Dakhel et al. (2024) proposed MuTAP, which augments prompts with mutation-testing information, and it reported  reported a mutation score of 93.57% with llama-2-chat and few-shot prompting. A study by Radaideh et al. (2026) compared one-shot and multi-agent generation on a Java system using mutation score as the primary metric, and found that one-shot generation with full project context outperformed a multi-agent loop when the loop isolated classes and could not construct cross-class dependencies.

Combined, these studies indicate that mutation feedback asks a different question from coverage feedback: not "was this line executed?" but "would this test detect a fault?" The two signals are complementary, and the evidence reviewed here suggests that coverage alone overstates test quality (Dakhel et al., 2024; Abdullin et al., 2025; Radaideh et al., 2026).


### 3.4 Static Context and Hallucination
Previous research has established that insufficient context is a documented cause of invalid generated tests. Yang et al. (2024) reported unresolved symbols, parameter mismatches, and abstract instantiation as three main error categories in generated tests. To address this, ChatUniTest uses an adaptive focal-context mechanism that assembles the most relevant information about the focal method, its class, and dependent classes within the token limit (Chen et al., 2024). Similarly, Panta extracts static control-flow information to identify execution paths that need coverage (Gu et al., 2026), and TestWeaver computes backward slices and annotates them with runtime variable values before re-prompting (Le et al., 2026).

These studies indicate that static analysis improves generation, but it is applied during the loop. Particularly, it is  after a test fails, after a target is selected, or after a coverage plateau, which is the point where it stops increasing, is detected. Static analysis in LLM-based test generation is not thoroughly investigated and there is not data on injecting extracted branch conditions or type constraints into the first prompt in reviewed systems. Although ChatUniTest's Object Construction Graphs are the closest existing mechanism,they guide driver-code construction and have no branch conditions encoding (Chen et al., 2024).

### 3.5 Agent Architectures Without Closed-Loop Feedback

The literature on multi-agent test generation has highlighted several architectural designs. Radaideh et al. (2026) describe CANDOR, which isolates each class and iterates on generated tests. In a recent study, CANDOR achieved a 35.92% mutation score in 50 hours on a 25-class Java system, whilst a one-shot prompt with full project context achieved 88.35% in 45 seconds (Radaideh et al., 2026). The authors attribute the failure to CANDOR's initialization stage, which restricts the model's view to a single class and prevents construction of cross-class dependencies.

This result conflicts with the assumption that more agents and more iterations improve outcomes. The evidence presented in this section suggests that architecture matters more than iteration count, and that closed-loop feedback with full context can outperform a complex multi-agent design when the codebase fits within a single prompt.

## References
Abdullin, A., Derakhshanfar, P., & Panichella, A. (2025). Test Wars: A Comparative Study of SBST, Symbolic Execution, and LLM-Based Approaches to Unit Test Generation. 221–232. https://doi.org/10.1109/ICST62969.2025.10989033

Andrzejewski, M., Dubicka, N., Podolak, J., Kowal, M., & Siłka, J. (2025). Automated Test Generation Using Large Language Models. Data, 10(10). https://doi.org/10.3390/data10100156

Bhatia, S., Gandhi, T., Kumar, D., & Jalote, P. (2024). Unit Test Generation using Generative AI: A Comparative Performance Analysis of Autogeneration Tools. Proc. - Int. Workshop Large Lang. Models Code, LLM4Code, 54–61. https://doi.org/10.1145/3643795.3648396

Chen, Y., Hu, Z., Zhi, C., Han, J., Deng, S., & Yin, J. (2024). ChatUniTest: A Framework for LLM-Based Test Generation. FSE Companion - Companion Proc. ACM Int. Conf. Found. Softw. Eng., 572–576. https://doi.org/10.1145/3663529.3663801

Dakhel, A. M., Nikanjam, A., Majdinasab, V., Khomh, F., & Desmarais, M. C. (2024). Effective test generation using pre-trained Large Language Models and mutation testing. Information and Software Technology, 171. https://doi.org/10.1016/j.infsof.2024.107468

Escalante-Viteri, A., & Mauricio, D. (2025). Artificial Intelligence in Software Testing: A Systematic Review of a Decade of Evolution and Taxonomy. Algorithms, 18(11). https://doi.org/10.3390/a18110717

Gu, S., Nashid, N., & Mesbah, A. (2026). LLM Test Generation via Iterative Hybrid Program Analysis. Proc. - IEEE/ACM Int. Conf. Softw. Eng., ICSE, 655–666. https://doi.org/10.1145/3744916.3764553

Le, C. C., Van, C. D., Vu, T. D., Pham, M. V. T., Phan, N. H., Phan, H., & Nguyen, T. N. (2026). TestWeaver: Execution-aware, Feedback-driven Regression Testing Generation with Large Language Models. Proc. - IEEE/ACM Int. Conf. Softw. Eng., ICSE, 1288–1300. https://doi.org/10.1145/3744916.3787805

Lemieux, C., Inala, J. P., Lahiri, S. K., & Sen, S. (2023). CodaMosa: Escaping Coverage Plateaus in Test Generation with Pre-trained Large Language Models. 2023 IEEE/ACM 45th International Conference on Software Engineering (ICSE), 919–931. https://doi.org/10.1109/ICSE48619.2023.00085

Lops, A., Narducci, F., Ragone, A., Trizio, M., & Bartolini, C. (2025). LLMs for Automated Unit Test Generation and Assessment in Java: The AgoneTest Framework. 2025 40th IEEE/ACM International Conference on Automated Software Engineering (ASE), 2401–2413. https://doi.org/10.1109/ASE63991.2025.00198

Ouédraogo, W. C., Kaboré, A. K., Li, Y., Tian, H., Koyuncu, A., Klein, J., Lo, D., & Bissyandé, T. F. (2026). Prompt engineering in LLMs for automated unit test generation: A large-scale study. Empirical Software Engineering, 31(4). https://doi.org/10.1007/s10664-026-10840-4

Pan, R., Kim, M., Krishna, R., Pavuluri, R., & Sinha, S. (2025). ASTER: Natural and Multi-language Unit Test Generation with LLMs. IEEE/ACM Int. Conf. Softw. Eng. - Softw. Eng. Pract., (2025), 413–424. https://doi.org/10.1109/ICSE-SEIP66354.2025.00042

Radaideh, M., Corriveau, J.-P., & Shi, W. (2026). Understanding the Effect of Agentic Iteration on LLM-Based Unit Test Generation: A Comparison with Human-Written Tests Using Mutation Testing. IEEE Int. Conf. Big Data Artif. Intell., BDAI, 128–132. https://doi.org/10.1109/BDAI70753.2026.11655121

Rodríguez, M., Rossi, G., & Fernandez, A. (2026). Evaluating Large Language Models for the Generation of Unit Tests with Equivalence Partitions and Boundary Values. Commun. Comput. Info. Sci., 2649 CCIS, 46–60. https://doi.org/10.1007/978-3-032-06336-6_4

Walczak, J., Tomalak, P., & Laskowski, A. (2026). Impact of code context and prompting strategies on automated unit test generation with modern general-purpose large language models. Journal of Systems and Software, 237. https://doi.org/10.1016/j.jss.2026.112834

Wang, J., Huang, Y., Chen, C., Liu, Z., Wang, S., & Wang, Q. (2024). Software Testing With Large Language Models: Survey, Landscape, and Vision. IEEE Transactions on Software Engineering, 50(4), 911–936. https://doi.org/10.1109/TSE.2024.3368208

Yang, L., Yang, C., Gao, S., Wang, W., Wang, B., Zhu, Q., Chu, X., Zhou, J., Liang, G., Wang, Q., & Chen, J. (2024). On the Evaluation of Large Language Models in Unit Test Generation. 2024 39th IEEE/ACM International Conference on Automated Software Engineering (ASE), 1607–1619. https://ieeexplore.ieee.org/document/10765033

Zhang, J., Hu, X., Xia, X., Cheung, S.-C., & Li, S. (2026). Automated Unit Test Generation via Chain-of-Thought Prompt and Reinforcement Learning from Coverage Feedback. ACM Transactions on Software Engineering and Methodology, 35(4). https://doi.org/10.1145/3745765

Zhang, Q., Kang, R., Liu, Y., & Cao, X. (2025). Large Language Model-Enhanced Test Case Generation*. Int. Conf. Cloud Comput., Big Data Appl. Softw. Eng., CBASE, 793–797. https://doi.org/10.1109/CBASE67452.2025.11335495

Zhou, Z., Paltenghi, M., Kim, M., & Pradel, M. (2026). Change and Cover: Last-Mile, Pull Request-Based Regression Test Augmentation. Proc. - IEEE/ACM Int. Conf. Softw. Eng., ICSE, 1262–1274. https://doi.org/10.1145/3744916.3787768






