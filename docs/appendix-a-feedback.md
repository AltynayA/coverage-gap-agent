# Appendix A: Practitioner Discussion Transcripts

This appendix contains the anonymized raw responses from the three preliminary practitioner discussions referenced in Section 2. These transcripts are provided to verify the quotes and findings presented in the main report.

## Participant 1
*   **Role:** Software developer
*   **Experience:** 1–3 years writing/reviewing unit tests
*   **Primary Languages:** Python, Java, JavaScript/TypeScript, Go, Rust
*   **Current Practice:** "Core logic project (business logic)."
*   **Coverage Blocker:** "Not enough time."
*   **Hardest code to cover:** "Code with complex logic, multiple conditions, and external API calls."
*   **Trust in generated tests:** "They compile and pass, They include meaningful assertions."
*   **When asked about weak assertions:** "Yes, a test passed because it only checked if the code ran without errors, not if the result was correct. I would notice this rarely."

## Participant 2
*   **Role:** Software developer
*   **Experience:** Less than 1 year writing/reviewing unit tests
*   **Primary Languages:** Python, JavaScript/TypeScript
*   **Current Practice:** "The most vulnerable to bugs, and errors."
*   **Coverage Blocker:** "Hard to mock or set up dependencies, Tests break when the code is refactored."
*   **Hardest code to cover:** "Vulnerable [code]."
*   **Trust in generated tests:** "They increase coverage, They detect a known bug when the code is broken, They are readable and follow project style."
*   **When asked about weak assertions:** "Dead code wrote Claude code. I would notice this rarely."

## Participant 3
*   **Role:** Software developer
*   **Experience:** 1–3 years writing/reviewing unit tests
*   **Primary Languages:** Python, JavaScript/TypeScript
*   **Current Practice:** "I prioritize complex logic, critical features, edge cases, and code with past bugs. I ask: if this breaks, how serious is it—and would existing tests catch it?"
*   **Coverage Blocker:** "Hard to construct inputs that reach specific branches, Not knowing which parts are uncovered, Tests break when the code is refactored, No specific target, so coverage is not a priority."
*   **Hardest code to cover:** "Asynchronous code, external API integrations, complex conditional logic."
*   **Trust in generated tests:** "They compile and pass, They include meaningful assertions, They detect a known bug when the code is broken, They are readable and follow project style, A colleague reviewed them."
*   **When asked about weak assertions:** "Yes, a test passed because the code ran without errors, but it never checked the result. I added a meaningful assertion. I would notice this sometimes."
*   **Additional comment:** "Generated tests should check meaningful behavior and edge cases, rather than just increase coverage numbers."