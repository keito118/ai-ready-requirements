# 01. The four layers

> Defines the four layers that requirements are placed into, and what each layer owns and does not own. When in doubt, come back to this table and ask "is this really this layer's responsibility?"

## Overview

```mermaid
flowchart TB
  APP["App / AI layer<br/>presentation, interaction, prompts, agent behavior"]
  SEM["Semantic layer<br/>metric definitions, dimensions, grain"]
  ONT["Ontology layer<br/>business concepts, relationships, states, rules"]
  DAT["Data layer<br/>sources, ingestion, quality, freshness, retention"]
  APP --> SEM --> ONT
  ONT -. stored as .-> DAT
  SEM -. computed from .-> DAT
  POL["Policy (cross-cutting)<br/>who may see what"] -. applied on every path .-> DAT
```

**Policy** (who may see what) is a cross-cutting rule, separate from the four layers. Some processing, such as a single-record lookup, never passes through the semantic layer, so access is not placed in any one layer; it is applied on every path that touches data.

Upper layers only *use* the layers below them; they never redefine what those layers mean. Meaning and rules go into **the lowest layer that everyone who needs them can reach**.

## Layer definitions

| Layer | In one phrase | Owns | Does not own | Typical implementation |
|---|---|---|---|---|
| Ontology | The business's things, relationships and rules | Entities (customer, contract, product), relationships (a customer has many contracts), states and transitions (contract: applied → active → cancelled), business rules (cancellations close at month end), the official meaning of terms | Calculation formulas, display labels, quirks of source systems | Conceptual model, knowledge graph, domain model, glossary, master data definitions |
| Semantic | The definition of the numbers | Metric formulas (revenue, churn rate), dimensions, grain, period definitions (fiscal or calendar month) | The definition of business concepts themselves, screen layout | Metrics layer, dbt semantic models, BI data models, metric catalog |
| Data | Where data comes from and how good it is | Source systems, ingestion method and frequency, data quality rules (missing, duplicate, inconsistent), history handling, retention, lineage | The meaning of metrics, business rule decisions | Data warehouse / lake, ETL / ELT, data quality checks, data catalog |
| App / AI | Who sees what, how, and what it does | Screens, reports, notifications, workflows, AI prompts and tools, agent permissions, UX defaults and sort orders | Business rules and metric definitions (if written here they drift from other consumers) | Web apps, BI dashboards, chat UIs, AI agents |

## Why this matters more with AI

- **AI believes what it is told.** If "churn rate" is defined once in a dashboard and again in a prompt, a person can reconcile them from context; an AI cannot, and the answers diverge.
- **Cheaper implementation multiplies misplacement.** When apps are fast to build, copies of the same rule spread into screens, prompts and batch jobs.
- **The lower layers become the shared language for AI.** With a solid ontology and semantic layer, each new agent or dashboard does not have to redefine the business from scratch.

## Three tests for the boundaries

| Test | If yes | If no | Example |
|---|---|---|---|
| T1: Can it be answered for a **single** record? | Ontology | Go to T2 | "Is this member active?" → ontology |
| T2: Is it a calculation or aggregation over a **set**? | Semantic | Go to T3 | "Active rate = active members ÷ enrolled members" → semantic |
| T3: Would it **change** if the system were replaced? | Data | Go to T4 | "Check-ins are in the entry system's checkin table" → data |
| T4: Is it about who may **see** what? | Policy (cross-cutting) | App / AI layer | "Location managers see only their own location's members" → policy |

T1 separates **classifications that can be decided one record at a time (ontology)** from **the result of counting them (semantic)**. The semantic layer then never invents new concepts; it only counts the ontology's concepts.

## Contracts between layers

Even with the layers separated, AI gets lost if the joins are undefined. Require these references between layers:

| Join | Required reference | Symptom when missing |
|---|---|---|
| Semantic → ontology | Every metric names the concept ID it counts, and the state and rule IDs it uses | Metrics carry their own embedded conditions and duplicate the concept's definition |
| Data → ontology | Every entity is mapped to the source ID that makes it unique (the matching key) | The same member is counted as two people |
| Data → semantic | Every metric is mapped to its source tables and columns, and their freshness (as of when) | AI answers "this month" with stale data |

For how to break the ontology layer down further, see [06 Ontology design guide](06_ontology-design.md).

## Common boundary cases

| Case | Layer | Why |
|---|---|---|
| What counts as an "active customer" | Ontology | It is the definition of a state and must mean the same thing in every department |
| Whether dormant customers are in the denominator of "activation rate" | Semantic | It is about how the metric is calculated |
| Contract data only reflects up to the previous day | Data | It is about freshness; the metric definition does not change |
| The churn rate threshold at which a dashboard turns red | App / AI | It is presentation. If the threshold is also a business standard (for example the alert line for board meetings), move it to the semantic layer |
| Which customer data an AI may use in its answers | Policy (cross-cutting) + App / AI (tool configuration) | Who may see what is decided in the policy; the agent inherits the permissions of the person using it |
