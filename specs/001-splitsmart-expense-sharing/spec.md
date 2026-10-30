# Feature Specification: SplitSmart Expense Sharing

**Feature Branch**: `not-created (no branch hook configured)`

**Created**: 2026-10-29

**Status**: Draft

**Input**: User description: "Read UserRequirement.md as the authoritative customer requirements. Write a testable SplitSmart feature specification: actors, user scenarios, functional and non-functional requirements, acceptance criteria, requirement IDs (FR-001, etc.), and open questions. Preserve all original customer features in the requirements inventory; label the Java Swing MVP subset separately. Do not write Java code. Do not edit UserRequirement.md."

## Clarifications

### Session 2026-10-29

- Q: For each expense, who should be recorded as the person who paid, and how should that relate to the users sharing the expense? → A: Record one payer from among the expense participants; the submitter may be someone else.
- Q: How should people sign in and gain or lose access to a group? → A: Users sign in with email and password; group owners invite and remove members; members may leave.
- Q: How should SplitSmart validate expense amounts and percentage splits, and allocate rounding remainders? → A: Require positive amounts and nonnegative percentages totaling exactly 100%; round shares to the currency's smallest unit and distribute remainder units in participant order.
- Q: How should a payment be recorded against a balance between two users? → A: Record payer, recipient, amount, and date against a specific balance; partial payments are allowed.
- Q: Where should group members receive a notification when another member creates an expense? → A: In-app notifications only.

## User Scenarios & Testing *(mandatory)*

### Actors

- **Registered user**: A person who signs in, participates in groups, enters expenses,
  reviews balances, records payments, and views reports.
- **Expense submitter**: A registered user who records an expense involving one or more
  other users.
- **Approver**: A group participant authorized by that group's approval process to
  review and approve or reject submitted expenses.
- **Group participant**: A registered user included in a group and potentially in its
  shared expenses.

### User Story 1 - Establish an Account and Group (Priority: P1)

**Requirements**: FR-001, FR-002, FR-006

A person creates or accesses their own account, signs in, and creates a group or joins an
existing group to organize shared expenses with other people.

**Why this priority**: Identifiable users and group membership are prerequisites for
attributing expenses and balances to the right people.

**Independent Test**: Create two accounts, sign in as each, create a group with both
participants, and verify that each can see the group while an unrelated user cannot.

**Acceptance Scenarios**:

1. **Given** a person has no account, **When** they register and sign in, **Then** they
   can access their own account and expense information.
2. **Given** a registered user, **When** they create a group and add another registered
   user, **Then** both users are listed as group participants.
3. **Given** a group owner and a registered user, **When** the owner invites that user,
  **Then** the user can join the group and is listed as a participant.
4. **Given** a group owner or group participant, **When** the owner removes a participant
  or a participant leaves, **Then** that user's group access ends.
5. **Given** a user who is not a group participant, **When** they try to view that
   group's expenses or balances, **Then** access is denied.

---

### User Story 2 - Record and Review a Shared Expense (Priority: P1)

**Requirements**: FR-003, FR-004, FR-005, FR-006, FR-008

A group participant records an expense with its amount, date, description, split method,
and involved users. The expense is reviewed before it affects group balances.

**Why this priority**: Expense entry and approval provide the source records from which
shared obligations are calculated.

**Independent Test**: Submit an expense for a group, approve it as an authorized reviewer,
and verify the recorded details and resulting participant shares.

**Acceptance Scenarios**:

1. **Given** a group participant and a valid group, **When** they submit an expense with
  an amount, date, description, participants, an equal split, and one payer selected
  from those participants, **Then** the expense is recorded as pending review with its
  submitter, payer, and shares shown for review.
2. **Given** a valid expense, **When** the submitter chooses nonnegative percentage
  shares totaling exactly 100%, **Then** each participant's proposed share is calculated
  from the assigned percentage, rounded to the currency's smallest unit, with any
  remaining units distributed in participant order so the shares equal the total.
3. **Given** a receipt image is available, **When** the submitter attaches it, **Then**
   it is associated with that expense; an expense without a receipt can also be submitted.
4. **Given** an expense awaiting review, **When** an authorized approver approves it,
   **Then** it becomes an approved expense and is included in balances.
5. **Given** an expense awaiting review, **When** an authorized approver rejects it,
   **Then** it is not included in balances and its review outcome is visible to the
   submitter.

---

### User Story 3 - Stay Informed of Group Activity (Priority: P2)

**Requirements**: FR-007

A group participant receives an in-app notification when another group member creates a
new expense and can open the expense to review its details and status.

**Why this priority**: Timely awareness helps participants review shared activity and
respond to expenses that require approval.

**Independent Test**: Create an expense as one participant and verify that other group
participants receive a notice that identifies the new expense and opens its status.

**Acceptance Scenarios**:

1. **Given** a group with multiple participants, **When** a participant creates an
  expense, **Then** the other group participants receive an in-app notification
  identifying that expense.
2. **Given** a participant has received an expense notification, **When** they open it,
   **Then** they can view the expense details and current review status.

---

### User Story 4 - Understand and Settle Balances (Priority: P1)

**Requirements**: FR-009, FR-010

A participant reviews what they owe to or are owed by each other participant and records
payments so the outstanding balances reflect settlement activity.

**Why this priority**: Accurate, understandable balances and payment records are the core
outcome of shared expense tracking.

**Independent Test**: Approve shared expenses among three participants, inspect each
pairwise balance, record a payment, and verify that only the relevant outstanding
obligation is reduced.

**Acceptance Scenarios**:

1. **Given** approved expenses involving multiple participants, **When** a participant
   views balances, **Then** the system shows the amount owed by each user to every other
   user based on approved expenses and recorded payments.
2. **Given** an expense is pending or rejected, **When** a participant views balances,
   **Then** that expense has not changed the balances.
3. **Given** an outstanding amount between two participants, **When** a payment is
  recorded for that obligation with payer, recipient, amount, and date, **Then** the
  payment is recorded against that pairwise balance and the remaining amount is updated;
  partial payments are allowed.
4. **Given** a payment exceeds the outstanding amount, **When** the user attempts to
   record it, **Then** the system prevents an invalid settlement or clearly identifies
   the excess for correction.

---

### User Story 5 - Review Expense and Settlement Reports (Priority: P2)

**Requirements**: FR-011

A participant reviews summaries of expenses, balances, and payments, and can request a
report using a date range and other available parameters.

**Why this priority**: Summaries help users understand group activity and verify that
payments and expenses have been accounted for.

**Independent Test**: Create approved and pending expenses and payments across dates,
request a date-filtered report, and verify that totals and included records match the
selected criteria.

**Acceptance Scenarios**:

1. **Given** a user has expenses, balances, or payments, **When** they request a summary,
   **Then** the report presents the relevant records and totals.
2. **Given** a user specifies a date range and other supported report parameters,
   **When** they generate a custom report, **Then** only records matching those criteria
   are included and the selected criteria are apparent.

---

### User Story 6 - Access SplitSmart on Required Platforms (Priority: P1)

**Requirements**: FR-012

A user accesses SplitSmart through its web product or its Android or iOS application and
uses the product capabilities available to their account.

**Why this priority**: The customer explicitly requires a primarily web-based product as
well as Android and iOS applications.

**Independent Test**: Verify that a user can sign in and access their account on the web,
Android, and iOS product experiences, and that each exposes the agreed customer-required
capabilities.

**Acceptance Scenarios**:

1. **Given** a user with an account and supported device or browser, **When** they access
  SplitSmart on the web, Android, or iOS, **Then** they can sign in and access the
  customer-required capabilities assigned to that platform.
2. **Given** a customer-required capability is available on one supported platform,
  **When** the user accesses that capability through another supported platform,
  **Then** it is available there or its documented platform limitation has been
  explicitly approved by the product owner.

---

### Edge Cases

- An expense must not be submitted with a zero or negative amount, a date that is not
  valid, no involved group participants, or participants outside the selected group.
- A percentage split must be rejected if any percentage is negative or the percentages
  do not total exactly 100%.
- Equal or percentage splitting may produce a remainder smaller than a currency unit;
  after rounding to the currency's smallest unit, remainder units must be assigned in
  participant order so displayed shares equal the expense amount.
- A group with one participant, a participant removed after an expense, or a group with
  no approved expenses must not produce misleading pairwise balances.
- A pending or rejected expense must not affect balances; repeated approval or payment
  actions must not apply the same change more than once.
- A payment for a settled, missing, or unrelated obligation, or one with a non-positive
  amount, must not silently alter another pairwise balance.
- Missing or inaccessible receipt images must not prevent users from viewing the other
  expense details, and the system must indicate that the attachment is unavailable.
- A report with an empty date range or no matching records must return a clear empty
  result rather than incorrect totals.
- Users must not receive access to another user's private account information or to a
  group's records when they are not authorized participants.

## Requirements *(mandatory)*

### Functional Requirements

The following inventory preserves the customer-requested capabilities. These are product
requirements independent of the provisional MVP selection below.

- **FR-001**: The system MUST provide each person using SplitSmart with an individual
  account, email-and-password sign-in, and access to their own expense and balance
  information.
- **FR-002**: The system MUST allow users to create groups; group owners MUST be able to
  invite and remove members, and a member MUST be able to leave a group.
- **FR-003**: The system MUST allow a user to create an expense with a positive amount,
  date, description, split method, the users involved in that expense, and exactly one
  payer selected from those participants; the submitter MAY be a different user.
- **FR-004**: The system MUST support equal and percentage-based expense splits, require
  nonnegative percentages totaling exactly 100%, and show each involved user's resulting
  share rounded to the currency's smallest unit. Any rounding remainder MUST be
  distributed in participant order so shares sum to the expense amount.
- **FR-005**: The system MUST allow a user to attach an optional receipt image to an
  expense.
- **FR-006**: The system MUST identify each expense's group participants and use only
  those participants when determining its shares and balance effects.
- **FR-007**: The system MUST notify other members of the relevant group when a member
  creates an expense. Notifications MUST be delivered in-app.
- **FR-008**: The system MUST provide an approval workflow that reviews expenses before
  they are included in balances. The workflow MUST support a review process; whether it
  uses one or multiple approval levels is an open question.
- **FR-009**: The system MUST track the amount each user owes to every other user based
  on approved expenses and recorded payments.
- **FR-010**: The system MUST allow users to record a payment with payer, recipient,
  positive amount, and date against a specific pairwise balance. Partial payments MUST be
  allowed, and the remaining balance MUST be updated accordingly.
- **FR-011**: The system MUST provide reports or summaries of users' expenses, balances,
  and payments, including custom reports with a date range and other parameters.
- **FR-012**: The product MUST be primarily web-based and MUST also provide applications
  that can run on Android and iOS.

### Non-Functional Requirements

- **NFR-001**: The system MUST restrict account, group, expense, receipt, and balance
  information to the account owner and authorized group participants, as appropriate.
- **NFR-002**: Changes to expenses, approvals, and payments MUST remain consistent when
  users revisit or refresh the relevant account and group views.
- **NFR-003**: Users MUST be able to complete the primary account, group, expense, and
  balance tasks using the supported product interfaces without relying on hidden or
  undocumented actions.
- **NFR-004**: The product MUST provide access to its customer-required capabilities on
  the web, Android, and iOS platforms; platform-specific differences MUST NOT silently
  remove a listed capability.
- **NFR-005**: Monetary shares, balances, and payment totals MUST be presented in a way
  that can be reconciled to their contributing expenses and payments.

### Java Swing MVP Subset (Implementation Decision)

This is a provisional implementation scope, not a change to the customer requirements
above. The Java Swing MVP subset covers individual accounts and sign-in (FR-001), group
management (FR-002), expense entry and split details with optional receipt (FR-003 through
FR-006), approval before balance changes (FR-008), pairwise balances (FR-009), payment
recording (FR-010), and basic expense, balance, and payment summaries from FR-011.

Group notifications (FR-007), custom report parameters beyond basic summaries (FR-011),
and delivery on the primarily web-based, Android, and iOS platforms (FR-012) remain in the
requirements inventory but are not claimed as satisfied by the Swing MVP. An explicit
platform architecture decision is required to determine how the Swing MVP relates to the
customer-required web and mobile product.

The project constitution separately constrains the implementation to Java 21 and Maven,
requires JUnit 5 tests and PlantUML architecture/workflow diagrams, requires `BigDecimal`
for monetary calculations, and requires human review of AI-generated work. These are
implementation and governance decisions, not customer requirements.

### Key Entities *(include if data involved)*

- **User Account**: A person's identity and access to their own SplitSmart information.
- **Group**: A managed collection of users that organizes shared expenses and balances.
- **Group Membership**: The relationship between a user and a group, including whether
  the user is its owner or a participant and the user's current access.
- **Expense**: A submitted shared cost with amount, date, description, submitter, one
  payer selected from its participants, group, participants, split method, proposed
  shares, optional receipt, and review status.
- **Expense Share**: A participant's assigned portion of an expense under its split method.
- **Approval**: A review decision and its reviewer, outcome, and associated expense.
- **Notification**: An in-app notice of a new group expense delivered to relevant
  participants.
- **Balance**: The outstanding amount owed by one user to another, derived from approved
  expense shares and recorded payments.
- **Payment**: A record of payer, recipient, positive amount, and date associated with a
  specific pairwise balance; a payment may settle part or all of that balance.
- **Report**: A summary or filtered view of expenses, balances, and payments.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: In a representative usability test, at least 90% of users can create an
  account, join a group, and submit a valid shared expense without assistance.
- **SC-002**: For every approved test expense, the sum of participant shares equals the
  expense amount under the documented split and rounding rules.
- **SC-003**: In all balance verification test cases, displayed pairwise balances equal
  approved expense obligations minus recorded payments, with no pending or rejected
  expense included.
- **SC-004**: After a valid payment is recorded, the corresponding outstanding balance
  reflects that payment in the next displayed balance view.
- **SC-005**: In a report verification test set, every included record matches the
  selected date range and parameters, and the reported totals equal the included records.
- **SC-006**: All customer-requested capabilities in FR-001 through FR-012 have a recorded
  verification result before the full product is declared to meet this specification.
- **SC-007**: In an authorization test set, no non-member can retrieve protected group
  expenses, receipts, or balances, and one user's private account information is not
  exposed to another user.

## Assumptions

- Each user has a unique account; a group consists of registered users.
- An expense affects balances only after approval, as stated by the customer's approval
  requirement.
- The person who paid an expense is identifiable so the system can determine who is owed
  and calculate pairwise balances.
- Basic summaries are included in the provisional MVP; advanced custom report parameters
  are retained for the full product scope.
- The Java Swing MVP is a desktop implementation slice, not a replacement for the
  customer's web, Android, or iOS requirements.
- No data retention period or supported currency list was specified; this draft does not
  choose one.

## Open Questions

1. What product architecture and delivery sequence will satisfy the primarily web-based,
  Android, and iOS requirements while accommodating the requested Java Swing MVP?
2. Who may approve an expense, and should approval be a single review or multiple ordered
  approval levels configurable per group?