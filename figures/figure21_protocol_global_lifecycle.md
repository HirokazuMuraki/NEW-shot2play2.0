# Figure21 Protocol Global Lifecycle

## Document Information

| Item | Value |
|---|---|
| Figure ID | Figure21 |
| Title | Protocol Global Lifecycle |
| Version | NEW-shot2play Protocol Suite Version 2.0 |
| Revision | Design Definition Version 2.0 |
| Status | Design Definition |
| Purpose | Global lifecycle model integrating authentication, authorization, enforcement, consumption, entitlement state, failure recovery, and audit |

---

# 1. Purpose

Figure21 defines the global lifecycle of the NEW-shot2play Protocol Suite
Version 2.0.

The model integrates the major protocol stages defined by the preceding
figures into one logical lifecycle.

The lifecycle provides a continuous relationship between:

- Authentication
- Authorization
- Enforcement
- Service Execution
- Consumption
- Entitlement State
- Failure Handling
- Recovery
- Audit
- Completion

---

# 2. Global Lifecycle Principle

A protected service transaction SHALL proceed through a controlled
sequence.

The representative lifecycle is:

Transaction Initiation

        ↓

Authentication

        ↓

Authorization Request

        ↓

Authorization Evaluation

        ↓

Authorization Decision

        ↓

Enforcement

        ↓

Service Execution

        ↓

Consumption

        ↓

Entitlement State Update

        ↓

Audit

        ↓

Transaction Completion

Each stage SHALL remain correlated with the logical transaction.

---

# 3. Transaction Initiation

A transaction begins when a protected service operation is requested.

The transaction initiation stage SHOULD establish:

- Transaction Identifier
- Subject Reference
- Resource Reference
- Requested Action
- Request Time
- Request Context

The Transaction Identifier SHALL remain associated with subsequent
processing.

---

# 4. Authentication

The authentication stage establishes the identity or authentication
context associated with the transaction.

Representative authentication context includes:

- Subject Identifier
- Device Identifier
- Authentication Identifier
- Authentication Method
- Challenge Reference
- Authentication Time
- Authentication Result

Successful authentication permits progression to authorization.

Authentication failure SHALL terminate or reject the protected
transaction according to the security policy.

---

# 5. Authorization Request

The authorization request establishes the decision context.

Representative inputs include:

- Subject
- Resource
- Action
- Transaction
- Authentication Context
- Entitlement Context
- Policy Context

The request SHALL be evaluated before protected service enforcement.

---

# 6. Authorization Evaluation

The authorization evaluation stage determines whether the requested
action is permitted.

Representative evaluation inputs include:

- Subject
- Resource
- Action
- Policy
- Entitlement
- Authentication Context
- Transaction Context

The evaluation SHALL produce an authorization decision.

---

# 7. Authorization Decision

The authorization decision SHALL produce a decision result.

Representative results include:

- Allow
- Deny
- Conditional Allow
- Indeterminate
- Error

The decision SHOULD include:

- Decision Identifier
- Transaction Identifier
- Policy Reference
- Entitlement Reference
- Decision Time
- Decision Expiration
- Decision Result

A Deny or Error decision SHALL NOT proceed to protected service
execution.

---

# 8. Enforcement

The enforcement stage applies the authorization decision to the
requested protected operation.

The enforcement point SHALL verify:

- Decision validity
- Transaction correlation
- Resource correlation
- Action correlation
- Decision expiration
- Required security conditions

Only an effective Allow decision SHALL permit protected execution.

---

# 9. Service Execution

Service execution performs the protected operation.

The service execution stage SHOULD establish:

- Service Action Identifier
- Transaction Identifier
- Resource Identifier
- Execution Time
- Execution Result

Authorization success SHALL NOT by itself imply service execution
success.

---

# 10. Consumption

Where the protected operation consumes an entitlement, the consumption
stage SHALL create a Consumption Transaction.

Representative data includes:

- Consumption Transaction Identifier
- Transaction Identifier
- Entitlement Identifier
- Consumed Amount
- Consumption Time
- State Version
- Transaction Status

Consumption SHALL be bound to the logical transaction.

---

# 11. Entitlement State Update

A successful consumption operation MAY cause an entitlement state
transition.

Representative states include:

- Active
- Reserved
- Consumed
- Exhausted
- Suspended
- Revoked
- Expired

The state transition SHOULD record:

- Previous State
- New State
- State Version
- Update Time
- Update Transaction Identifier

The state transition SHALL remain traceable to the transaction.

---

# 12. Audit

Audit records SHALL preserve the relevant lifecycle events.

Representative audit events include:

- Transaction Initiation
- Authentication
- Authorization Request
- Authorization Decision
- Enforcement
- Service Execution
- Consumption
- Entitlement State Update
- Failure
- Recovery
- Compensation
- Reconciliation
- Transaction Completion

The Transaction Identifier SHOULD provide the primary correlation key.

---

# 13. Failure Branch

A failure MAY occur at any processing stage.

Representative failure points include:

- Authentication
- Authorization
- Enforcement
- Service Execution
- Consumption
- Entitlement State Update
- Audit

A failure SHALL be classified according to the stage in which it occurs.

The failure record SHOULD include:

- Failure Identifier
- Transaction Identifier
- Failure Stage
- Failure Classification
- Failure Time
- Failure Reason
- Retryability

---

# 14. Recovery Branch

Recoverable failures MAY enter the recovery state.

The recovery process SHOULD include:

- Recovery Identifier
- Original Transaction Identifier
- Recovery State
- Retry Count
- Recovery Action
- Recovery Result

Recovery SHALL remain correlated with the original transaction.

---

# 15. Retry

A retry MAY repeat a failed processing stage.

A retry SHALL NOT unintentionally create a duplicate logical transaction.

Idempotency controls SHOULD be used where repeated execution could
cause duplicate effects.

The Consumption Transaction Identifier SHOULD remain stable across
retry operations where the retry represents the same logical
consumption.

---

# 16. Compensation

Where a partially completed transaction cannot be safely retried,
compensation MAY be performed.

Compensation SHOULD reference:

- Original Transaction Identifier
- Original Action
- Compensation Identifier
- Compensation Action
- Compensation Result

Compensation SHALL be auditable.

---

# 17. Reconciliation

Where the protocol state becomes inconsistent, reconciliation MAY be
performed.

Reconciliation SHOULD compare:

- Authorization Records
- Enforcement Records
- Service Results
- Consumption Records
- Entitlement State
- Audit Records

The reconciliation result SHALL establish the final consistent state.

---

# 18. Completion

A transaction SHALL reach a final lifecycle state.

Representative final outcomes include:

- Completed
- Denied
- Failed
- Recovered
- Compensated
- Reconciled
- Expired

The final outcome SHALL be recorded in the transaction and audit
context.

---

# 19. State Relationships

The global lifecycle can be represented as:

INITIATED

        ↓

AUTHENTICATING

        ↓

AUTHENTICATED

        ↓

AUTHORIZING

        ↓

AUTHORIZED

        ↓

ENFORCING

        ↓

EXECUTING

        ↓

CONSUMING

        ↓

ENTITLEMENT_UPDATED

        ↓

AUDITING

        ↓

COMPLETED

Failure states MAY branch from intermediate states.

Recoverable failures MAY transition to:

RECOVERY

        ↓

RETRY

        ↓

original processing state

Non-recoverable failures MAY transition to:

FAILED

Compensatable failures MAY transition to:

COMPENSATION

        ↓

RECONCILIATION

        ↓

FINAL STATE

---

# 20. Global Transaction Correlation

The Transaction Identifier SHALL remain available throughout the
lifecycle.

Representative correlation:

Transaction ID

        ↓

Authentication ID

        ↓

Authorization Request ID

        ↓

Authorization Decision ID

        ↓

Service Action ID

        ↓

Consumption Transaction ID

        ↓

Entitlement State Version

        ↓

Failure / Recovery / Compensation / Reconciliation IDs

        ↓

Audit Records

This correlation permits reconstruction of the complete logical
transaction.

---

# 21. Security Boundary

The lifecycle SHALL maintain security boundaries between:

- Authentication
- Authorization
- Enforcement
- Service Execution
- Entitlement Management
- Audit

No stage SHALL implicitly inherit authority beyond the authority
explicitly provided by the protocol.

---

# 22. Authorization-to-Consumption Binding

An authorization decision SHALL be associated with the protected
transaction that consumes the relevant entitlement.

The consumption operation SHALL NOT rely solely on a prior generic
authorization state without verifying the transaction and entitlement
context required by the policy.

---

# 23. Failure-to-Recovery Binding

Every recovery operation SHOULD reference the failure and original
transaction that caused the recovery.

Representative relationship:

Transaction

        ↓

Failure

        ↓

Recovery

        ↓

Retry / Compensation / Reconciliation

        ↓

Final Outcome

This prevents recovery processing from becoming detached from the
original transaction.

---

# 24. Audit Closure

A lifecycle SHOULD not be considered fully closed until the relevant
transaction outcome and security-relevant events have been recorded.

The audit closure SHOULD include:

- Final Transaction Result
- Final Entitlement State
- Recovery Result, if applicable
- Compensation Result, if applicable
- Reconciliation Result, if applicable
- Completion Time

---

# 25. Version 2.0 Representative Flow

A normal successful transaction is:

Transaction Initiation

        ↓

Authentication

        ↓

Authorization

        ↓

Allow

        ↓

Enforcement

        ↓

Service Execution

        ↓

Consumption

        ↓

Entitlement State Update

        ↓

Audit

        ↓

Completed

A failure transaction is:

Transaction

        ↓

Failure

        ↓

Recovery

        ↓

Retry / Compensation / Reconciliation

        ↓

Final Outcome

Both paths remain correlated by the Transaction Identifier.

---

# 26. Security Properties

The global lifecycle provides:

- End-to-end transaction control
- Authentication-to-authorization continuity
- Authorization-to-enforcement continuity
- Enforcement-to-consumption continuity
- Entitlement state traceability
- Failure containment
- Recovery traceability
- Compensation traceability
- Reconciliation traceability
- Audit closure
- Transaction reconstruction
- Duplicate-effect prevention through idempotency controls

---

# 27. Non-Goals

Figure21 does not define:

- A specific implementation language
- A specific database
- A specific logging platform
- A specific workflow engine
- A specific transaction manager
- A specific cloud service
- A specific user interface

Implementation details MAY be defined elsewhere.

---

# 28. Design Freeze Decision

Figure21 is designated as the global lifecycle reference model for the
NEW-shot2play Protocol Suite Version 2.0.

Future modifications require Design Freeze review.

