# Project Proposal

## Problem Statement
Developer rely on code coverage metrics to idenitfy untested lines.However, identifying these gaps is slow, manual, and error-prone.

## Target Users
Python developers who need to raise test coverage 

## Project Aim
Build an Agentic AI that automatically finds uncovered code and generates unit tests to cover it.

## Objectives
1. Run existing tests and parse coverage reports to find gaps
2. Use static analysis to understand code context around each gap
3. Use an LLM to generate tests for each gap
4. Evaluate whether the generated tests improve coverage
5. Iterate until coverage stops improving

## Expected Results
A working agent (CLI tool) that runs iterative loop (coverage analysis, static
analysis, LLM test generation, test execution). 

## Project Scope

in:
- python projects
- unit tests only
- cli interface (command-line tool)
- coverage metrics (line and branch)

out:
- Integration tests or end-to-end tests
- Java or other languages
- IDE plugins (future work)
- GUI interface
- Cost optimization




