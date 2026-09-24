---
name: sng-user-story-invest-analysis
description: 'Ingest User Stories and evaluate them using an AI-adapted INVEST.'
---

# Role
Use Product Manager agent to evaluate User Stories.

# User Story INVEST Analysis & Context Refinement

**Goal:** Evaluate User Stories against an AI-adapted INVEST framework, verify end-to-end traceability, and produce a context-engineered Markdown report optimized for autonomous agents.

---

## CRITICAL LLM INSTRUCTIONS

- **MANDATORY:** Execute ALL steps in the FLOW section IN EXACT ORDER.
- DO NOT skip steps or change the sequence.
- HALT immediately when halt-conditions or user verification points are met.
- Each action within a step is a REQUIRED action to complete that step.
- Sections outside FLOW (Validation, Guardrails, Output, Critical-Context) provide essential guidelines—review and apply throughout execution.
- **DO NOT WRITE APPLICATION IMPLEMENTATION CODE.** Output evaluations, actionable directives, and Gherkin scenarios only.
- **STRICT TRACEABILITY:** Every story MUST map to at least one `FR-x` and `AD-x`. If traceability is absent, mark as `❌ Fail`.
- **NO CONTEXT ASSUMPTIONS:** Never hallucinate architecture decisions (`AD-x`) or non-functional requirements (`NFR-x`). Explicitly log ambiguity if context is missing.

---

## INPUT SCHEMA & ARTEFACTS

Expect the following inputs or inspect the workspace for them:
- **User Stories Batch:** Markdown or text list of stories to evaluate.
- **Architecture Spine (`ARCHITECTURE-SPINE.md`):** Contains Architecture Decisions (`AD-x`), Non-Functional Requirements (`NFR-x`).
- **Product Requirement Document (`prd.md`):** Contains Functional Requirements (`FR-x`).
- **UX Specs (`DESIGN.md`):** Contains UX Design Rules (`UX-DRx`).

---

## FLOW

### Step 1: Workspace Inspection & Context Loading

**Action:** Scan the workspace to verify the presence of required BMAD project artifacts.

1. Locate and inspect the following files:
   - `./prd.md`
   - `./ARCHITECTURE-SPINE.md`
   - `./DESIGN.md`
2. Extract and index all reference keys (`FR-x`, `AD-x`, `NFR-x`, `UX-DRx`).
3. Load the target User Stories batch to be evaluated.
4. If any mandatory artifact is missing, warn the user and explicitly document which context files are unavailable before proceeding.

---

### Step 2: Traceability & INVEST Evaluation Loop

**Action:** For each User Story in the batch, perform a multi-dimensional AI-adapted INVEST audit.

1. **Traceability Check:**
   - Verify mapping to at least one `FR-x` and one `AD-x`.
   - If missing, flag as `❌ Fail` due to missing architectural grounding.
2. **INVEST Criteria Audit:**
   - **Independent:** Can a Dev Agent execute this story without unresolved cross-task dependencies?
   - **Negotiable:** States *what* and *why* without hardcoding implementation details (unless bound by an `AD-x`).
   - **Valuable:** Clear business value directly traceable to an `FR-x` or `UX-DRx`.
   - **Estimable:** Contains sufficient architectural constraints (`AD-x`, `NFR-x`) so an LLM can implement it without hallucinating context.
   - **Small (Context-Engineered):** Fits within a single LLM context window and single task execution pass.
   - **Testable:** Includes unambiguous Acceptance Criteria formatted in Gherkin syntax (`Given-When-Then`).

---

### Step 3: Gherkin Refinement & Dev Directives Generation

**Action:** Transform ambiguous or incomplete stories into context-rich execution directives for autonomous Dev Agents.

1. Formulate specific **Context Requirements** linking exact `AD-x` and `NFR-x` rules.
2. Define a **Risks & Splitting Strategy** if the story exceeds single-pass context limits.
3. Write clean, complete **Gherkin Acceptance Criteria** covering both Happy Paths and Edge/Fallback Cases.

---

### Step 4: Report Compilation & Delivery

**Action:** Write the consolidated analysis into `user-story-invest-analysis.md` using the exact layout specified below.

```markdown
# User Story Analysis & Context Engineering Report (INVEST + BMAD)

**Analysis Date:** YYYY-MM-DD  
**Evaluating Agent:** [Agent Name / Model]  
**Consulted Artifacts:** `prd.md`, `ARCHITECTURE-SPINE.md`, `DESIGN.md`  

## 1. Executive Summary & Gate Readiness
| Story ID | Short Title | INVEST Status | Traceability (FR / AD / UX) | Action Item |
| :--- | :--- | :--- | :--- | :--- |
| Story-X.X | [Title] | ✅ Approved / ⚠️ Adjust / ❌ Split | FR-x, AD-x, UX-DRx | [Required Action] |

---

## 2. Evaluation & Refinement Detail

### [Story-ID] - [Story Title]

#### Original User Story / Context
> **As a** [Role]  
> **I want to** [Capability]  
> **So that** [Business Value]  

#### INVEST Evaluation
* **Independent:** [Pass/Fail] - [Reason]
* **Negotiable:** [Pass/Fail] - [Reason]
* **Valuable:** [Pass/Fail] - [Reason]
* **Estimable:** [Pass/Fail] - [Reason]
* **Small:** [Pass/Fail] - [Reason]
* **Testable:** [Pass/Fail] - [Reason]

#### Directives for the Dev Agent
* **Context Requirements:** [Relevant Architecture AD-x / NFR-x]
* **Risks & Splitting:** [Splitting strategy if exceeding context window limits]

#### Optimized Proposal (Gherkin Format)
```gherkin
Feature: [Feature Name]
  Traceability: FR-x, AD-x, UX-DRx

  Scenario: [Main Happy Path]
    Given [Initial system state]
    When [User action or event]
    Then [Expected result]
    And [Performance or security invariant (NFR-x)]

  Scenario: [Edge Case or Fallback]
    Given [Failure or offline condition]
    When [Execution attempt]
    Then [Error message or degraded behavior]