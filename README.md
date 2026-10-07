## Title
Development of an Al Agent for Automated Code Coverage Gap Detection and Unit Test

## How it works

1. Run the unit tests and collect the coverage report
2. Find uncovered lines and branches
3. Analyze the code around each gap with static analysis tools
4. Ask the LLM to write a test for that gap
5. Run the new tests and check the coverage again

## Links

- YouTrack: [link]
- Documentation: `docs/`
