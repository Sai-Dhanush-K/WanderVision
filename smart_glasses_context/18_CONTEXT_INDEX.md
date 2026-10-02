# Context Pack Index

Read files in this order when onboarding to the project.

## Core understanding

1. `00_PROJECT_CONTEXT.md`
2. `01_REQUIREMENTS.md`
3. `02_DOMAIN_MODEL.md`

## Architecture contracts

4. `03_STATE_SCHEMA.md`
5. `04_EVENT_LIFECYCLE.md`
6. `05_PRIORITY_RULES.md`
7. `06_SPATIAL_AND_TRACKING.md`
8. `07_CLOUD_CONTRACTS.md`
9. `08_FAILURES_AND_RELIABILITY.md`
10. `09_CONCURRENCY_AND_MODULES.md`
11. `10_CONFIG_AND_THRESHOLDS.md`

## Validation and research

12. `11_EXPERIMENT_PROTOCOL.md`
13. `12_ACCEPTANCE_TESTS.md`
14. `14_RESEARCH_CONTEXT.md`

## Architecture governance

15. `15_DECISIONS_AND_ADRS.md`
16. `16_GLOSSARY.md`

## Coding-agent guidance

17. `13_IMPLEMENTATION_PLAN.md`
18. `17_ANTIGRAVITY_INSTRUCTIONS.md`

## Suggested agent onboarding prompt

Before writing code:

1. Read `00_PROJECT_CONTEXT.md`.
2. Read `01_REQUIREMENTS.md`.
3. Read `02_DOMAIN_MODEL.md`.
4. Read `03_STATE_SCHEMA.md`.
5. Read `04_EVENT_LIFECYCLE.md`.
6. Read `05_PRIORITY_RULES.md`.
7. Read `09_CONCURRENCY_AND_MODULES.md`.
8. Read `15_DECISIONS_AND_ADRS.md`.
9. Read `17_ANTIGRAVITY_INSTRUCTIONS.md`.

Then:
- summarize the architecture
- identify any contradictions
- do not implement until contradictions are resolved
- propose the initial repository structure
- begin with domain models and tests

## Important

These documents describe the current architecture.

They are not permission to add arbitrary complexity.

If a future feature requires an architectural change:
1. explain why
2. identify which ADR changes
3. update the relevant context document
4. only then implement
