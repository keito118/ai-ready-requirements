# 05. Worked example: membership churn at a fitness chain

> A filled-in example for a fictional membership-based fitness chain (20 locations) that wants "a dashboard and an AI assistant to reduce cancellations". Five requests from the interviews are placed into the four layers.

## 1. Original requests

| ID | Original request (in the stakeholder's words) | Raised by | Interview No |
|---|---|---|---|
| REQ-001 | I want to see the churn rate every week, and when it goes up I want the AI to explain why | Location manager | G-01, S-01 |
| REQ-002 | Revenue should be shown before tax, and the suspension fee from paused members shouldn't be mixed into membership revenue | Accounting | S-02 |
| REQ-003 | Each location counts "active members" differently; we want one way | Head office | O-02, S-05 |
| REQ-004 | List the members who look likely to quit, and have the AI draft a message to reach out to them | Front desk staff | A-03 |
| REQ-005 | Check-ins don't show up until the next day | Location manager | D-02 |

## 2. Placement results

| ID | Request | Requirement | Layer | Reuse | Impact | Change | Owner | AI use | Total | Reason |
|---|---|---|---|---|---|---|---|---|---|---|
| ONT-001 | REQ-003 | An active member has a valid contract and has checked in at least once in the last 30 days | Ontology | 3 | 3 | 3 | 3 | 3 | 15 | The term meant different things at different locations; define it once for the whole company |
| ONT-002 | REQ-001, 004 | Member states are joined → enrolled → paused → cancelled. Paused members can return to enrolled. Cancelled members re-join with a new contract | Ontology | 3 | 3 | 3 | 3 | 3 | 15 | Churn rate, churn risk and the AI's explanations all depend on these states |
| ONT-003 | REQ-001 | Cancellations take effect at month end and must be requested by the 20th of the previous month | Ontology | 3 | 3 | 2 | 3 | 3 | 14 | A business rule; the basis for the AI's answer to "can I cancel this month?" |
| SEM-001 | REQ-001 | Weekly churn rate = members whose cancellation was confirmed that week ÷ enrolled members at the start of the week (paused members excluded from the denominator) | Semantic | 3 | 2 | 2 | 2 | 3 | 12 | Location managers, head office and the AI must all use the same number |
| SEM-002 | REQ-002 | Membership revenue = fees of enrolled members, before tax. Suspension fees are a separate metric, "suspension revenue" | Semantic | 3 | 3 | 2 | 2 | 2 | 12 | Must match accounting's monthly close |
| POL-001 | REQ-001, 004 | Access: location managers and staff see their own location only; head office sees all. AI agents inherit the permissions of the person using them | Policy | 3 | 3 | 2 | 3 | 3 | 14 | Prevents other locations' member data from leaking through the AI |
| SEM-004 | REQ-004 | Churn-risk member = enrolled, no check-in in the last 14 days, and last month's check-ins fell to half or less of the month before | Semantic | 2 | 2 | 1 | 2 | 2 | 9 | The flow said app layer, but it scores 9. Head office campaigns and the AI's message drafts use the same condition, so it becomes a shared definition |
| DAT-001 | REQ-005 | Ingest check-ins from the entry system every 15 minutes (currently daily) | Data | - | - | - | - | - | - | A freshness issue; definitions do not change |
| DAT-002 | REQ-001 | Ingest cancellation requests from the membership system daily. Flag records whose cancellation date is not a month end as quality errors | Data | - | - | - | - | - | - | Stops data that breaks ONT-003 at the entrance |
| APP-001 | REQ-001 | Dashboard for location managers: SEM-001 by location, compared with the same week last year | App / AI | 1 | 1 | 1 | 1 | 1 | 5 | Managers want to change the presentation often |
| APP-002 | REQ-001 | AI that explains churn: references SEM-001 and ONT-002 and explains which member segments moved most. The churn formula is not written in the prompt | App / AI | 1 | 2 | 1 | 1 | 1 | 6 | Behavior lives in the app layer; definitions are referenced from below |
| APP-003 | REQ-004 | Churn-risk member list (SEM-004) and AI-drafted outreach messages. Staff review before sending | App / AI | 1 | 2 | 1 | 1 | 1 | 6 | Each location may adjust the tone of the messages |

## 3. How the AI walks through it

> Following the processing order in [07 How AI processes a prompt](07_ai-processing-flow.md) (app / AI → ontology → semantic → data), and the reference order inside the ontology from the [06 Ontology design guide](06_ontology-design.md), this section traces how the AI handles real questions. The policy (POL-001) is checked in each layer.

### 3.1 Ontology elements and scope

| ID | Element | Scope | Point in time / AI permission |
|---|---|---|---|
| ONT-002 | STA states and events | Core | Status is evaluated as of the end of the previous day |
| ONT-003 | RUL rule (deadline) | Core | |
| ONT-001 | RUL rule (derived classification) | Core | Point in time **undecided** (found in 3.2 below) |
| ONT-004 | ACT action: pause request. Performer = location staff; precondition = enrolled and requested by the 20th of the previous month; result = enrolled → paused | Core | AI may act only after a person confirms |

> ONT-004 is an action added in this section to handle the request in 3.3.

### 3.2 "Why did churn among active members at the Shibuya location go up last month?" (location manager)

| Step | References | What the AI settled |
|---|---|---|
| App / AI: receive | APP-002, POL-001 | Asker = Shibuya location manager, who may use the explanation AI |
| VOC | "Active member" → ONT-001, "churn" → cancelled state in ONT-002, "Shibuya location" → location | Every word mapped to an official concept |
| STR | Member — contract — location | Shibuya members = members with a contract at Shibuya |
| STA | ONT-002, cancellation request and check-in events | Members who became cancelled as of the end of last month |
| RUL | ONT-003, ONT-001 | "Last month's cancellations" = cancellations confirmed at the end of last month. But **as of when a member had to be active** is not defined |
| Semantic | SEM-001, POL-001 | Count churn by location for last month. POL-001 limits the result to Shibuya |
| Data | DAT-001, DAT-002 | Last month is final, no quality errors |
| App / AI: answer | Explained in terms of events | For example: "Most members who cancelled last month checked in twice a month or less in the two months before" |

Because the point in time for ONT-001 is undefined, the AI uses the default (end of the previous day) and says so in its answer. This goes on the open issues list for the business owner.

### 3.3 "Pause this member's membership" (location staff)

| Step | References | What the AI settled |
|---|---|---|
| App / AI: receive | POL-001 | Asker = location staff; the member belongs to their own location |
| VOC → STA | Member, ONT-002 | The member is currently enrolled |
| RUL | Pauses are monthly, requested by the 20th of the previous month | After the 20th, the pause starts the month after next |
| ACT | ONT-004 | Preconditions met. AI permission is "after a person confirms", so the AI drafts the request and waits for staff to confirm |

Operation requests skip the semantic layer and go straight from the ontology (ACT) to a write in the data layer.

### 3.4 Open issues

| No | Issue | Related ID | Owner |
|---|---|---|---|
| 1 | The point in time for the active member test (at the cancellation request, or at the end of the previous month) | ONT-001, SEM-001 | Head office (business owner) |

## 4. What this example shows

- **One request splits across layers.** REQ-001 ("weekly churn rate, explained by AI") spans the ontology (states, closing rule), the semantic layer (churn rate), the data layer (cancellation data) and the app / AI layer (dashboard, explaining AI).
- **Prompts get thin.** The prompt for APP-002 only says "explain using SEM-001 and ONT-002", so it does not need to change when the churn definition changes.
- **Requirements where the flow and the score disagree are worth the discussion.** SEM-004 (churn risk) started out as "an app filter", but scoring it showed that head office needs the same definition.
- **The ontology is where interviews get difficult.** A situation like REQ-003, where the same word counts different things, is the first problem to solve before bringing in AI.
- **Walking through it as the AI exposes gaps.** As in 3.2, tracing real questions in reference order reveals gaps such as an undefined point in time while you are still interviewing.
