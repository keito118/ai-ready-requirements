# 03. Interview guide

> Questions for interviewing business stakeholders. As AI speeds up implementation, more of the requirements effort shifts into these interviews. The questions are ordered so that asking each layer's set gives you what you need to place requirements.
>
> How to use: start with "0. Overall", then dig into sections 1–4 for each business term or number that comes up. Copy the answers as-is into the "Original request" column of the [04 placement matrix](04_placement-matrix.md).

## 0. Overall

| No | Question | Why ask |
|---|---|---|
| G-01 | In this work, who ultimately decides what? | Starting point for finding the concepts and numbers behind decisions |
| G-02 | Which documents, screens or numbers do you look at to make that decision today? | Shows the existing definitions and how scattered they are |
| G-03 | What happens when that decision is wrong? | Evidence for the "impact of drift" score |
| G-04 | Which tasks would you like AI to take over, and which not? | Scope of the app / AI layer, and the definitions AI will need |

## 1. Ontology (concepts, relationships, rules)

> Ask in the reference order of the [06 Ontology design guide](06_ontology-design.md): vocabulary → structure → states and events → rules → actions. Do not discuss rules before the vocabulary is agreed.

| No | Element | Question | If you hear this |
|---|---|---|---|
| O-01 | VOC | Which words do you use most at work? Do you call the same thing by different names? | Candidate terms and synonyms |
| O-02 | VOC | Does that word mean something different in other departments? | If yes, settle an official definition or split it into separate concepts (most important question) |
| O-03 | VOC | When is something *not* an X? | Counter-examples; the most effective way to prevent AI misreadings |
| O-04 | STR | What "things" do you work with, and what counts as one of each? | Entities and what makes each record unique |
| O-05 | STR | How are X and Y related? Is it one-to-many? | Relationships and cardinality |
| O-06 | STA | What states can X be in, and what makes it change? | States and events. Ask about exceptional ones too (sent back, reactivated) |
| O-07 | STA | When you say "current status", as of when (today, end of yesterday, month end)? | Point-in-time definition |
| O-08 | RUL | Are there rules that must always hold? Who set them? | Business rules and decision owner |
| O-09 | RUL | When did that rule last change? | Change frequency and valid period |
| O-10 | ACT | Who changes or processes what, and under which conditions? | Action performer, preconditions and result |
| O-11 | ACT | May AI do that processing (automatically / after a person confirms / never)? | AI permission |

## 2. Semantic (definitions of numbers)

| No | Question | If you hear this |
|---|---|---|
| S-01 | Which numbers do you look at most? | Candidate metrics |
| S-02 | For that number, what is added, what is excluded, and what is it divided by? | Formula: tax-inclusive or not, how cancellations and returns are treated |
| S-03 | When does "this month" start and end? | Period definition (calendar month, fiscal month, closing date) |
| S-04 | How do you slice that number (by region, by owner...)? | Dimensions and grain |
| S-05 | Do different departments report different numbers under the same name? | Split definitions: unify them, or separate them into differently named metrics |
| S-06 | Who may see that number, and who must not? | Row- and column-level access (record it as policy). Confirm it also applies through AI |
| S-07 | For per-record checks, how are the values counted (such as recent check-ins)? | How per-record values are counted. The rule goes in the ontology (RUL); how its values are counted goes in the semantic layer |

## 3. Data (sources, quality, freshness)

| No | Question | If you hear this |
|---|---|---|
| D-01 | Which system, or whose input, does that data come from? | Source and the person responsible for input |
| D-02 | How fresh does the data need to be (real time, previous day, monthly)? | Ingestion frequency |
| D-03 | Is the data ever wrong or missing? | Candidate quality rules |
| D-04 | Do you ever need to see past states (values before a change)? | History handling |
| D-05 | How long must it be kept? Is there data that must be deleted? | Retention, personal data handling |

## 4. App / AI (presentation, interaction, AI behavior)

| No | Question | If you hear this |
|---|---|---|
| A-01 | Who uses it, when, and on what device? | Usage scenarios |
| A-02 | What should be obvious at a glance? | Screen priorities, highlighting |
| A-03 | Which task takes the most effort today? | Candidates for automation or AI |
| A-04 | What evidence would make you trust an AI's answer? | How sources are shown. The sources should be lower-layer definitions |
| A-05 | When the AI is wrong, who notices and how is it fixed? | Human review points, agent permissions |

## Signals to watch for during interviews

| Signal | Meaning | Ask next |
|---|---|---|
| People use the same word with different meanings | The ontology is undefined | O-02, O-03, O-08 |
| "The number on this screen is a bit different from that report" | Semantic definitions have split | S-02, S-05 |
| "I always fix it up in Excel before using it" | Data quality issues are being absorbed by the app | D-03 |
| Someone explains a business rule as "this is what we tell the AI" | A rule is buried in a prompt | Pull it out as an official rule with O-08, and confirm what AI may do with O-11 |
| "Just put it on the screen for now" | Presentation is being discussed before the definition is settled | Go back to S-01 to S-03 |
