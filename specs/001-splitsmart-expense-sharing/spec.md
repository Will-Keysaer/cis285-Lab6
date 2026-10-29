# Feature Specification: SplitSmart Expense Sharing

**Feature Branch**: `Not created`

**Created**: 2026-10-29

**Status**: Draft

**Input**: User description: Use `UserRequirement.md` as the authoritative SplitSmart customer requirements. Specify actors, scenarios, functional and non-functional requirements, acceptance criteria, IDs, and open questions; retain all customer features and identify a Java Swing MVP subset separately.

## Clarifications

### Session 2026-10-29

- Q: For a group expense, how should the person who paid relate to the people sharing the cost? → A: Select one payer separately from participants; the payer must belong to the group but need not share the expense.
- Q: Which sign-in approach should SplitSmart require for user accounts? → A: Email and password with account recovery.
- Q: Who should be allowed to add and remove members from a SplitSmart group? → A: Only the creator/designated managers can invite or remove members; invitees must accept to join.
- Q: What rules should SplitSmart use for accepted expense amounts, percentage totals, and rounding to cents? → A: Amount must be positive and use currency precision; percentages must be nonnegative and total exactly 100%; distribute leftover cents by largest fractional remainder.
- Q: Should a payment recorded by one group member change the balance immediately, or only after the recipient confirms it? → A: The payment sender records the payment and the balance changes immediately.

## Actors

- **Account holder**: A person who signs in to SplitSmart and views their own expenses and balances.
- **Group creator or manager**: An account holder who creates and manages a group and its membership.
- **Expense submitter**: A group member who records an expense and identifies the people who share it.
- **Expense participant**: A group member included in an expense and its allocation.
- **Approver**: A group member authorized to review an expense before it affects balances. This role may overlap with other roles.

## User Scenarios & Testing

### User Story 1 - Set up an account and group (Priority: P1)

As a person sharing expenses with others, I want my own account and a group with the relevant people so that shared activity and balances are organized.

**Why this priority**: Accounts and groups establish the people and context required for every shared-expense journey.

**Independent Test**: Using test accounts, create a group, invite and remove members as a manager, verify invite acceptance is required to join, and confirm members can access the group's shared activity while an unrelated account cannot.

**Acceptance Scenarios**:

1. **AC-001 — FR-001**: Given a person without an account, when they create an account using an email address and password, then they can sign in, access their own expense and balance information, and use the account recovery process if they cannot sign in.
2. **AC-002 — FR-001, NFR-002**: Given two signed-in account holders, when either views personal information, then they see only their own private information and information shared with groups they belong to.
3. **AC-003 — FR-002**: Given an account holder who creates a group or is designated as its manager, when they invite another account holder, then the invitee joins only after accepting the invitation.
4. **AC-004 — FR-002**: Given a group member who is not its creator or a designated manager, when they attempt to invite or remove members, then the system denies the membership change. Given a group manager, when they remove a member or update group details, then the group reflects the change and prior expense records remain attributable to their original participants.

---

### User Story 2 - Record and allocate a shared expense (Priority: P1)

As a group member recording a shared expense, I want to identify who paid and allocate its cost among the people involved so that each person's share is clear.

**Why this priority**: Expense entry is the source of the balances users need to track.

**Independent Test**: In a test group, record expenses using equal and percentage allocations, then verify participant shares and the expense details.

**Acceptance Scenarios**:

1. **AC-005 — FR-003**: Given a group member, when they submit an expense with a positive amount at the group's currency precision, date, description, allocation method, selected payer, and selected participants, then the expense records those values and identifies its submitter.
2. **AC-006 — FR-003**: Given an expense form, when the submitter attaches a supported receipt image, then the image is associated with that expense; when no image is attached, the expense can still be submitted.
3. **AC-007 — FR-004**: Given an expense using an equal split, when the submitter selects the participants, then the expense is allocated across those participants and the shares sum to the entered amount.
4. **AC-008 — FR-005**: Given an expense using percentage allocation, when valid participant percentages totaling 100% are submitted, then the resulting shares sum to the entered amount.
5. **AC-009 — FR-003, FR-005**: Given a zero or negative amount, an amount beyond the group's currency precision, negative participant percentages, percentages that do not total exactly 100%, or an allocation that assigns a share to a non-participant, when the submitter attempts to submit, then the system identifies the invalid input and does not accept it.

---

### User Story 3 - Review expenses and receive updates (Priority: P1)

As a group member, I want expenses reviewed before they affect balances and to be notified when a group member submits one so that shared records are visible and trustworthy.

**Why this priority**: Review is an explicit customer requirement and prevents unapproved submissions from changing amounts owed.

**Independent Test**: Submit an expense in a group with a reviewer, verify its pending state and notification, then approve or reject it and verify its effect.

**Acceptance Scenarios**:

1. **AC-010 — FR-006**: Given a newly submitted expense, when it awaits review, then it is visibly pending and does not change any balance.
2. **AC-011 — FR-007**: Given a pending expense and an authorized approver, when the approver approves it, then its allocation is accepted and reflected in balances.
3. **AC-012 — FR-007**: Given a pending expense, when an authorized approver rejects it, then it is not included in balances and its review outcome is visible to relevant group members.
4. **AC-013 — FR-008**: Given a group member submits an expense, when submission succeeds, then the other members of that group are notified of the new expense and its review status.

---

### User Story 4 - Check balances and record payments (Priority: P1)

As a group member, I want to see what I owe or am owed and record a payment so that settlements are reflected in the shared records.

**Why this priority**: Accurate balances and payments complete the core expense-sharing value.

**Independent Test**: Approve a known expense, verify the resulting pairwise amounts owed, record a settlement payment, and verify the updated amounts.

**Acceptance Scenarios**:

1. **AC-014 — FR-009**: Given approved expenses in a group, when a member views balances, then the system shows the amount owed by each user to every other user based on those expenses.
2. **AC-015 — FR-009, FR-010**: Given a recorded payment between two users, when either views the affected balance, then the amount owed is reduced by the payment amount and does not become negative solely because of an overpayment.
3. **AC-016 — FR-010**: Given a payment sender, when they record the payment's sender, recipient, amount, and date, then it immediately affects the corresponding balance and the payment history identifies who recorded it and when.

---

### User Story 5 - Review expense, balance, and payment reports (Priority: P2)

As a group member, I want summaries and filtered reports so that I can understand spending and settlements over a chosen period.

**Why this priority**: Reports make historical activity useful after core entries and balances are available.

**Independent Test**: Create test expenses and payments across multiple dates, generate a summary and a date-filtered report, and compare the results with the source records.

**Acceptance Scenarios**:

1. **AC-017 — FR-011**: Given expense, balance, and payment records, when a member opens a summary, then it reports those categories for information they are authorized to view.
2. **AC-018 — FR-012**: Given a custom report and a valid date range, when the member runs it, then only records matching that range and the selected supported parameters are included.
3. **AC-019 — FR-012**: Given an invalid date range, when the member runs a report, then the system explains the invalid range and does not present it as a valid report.

### Edge Cases

- An expense has one participant, no participants, or participants outside the selected group.
- Percentage allocations are below or above 100%, contain negative values, or produce fractional-cent shares.
- An expense is rejected, edited while pending, or submitted by someone who is not a group member.
- The expense submitter is also the only configured approver.
- A payment is zero, negative, greater than the outstanding amount, or recorded twice.
- A report has no matching records, an end date before its start date, or a date boundary that falls on a recorded transaction.
- A member leaves a group that has historical expenses or unsettled balances.
- Receipt image upload is omitted or cannot be accepted.

### Cross-Cutting Acceptance Scenarios

1. **AC-020 — NFR-001**: Given a supported web browser and an account holder, when they access SplitSmart, then they can complete the supported product journeys through the primary web-based experience.
2. **AC-021 — NFR-003**: Given equal and percentage allocations that produce fractional cents, when shares are calculated, then leftover cents are distributed by largest fractional remainder and the displayed shares and resulting balances reconcile to the expense total at the smallest supported currency unit.
3. **AC-022 — NFR-004**: Given the specified 50-member group and 10,000 expense/payment record test dataset under ordinary operating conditions, when account, expense, balance, and report views are requested, then at least 95% are available within 3 seconds.
4. **AC-023 — NFR-005**: Given submitted expenses and recorded payments, when a group member views the relevant history, then each expense is identifiable as pending, approved, or rejected and payments remain identifiable as settlements.

## Requirements

### Functional Requirements

- **FR-001 — Customer baseline**: The system MUST allow each person to create an individual account using an email address and password, sign in to access their own expense and balance information, and recover access if they cannot sign in.
- **FR-002 — Customer baseline**: The system MUST allow users to create and manage groups of people and their membership for shared expenses and balances. Only the group creator or designated managers MAY invite or remove members; an invitee MUST accept an invitation before joining.
- **FR-003 — Customer baseline**: The system MUST allow a group member to create an expense with a positive amount at the group's configured currency precision, date, description, allocation method, one payer who belongs to the group, the involved participants, and an optional receipt image. The payer MUST be selected separately from participants and MAY be excluded from the expense allocation.
- **FR-004 — Customer baseline**: The system MUST support dividing an expense equally among the selected participants.
- **FR-005 — Customer baseline**: The system MUST support dividing an expense by nonnegative participant percentages totaling exactly 100%. The resulting shares MUST be rounded to the smallest supported currency unit using the largest-remainder method and MUST reconcile to the expense total.
- **FR-006 — Customer baseline**: The system MUST keep a submitted expense out of balances until it has been approved.
- **FR-007 — Customer baseline**: The system MUST provide a review and approval process for expenses and record whether a submitted expense was approved or rejected.
- **FR-008 — Customer baseline**: The system MUST notify other members of the relevant group when a new expense is submitted.
- **FR-009 — Customer baseline**: The system MUST track how much each user owes every other user based on approved expenses and recorded payments.
- **FR-010 — Customer baseline**: The system MUST allow the payment sender to record a payment's sender, recipient, amount, and date. The payment MUST immediately affect the corresponding balance, and its history MUST identify who recorded it and when.
- **FR-011 — Customer baseline**: The system MUST provide reports or summaries of users' expenses, balances, and payments, limited to information those users may access.
- **FR-012 — Customer baseline**: The system MUST allow users to generate reports for specified date ranges and supported report parameters.

### Customer Requirements Inventory

All original customer feature areas are retained here. The Java Swing MVP subset below is a proposed, narrower delivery slice and does not remove any inventory item.

| Original customer feature | Requirement coverage | Customer baseline |
|---|---|---|
| Individual user accounts and access to own expense and balance information | FR-001 | Yes |
| Create and manage groups | FR-002 | Yes |
| Expense amount, date, description, equal or percentage allocation, optional receipt image, and involved users | FR-003–FR-005 | Yes |
| Notify group members when another member creates an expense | FR-008 | Yes |
| Review and approve expenses before balance changes | FR-006–FR-007 | Yes |
| Track amounts owed between every pair of users | FR-009 | Yes |
| Record payments and update balances | FR-010 | Yes |
| Expense, balance, and payment summaries and custom date-range reports | FR-011–FR-012 | Yes |
| Primarily web-based product | NFR-001 | Yes |

### Non-Functional Requirements

- **NFR-001 — Customer baseline**: The product MUST be primarily web-based. A desktop Swing application alone does not satisfy this requirement.
- **NFR-002 — Privacy**: A signed-in user MUST be limited to their own private account information and information shared with groups they belong to. Unauthorized access attempts MUST NOT reveal protected expense, balance, or payment details.
- **NFR-003 — Financial integrity**: Each approved expense's participant shares MUST reconcile to its total at the currency's smallest supported unit. Fractional-cent remainders MUST be distributed using the largest-remainder method; balance and payment totals MUST reconcile to their underlying records.
- **NFR-004 — Usability and performance assumption**: For a group of up to 50 members and 10,000 expense/payment records, at least 95% of account, expense, balance, and report views MUST be available within 3 seconds under ordinary operating conditions.
- **NFR-005 — Reviewability**: Expense review outcomes and recorded payments MUST remain identifiable in the relevant group history so that users can distinguish pending, approved, rejected, and settled activity.

### Acceptance Criteria Traceability

| Criterion | Requirement(s) | Verifiable result |
|---|---|---|
| AC-001–AC-002 | FR-001, NFR-002 | Account access works and private information is isolated. |
| AC-003–AC-004 | FR-002 | A group's membership can be managed without losing historical attribution. |
| AC-005–AC-006 | FR-003 | Expense details and an optional image are recorded. |
| AC-007–AC-009 | FR-004–FR-005 | Equal and percentage allocations reconcile; invalid allocations are rejected. |
| AC-010–AC-012 | FR-006–FR-007 | Only approved expenses affect balances; review outcomes are visible. |
| AC-013 | FR-008 | Other group members receive an expense notification. |
| AC-014–AC-016 | FR-009–FR-010 | Pairwise balances and payments reconcile. |
| AC-017–AC-019 | FR-011–FR-012 | Summaries and valid date-filtered reports match source records. |
| AC-020 | NFR-001 | Users can access the product through its primary web-based experience. |
| AC-021 | NFR-003 | Allocation shares and balances reconcile at the smallest supported currency unit. |
| AC-022 | NFR-004 | At least 95% of specified views meet the 3-second target for the defined test dataset. |
| AC-023 | NFR-005 | History distinguishes review and settlement states. |

### Key Entities

- **Account**: A person’s sign-in identity and private expense and balance information.
- **Group**: A named collection of accounts whose members share expenses and balances, with a creator/designated manager role and membership invitations that require invitee acceptance.
- **Expense**: A submitted monetary item with one group-member payer, date, description, participants, allocation method and shares, optional receipt, and review status. The payer may be a non-participant.
- **Expense allocation**: A participant’s equal or percentage-based share of an expense.
- **Review decision**: An approver's decision and status for a submitted expense.
- **Notification**: An update informing relevant group members about a newly submitted expense.
- **Balance**: The amount one user owes another, derived from approved expense allocations and payments.
- **Payment**: A recorded settlement from one user to another, with an amount, payment date, recording account, and record timestamp; recording it immediately affects the corresponding balance.
- **Report**: A summary or filtered view of authorized expense, balance, or payment records.

## Success Criteria

### Measurable Outcomes

- **SC-001**: In usability testing, at least 90% of first-time participants can create an account, join or create a group, and submit an expense without assistance.
- **SC-002**: All defined equal-split and percentage-split test cases reconcile to the entered total at the smallest supported currency unit.
- **SC-003**: All balance test cases match the sum of approved allocations minus recorded payments for each user pair.
- **SC-004**: At least 95% of the defined expense, balance, and report views meet the 3-second target for the NFR-004 test dataset.
- **SC-005**: 100% of authorization test cases prevent users outside the relevant account or group from viewing protected information.
- **SC-006**: Every customer feature area in the Customer Requirements Inventory has a linked functional or non-functional requirement and at least one verification criterion.

## Assumptions

- A person must have an account before being added as a group member.
- For the initial workflow, each group uses a single authorized review step; multiple approval levels can be added if group needs require them.
- Notifications are in-app in the proposed MVP. The customer did not specify a delivery channel.
- For a percentage split, percentages must total exactly 100%; fractional-cent remainders are assigned deterministically so shares reconcile exactly.
- The currency or currencies supported are not specified. The initial release uses one configured currency per group until this is decided.
- The expense submitter and payer are separate roles; the submitter records one group-member payer, who may be outside the participant allocation.
- The performance dataset and target in NFR-004 are planning assumptions, not customer-supplied service-level commitments.

## Open Questions

1. Does the customer approve a Swing desktop MVP despite the stated primarily web-based product requirement, or must the MVP itself be web-based?
2. Who may approve an expense, may the submitter approve their own expense, and when are multiple approval levels needed?
3. Which notification channels are required beyond in-app notifications?
4. Which currencies and exchange-rate handling are required?
5. Should reports be exportable, and which report filters beyond date range are required?
6. Can a group member edit or delete a pending or approved expense, and what history must be retained for those changes?

## Proposed Java Swing MVP Subset (Implementation Decision)

This section identifies a proposed implementation subset; it does not redefine the customer requirements or claim that a Swing desktop program fulfills NFR-001. The project constitution treats Java Swing as an implementation decision and requires documented customer approval before it replaces the web-based delivery requirement.

**Candidate MVP scope**:

- Account access and group creation/member management (FR-001–FR-002).
- Expense creation with equal splits and percentage splits (FR-003–FR-005).
- A single review step, with in-app notifications (FR-006–FR-008).
- Pairwise balance display and payment recording (FR-009–FR-010).
- Basic expense, balance, and payment summaries (FR-011).

**Deferred from the candidate MVP, but retained in the customer inventory**:

- Optional receipt-image attachment (FR-003).
- Custom report parameters and date-range reports (FR-012).
- Multiple approval levels and notification channels beyond in-app updates (FR-007–FR-008).
- A primary web-based product experience (NFR-001); this remains a customer requirement and cannot be considered met by a Swing-only MVP.

**Project implementation constraints, not customer requirements**: Any Java implementation is governed by the project constitution, including Java 21, Maven, JUnit 5, PlantUML when diagrams are needed, object-oriented separation of GUI and business logic, `BigDecimal` for monetary values, and human review of AI-generated output. This specification contains no Java code.
