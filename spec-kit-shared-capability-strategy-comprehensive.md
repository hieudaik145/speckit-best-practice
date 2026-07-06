# Spec-Kit Shared Capability Strategy
## Comprehensive Architecture Reference

# 1. Purpose

This document defines how shared business logic should be identified, evaluated, extracted, and organized within a Spec-Kit repository.

Unlike a traditional architecture guideline, this document is specifically optimized for Spec-Kit where every specification generates downstream artifacts:

- spec.md
- plan.md
- tasks.md
- implementation
- tests

The goal is to prevent:

- excessive duplication
- excessive fragmentation
- common/shared dumping grounds
- unreadable specifications
- unstable repository structures

while maintaining specifications that remain understandable by both humans and AI.

---

# 2. Why Spec-Kit Changes The Architecture Trade-Off

## Traditional Requirements

In traditional projects, duplicated requirements are annoying but often manageable.

Example:

Product A Requirements

- Candidate must be active
- Candidate age >= 18

Product B Requirements

- Candidate must be active
- Candidate age >= 18

The duplication exists only in documentation.

---

## Spec-Kit Requirements

In Spec-Kit, duplication propagates.

Example:

Product A Spec
    -> Plan A
    -> Tasks A
    -> Code A
    -> Tests A

Product B Spec
    -> Plan B
    -> Tasks B
    -> Code B
    -> Tests B

The same requirement may be maintained in multiple places.

This means the extraction threshold is lower than in traditional architecture.

---

# 3. Architectural Vocabulary

The most important lesson from our analysis is:

Shared Logic != Shared Capability

To make decisions consistently, we need four levels.

---

## Level 1: Business Rule

Definition:

A single atomic business constraint.

Examples:

- Candidate must be active
- Age must be >= 18
- Remove dashes from DLN
- Convert DLN to uppercase

Characteristics:

- One condition
- One validation
- One acceptance criterion
- Minimal complexity

Example:

Rule:

"Candidate must be active"

Acceptance Criteria:

- Active -> pass
- Inactive -> fail

Decision:

Never create a dedicated specification.

Keep inside a parent specification.

---

## Level 2: Business Rule Set

Definition:

A cohesive collection of related business rules solving one narrow concern.

Example:

DLN Normalization

Rules:

1. Trim spaces
2. Remove dashes
3. Remove special characters
4. Convert to uppercase
5. Validate prefix
6. Validate length
7. Validate country format
8. Remove duplicate separators
9. Generate normalized value

Characteristics:

- Multiple rules
- Single responsibility
- Usually supports a larger capability
- Usually not directly visible to business stakeholders

Business users do not typically ask for:

"DLN Normalization"

Instead they ask for:

"Candidate Enrichment"

Decision:

May remain inside a capability specification.

May be extracted if complexity and reuse justify it.

---

## Level 3: Shared Domain Service

Definition:

A reusable rule set with enough complexity and reuse to justify its own lifecycle.

Examples:

- DLN Normalization
- Address Standardization
- Identity Matching
- Fraud Scoring
- Candidate Eligibility

Characteristics:

- Multiple consumers
- Multiple rules
- Shared implementation
- Shared tests
- Evolves independently

Example:

Product A Candidate Enrichment
    -> DLN Normalization

Product B Candidate Enrichment
    -> DLN Normalization

Decision:

Often deserves its own specification.

---

## Level 4: Business Capability

Definition:

A business-recognizable outcome that delivers value to stakeholders.

Examples:

- Candidate Preparation
- Candidate Selection
- Candidate Validation
- Candidate Enrichment
- Report Aggregation

Characteristics:

- Business ownership
- End-to-end outcome
- Multiple workflows
- Multiple acceptance criteria
- Appears in process diagrams

Decision:

Always deserves its own specification.

---

# 4. Decision Framework

When a new requirement appears:

Question 1:

Can it be described as a single rule?

Examples:

- Candidate must be active
- Remove dashes

If YES:

Keep inside parent specification.

STOP.

---

Question 2:

Does it contain multiple related business rules?

Example:

DLN Normalization

If NO:

Keep inside parent specification.

STOP.

---

Question 3:

Is it reused by multiple products or capabilities?

Example:

Product A -> DLN Normalization

Product B -> DLN Normalization

If NO:

Keep inside parent capability.

STOP.

---

Question 4:

Would it generate meaningful:

- plan.md
- tasks.md
- implementation
- tests

If NO:

Keep inside parent capability.

STOP.

---

Question 5:

Can it evolve independently?

Example:

Today:

DLN Rule Set Version 1

Future:

DLN Rule Set Version 2

without changing Product A or Product B.

If YES:

Create Shared Domain Service Specification.

---

Question 6:

Does it deliver a stakeholder-recognizable outcome?

Example:

Candidate Validation

If YES:

Create Business Capability Specification.

---

# 5. Shared Capability Extraction Strategy

## Do Not Extract Because Logic Is Shared

Bad reasoning:

"This rule appears in Product A and Product B."

That alone is not enough.

---

## Extract Because The Capability Deserves Ownership

Good reasoning:

"DLN Normalization contains 9 business rules, has multiple consumers, and generates meaningful plans/tasks/tests."

This justifies extraction.

---

# 6. Repository Organization Strategy

## Recommended Structure

specs/

001-candidate-preparation

002-candidate-selection

003-candidate-validation

004-response-assembly

020-dln-normalization

021-candidate-eligibility

022-identity-validation

023-report-aggregation

---

Explanation:

001-004

Business capabilities.

020+

Shared domain services.

---

# 7. Naming Strategy

## Do Not Name By Usage

Bad:

020-shared-dln-normalization

021-shared-identity-validation

Problem:

"shared" describes consumers.

It does not describe responsibility.

---

## Name By Responsibility

Good:

020-dln-normalization

021-identity-validation

022-candidate-eligibility

Reason:

The name remains valid regardless of how many consumers exist.

---

# 8. Anti-Patterns

## Anti-Pattern 1: Common Shared Specification

Example:

003-common-shared

Containing:

- Validation
- Eligibility
- Aggregation
- Notifications

Problem:

Becomes a dumping ground.

Rejected.

---

## Anti-Pattern 2: Product-First Structure

Example:

product-a/

product-b/

common/

Problem:

Creates duplicated requirements and duplicated ownership.

Rejected.

---

## Anti-Pattern 3: Micro Specifications

Example:

020-remove-dashes

021-uppercase-dln

022-trim-spaces

Problem:

Artificial boundaries.

Too many files.

Rejected.

---

## Anti-Pattern 4: Duplicate Everything

Problem:

Spec duplication becomes:

Plan duplication
Task duplication
Code duplication
Test duplication

Rejected.

---

# 9. Worked Examples

## Example A: Candidate Eligibility

Contains:

- Active validation
- Age validation
- Source validation
- Status validation
- Blacklist validation

Used By:

- Product A
- Product B

Decision:

Create:

021-candidate-eligibility

Reason:

Multiple rules.
Multiple consumers.
Meaningful lifecycle.

---

## Example B: Identity Validation

Today:

Provider A

Future:

Provider B
Provider C

Used By:

- Candidate Validation
- Candidate Enrichment

Decision:

Create:

022-identity-validation

Reason:

Independent evolution.

---

## Example C: DLN Normalization

Contains:

9 normalization rules.

Used By:

- Product A Candidate Enrichment
- Product B Candidate Enrichment

Not a business capability.

But:

- Multiple rules
- Multiple consumers
- Shared implementation
- Shared tests

Decision:

Create:

020-dln-normalization

Classification:

Shared Domain Service.

---

## Example D: Remove Dashes

Rule:

Remove '-' from DLN.

Decision:

Do NOT create:

020-remove-dashes

Reason:

Single rule.

Belongs inside DLN Normalization.

---

# 10. Evolution Strategy

Stage 1

Start with capability specifications.

Example:

001-candidate-preparation
002-candidate-selection
003-candidate-validation
004-response-assembly

---

Stage 2

As shared logic grows:

Extract shared domain services.

Example:

020-dln-normalization
021-candidate-eligibility

---

Stage 3

As products diverge:

Introduce product-specific capability specifications only when business behavior genuinely differs.

---

# 11. Final Principles

1. Organize around business capabilities.

2. Numbering is an identifier, not workflow order.

3. Avoid generic common/shared specifications.

4. Avoid micro specifications.

5. Shared logic does not automatically become a shared specification.

6. Shared domain services are a valid middle layer between rules and capabilities.

7. Name specifications by responsibility, not by usage.

8. Extract earlier than traditional architecture because Spec-Kit amplifies duplication.

9. Optimize for long-term readability for both humans and AI.

10. The goal is not maximum DRY.

The goal is sustainable specification architecture.
