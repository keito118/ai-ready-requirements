# 04. Placement matrix: {System name}

> How to use this template: replace every `{ }`, and delete the guidance lines that start with `>` in the final version. See [05 worked example](05_worked-example.md) for a filled-in version.

## Document control

| Item | Value |
|---|---|
| Document ID | {PRJ}-LP-001 |
| Version | 0.1 |
| Author | {Name} |
| Status | Draft / In review / Approved |

## ID prefixes

| Prefix | Meaning |
|---|---|
| `REQ-` | An original request from an interview (one per request) |
| `ONT-` | A requirement placed in the ontology layer |
| `SEM-` | A requirement placed in the semantic layer |
| `DAT-` | A requirement placed in the data layer |
| `APP-` | A requirement placed in the app / AI layer |

> When used together with a requirements definition (such as `FR-` IDs from doc-templates), write the `FR-` number in the "Related ID" column so requirements stay traceable.

## 1. Original requests

| ID | Original request (in the stakeholder's words) | Raised by | Interview No |
|---|---|---|---|
| REQ-001 | | | |

## 2. Placement results

> Split each request by layer and write one requirement per row. Scores use the five criteria in [02 placement criteria](02_decision-criteria.md) (1–3 each). The data layer is decided by Q3 of the flow rather than by score, so its score cells can be "-".

| ID | Request | Requirement | Layer | Reuse | Impact | Change | Owner | AI use | Total | Reason | Decision owner | Related ID |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| ONT-001 | REQ-001 | | Ontology | | | | | | | | | |
| SEM-001 | REQ-001 | | Semantic | | | | | | | | | |
| DAT-001 | REQ-001 | | Data | - | - | - | - | - | - | | | |
| APP-001 | REQ-001 | | App / AI | | | | | | | | | |

## 3. Requirements by layer

> When handing over to implementation teams, regrouping by layer makes ownership clear.

### 3.1 Ontology

> Elements, scope and required fields follow the [06 Ontology design guide](06_ontology-design.md). Order the rows by element (VOC → STR → STA → RUL → ACT), so that each row only references rows above it.

| ID | Element (VOC / STR / STA / RUL / ACT) | Scope (core / domain name) | Official name | Definition | Counter-examples (not included) | Point in time (STA) | AI permission (ACT) | References | Decision owner | Status | Valid period |
|---|---|---|---|---|---|---|---|---|---|---|---|
| ONT-001 | | | | | | | | | | Draft | YYYY-MM-DD– |

### 3.2 Semantic

| ID | Kind (metric / dimension / period / access) | Definition or formula | Ontology referenced | Decision owner |
|---|---|---|---|---|
| SEM-001 | | | ONT- | |

### 3.3 Data

| ID | Data | Source | Ingestion frequency | Quality rules | Retention |
|---|---|---|---|---|---|
| DAT-001 | | | | | |

### 3.4 App / AI

| ID | Screen, feature or agent | Users | Definitions referenced | Behavior and display |
|---|---|---|---|---|
| APP-001 | | | SEM- / ONT- | |

## 4. Placement checklist

- [ ] Every `REQ-` is placed in at least one layer
- [ ] No definition or rule is written in more than one layer
- [ ] No `APP-` requirement holds its own business rules or metric formulas (it references them)
- [ ] Anything planned for an AI prompt is only a reference to `ONT-` / `SEM-` definitions
- [ ] Access rules are defined in `SEM-` and apply equally through AI agents
- [ ] Requirements scoring around 8–9, and requirements where the flow and score disagreed, have a written reason
- [ ] Every `ONT-` and `SEM-` has a decision owner
- [ ] Each `ONT-` only references earlier elements (no references in the other direction)
- [ ] Every `SEM-` references the `ONT-` it counts
- [ ] States (STA) have a point-in-time definition, and actions (ACT) have an AI permission

## 5. Open issues

| No | Issue | Related ID | Owner | Due |
|---|---|---|---|---|
| 1 | | | | |
