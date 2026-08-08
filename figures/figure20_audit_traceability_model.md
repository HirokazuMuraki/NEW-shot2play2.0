# Figure20 Audit and Traceability Model

## Document Information

| Item | Value |
|---|---|
| Figure ID | Figure20 |
| Title | Audit and Traceability Model |
| Version | NEW-shot2play Protocol Suite Version 2.0 |
| Revision | Design Definition Version 2.0 |
| Status | Design Definition |
| Purpose | Definition of transaction traceability, audit correlation, security event recording, and recovery history |

---

# 1. Purpose

Figure20 defines the audit and traceability model for the NEW-shot2play
Protocol Suite Version 2.0.

The model provides a common traceability structure across:

- Authentication
- Authorization
- Enforcement
- Service Action
- Consumption Transaction
- Entitlement State
- Failure Handling
- Recovery
- Compensation
- Reconciliation

The model ensures that a protocol transaction can be reconstructed
from its related records.

---

# 2. Traceability Principle

Each protected transaction SHALL have a unique Transaction Identifier.

The Transaction Identifier SHALL provide the primary correlation
reference across the processing chain.

Representative correlation identifiers include:

- Transaction Identifier
- Authentication Identifier
- Authorization Request Identifier
- Authorization Decision Identifier
- Entitlement Identifier
- Consumption Transaction Identifier
- Recovery Identifier
- Compensation Identifier
- Reconciliation Identifier

---

# 3. Transaction Trace

The principal trace sequence is:

Transaction Initiation

        ↓

Authentication Context

        ↓

Authorization Request

        ↓

Authorization Decision

        ↓

Service Enforcement

        ↓

Service Action

        ↓

Consumption Transaction

        ↓

Entitlement State Update

        ↓

Audit Record

The trace SHALL preserve the relationship between these stages.

---

# 4. Authentication Trace

The authentication stage SHOULD provide a traceable reference.

Representative information includes:

- Authentication Identifier
- Subject Identifier
- Device Identifier
- Authentication Method
- Authentication Time
- Authentication Result
- Challenge Identifier
- Registration Reference

The authentication trace SHALL NOT expose private authentication
material.

---

# 5. Authorization Trace

The authorization stage SHOULD record:

- Authorization Request Identifier
- Transaction Identifier
- Subject Identifier
- Resource Identifier
- Action
- Policy Reference
- Entitlement Reference
- Evaluation Time
- Decision
- Decision Identifier
- Decision Expiration

The authorization trace SHALL allow the decision to be correlated with
the protected action.

---

# 6. Enforcement Trace

The Service Enforcement Point SHOULD record:

- Transaction Identifier
- Decision Identifier
- Enforcement Time
- Protected Resource
- Requested Action
- Enforcement Result
- Enforcement Failure Reason

The enforcement record SHALL establish whether the authorization
decision was actually applied.

---

# 7. Service Action Trace

The service execution stage SHOULD record:

- Transaction Identifier
- Service Action Identifier
- Service Identifier
- Resource Identifier
- Action
- Execution Time
- Execution Result
- Service Result Reference

A successful authorization SHALL NOT be interpreted as successful
service execution.

---

# 8. Consumption Trace

A Consumption Transaction SHOULD contain:

- Consumption Transaction Identifier
- Transaction Identifier
- Entitlement Identifier
- Consumed Amount
- Consumption Time
- Consumption Result
- Idempotency Key
- State Version
- Transaction Status

The Consumption Transaction Identifier SHALL be stable across retry
operations.

---

# 9. Entitlement State Trace

The Entitlement State record SHOULD contain:

- Entitlement Identifier
- Subject Identifier
- Resource Identifier
- Current State
- Previous State
- State Version
- Update Time
- Update Transaction Identifier

State transitions SHOULD be traceable to the transaction that caused
the transition.

---

# 10. Failure Trace

A failure record SHOULD contain:

- Transaction Identifier
- Failure Identifier
- Failure Boundary
- Failure Classification
- Failure Time
- Failure Reason
- Retryability
- Related Decision Identifier
- Related Consumption Identifier

The failure record SHALL distinguish the failure stage.

Representative failure boundaries include:

- Authorization
- Enforcement
- Service Action
- Consumption
- Entitlement State Update

---

# 11. Recovery Trace

A recovery record SHOULD contain:

- Recovery Identifier
- Transaction Identifier
- Recovery State
- Recovery Start Time
- Recovery Action
- Retry Count
- Last Retry Time
- Recovery Result
- Final State

Recovery processing SHALL remain correlated with the original
transaction.

---

# 12. Compensation Trace

A compensation record SHOULD contain:

- Compensation Identifier
- Original Transaction Identifier
- Original Action
- Compensation Action
- Compensation Time
- Compensation Result
- Related Entitlement Identifier

Compensation SHALL be traceable to the transaction that caused the
compensating action.

---

# 13. Reconciliation Trace

A reconciliation record SHOULD contain:

- Reconciliation Identifier
- Transaction Identifier
- Compared Records
- Detected Inconsistency
- Reconciliation Action
- Reconciliation Time
- Reconciliation Result
- Final Consistent State

Reconciliation SHALL provide a traceable explanation of how an
inconsistent state was resolved.

---

# 14. Audit Event

An Audit Event represents a security-relevant or transaction-relevant
event.

Representative event categories include:

- Authentication
- Registration
- Authorization Request
- Authorization Decision
- Enforcement
- Service Action
- Consumption
- State Update
- Failure
- Retry
- Compensation
- Reconciliation
- Administrative Change

Each Audit Event SHOULD contain:

- Event Identifier
- Event Type
- Event Time
- Transaction Identifier
- Actor or Subject Reference
- Resource Reference
- Result
- Related Identifier

---

# 15. Common Correlation Key

The preferred correlation model is:

Transaction ID

        ├── Authentication ID
        ├── Authorization Request ID
        ├── Authorization Decision ID
        ├── Enforcement Record
        ├── Service Action ID
        ├── Consumption Transaction ID
        ├── Entitlement State Version
        ├── Failure ID
        ├── Recovery ID
        ├── Compensation ID
        └── Reconciliation ID

The Transaction Identifier SHALL be sufficient to identify the logical
transaction across the protocol processing chain.

---

# 16. Traceability Graph

The audit system SHOULD support the following relationship:

Transaction

        ↓

Authentication Context

        ↓

Authorization

        ↓

Enforcement

        ↓

Service Action

        ↓

Consumption

        ↓

Entitlement State

        ↓

Audit / Recovery History

Each node SHOULD reference the preceding and relevant succeeding
transaction context.

---

# 17. Audit Integrity

Audit records SHOULD be protected against unauthorized modification.

Protection mechanisms MAY include:

- Access Control
- Integrity Protection
- Append-Only Storage
- Cryptographic Hashing
- Digital Signature
- Secure Retention
- Immutable Storage

The specific mechanism is implementation-dependent.

---

# 18. Audit Confidentiality

Audit records SHALL NOT contain unnecessary sensitive authentication
material.

In particular, audit records SHALL NOT contain:

- Private Keys
- Authentication Secrets
- Raw Credential Material
- Unnecessary Personal Data

Sensitive identifiers SHOULD be represented using appropriate
references or protected representations.

---

# 19. Audit Retention

Audit records SHOULD be retained for a period sufficient to support:

- Security Investigation
- Transaction Dispute Resolution
- Recovery Analysis
- Compliance Requirements
- Operational Diagnostics

Retention duration is implementation- and policy-dependent.

---

# 20. Time and Ordering

Audit events SHOULD contain a reliable event timestamp.

Where distributed components are involved, the system SHOULD preserve:

- Event Time
- Processing Time
- Transaction Ordering
- State Version

Where exact global ordering cannot be guaranteed, transaction
identifiers and state versions SHALL provide correlation.

---

# 21. Administrative Trace

Administrative operations affecting security-relevant state SHOULD be
auditable.

Examples include:

- Device Registration
- Device Revocation
- Registration Termination
- Entitlement Modification
- Policy Modification
- Authorization Configuration Change
- Recovery Override
- Manual Reconciliation

Administrative events SHOULD identify the responsible administrative
actor or system component.

---

# 22. Security Investigation

The traceability model SHOULD allow an investigator to start from any
major identifier and reconstruct the associated transaction.

Examples:

Transaction ID

        ↓

Authorization Decision

        ↓

Service Action

        ↓

Consumption

        ↓

Entitlement State

or:

Entitlement ID

        ↓

Consumption History

        ↓

Transaction ID

        ↓

Authorization Decision

        ↓

Authentication Context

---

# 23. Dispute Resolution

The audit model SHOULD support reconstruction of:

- What was requested
- Who or what requested it
- Which authorization decision was made
- Which policy and entitlement were evaluated
- Whether enforcement occurred
- Whether service execution occurred
- Whether consumption occurred
- Which entitlement state was changed
- Whether recovery or compensation occurred

The reconstructed sequence SHOULD be independently verifiable from
the retained records.

---

# 24. Recovery and Audit Relationship

Every recovery operation SHOULD reference the original transaction.

Representative relationship:

Original Transaction

        ↓

Failure Record

        ↓

Recovery Record

        ↓

Retry / Compensation / Reconciliation

        ↓

Final Outcome

This relationship SHALL prevent recovery activity from becoming
detached from the original transaction history.

---

# 25. Version 2.0 Example

Representative transaction:

Transaction ID = T-2026-XXXX

        ↓

Authentication ID

        ↓

Authorization Decision = Allow

        ↓

Service Enforcement = Applied

        ↓

Service Action = Completed

        ↓

Consumption Transaction = Recorded

        ↓

Entitlement State Version = N+1

        ↓

Audit Record = Complete

If a failure occurs:

Transaction ID = T-2026-XXXX

        ↓

Failure Record

        ↓

Recovery ID

        ↓

Retry / Compensation / Reconciliation

        ↓

Final Outcome

All records remain correlated to the same logical transaction.

---

# 26. Security Properties

The model provides:

- End-to-end transaction traceability
- Authorization traceability
- Enforcement traceability
- Consumption traceability
- Entitlement state traceability
- Failure traceability
- Recovery traceability
- Compensation traceability
- Reconciliation traceability
- Administrative accountability
- Audit integrity
- Investigation support
- Dispute resolution support

---

# 27. Non-Goals

Figure20 does not define:

- A specific logging platform
- A specific SIEM
- A specific database
- A specific log transport protocol
- A specific retention product
- A specific cryptographic storage technology

Implementation-specific mechanisms MAY be defined elsewhere.

---

# 28. Design Freeze Decision

Figure20 is designated as the Audit and Traceability reference model
for NEW-shot2play Protocol Suite Version 2.0.

Future modifications require Design Freeze review.

