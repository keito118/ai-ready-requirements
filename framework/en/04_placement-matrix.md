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
| `APP-` | A requirement placed in the app / AI layer |
| `ONT-` | A requirement placed in the ontology layer (an object or a piece of logic) |
| `SEM-` | A requirement placed in the semantic layer (a view that implements logic) |
| `DAT-` | A requirement placed in the data layer |
| `POL-` | A cross-cutting policy requirement: who may see and do what |

> When used together with a requirements definition (such as `FR-` IDs from doc-templates), write the `FR-` number in the "Related ID" column so requirements stay traceable.

## 1. Original requests

| ID | Original request (in the stakeholder's words) | Raised by | Interview No |
|---|---|---|---|
| REQ-001 | | | |

## 2. Placement results

> Split each request by layer and write one requirement per row. Order the rows in AI processing order (APP → ONT → SEM → DAT), with POL last. Scores use the five criteria in [02 placement criteria](02_decision-criteria.md) (1–3 each) and decide between the ontology and the app layer. SEM, DAT and POL are decided by the decision flow, so their score cells can be "-".

| ID | Request | Requirement | Layer | Reuse | Impact | Change | Owner | AI use | Total | Reason | Decision owner | Related ID |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| APP-001 | REQ-001 | | App / AI | | | | | | | | | |
| ONT-001 | REQ-001 | | Ontology | | | | | | | | | |
| SEM-001 | REQ-001 | | Semantic | - | - | - | - | - | - | | | ONT- |
| DAT-001 | REQ-001 | | Data | - | - | - | - | - | - | | | |
| POL-001 | REQ-001 | | Policy | - | - | - | - | - | - | | | |

## 3. Requirements by layer

> When handing over to implementation teams, regrouping by layer makes ownership clear.

### 3.1 App / AI

| ID | Screen, feature or agent | Users | User context received | Ontology referenced | Behavior and display |
|---|---|---|---|---|---|
| APP-001 | | | Role, department | ONT- | |

### 3.2 Ontology: objects

> Required fields follow the [06 Ontology design guide](06_ontology-design.md).

| ID | Official name (noun) | Labels (synonyms) | Description | Counter-examples | What makes it unique | States and point in time | Binding | Decision owner | Status | Valid period |
|---|---|---|---|---|---|---|---|---|---|---|
| ONT-001 | | | | | | | DAT- | | Draft | YYYY-MM-DD– |

### 3.3 Ontology: logic

| ID | Name (verb) | Kind (check / calculation / relation / cause / operation) | Inputs | Output | Description | Department versions | Binding | Decision owner | Status | Valid period |
|---|---|---|---|---|---|---|---|---|---|---|
| ONT-002 | | | ONT- | ONT- | | Default only | SEM- | | Draft | YYYY-MM-DD– |

### 3.4 Semantic

| ID | View | Logic implemented | Aggregation, grain, period handling | Sources | Freshness |
|---|---|---|---|---|---|
| SEM-001 | | ONT- | | DAT- | |

### 3.5 Data

| ID | Data | Source | Ingestion frequency | Quality rules | Retention |
|---|---|---|---|---|---|
| DAT-001 | | | | | |

### 3.6 Policy

| ID | Subject | Rule | Layers enforced in | Applied to AI as |
|---|---|---|---|---|
| POL-001 | | | App / ontology / semantic / data | Inherits the user's permissions / only after a person confirms / forbidden |

## 4. Placement checklist

- [ ] Every `REQ-` is placed in at least one layer
- [ ] No meaning or rule is written in more than one place
- [ ] No `APP-` requirement holds its own business rules or formulas (it references the ontology)
- [ ] Anything planned for an AI prompt is only a reference to `ONT-` definitions
- [ ] Every check and calculation logic is bound to a `SEM-`, and every `SEM-` names the logic it implements
- [ ] Every logic with department-specific versions has a default version
- [ ] Every object with states has a point-in-time definition
- [ ] Access scope and AI permissions are defined in `POL-` and the same rules apply in all four layers
- [ ] Requirements scoring around 8–9, and requirements where the flow and score disagreed, have a written reason
- [ ] Every `ONT-` and `POL-` has a decision owner

## 5. Open issues

| No | Issue | Related ID | Owner | Due |
|---|---|---|---|---|
| 1 | | | | |
