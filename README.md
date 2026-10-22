
# Development of an Al Agent for Automated Code Coverage Gap Detection and Unit Test Generation

**Author:** Altynay Ayazbayeva  
**Supervisor:** Praveen Kumar

**Institution:** Astana IT University

## Project Overview
Developers rely on code coverage metrics to identify untested lines, but identifying these gaps is slow, manual, and error-prone. This project builds an Agentic AI tool that automatically finds uncovered code and generates unit tests to cover it.

The system combines **proactive static analysis** (to extract branch constraints before generation) with **coverage-driven feedback** (to iteratively improve the generated tests). It is evaluated against existing baselines (CoverUp, Pynguin) using the CodaMosa benchmark.

*   **Target Users:** Python developers and QA engineers.
*   **Expected Outcome:** A Python CLI agent and a VS Code extension (MVP) that improve line and branch coverage on the CodaMosa benchmark.
*   **Scope:** Unit test generation for Python only. Integration testing and cost optimization are out of scope.

(For full details, see the [Project Proposal](docs/01-project-proposal.md)).

## Current Status

**Phase:** Pre-defense 1 (24 October 2026).
The documentation and architectural design are complete. Implementation of the agent core and VS Code extension will begin in the next phase.

## Selected Technologies

- **Language:** Python 3.10+
- **Coverage Measurement:** SlipCover
- **Testing Framework:** pytest
- **LLM Integration:** OpenAI / Anthropic API (configurable)
- **UI:** VS Code Extension API
  
## Repository Structure

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
```
## Links

- YouTrack: https://altynay-ayazbayeva.youtrack.cloud/projects/CGA
- Documentation: See `docs/`
