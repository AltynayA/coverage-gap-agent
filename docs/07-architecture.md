# 7. Architecture and Diagrams

## 7.1 Architectural Overview

The system is designed as an agentic, feedback-driven pipeline that automates Python unit test generation. The architecture is directly based on  the requirements specification (Section 6) and the technology comparison (Section 5). It consists of five main components working in a closed loop.

The design follows a modular architecture to satisfy **NFR-7 (Maintainability)**. The static analysis, LLM interaction, test execution, and coverage measurement are decoupled, so  any component (e.g.,  the coverage tool or the LLM provider) can be replaced  without affecting the core workflow.

The central data flow is:
1. **Analyze:** The agent extracts static constraints (branch conditions, types) from the target code.
2. **Generate:** It constructs a prompt containing these constraints and asks the LLM for tests.
3. **Execute:** It runs the generated pytest files.
4. **Measure:** It calculates the coverage change and checks for execution errors.
5. **Loop:** If coverage improves but does not reach 100%, it feeds the gaps and errors back into the next prompt.

## 7.2 System Context Diagram

The System Context Diagram defines the boundaries of the system, showing who uses it and what external services it relies on. It aligns with the Stakeholders section (Section 6.2) and FR-8 (LLM Configuration).

<p align="center"><img src="../diagrams/system-context-diagram.png" alt="System context diagram"  width="400">
</p>

