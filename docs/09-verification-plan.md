# 9. Verification and Evaluation Plan

## 9.1 Verification of Requirements

The system will be tested to ensure it meets the functional and non-functional requirements defined in Section 6. The primary verification method is checking the acceptance criteria for each requirement, supplemented by unit tests.

| Requirement | Verification | Pass condition |
|---|---|---|
| FR-1 Proactive analysis | Inspect the initial prompt for three sample modules | Every branch predicate appears in the first prompt |
| FR-2 Coverage measurement | Compare with a manual `coverage.py` run | Line and branch percentages match |
| FR-3 Iterative loop | Log inspection on a module below 100% after round one | At least two iterations, stops on a defined condition |
| FR-4 Execution feedback | Inspect the next prompt after a failing round-one test | Error message is included |
| FR-5 pytest output | Run generated files in a clean environment | Runs under `pytest` without edits |
| NFR-2 Overhead | Time the suite with and without SlipCover | Overhead below 10% |
| NFR-3 Reproducibility | Run the same configuration twice | Identical tests and coverage |

Unit tests cover the static analyzer (on hand-written modules with known constraints), the prompt builder, the coverage delta calculation, and each stopping condition.

## 9.2 Outcome Metrics

- **Line and branch coverage** (primary, per NFR-1)
- **Mutation score** via MutPy (secondary, per NFR-4), since coverage alone overstates test quality
- **First-round coverage**, which isolates the effect of proactive analysis
- **Tokens and time per module**, for cost and time budgets (NFR-5, NFR-6)

## 9.3 Measurement Conditions

Every run records the model version, temperature, seed, per-module token and time budgets (10 minutes by default), the stopping condition (100% coverage, no improvement for three iterations, or budget exhausted), the Python and library versions, and the benchmark module list. The prompting strategy is held fixed, as established in Section 3.2.1. Non-deterministic runs are repeated at least three times and reported as median and range.

## 9.4 Benchmark and Baselines

- **Benchmark:** At least 30 modules from the CodaMosa suite (NFR-1), plus the Pynguin benchmark for comparison (FR-7).
- **CoverUp:** The direct comparison for NFR-1, since it uses the same coverage loop with reactive static analysis.
- **Pynguin (DynaMOSA):** The search-based coverage floor (Section 3.4.1).
- **Ablation:** The proposed system with proactive analysis disabled. This tests the central claim directly.

Baselines are run under the exact same conditions. Published numbers are not used, since they were measured under different budgets and tooling.

## 9.5 Success Criteria

| Criterion | Target |
|---|---|
| Line+branch coverage vs. CoverUp (NFR-1) | At least 5 percentage points higher, same token budget |
| Coverage vs. Pynguin | Higher on the same modules |
| First-round coverage vs. ablation | Higher with proactive analysis enabled |
| Reproducibility (NFR-3) | Identical outputs for identical configurations |

Results that miss a target are reported with an explanation.

## 9.6 Limitations

The benchmark consists of published modules, so results may not generalize to industrial codebases. LLM output depends on the model version used. Mutant sampling may be needed for large modules, and the evaluation covers unit tests only.