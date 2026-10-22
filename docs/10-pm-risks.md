# 10. Project Management and Risks

## 10.1 Repository and Tracking

- **Repository:** [GitHub link](https://github.com/AltynayA/coverage-gap-agent)
- **YouTrack:** [YouTrack link](https://altynay-ayazbayeva.youtrack.cloud/projects/CGA)
- **Supervisor:** Praveen Kumar
- **Author:** [Altynay Ayazbayeva](https://github.com/AltynayA) (individual project)

Each YouTrack task records the owner, expected outcome, acceptance criteria, deadline, and priority (Must, Should, Could). Commit messages include the task ID (e.g., `CGA-3: Complete literature review`) to ensure traceability between the repository history and the project board. The `main` branch serves as the reviewed baseline.

## 10.2 Responsibilities

The author is responsible for all areas: documentation, design, implementation, evaluation, and the VS Code extension. The supervisor reviews significant changes at agreed checkpoints. Although this is an individual project without a second developer for code reviews, every commit and pull request states what changed, which task it closes, and how it was checked to ensure a traceable history.

## 10.3 Milestones

| Milestone | Target Date | Content |
|---|---|---|
| Pre-defense 1 | 24 Oct 2026 | Documentation (Sections 1-11), prototype mockups, YouTrack link |
| Core Agent | 15 Dec 2026 | Static analyzer, prompt builder, SlipCover integration, and iterative loop working on sample modules |
| Pre-defense 2 | 20 Jan 2027 | Benchmark results on CodaMosa subset, mutation reporting, CLI implementation |
| VS Code MVP | 15 Mar 2027 | Gap display, agent integration, test review diff view |
| Final Defense | 15 May 2027 | Complete system, full evaluation, final report |

Each submission is tagged (`pre-defense-1`, `pre-defense-2`, `final-pre-defense`) and includes the repository link, a summary of completed work, known limitations, and outstanding tasks. Tags are never moved after submission.

## 10.4 Task Breakdown (YouTrack)

| ID | Task | Priority | Maps to |
|---|---|---|---|
| CGA-1 | Project proposal & Scope | Must | Section 1 |
| CGA-2 | Evidence of relevance (Practitioner feedback) | Must | Section 2 |
| CGA-3 | Literature review & summary table | Must | Section 3 |
| CGA-4 | Existing systems comparison | Must | Section 4 |
| CGA-5 | Technology comparison | Must | Section 5 |
| CGA-6 | Requirements specification | Must | Section 6 |
| CGA-7 | Architecture & diagrams | Must | Section 7 |
| CGA-8 | UX / prototype mockups | Must | Section 8 |
| CGA-9 | Verification and evaluation plan | Must | Section 9 |
| CGA-10 | Project management and risks | Must | Section 10 |
| CGA-11 | Supervisor feedback log | Must | Section 11 |
| CGA-12 | Static analyzer for branch predicates | Must | FR-1 |
| CGA-13 | SlipCover integration | Must | FR-2, NFR-2 |
| CGA-14 | Generate-execute-measure loop | Must | FR-3, FR-4 |
| CGA-15 | pytest-compatible output | Must | FR-5 |
| CGA-16 | Evaluation harness on CodaMosa subset | Should | FR-7 |
| CGA-17 | Mutation reporting (MutPy) | Should | FR-6, NFR-4 |
| CGA-18 | Token and time budget enforcement | Should | FR-8, NFR-5, NFR-6 |
| CGA-19 | Command-line interface | Must | Section 1.6 |
| CGA-20 | VS Code extension MVP | Must | Section 1.6 |
| CGA-21 | Baselines under identical conditions | Must | Section 9.4 |

## 10.5 Risks

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| **LLM API cost exceeds budget** | Medium | High | Per-module token budget (NFR-6); cache responses; use cheaper models during development. |
| **SlipCover incompatible with required Python version** | Low | High | Pin Python version early; fall back to Coverage.py with documented justification if necessary. |
| **Mutation testing too slow** | High | Medium | Sample mutants; run mutation testing only on passing test suites. |
| **Non-deterministic output hurts reproducibility** | High | Medium | Fixed seed and temperature; repeat runs and report variance. |
| **Baselines hard to configure identically** | Medium | High | Start baseline setup early; document exact configuration (Section 9.3). |
| **Time pressure before final defense** | Medium | High | Cut scope using Must/Should/Could priorities defined in Section 6.6. |
| **Delayed supervisor feedback** | Medium | Medium | Send deliverables early, list outstanding feedback in the submission summary. |