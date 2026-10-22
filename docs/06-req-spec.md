# Requirements Specification

## 1. Purpose and Scope

This document specifies the functional and non-functional requirements for an agentic AI system that improves unit test coverage for Python modules. The system extracts static constraints before generation, uses coverage feedback to drive iteration, and reports both coverage and mutation score. Requirements are derived from the literature review (Sections 4–5) and from the proposed system's technology choices.

## 2. Stakeholders

- **Primary user:** a Python developer who wants to improve test coverage on a module or package.
- **Secondary user:** a QA or test engineer who reviews generated tests before merging.
- **Project supervisor:** reviews deliverables and provides feedback at agreed checkpoints.

## 3. Functional Requirements

Priorities use MoSCoW (Must, Should, Could) model and each requirement has an acceptance criterion that can be verified.

### FR-1: Proactive static analysis (Must)

The system shall extract branch predicates, type constraints, and object-construction requirements from the module under test **before** the first LLM call, and inject them into the initial prompt.

**Acceptance criterion:** For a module with at least one conditional branch, the initial prompt contains the branch predicate for that branch. Verified by inspecting the prompt log for three sample modules.

### FR-2: Coverage measurement (Must)

The system shall measure line and branch coverage after each generation round using SlipCover.

**Acceptance criterion:** The coverage report contains separate line and branch percentages. Verified by running the system on a module with known coverage and comparing against a manual `coverage.py` run.

### FR-3: Iterative generation loop (Must)

The system shall repeat the generate → execute → measure → re-prompt cycle until a stopping condition is met. Stopping conditions: coverage reaches 100%, no coverage improvement across N consecutive iterations (default N = 3), or the time budget expires.

**Acceptance criterion:** The loop runs at least two iterations on a module that does not reach 100% coverage in the first iteration. Verified by log inspection.

### FR-4: Execution feedback and repair (Must)

The system shall execute generated tests after each round and feed failing test error messages back into the next prompt.

**Acceptance criterion:** When a generated test fails, the next prompt contains the error message. Verified by inspecting the prompt log for a module where a first-round test fails.

### FR-5: Test output format (Must)

The system shall write generated tests as pytest-compatible Python files.

**Acceptance criterion:** Generated tests run under `pytest` without modification. Verified by executing the generated files in a clean environment.

### FR-6: Coverage and mutation reporting (Should)

The system shall report line coverage, branch coverage, and mutation score per module, and an aggregate over the benchmark suite.

**Acceptance criterion:** The final report contains all three metrics for each module and an aggregate row. Verified by running on the CodaMosa benchmark subset and checking the report format.

### FR-7: Benchmark harness (Should)

The system shall be runnable on the CodaMosa benchmark suite and on the Pynguin benchmark, so that its coverage can be compared against published baselines.

**Acceptance criterion:** A single command runs the system on a specified benchmark subset and writes results in a CSV file with columns matching the baseline reports.

### FR-8: Configuration for LLM and budget (Should)

The system shall expose configuration for the LLM model, temperature, token budget per module, and time budget per module.

**Acceptance criterion:** Changing the model in the configuration file changes the model used in the API call, with no code change.

### FR-9: Prompt and result logging (Could)

The system shall log every prompt sent to the LLM and every response received, with timestamps and token counts.

**Acceptance criterion:** A log file exists for each run, containing one entry per prompt with the fields above.

## 4. Non-Functional Requirements

### NFR-1: Coverage improvement (Must)

The system shall achieve line+branch coverage at least 5 percentage points above CoverUp on the same benchmark subset, under the same token budget.

**Acceptance criterion:** Measured on at least 30 modules from the CodaMosa suite. Verified in the verification and evaluation plan.

### NFR-2: Measurement overhead (Must)

The coverage measurement tool shall add negligible per-execution overhead, so that the loop can run many iterations within the time budget.

**Acceptance criterion:** SlipCover is used. Measured overhead per test execution is below 10% of native execution time on a representative module.

### NFR-3: Reproducibility (Must)

The system shall produce the same results (same tests, same coverage) when run twice with the same configuration, model version, temperature, and seed.

**Acceptance criterion:** Two runs with identical configuration produce identical coverage reports. Verified by running the same module twice and comparing.

### NFR-4: Fault detection (Should)

The system shall report mutation score alongside coverage, so that test quality is assessed on both dimensions.

**Acceptance criterion:** Mutation score is included in the final report for every module where a mutation tool (e.g., MutPy) can run.

### NFR-5: Time budget (Should)

The system shall complete one module within a configurable time budget (default 10 minutes), including LLM calls and test execution.

**Acceptance criterion:** The system terminates within the budget for 90% of modules in a 30-module run. Verified by log timestamps.

### NFR-6: Cost control (Should)

The system shall report token consumption per module and enforce a configurable token budget.

**Acceptance criterion:** The system stops generation for a module when the token budget is exhausted and records the reason in the log.

### NFR-7: Maintainability (Could)

The static analysis, generation, and coverage measurement components shall be separate modules with defined interfaces, so that any one can be replaced without changing the others.

**Acceptance criterion:** Each component has a documented interface. Replacing the coverage tool requires changes only in the coverage module.

## 5. Traceability to Literature Review

| Requirement | Source in literature review |
|---|---|
| FR-1 | Section 3.4 (reactive static analysis gap); Section 6 (proactive injection) |
| FR-2, FR-6 | Section 2.2 (coverage feedback); Section 4.3 (coverage and mutation complementary) |
| FR-3, FR-4 | Section 3.2 (execution feedback); Section 3.1 (coverage loop) |
| FR-5 | Section 3.1 (CoverUp, TestWeaver output pytest) |
| FR-7 | Section 4.1, 4.2 (baselines) |
| NFR-1 | Section 6 (gap: proposed system should exceed baseline) |
| NFR-2 | Section 2.2 (feedback loop); Section 4.1 (cost of iteration) |
| NFR-4 | Section 4.3 (mutation score as complementary metric) |
| NFR-6 | Section 4.1 (LLM inference cost as justification for the floor) |

## 6. Priorities Summary

| Priority | Count | Requirements |
|---|---|---|
| Must | 6 | FR-1, FR-2, FR-3, FR-4, FR-5, NFR-1, NFR-2, NFR-3 |
| Should | 5 | FR-6, FR-7, FR-8, NFR-4, NFR-5, NFR-6 |
| Could | 2 | FR-9, NFR-7 |

If the project is under time pressure, the Must requirements define the minimum viable system. The Should requirements define the version needed for the full evaluation. The Could requirements are quality-of-life additions.