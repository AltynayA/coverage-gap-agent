# 2. Evidence of Relevance

## 2.1 Approach

To validate the project's direction, preliminary discussions were held with three software developers who responded to postings in Astana Hub and Telegram. All participants have between less than one and three years of experience writing unit tests. The full anonymized transcripts of these discussions are available in Appendix A (`appendix-a-feedback.md`). 

## 2.2 Key Findings

*   **The Coverage Blocker:** Participants identified "not enough time," "hard to construct inputs that reach specific branches," and "complex conditional logic" as the main obstacles to higher coverage.
*   **Hardest Code:** Asynchronous code and external API integrations were cited as the hardest areas to test.
*   **Trust:** All three participants require tests to have "meaningful assertions" to trust them. One explicitly warned: *"Generated tests should check meaningful behavior and edge cases, rather than just increase coverage numbers."*
*   **Weak Assertions:** Two of the three participants admitted they have had tests pass without actually checking anything useful, and they "rarely" or only "sometimes" notice when an assertion is weak.

## 2.3 Implications for the Project

These practitioner findings form a baseline for the project direction:

1.  **Focus on complex branches:** Since developers struggle to manually construct inputs for complex conditions and asynchronous code, the proposed tool should focus on automatically analyzing these specific code structures.
2.  **Go beyond coverage numbers:** Developers reported that they miss weak assertions and want tests to check meaningful behavior, the tool must evaluate test quality beyond code coverage. This is a confirmation that measuring whether the generated tests actually catch faults (using mutation testing), not just whether they execute the code is neccessary. 

## 2.4 Verified Findings and Assumptions

| Finding | Status | Source |
| :--- | :--- | :--- |
| Complex branches are a primary obstacle to coverage. | **Verified** | Discussions (n=3) |
| Developers require meaningful assertions to trust tests. | **Verified** | Discussions (n=3) |
| Developers often miss weak assertions in tests. | **Verified** | Discussions (n=3) |
| Automatically extracting branch constraints will reduce time spent writing tests. | **Assumption** | To be tested in evaluation |
| Developers will prefer generated tests over manual writing. | **Assumption** | To be tested in evaluation |

## 2.5 Limitations

The sample size (n=3) is small and represents entry-level or junior developers from professional and student networks. The results are used to validate the direction of the project and identify workflow bottlenecks. The project relies on the academic literature (Section 3) to establish the broader research gap.