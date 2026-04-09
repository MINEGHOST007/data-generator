# Generic Synthetic Data Generation for Decisioning Strategies

- **Campaign**: Codeathon
- **Max Team Size**: 5
- **Registration**: 3/31/2026 - 4/6/2026
- **Challenge**: 4/10/2026 12:00 AM - 4/25/2026 5:00 AM

---

## Problem Statement

**Primary Value** — Addresses the enterprise challenge of validating complex credit decisioning and policy logic without reliance on sensitive production data, which limits test coverage, slows change, and increases operational and regulatory risk. This challenge focuses on enabling safe, repeatable, and comprehensive testing of decisioning behavior across normal, edge, and failure scenarios.

**Expected Role of AI** — AI is expected to augment synthetic data modeling and scenario exploration, not introduce nondeterminism into credit validation. Appropriate use of AI includes:
- Assisting in generating precise, interdependent combinations of customer/Account/Rules attributes
- Supporting systematic exploration of rule boundaries, edge cases, and rare scenarios
- Improving coverage and efficiency compared to manual test data construction

AI generated data must be controlled, explainable, and compliant with data and risk guardrails.

---

## What This Project Actually Is

This is not just "generate fake data." It is a **policy-aware synthetic test-data factory** for decisioning systems. The system should generate **deterministic, explainable, coverage-driven synthetic scenarios** so teams can validate decision logic without production PII. That is exactly the space served by vendors like Tonic.ai, MOSTLY AI, and Gretel, which focus on privacy-safe, high-fidelity, structured synthetic data for software testing, analytics, and AI workflows. [tonic.ai], [mostly.ai], [docs.gretel.ai]

---

## What the Input Should Be

The challenge should specify inputs such as:

- Data schema / entity model (customer, account, product, bureau, score fields)
- Rule definitions / policy constraints / scorecards
- Desired test coverage goals (normal, edge, negative, rare cases)
- Optional seed/sample masked data for fidelity
- Distribution constraints or fairness/compliance constraints

MOSTLY AI explicitly distinguishes **AI-generated synthetic data** from simple rule-based mock data, and its SDK supports training generators, probing, conditional generation, and quality/privacy reports. Gretel also supports tabular, conditional, and privacy-preserving workflows. [mostly.ai], [mostly-ai.github.io], [trainer.do...gretel.ai], [docs.gretel.ai]

---

## What the Output Should Be

A well-defined output set would be:

- Synthetic datasets (CSV/Parquet/JSON)
- Scenario manifest explaining why each record exists
- Rule coverage report
- Boundary-condition dataset
- Invalid/conflicting combination report
- Data quality/privacy scorecard

Tonic's positioning is especially relevant here: it emphasizes realistic, referentially intact test data, de-identification, generation from scratch, and even mock APIs for development/testing workflows. [tonic.ai]

---

## What Good Looks Like

- **Clear architecture and data flow**
  - Explicit separation between rule constraints, data models, and generation logic
  - Clear inputs (Scenarios, rules, constraints) and outputs (synthetic datasets, scenarios)

- **Deterministic behavior and explainability**
  - Predictable data generation aligned to defined rules and scenarios
  - Ability to explain why specific attribute combinations were generated
  - Clear linkage between generated data and the rules being validated

- **Tests, failure handling, and observability**
  - Automated validation of rule coverage, boundary conditions, and negative paths
  - Detection of invalid, conflicting, or incomplete rule combinations
  - Visibility into generation outcomes, coverage gaps, and failures

- **Reuse or extensibility potential**
  - Synthetic data framework reusable across multiple decisioning systems
  - Ability to onboard new rule sets, policies, or domains with minimal changes
  - Design suitable for integration into CI/CD and automated testing pipelines

---

## Closest Market Leaders / Reference Products

- **Tonic.ai** — strong for test data generation, de-identification, and realistic relational data for software QA and development. [tonic.ai]
- **MOSTLY AI** — strong for structured/tabular synthetic data, conditional generation, simulations, and privacy-safe sharing. [mostly.ai], [mostly.ai], [mostly.ai]
- **Gretel** — strong for synthetic tabular/text/time-series data, privacy controls, conditional generation, and quality/privacy reports. [trainer.do...gretel.ai], [docs.gretel.ai], [synthetics...gretel.ai]

---

## Better One-Line Rewrite

Build a deterministic synthetic-data engine that takes credit schemas, decision rules, and coverage goals as input and produces privacy-safe, explainable test datasets plus coverage and validation reports.
