## Title
Development of an Al Agent for Automated Code Coverage Gap Detection and Unit Test

**Author:** Altynay Ayazbayeva  
**Supervisor:** Praveen Kumar
**Institution:** Astana IT University

## Project Overview
Developers rely on code coverage metrics to identify untested lines, but identifying these gaps is slow, manual, and error-prone. This project builds an Agentic AI tool that automatically finds uncovered code and generates unit tests to cover it.

The system combines **proactive static analysis** (to extract branch constraints before generation) with **coverage-driven feedback** (to iteratively improve the generated tests). It is evaluated against existing baselines (CoverUp, Pynguin) using the CodaMosa benchmark.

## How it works

1. Run the unit tests and collect the coverage report
2. Find uncovered lines and branches
3. Analyze the code around each gap with static analysis tools
4. Ask the LLM to write a test for that gap
5. Run the new tests and check the coverage again

6. ## Repository Structure

The repository is organized into documentation and source code:

```text
├── README.md
├── config/           # LLM and budget configuration examples
├── diagrams/         # architecture diagrams (mmd, png)
├── docs/             # project deliverables
│   ├── 01-project-proposal.md
│   ├── 02-evidence-of-relevance.md
│   ├── 03-lit-review.md
│   ├── 04-existing-systems-comparison.md
│   ├── 05-technology-comparison.md
│   ├── 06-req-spec.md
│   ├── 07-architecture.md
│   ├── 08-prototype.md
│   ├── 09-verification-evaluation-plan.md
│   ├── 10-project-management.md
│   ├── 11-supervisor-feedback.md
│   └── appendix-a-evidence.md
├── src/              # agent core source code (Python)
└── tests/            # automted tests for the agent

## Links

- YouTrack: https://altynay-ayazbayeva.youtrack.cloud/projects/CGA
- Documentation: See `docs/`
