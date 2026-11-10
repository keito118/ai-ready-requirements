# 05. Worked example: membership churn at a fitness chain

> A filled-in example for a fictional membership-based fitness chain (20 locations) that wants "a dashboard and an AI assistant to reduce cancellations". Five requests from the interviews are placed into the four layers and the cross-cutting policy.

## 1. Original requests

| ID | Original request (in the stakeholder's words) | Raised by | Interview No |
|---|---|---|---|
| REQ-001 | I want to see the churn rate every week, and when it goes up I want the AI to explain why | Location manager | G-01, O-09 |
| REQ-002 | Revenue should be shown before tax, and the suspension fee from paused members shouldn't be mixed into membership revenue | Accounting | S-01 |
| REQ-003 | Each location counts "active members" differently; we want one way | Head office | O-02, S-04 |
| REQ-004 | List the members who look likely to quit, and have the AI draft a message to reach out to them | Front desk staff | A-03 |
| REQ-005 | Check-ins don't show up until the next day | Location manager | D-02 |

## 2. Placement results

| ID | Request | Requirement | Layer | Reuse | Impact | Change | Owner | AI use | Total | Reason |
|---|---|---|---|---|---|---|---|---|---|---|
| APP-001 | REQ-001 | Dashboard for location managers: churn rate (ONT-005) by location, compared with the same week last year | App / AI | 1 | 1 | 1 | 1 | 1 | 5 | Managers want to change the presentation often |
| APP-002 | REQ-001 | AI that explains churn using ONT-005 and ONT-008. Neither definitions nor formulas are written in the prompt | App / AI | 1 | 2 | 1 | 1 | 1 | 6 | Behavior lives in the app layer; meaning is referenced from the ontology |
| APP-003 | REQ-004 | Churn-risk member list (ONT-007) and AI-drafted outreach messages. Staff review before sending | App / AI | 1 | 2 | 1 | 1 | 1 | 6 | Each location may adjust the tone of the messages |
| ONT-001 | REQ-001, 003, 004 | Object "member": states are joined → enrolled → paused → cancelled. Paused members can return to enrolled. Cancelled members re-join with a new contract. Status is evaluated as of the end of the previous day | Ontology | 3 | 3 | 3 | 3 | 3 | 15 | Every piece of logic depends on these states |
| ONT-002 | REQ-003, 005 | Object "check-in": a record of a member entering a location. One entry is one record | Ontology | 3 | 2 | 3 | 2 | 3 | 13 | Input to the active-member and churn-risk checks |
| ONT-003 | REQ-003 | Check "is active (member, date) → true/false": an enrolled member with a valid contract on that date who checked in at least once in the 30 days up to it. Paused members are not active | Ontology | 3 | 3 | 3 | 3 | 3 | 15 | The term meant different things at different locations; define it once for the whole company |
| ONT-004 | REQ-001 | Calculation "determine cancellation date (member, request date) → date": cancellations take effect at month end. Requests by the 20th take effect at the end of that month; later requests at the end of the next month | Ontology | 3 | 3 | 2 | 3 | 3 | 14 | A business rule; the basis for the AI's answer to "can I cancel this month?" |
| ONT-005 | REQ-001 | Calculation "calculate churn rate (location, period) → metric": members whose cancellation was confirmed in the period ÷ members enrolled at the start of the period (paused members excluded) | Ontology | 3 | 2 | 2 | 2 | 3 | 12 | Location managers, head office and the AI must all use the same meaning |
| ONT-006 | REQ-002 | Calculation "calculate membership revenue (location, period) → amount": fees of enrolled members, before tax. Suspension fees are a separate calculation, "calculate suspension revenue" | Ontology | 3 | 3 | 2 | 2 | 2 | 12 | Must match accounting's monthly close |
| ONT-007 | REQ-004 | Check "is at churn risk (member, date) → true/false": enrolled, no check-in in the last 14 days, and last month's check-ins fell to half or less of the month before | Ontology | 2 | 2 | 1 | 2 | 2 | 9 | The flow said app layer, but it scores 9. Head office campaigns and the AI's message drafts use the same condition, so it becomes a shared definition |
| ONT-008 | REQ-001 | Cause "list churn drivers (location, period) → member segments": compares cancelled members by segment (tenure, visit frequency, plan) and returns the segments that grew most | Ontology | 2 | 2 | 2 | 2 | 1 | 9 | Location managers and head office use the same segments |
| SEM-001 | REQ-001 | Weekly churn rate view: implements ONT-005, with cancellation dates following ONT-004 | Semantic | - | - | - | - | - | - | Meaning is in ONT-005; this is only the aggregation |
| SEM-002 | REQ-002 | Membership revenue view: implements ONT-006 | Semantic | - | - | - | - | - | - | Same as above |
| SEM-003 | REQ-003 | Active member view: implements ONT-003 (counts check-ins per member over a 30-day window) | Semantic | - | - | - | - | - | - | Same as above |
| SEM-004 | REQ-004 | Churn risk view: implements ONT-007 | Semantic | - | - | - | - | - | - | Same as above |
| DAT-001 | REQ-005 | Ingest check-ins from the entry system every 15 minutes (currently daily) | Data | - | - | - | - | - | - | A freshness issue; neither meaning nor calculation changes |
| DAT-002 | REQ-001 | Ingest cancellation requests from the membership system daily. Flag records whose cancellation date does not follow ONT-004 as quality errors | Data | - | - | - | - | - | - | Stops data that breaks the rule at the entrance |
| POL-001 | REQ-001, 004 | Access: location managers and staff see their own location only; head office sees all. AI agents inherit the permissions of the person using them, enforced in all four layers | Policy | - | - | - | - | - | - | Prevents other locations' member data from leaking through the AI |
| POL-002 | REQ-004 | AI operations: AI may change a member's status only after a person confirms. Phone numbers are never passed to AI | Policy | - | - | - | - | - | - | Prevents mistaken operations and personal data leaks |

## 3. How the AI walks through it

> Following the order in which AI processes a prompt (app / AI → ontology → semantic → data), this section traces real questions.

### 3.1 "Why did churn among active members at the Shibuya location go up last month?" (location manager)

| Step | Layer | References | What the AI settled |
|---|---|---|---|
| 1. Receive | App / AI | APP-002, user context, POL-001 | The person asking is a location manager at Shibuya and may use this agent |
| 2. Settle the meaning | Ontology | From labels: ONT-001, ONT-003, ONT-005, ONT-008 | Use ONT-005 (location = Shibuya, period = last month) and ONT-008. But **ONT-005 has no argument for limiting it to active members, and when a member had to be active is undefined** |
| 3. Calculate | Semantic | SEM-001, SEM-003, POL-001 | Calculate on Shibuya's rows only |
| 4. Fetch values | Data | DAT-001, DAT-002 | Last month is final, no quality errors |
| 5. Answer | App / AI | Explained in ontology terms | For example: "Shibuya's churn rate last month was 3.1%. The increase came from members in their first year who checked in twice a month or less." It states that the figure could not be limited to active members |

### 3.2 "Pause this member's membership" (location staff)

| Step | Layer | References | What the AI settled |
|---|---|---|---|
| 1. Receive | App / AI | User context, POL-001 | The person asking is Shibuya staff; the member belongs to their location |
| 2. Settle the meaning | Ontology | ONT-001, ONT-009 (below) | The member is enrolled. The preconditions for a pause (enrolled, requested by the 20th of the previous month) hold. After the 20th, the pause starts the month after next |
| 3. Calculate | Semantic | None | No calculation, so this layer is skipped |
| 4. Write values | Data | POL-002 | The AI drafts the request and writes it after staff confirm |
| 5. Answer | App / AI | | Shows a confirmation screen |

One operation was added to handle this request:

| ID | Requirement | Layer |
|---|---|---|
| ONT-009 | Operation "request pause (member, start month) → member": precondition = enrolled and requested by the 20th of the month before the start month; result = enrolled → paused | Ontology |

### 3.3 Open issues

| No | Issue | Related ID | Owner |
|---|---|---|---|
| 1 | Does churn rate need to be limited to active members? If so, as of when must a member have been active (start of the period, or at the cancellation request)? | ONT-005, ONT-003, SEM-001 | Head office (business owner) |

## 4. What this example shows

- **One request splits across layers.** REQ-001 ("weekly churn rate, explained by AI") spans the app / AI layer (dashboard, explaining AI), the ontology (member, cancellation date, churn rate, churn drivers), the semantic layer (churn rate view), the data layer (cancellation data) and the policy (access).
- **Numbers split into meaning and implementation.** The meaning of churn rate is ONT-005; its aggregation is SEM-001. When the meaning changes, the view changes with it, so description and view always agree.
- **Prompts get thin.** The prompt for APP-002 only says "explain using ONT-005 and ONT-008", so it does not need to change when the churn definition changes.
- **Requirements where the flow and the score disagree are worth the discussion.** ONT-007 (churn risk) started out as "an app filter", but scoring it showed that head office needs the same definition.
- **Walking through it as the AI exposes gaps.** As in 3.1, tracing real questions in processing order shows, while you are still interviewing, that "churn among active members" cannot be expressed with the current logic.
