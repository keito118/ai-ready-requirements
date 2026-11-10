# 03. Interview guide

> Questions for interviewing business stakeholders. As AI speeds up implementation, more of the requirements effort shifts into these interviews. The questions follow the order in which AI processes a prompt (app / AI → ontology → semantic → data), and end with the cross-cutting policy.
>
> How to use: start with "0. Overall", then dig into sections 1–5 for each business term or number that comes up. Copy the answers as-is into the "Original request" column of the [04 placement matrix](04_placement-matrix.md).

## 0. Overall

| No | Question | Why ask |
|---|---|---|
| G-01 | In this work, who ultimately decides what? | Starting point for finding the concepts and numbers behind decisions |
| G-02 | Which documents, screens or numbers do you look at to make that decision today? | Shows the existing definitions and how scattered they are |
| G-03 | What happens when that decision is wrong? | Evidence for the "impact of drift" score |
| G-04 | Which tasks would you like AI to take over, and which not? | Scope of the app / AI layer, and the definitions and policies AI will need |

## 1. App / AI (the entrance and the exit)

| No | Question | If you hear this |
|---|---|---|
| A-01 | Who uses it, when, and on what device? | Usage scenarios, and the user context to receive (role, department) |
| A-02 | What should be obvious at a glance? | Screen priorities, highlighting |
| A-03 | Which task takes the most effort today? | Candidates for automation or AI |
| A-04 | What evidence would make you trust an AI's answer? | How sources are shown. The sources should be ontology definitions |
| A-05 | When the AI is wrong, who notices and how is it fixed? | Human review points |

## 2. Ontology (meaning)

> Ask in the order of the [06 Ontology design guide](06_ontology-design.md): objects, then logic. Do not discuss rules before the vocabulary is agreed. If a missing concept comes up along the way, go back to the object questions.

| No | Element | Question | If you hear this |
|---|---|---|---|
| O-01 | Object | Which words do you use most at work? Do you call the same thing by different names? | Candidate concepts and labels (synonyms) |
| O-02 | Object | Does that word mean something different in other departments? | If yes, split it into separate concepts or make it logic with a department argument (most important question) |
| O-03 | Object | When is something *not* an X? | Counter-examples; the most effective way to prevent AI misreadings |
| O-04 | Object | What "things" do you work with, and what counts as one of each? | Objects and what makes each record unique |
| O-05 | Object | How are X and Y related? Is it one-to-many? | Relationships and cardinality |
| O-06 | Object | What states can X be in, and what makes it change? | States and transitions. Ask about exceptional ones too (sent back, reactivated) |
| O-07 | Object | When you say "current status", as of when (today, end of yesterday, month end)? | Point-in-time definition |
| O-08 | Logic (check) | How do you decide whether something is an X? Are there rules that must always hold? Who set them? | Check logic and decision owner |
| O-09 | Logic (calculation) | Which numbers do you look at most? In words, what does each one represent? | The description of the calculation logic. Ask for the formula in section 3 |
| O-10 | Logic | Do the criteria or the meaning of numbers differ by department? | Department-specific versions and the default version |
| O-11 | Logic (operation) | Who changes or processes what, and under which conditions? | Preconditions and result of the operation logic |
| O-12 | Common | When did that rule or definition last change? | Change frequency and valid period |

## 3. Semantic (implementing calculations)

> For numbers and checks whose meaning is settled in the ontology, ask how they are calculated. Do not start from the formula of a number whose meaning is not settled.

| No | Question | If you hear this |
|---|---|---|
| S-01 | For that number, what is added, what is excluded, and what is it divided by? | Aggregation: tax-inclusive or not, how cancellations and returns are treated |
| S-02 | When does "this month" start and end? | Period handling (calendar month, fiscal month, closing date) |
| S-03 | How do you slice that number, and at what grain (by region, by owner, daily, weekly...)? | Dimensions and grain |
| S-04 | Do different departments report different numbers under the same name? | Split definitions. Settle the meaning with O-02 and O-09 first, then implement |

## 4. Data (where values come from, freshness, quality)

| No | Question | If you hear this |
|---|---|---|
| D-01 | Which system, or whose input, does that data come from? | Source and the person responsible for input |
| D-02 | How fresh does the data need to be (real time, previous day, monthly)? | Ingestion frequency |
| D-03 | Is the data ever wrong or missing? | Candidate quality rules |
| D-04 | Do you ever need to see past states (values before a change)? | History handling |
| D-05 | How long must it be kept? Is there data that must be deleted? | Retention, personal data handling |

## 5. Policy (cross-cutting)

| No | Question | If you hear this |
|---|---|---|
| P-01 | Who may see this information or number, and who must not? | Row- and column-level access scope; the same scope must apply through AI |
| P-02 | May AI do that processing (automatically / after a person confirms / never)? | Whether AI may run an operation |
| P-03 | Is there information that must never be passed to AI (such as personal data)? | What to mask |

## Signals to watch for during interviews

| Signal | Meaning | Ask next |
|---|---|---|
| People use the same word with different meanings | The ontology is undefined | O-02, O-03, O-08 |
| "The number on this screen is a bit different from that report" | The meaning or the implementation of a number has split | Check the meaning with O-09, then S-01 and S-04 |
| "I always fix it up in Excel before using it" | Data quality issues are being absorbed by the app | D-03 |
| Someone explains a business rule as "this is what we tell the AI" | A rule is buried in a prompt | Pull it out as logic with O-08, and confirm what AI may do with P-02 |
| "Just put it on the screen for now" | Presentation is being discussed before the meaning is settled | Go back to O-09 |
