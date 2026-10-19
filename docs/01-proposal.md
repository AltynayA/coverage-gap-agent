# Project Proposal

## Project Title
Development of an Al Agent for Automated Code Coverage Gap Detection and Unit Test Generation

## Problem Statement
Developer rely on code coverage metrics to idenitfy untested lines.However, identifying these gaps is slow, manual, and error-prone.

## Target Users
Python developers who need to raise test coverage 

## Project Aim
Build an Agentic AI tool that automatically finds uncovered code and generates unit tests to cover it.

## Objectives
1. Run existing tests and parse coverage reports to find gaps
2. Use static analysis to understand code context around each gap
3. Use an LLM to generate tests for each gap
4. Evaluate whether the generated tests improve coverage
5. Iterate until coverage stops improving

## Expected Results
(a) A Python agent core with a command-line interface that runs the iterative loop.

(b) A VS Code extension (MVP) that displays gaps in the editor, runs the agent, and shows generated tests for review.

 (c) An evaluation comparing the agent with baselines on benchmark projects 

## Project Scope

| In | Out|
| :---     | :---:    | 
| Python projects     | Integration tests or end-to-end tests   | 
| Unit tests only     | IDE plugins (future work)    | 
| Agent core as a Python package with a CLI | Chat-based assistant |
| VS Code extension, MVP feature set     | GUI interface   | 
| Line and branch poverage metrics    | Cost optimization   | 




