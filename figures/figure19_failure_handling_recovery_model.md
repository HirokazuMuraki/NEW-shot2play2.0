# Figure19 Failure Handling and Recovery Model

## Document Information

| Item | Value |
|---|---|
| Figure ID | Figure19 |
| Title | Failure Handling and Recovery Model |
| Version | NEW-shot2play Protocol Suite Version 2.0 |
| Revision | Design Definition Version 2.0 |
| Status | Design Definition |
| Purpose | Definition of failure boundaries, recovery behavior, retry control, compensation, and authorization re-evaluation |

---

# 1. Purpose

Figure19 defines the failure handling and recovery model for the
authorization, enforcement, service execution, consumption, and
entitlement state transition flow.

The model ensures that a failure at any processing boundary does not
silently result in an inconsistent or unauthorized state.

The model distinguishes:

- Authorization Failure
- Enforcement Failure
- Service Action Failure
- Consumption Failure
- State Update Failure
- Retry
- Compensation
- Reconciliation
- Authorization Re-evaluation

---

# 2. Normal Processing Baseline

The normal processing sequence is:

Authorization Request

        ↓

Authorization Evaluation

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

Figure19 uses this sequence as the baseline for identifying failure
boundaries.

---

# 3. Failure Boundary Model

Each major processing boundary SHALL have an identifiable outcome.

The principal boundaries are:

- B1 Authorization
- B2 Enforcement
- B3 Service Action
- B4 Consumption
- B5 Entitlement State Update

A failure at one boundary SHALL NOT automatically be interpreted as
successful completion of the subsequent boundary.

---

# 4. Authorization Failure

Authorization failure includes:

- Missing authorization request
- Invalid authorization request
- Invalid authentication context
- Invalid entitlement
- Policy evaluation failure
- Authorization service unavailable
- Invalid authorization result

Default behavior:

Authorization Failure

        ↓

Protected Service Action Not Executed

        ↓

No Consumption

The operation MAY be retried after the underlying condition is
resolved.

---

# 5. Enforcement Failure

An enforcement failure occurs when the Service Enforcement Point
cannot reliably establish whether the requested action is authorized.

Examples include:

- Missing decision
- Invalid decision
- Expired decision
- Decision binding mismatch
- Integrity failure
- Authorization service communication failure

Default behavior:

Enforcement Failure

        ↓

Fail Closed

        ↓

Protected Service Action Not Executed

---

# 6. Service Action Failure

A Service Action may fail after authorization has been granted.

Examples include:

- Service unavailable
- Application error
- External service error
- Resource unavailable
- Business rule failure
- Timeout

The implementation SHALL distinguish:

- Authorization succeeded
- Service Action failed

Authorization success SHALL NOT by itself imply successful
consumption.

---

# 7. Consumption Failure

A Consumption Transaction may fail after the Service Action has
completed.

Examples include:

- Transaction persistence failure
- Database unavailable
- Duplicate transaction
- Idempotency conflict
- Concurrency conflict
- Transaction timeout

The implementation SHALL prevent an inconsistent entitlement state.

The recovery strategy MAY include:

- Retry
- Idempotent replay
- Transaction rollback
- Compensation
- Reconciliation

---

# 8. State Update Failure

An Entitlement State Update may fail after the Consumption
Transaction has been recorded.

Examples include:

- State store unavailable
- Optimistic concurrency conflict
- Version mismatch
- Transaction conflict
- Persistence failure

The system SHALL preserve the relationship between the Consumption
Transaction and the intended state update.

The system SHALL NOT silently discard a successful consumption record.

---

# 9. Retry Model

Retry MAY be performed when the failure condition is transient.

Retry SHALL be controlled by:

- Retry Count
- Backoff
- Maximum Retry Duration
- Idempotency Key
- Transaction Identifier
- Failure Classification

A retry SHALL NOT create an additional entitlement consumption.

---

# 10. Retryable Failures

Typical retryable failures include:

- Temporary network failure
- Temporary service unavailability
- Database timeout
- Transient concurrency conflict
- Temporary external dependency failure

Retryability SHALL be determined by the applicable Service Profile
and implementation policy.

---

# 11. Non-Retryable Failures

Typical non-retryable failures include:

- Invalid authorization
- Expired authorization
- Invalid entitlement
- Policy denial
- Subject mismatch
- Resource mismatch
- Integrity failure
- Malformed request

A non-retryable failure SHALL NOT be repeatedly retried without a
new authorization or corrected request.

---

# 12. Authorization Re-Evaluation

If an authorization result has expired or is no longer applicable,
a new authorization evaluation MAY be required.

The re-evaluation sequence is:

Previous Authorization Result

        ↓

Expired / Invalid / Not Applicable

        ↓

New Authorization Request

        ↓

New Authorization Evaluation

        ↓

New Authorization Decision

A previous decision SHALL NOT be assumed to remain valid.

---

# 13. Compensation

Compensation MAY be required when a Service Action has succeeded but
a subsequent transaction boundary has failed.

Representative sequence:

Service Action Successful

        ↓

Consumption / State Update Failure

        ↓

Compensation Required

        ↓

Compensating Action

        ↓

Final Consistent State

Compensation behavior SHALL be defined by the applicable Service
Profile.

---

# 14. Rollback

Rollback MAY be used when the implementation provides atomic
transaction semantics.

Where rollback is not possible, the implementation SHALL use
compensation or reconciliation.

Rollback SHALL NOT be assumed to be available across independent
external services.

---

# 15. Reconciliation

Reconciliation SHALL be available where independent systems may
temporarily contain inconsistent states.

Representative reconciliation inputs include:

- Authorization Request
- Authorization Decision
- Service Result
- Consumption Transaction
- Entitlement State
- Transaction Identifier
- State Version

Reconciliation SHALL identify incomplete or conflicting transactions.

---

# 16. Recovery State

A transaction requiring recovery MAY enter an explicit recovery
state.

Representative states include:

- Pending
- Processing
- Completed
- Failed
- Retryable
- Compensation Required
- Reconciliation Required
- Cancelled

Recovery state SHALL remain distinguishable from successful
completion.

---

# 17. Idempotency During Recovery

Recovery processing SHALL preserve idempotency.

The same Consumption Transaction Identifier SHALL identify the same
logical consumption attempt.

Repeated recovery processing SHALL NOT:

- Consume an entitlement twice
- Create duplicate successful transactions
- Apply the same compensating action multiple times

---

# 18. Concurrency During Recovery

Recovery processing MAY race with normal processing.

The implementation SHALL therefore apply the applicable concurrency
controls.

The implementation SHALL prevent:

- Double consumption
- Conflicting compensation
- Lost state updates
- Invalid state regression
- Multiple completion records

---

# 19. Audit and Traceability

Each failure and recovery transition SHOULD be auditable.

Representative information includes:

- Transaction Identifier
- Authorization Request Identifier
- Decision Identifier
- Entitlement Identifier
- Failure Classification
- Failure Time
- Retry Count
- Recovery Action
- Compensation Identifier
- Reconciliation Result
- Final State

The audit chain SHOULD allow reconstruction of the transaction
history.

---

# 20. Fail-Closed Boundary

Authorization and enforcement failures SHALL default to denying the
protected action unless policy explicitly defines another behavior.

The system SHALL NOT execute a protected action merely because a
previous stage was successful.

Examples:

Authentication Success ≠ Authorization Success

Authorization Success ≠ Service Success

Service Success ≠ Consumption Success

Consumption Success ≠ State Update Success

---

# 21. Recovery Decision Model

Recovery SHALL classify the failure before selecting a recovery
action.

Representative model:

Failure Detected

        ↓

Failure Classification

        ↓

Retryable?

YES → Retry / Resume

NO → Compensation / Reconciliation / Final Failure

If authorization validity has changed:

Authorization Re-Evaluation

        ↓

New Authorization Decision

---

# 22. Final Outcome

A recovered transaction SHALL end in an explicit final outcome.

Representative final outcomes are:

- Completed
- Failed
- Cancelled
- Compensated
- Reconciled

The system SHALL NOT leave a transaction indefinitely without a
defined operational state.

---

# 23. Version 2.0 Example

Representative example:

Authorization = Allow

        ↓

Service Enforcement

        ↓

Discount Applied

        ↓

Consumption Transaction Persistence Failure

        ↓

Retry

        ↓

Same Consumption Transaction Identifier

        ↓

Consumption Recorded

        ↓

Entitlement State Updated

If retry remains unsuccessful:

Consumption Persistence Failure

        ↓

Recovery State

        ↓

Reconciliation Required

        ↓

Final Consistent State

---

# 24. Security Properties

The model provides:

- Fail-closed enforcement
- Explicit failure boundaries
- Controlled retry
- Idempotent recovery
- Authorization re-evaluation
- Compensation support
- Reconciliation support
- Concurrency protection
- State consistency
- Auditability
- Transaction traceability

---

# 25. Non-Goals

Figure19 does not define:

- A specific queue technology
- A specific database
- A specific distributed transaction protocol
- A specific retry library
- A specific workflow engine
- A specific monitoring platform

Implementation-specific mechanisms MAY be defined elsewhere.

---

# 26. Design Freeze Decision

Figure19 is designated as the Failure Handling and Recovery
reference model for NEW-shot2play Protocol Suite Version 2.0.

Future modifications require Design Freeze review.

