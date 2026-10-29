<!--
Sync Impact Report
Version change: unversioned scaffold -> 1.0.0 (initial adoption)
Modified principles: unresolved scaffold slots -> Customer Requirements and Traceability;
  Java 21 and Maven; Object-Oriented Separation; Financial Accuracy; Automated Testing;
  PlantUML Design Records; Human Review of AI Output; Swing Is an Implementation Decision
Added sections: Additional Constraints; Development Workflow
Removed sections: none
Follow-up TODOs: Confirm the original ratification date.
-->

# SplitSmart Constitution

## Core Principles

### I. Customer Requirements and Traceability
The customer requirements in `UserRequirement.md` are the authoritative product baseline.
Every customer requirement MUST be traceable to relevant design decisions, implementation,
and verification evidence. Implementation choices MUST be identified separately and MUST NOT
be presented as customer requirements. A change to a customer requirement requires documented
customer approval before the baseline is changed.

### II. Java 21 and Maven
Application code MUST target Java 21 and use Maven for dependency management, builds, and
standard verification. Build configuration MUST make the Java and test requirements explicit
so that contributors can reproduce verification consistently.

### III. Object-Oriented Separation
The application MUST use clear object-oriented boundaries. GUI code MUST remain separate from
business logic; business rules and balance calculations MUST NOT be implemented in Swing
event handlers or view classes. Business logic MUST be testable without launching the GUI.
This separation limits UI changes from altering domain behavior.

### IV. Financial Accuracy
All monetary values and calculations MUST use `BigDecimal`; binary floating-point types MUST
NOT be used for money. Currency scale and rounding behavior MUST be explicit at calculation
and persistence boundaries, and tests MUST cover rounding and representative balance updates.

### V. Automated Testing
JUnit 5 MUST be used for automated Java tests. Tests MUST verify business behavior, including
expense splits, approvals, payments, and resulting balances, and MUST be runnable through
Maven verification. GUI-specific tests MUST NOT replace tests of business logic independent of
the GUI.

### VI. PlantUML Design Records
PlantUML MUST be used for maintained architecture and design diagrams when a diagram is needed
to communicate system structure or important interactions. Diagrams MUST reflect approved
design decisions and MUST be updated when those decisions change.

### VII. Human Review of AI Output
AI-generated code, tests, diagrams, and documentation MUST be reviewed and understood by a
human before acceptance. The reviewer MUST verify correctness, requirement traceability, and
test results; AI output MUST NOT be treated as evidence of approval or verification by itself.

### VIII. Swing Is an Implementation Decision
Java Swing is an implementation choice, not a customer requirement. The original customer
request specifies a primarily web-based product. A Swing interface MUST NOT be represented as
satisfying that web-based requirement. Implementing Swing as the product's primary interface
requires documented customer approval of the resulting change in delivery platform.

## Additional Constraints

`UserRequirement.md` describes the customer baseline: user accounts, group management, expense
creation (including equal or percentage splits and optional receipt images), notifications,
expense approval, balance tracking, payment tracking, reporting, and primary web-based delivery.
This constitution does not modify that file.

Java 21, Maven, Swing, JUnit 5, PlantUML, object-oriented design, GUI/business-logic separation,
and `BigDecimal` are implementation constraints. They do not supersede customer requirements.
In particular, the Swing and web-based delivery mismatch MUST remain visible in planning and
review until the customer resolves it.

## Development Workflow

Each change MUST identify the customer requirement or implementation decision it addresses.
The change MUST include or update traceability evidence linking relevant requirements to design,
code, and tests. Maven verification MUST pass before the change is accepted, and a human reviewer
MUST inspect AI-generated contributions under Principle VII.

## Governance

Amendments MUST be proposed with their rationale and impact on customer requirements,
implementation decisions, traceability, and existing work. The project owner or authorized
reviewer MUST approve amendments before adoption. Approved amendments MUST update this document
and its version/date fields; changes to customer requirements also require documented customer
approval and an update to the authoritative requirements source.

Constitution versions use MAJOR.MINOR.PATCH. Increase MAJOR for incompatible governance or
principle changes, MINOR for added principles or materially expanded obligations, and PATCH for
clarifications that do not change obligations.

Reviews before acceptance and release MUST check compliance with these principles, verify
traceability for affected requirements, and record unresolved deviations. A deviation MUST have
an explicit rationale and approval; it MUST NOT silently weaken a customer requirement.

**Version**: 1.0.0 | **Ratified**: TODO(RATIFICATION_DATE): original adoption date unavailable | **Last Amended**: 2026-10-29
