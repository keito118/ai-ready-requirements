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

| No | Question | If you hear this |
|---|---|---|
| O-01 | List the "things" you work with (customers, contracts, deals, products...) | Candidate entities |
| O-02 | Does that word mean something different in other departments? | If yes, an official definition is needed (most important question) |
| O-03 | How are X and Y related? Is it one-to-many? | Relationships and cardinality |
| O-04 | What states can X be in, and how does it move between them? | State transitions. Ask about exceptional ones too (sent back, reactivated) |
| O-05 | Are there rules that must always hold? | Business rules. Also ask who set them (decision owner) |
| O-06 | When did that rule last change? | Evidence for change frequency |

## 2. Semantic (definitions of numbers)

| No | Question | If you hear this |
|---|---|---|
| S-01 | Which numbers do you look at most? | Candidate metrics |
| S-02 | For that number, what is added, what is excluded, and what is it divided by? | Formula: tax-inclusive or not, how cancellations and returns are treated |
| S-03 | When does "this month" start and end? | Period definition (calendar month, fiscal month, closing date) |
| S-04 | How do you slice that number (by region, by owner...)? | Dimensions and grain |
| S-05 | Do different departments report different numbers under the same name? | Split definitions: unify them, or separate them into differently named metrics |
| S-06 | Who may see that number, and who must not? | Row- and column-level access. Confirm it also applies through AI |

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
| People use the same word with different meanings | The ontology is undefined | O-02, O-05 |
| "The number on this screen is a bit different from that report" | Semantic definitions have split | S-02, S-05 |
| "I always fix it up in Excel before using it" | Data quality issues are being absorbed by the app | D-03 |
| Someone explains a business rule as "this is what we tell the AI" | A rule is buried in a prompt | Pull it out as an official rule with O-05 |
| "Just put it on the screen for now" | Presentation is being discussed before the definition is settled | Go back to S-01 to S-03 |
