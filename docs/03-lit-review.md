# Literature Review

## 1. Introduction


## 2. Research Context

Software testing plays a critical role in the software development cycle and maintenance of its quality and functionality (Escalante-Viteri & Mauricio, 2025). According to Wang et al. (2024), automated unit test case generation remains an ongoing challenge within this scope. To address this, previous research has established the possibility of Machine Learning-based code synthesis techniques, specifically the prospect of Large Language Models (LLMs) due to their advancement in recent years. The capacity for solving complex practical tasks in code generation is illustrated by advanced models such as ChatGPT and LLaMA (Wang et al., 2024). Considering that a number of studies have begun to examine the application of LLM in generating unit tests (Dakhel et al., 2024; Pan et al., 2025), it is important to establish that their evaluation is handled through code coverage. Andrzejewski et al. (2025) define code coverage as a metric showing the proportion of source code exercised during test runs.


Two observations frame the rest of this review. First, prompting strategy affects code coverage and correctness of generated tests, but the effect is inconsistent across different models, datasets, and code difficulty. Second, the strongest recent systems do not rely on prompting alone, instead they measure the result of a generation round and use that measurement to guide the next round. The review therefore treats prompting as a background finding and focuses on feedback mechanisms.


## 2.1  Prompting Strategy Background
Research through the last two years confirms that prompting strategy affects code coverage of LLM-generated unit tests (Walczak et al., 2026; Yang et al., 2024; Ouédraogo et al., 2026). Several studies have successfully compared prompting strategies among each other (Andrzejewski et al., 2025; Lops et al., 2025) and compared them with external baselines such as manually written tests or traditional tools like Pynguin and EvoSuite (Rodríguez et al., 2026; Bhatia et al., 2024; Ouédraogo et al., 2026). These studies are consistent in measuring prompting strategy using standard metrics such as code coverage, compilation success rate, and mutation score. However, these studies take different variables into account such as model family and code context size (Walczak et al., 2026), reinforcement learning and model fine-tuning (Zhang et al., 2026), integrating a retrieval-augmented generation (RAG) framework (Zhang et al., 2025), or model type and case difficulty (Andrzejewski et al., 2025).
Consequently, the question of how isolating prompting strategy as a single controlled variable affects these metrics remains unresolved. Although prompting strategy is a legitimate variable, its effect is not consistent . This variation justifies the systems reviewed in Section 3, which measure the result of generation and feed it back rather than relying on prompt design solely.

Consequently, it is important to highlight the prompting strategy factor when building this system.  First, prompting strategy is a leigitimate variable with a  effect measurable on coverage. Second, the extent of that effect depends on the model, the dataset, and the code under test difficulty. Thus, prompting is operationalized as a background condition within the scope of this project. The future system assumes **a fixed prompting strategy** and focuses on how measured results are fed back into the next generation iteration.



## References
Andrzejewski, M., Dubicka, N., Podolak, J., Kowal, M., & Siłka, J. (2025). Automated Test Generation Using Large Language Models. Data, 10(10). https://doi.org/10.3390/data10100156

Bhatia, S., Gandhi, T., Kumar, D., & Jalote, P. (2024). Unit Test Generation using Generative AI: A Comparative Performance Analysis of Autogeneration Tools. Proc. - Int. Workshop Large Lang. Models Code, LLM4Code, 54–61. https://doi.org/10.1145/3643795.3648396

Dakhel, A. M., Nikanjam, A., Majdinasab, V., Khomh, F., & Desmarais, M. C. (2024). Effective test generation using pre-trained Large Language Models and mutation testing. Information and Software Technology, 171. https://doi.org/10.1016/j.infsof.2024.107468

Escalante-Viteri, A., & Mauricio, D. (2025). Artificial Intelligence in Software Testing: A Systematic Review of a Decade of Evolution and Taxonomy. Algorithms, 18(11). https://doi.org/10.3390/a18110717

Lops, A., Narducci, F., Ragone, A., Trizio, M., & Bartolini, C. (2025). LLMs for Automated Unit Test Generation and Assessment in Java: The AgoneTest Framework. 2025 40th IEEE/ACM International Conference on Automated Software Engineering (ASE), 2401–2413. https://doi.org/10.1109/ASE63991.2025.00198

Ouédraogo, W. C., Kaboré, A. K., Li, Y., Tian, H., Koyuncu, A., Klein, J., Lo, D., & Bissyandé, T. F. (2026). Prompt engineering in LLMs for automated unit test generation: A large-scale study. Empirical Software Engineering, 31(4). https://doi.org/10.1007/s10664-026-10840-4

Pan, R., Kim, M., Krishna, R., Pavuluri, R., & Sinha, S. (2025). ASTER: Natural and Multi-language Unit Test Generation with LLMs. IEEE/ACM Int. Conf. Softw. Eng. - Softw. Eng. Pract., (2025), 413–424. https://doi.org/10.1109/ICSE-SEIP66354.2025.00042

Rodríguez, M., Rossi, G., & Fernandez, A. (2026). Evaluating Large Language Models for the Generation of Unit Tests with Equivalence Partitions and Boundary Values. Commun. Comput. Info. Sci., 2649 CCIS, 46–60. https://doi.org/10.1007/978-3-032-06336-6_4

Walczak, J., Tomalak, P., & Laskowski, A. (2026). Impact of code context and prompting strategies on automated unit test generation with modern general-purpose large language models. Journal of Systems and Software, 237. https://doi.org/10.1016/j.jss.2026.112834

Wang, J., Huang, Y., Chen, C., Liu, Z., Wang, S., & Wang, Q. (2024). Software Testing With Large Language Models: Survey, Landscape, and Vision. IEEE Transactions on Software Engineering, 50(4), 911–936. https://doi.org/10.1109/TSE.2024.3368208

Yang, L., Yang, C., Gao, S., Wang, W., Wang, B., Zhu, Q., Chu, X., Zhou, J., Liang, G., Wang, Q., & Chen, J. (2024). On the Evaluation of Large Language Models in Unit Test Generation. 2024 39th IEEE/ACM International Conference on Automated Software Engineering (ASE), 1607–1619. https://ieeexplore.ieee.org/document/10765033

Zhang, J., Hu, X., Xia, X., Cheung, S.-C., & Li, S. (2026). Automated Unit Test Generation via Chain-of-Thought Prompt and Reinforcement Learning from Coverage Feedback. ACM Transactions on Software Engineering and Methodology, 35(4). https://doi.org/10.1145/3745765

Zhang, Q., Kang, R., Liu, Y., & Cao, X. (2025). Large Language Model-Enhanced Test Case Generation*. Int. Conf. Cloud Comput., Big Data Appl. Softw. Eng., CBASE, 793–797. https://doi.org/10.1109/CBASE67452.2025.11335495





