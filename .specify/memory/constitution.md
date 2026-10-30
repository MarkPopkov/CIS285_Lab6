<!--
Sync Impact Report
Version change: unversioned scaffold -> 1.0.0
Modified principles: none; the scaffold's principle slots were unfilled.
Added principles: Customer Requirements and Traceability; Java 21 and Maven;
	Object-Oriented Separation of Concerns; Monetary Precision;
	Automated Verification, Modeling, and Human Accountability.
Added sections: Product and Platform Constraints; Delivery and Review.
Removed sections: none.
Follow-up TODOs: Confirm the original ratification date; resolve the client
	architecture that satisfies web, Android, and iOS requirements alongside
	the requested Java Swing implementation constraint.
-->
# SplitSmart Constitution

## Core Principles

### I. Customer Requirements and Traceability
Customer requirements are the authoritative statement of user-visible outcomes. Every
requirement MUST have a stable identifier and traceable links to its design decisions,
implementation, and verification evidence. Implementation choices MUST be recorded
separately and MUST NOT be presented as customer requirements. This distinction keeps
technical constraints from silently changing the promised product.

### II. Java 21 and Maven
The Java application MUST target Java 21 and use Maven for dependency management, builds,
and test execution. The desktop GUI MUST be implemented using Java Swing. Any deviation
MUST be documented and approved before implementation. These constraints provide a
consistent, reproducible baseline; they do not redefine the customer-facing platform
requirements.

### III. Object-Oriented Separation of Concerns
The design MUST use cohesive object-oriented components with explicit responsibilities.
GUI code MUST be separated from business logic; business rules MUST be callable and
testable without constructing or displaying Swing components. Dependencies between the
GUI and business logic MUST flow through defined interfaces or application services.
This keeps user interaction changes from altering domain behavior and enables focused tests.

### IV. Monetary Precision
All monetary amounts and calculations MUST use `BigDecimal` with documented scale and
rounding rules. Binary floating-point types MUST NOT be used for money. Tests MUST cover
rounding and allocation behavior for equal and percentage-based splits so that balances
remain explainable and reproducible.

### V. Automated Verification, Modeling, and Human Accountability
Automated tests MUST use JUnit 5. Material architecture and workflow decisions MUST be
represented in PlantUML diagrams and kept consistent with the implementation. AI-generated
code, tests, diagrams, and documentation MUST receive human review before acceptance; the
reviewer MUST verify correctness, requirement alignment, and policy compliance. Automation
supports review but does not replace accountable human judgment.

## Product and Platform Constraints
The customer request describes account and group management, shared expense entry,
notifications, approval, balances, payments, reporting, and a primarily web-based product
with Android and iOS apps. These are customer-facing requirements, not implementation
decisions. Java Swing is an implementation constraint for a Swing client; it does not by
itself satisfy web, Android, or iOS delivery. The design MUST identify how each requested
platform will be supported and MUST surface any incompatibility for an explicit decision
before implementation. No implementation choice may silently waive a customer requirement.

## Delivery and Review
Each change MUST identify the customer requirement identifiers it addresses, or state that
it is technical work with no direct customer requirement. Verification MUST include JUnit 5
tests appropriate to the changed behavior and evidence that relevant requirements remain
covered. PlantUML diagrams MUST be updated when a change materially alters the architecture
or workflow. Human reviewers MUST check requirement traceability, monetary correctness,
GUI/business-logic separation, and any AI-generated contribution before acceptance.

## Governance
This constitution governs project design and delivery decisions. Amendments MUST be proposed
as a documented change, reviewed and approved by the project owner, and accompanied by an
impact assessment for affected requirements, designs, tests, and existing work. The
constitution version MUST follow semantic versioning: MAJOR for incompatible governance
changes, MINOR for new principles or materially expanded obligations, and PATCH for
non-semantic clarifications. The last-amended date MUST be updated for every accepted
amendment. Reviews of plans, changes, and releases MUST check compliance with these
principles; exceptions MUST be documented with rationale, scope, approver, and duration.

**Version**: 1.0.0 | **Ratified**: TODO(RATIFICATION_DATE): Original adoption date is not available in project context. | **Last Amended**: 2026-10-29
