
# Spec-Kit Decomposition Approach

## Guiding Principle

Decompose specifications based on **business capabilities** and **readability**, not purely on workflow steps or technical implementation details.

The goal is to keep specifications understandable for both humans and AI while avoiding premature fragmentation.

---

## Initial Structure

Start with one specification per major business capability (phase).

Example:

```text
specs/

001-candidate-preparation/
    spec.md

002-candidate-selection/
    spec.md

003-candidate-validation/
    spec.md

004-response-assembly/
    spec.md
```

Each specification represents a cohesive business capability that owns its own business rules.

---

## Functional Steps as User Stories

Functional steps should initially be modeled as user stories inside the capability specification.

Example:

### 003-candidate-validation/spec.md

```text
User Story:
Validate Candidate Identity

User Story:
Validate Candidate Risk Profile

User Story:
Validate Compliance Rules

User Story:
Aggregate Validation Result
```

The capability spec remains the primary container for related business behavior.

---

## Product Variations

Avoid creating separate Product A, Product B, and Common specifications too early.

Instead, document variations within the relevant capability specification.

Example:

```text
Shared Rules
- Rule A
- Rule B

Product A Rules
- Product A specific behavior

Product B Rules
- Product B specific behavior
```

This prevents duplicated requirements and keeps ownership clear.

---

## When to Split a Specification

Do not decompose because it is theoretically possible.

Decompose when the specification becomes difficult to:

- Read
- Review
- Maintain
- Navigate
- Use effectively with AI tools

Typical signals include:

- Excessive document length
- Multiple unrelated business concepts
- Different ownership boundaries
- Frequent independent changes

---

## Evolution Path

### Phase 1

Start simple:

```text
Preparation
Selection
Validation
Response Assembly
```

Each capability owns its user stories.

### Phase 2

As complexity grows, extract child specifications.

Example:

```text
003-candidate-validation

    ├── Identity Validation
    ├── Risk Validation
    ├── Compliance Validation
    └── Report Aggregation
```

Possible resulting structure:

```text
003-candidate-validation

010-identity-validation

011-risk-validation

012-compliance-validation
```

The parent specification becomes the orchestration or capability-level specification.

---

## Numbering Strategy

Treat Spec-Kit numbering as a stable identifier, not workflow order.

Example:

```text
001-candidate-preparation
002-candidate-selection
003-candidate-validation
004-response-assembly
```

If a new capability is introduced later:

```text
005-candidate-enrichment
```

Do not renumber existing specifications.

Identifiers remain stable while relationships and workflows evolve.

---

## Decision Rule

Start with the largest business capability that remains understandable.

Keep functional steps as user stories inside that capability.

Only extract child specifications when readability, ownership, or maintainability requires it.

In short:

> Do not decompose because you can. Decompose when the specification becomes difficult for humans or AI to understand and maintain.
