# Literature Review

## 1. Introduction


## 2. Research Context

Software testing plays a critical role in the software development cycle and maintenance of its quality and functionality (Escalante-Viteri & Mauricio, 2025). According to Wang et al. (2024), automated unit test case generation remains an ongoing challenge within this scope. To address this, previous research has established the possibility of Machine Learning-based code synthesis techniques, specifically the prospect of Large Language Models (LLMs) due to their advancement in recent years. The capacity for solving complex practical tasks in code generation is illustrated by advanced models such as ChatGPT and LLaMA (Wang et al., 2024). Considering that a number of studies have begun to examine the application of LLM in generating unit tests (Dakhel et al., 2024; Pan et al., 2025), it is important to establish that their evaluation is handled through code coverage. Andrzejewski et al. (2025) define code coverage as a metric showing the proportion of source code exercised during test runs.


Two observations frame the rest of this review. First, prompting strategy affects code coverage and correctness of generated tests, but the effect is inconsistent across different models, datasets, and code difficulty. Second, the strongest recent systems do not rely on prompting alone, instead they measure the result of a generation round and use that measurement to guide the next round. The review therefore treats prompting as a background finding and focuses on feedback mechanisms.