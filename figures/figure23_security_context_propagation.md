# Figure23 Security Context Propagation

## Document Information

| Item | Value |
|---|---|
| Figure ID | Figure23 |
| Title | Security Context Propagation |
| Version | NEW-shot2play Protocol Suite Version 2.0 |
| Revision | Design Definition Version 2.0 |
| Status | Design Definition |
| Purpose | Security-context propagation and validation model across authentication, entitlement, policy evaluation, authorization, enforcement, and audit |

---

# 1. Purpose

Figure23 defines how security context is propagated across the logical
protocol domains of the NEW-shot2play Protocol Suite Version 2.0.

The model establishes that security context SHALL NOT be treated as
implicitly trusted merely because it originated from an upstream domain.

Each receiving domain SHALL validate the context required for its own
security responsibility.

The representative flow is:

Authentication Context

↓

Entitlement Context

↓

Policy Evaluation Context

↓

Authorization Context

↓

Enforcement Context

↓

Audit Context

---

# 2. Core Principle

Security context propagation SHALL be based on explicit context
transfer and explicit validation.

The protocol SHALL distinguish between:

- Context Creation
- Context Propagation
- Context Validation
- Context Binding
- Context Consumption
- Context Recording

A receiving domain SHALL establish whether the received context is
applicable to the current transaction before using it for a
security-sensitive operation.

---

# 3. Security Context

A Security Context represents information established or derived during
the protocol lifecycle.

Representative elements include:

- Subject Identifier
- Authentication Identifier
- Authentication Result
- Authentication Time
- Authentication Method
- Entitlement Identifier
- Entitlement State
- Policy Identifier
- Policy Version
- Authorization Decision Identifier
- Authorization Result
- Resource Identifier
- Action Identifier
- Transaction Identifier
- Expiration
- State Version
- Audit Correlation Identifier

Not every context SHALL contain every element.

Each domain SHALL receive only the context required for its defined
responsibility.

---

# 4. Context Authority

Security context SHALL have an explicit authority.

Representative authority assignments are:

| Context | Authoritative Domain |
|---|---|
| Authentication Context | Authentication Domain |
| Entitlement Context | Entitlement Domain |
| Policy Context | Policy Evaluation Domain |
| Authorization Context | Authorization Domain |
| Enforcement Context | Enforcement Domain |
| Audit Context | Audit Domain |

A downstream domain SHALL NOT modify an upstream authoritative context
without an explicitly defined protocol transition.

---

# 5. Authentication Context

The Authentication Domain establishes the authentication context.

Representative information:

- Subject Identifier
- Authentication Identifier
- Authentication Result
- Authentication Method
- Authentication Time
- Assurance Context

The authentication context establishes that the subject satisfied the
required authentication procedure.

Authentication context SHALL NOT itself establish authorization.

---

# 6. Entitlement Context

The Entitlement Domain provides the current entitlement context.

Representative information:

- Entitlement Identifier
- Entitlement Type
- Entitlement State
- Entitlement Version
- Validity Period
- Reservation State
- Consumption State

The entitlement context SHALL represent authoritative server-side
entitlement state.

A client-provided entitlement state SHALL NOT override authoritative
state.

---

# 7. Policy Evaluation Context

The Policy Evaluation Domain determines which policy conditions apply.

Representative information:

- Policy Identifier
- Policy Version
- Policy Condition
- Evaluation Input
- Evaluation Time
- Evaluation Result
- Policy Status

Policy evaluation MAY incorporate:

- Authentication Context
- Entitlement Context
- Resource Context
- Transaction Context
- Environmental Context

Policy evaluation SHALL be deterministic for the same defined inputs
and policy version unless explicitly defined otherwise.

---

# 8. Authorization Context

The Authorization Domain produces the authorization context.

Representative information:

- Authorization Decision Identifier
- Transaction Identifier
- Subject Identifier
- Resource Identifier
- Action Identifier
- Policy Identifier
- Entitlement Identifier
- Decision Result
- Decision Expiration
- Decision Version

The authorization context SHALL be bound to the security context used
to produce the decision.

---

# 9. Enforcement Context

The Enforcement Domain receives the authorization context and determines
whether it may be applied to the current operation.

Representative information:

- Decision Identifier
- Transaction Identifier
- Resource Identifier
- Action Identifier
- Decision Result
- Decision Expiration
- Entitlement Identifier
- Current Request Context

The Enforcement Domain SHALL validate the binding between the received
authorization context and the current request.

---

# 10. Audit Context

The Audit Domain records security-relevant context and lifecycle
events.

Representative information includes:

- Transaction Identifier
- Authentication Identifier
- Decision Identifier
- Entitlement Identifier
- Event Identifier
- Event Type
- Previous State
- New State
- Event Timestamp
- Recovery Identifier

Audit context SHOULD provide sufficient correlation information to
reconstruct the logical lifecycle.

---

# 11. Context Propagation Chain

The representative propagation chain is:

Authentication

↓

Authentication Context

↓

Entitlement

↓

Entitlement Context

↓

Policy Evaluation

↓

Policy Evaluation Context

↓

Authorization

↓

Authorization Context

↓

Enforcement

↓

Enforcement Context

↓

Audit

↓

Audit Context

Each transition represents a controlled context boundary.

---

# 12. Authentication-to-Entitlement Propagation

Authentication establishes the subject context required for entitlement
evaluation.

The receiving Entitlement Domain SHOULD validate:

- Subject Identifier
- Authentication Result
- Authentication Expiration
- Transaction Identifier
- Authentication Assurance

The Entitlement Domain SHALL NOT assume that authentication alone
authorizes entitlement consumption.

---

# 13. Entitlement-to-Policy Propagation

The Entitlement Domain provides authoritative entitlement state to the
Policy Evaluation Domain.

The receiving Policy Evaluation Domain SHOULD validate:

- Entitlement Identifier
- Entitlement State
- Entitlement Version
- Validity Period
- Subject Binding
- Transaction Binding

Policy evaluation SHALL use the authoritative entitlement state.

---

# 14. Policy-to-Authorization Propagation

The Policy Evaluation Domain provides the policy evaluation result to
the Authorization Domain.

Representative information includes:

- Policy Identifier
- Policy Version
- Evaluation Result
- Evaluation Timestamp
- Evaluation Context
- Relevant Conditions

The Authorization Domain SHALL ensure that the policy result corresponds
to the current authorization request.

---

# 15. Authorization-to-Enforcement Propagation

The Authorization Domain provides the authorization decision to the
Enforcement Domain.

The receiving Enforcement Domain SHOULD validate:

- Decision Identifier
- Transaction Identifier
- Subject Identifier
- Resource Identifier
- Action Identifier
- Policy Identifier
- Entitlement Identifier
- Decision Expiration
- Decision Integrity

A mismatched decision SHALL NOT be applied.

---

# 16. Enforcement-to-Audit Propagation

The Enforcement Domain provides execution evidence to the Audit Domain.

Representative information includes:

- Transaction Identifier
- Decision Identifier
- Execution Result
- Resource Identifier
- Action Identifier
- Entitlement Identifier
- Consumption Result
- Event Timestamp

The Audit Domain SHALL preserve the resulting lifecycle evidence.

---

# 17. Context Binding

Security context SHALL be bound to the transaction in which it is used.

Representative binding elements include:

- Transaction Identifier
- Subject Identifier
- Resource Identifier
- Action Identifier
- Entitlement Identifier
- Decision Identifier
- Policy Identifier

Context from another transaction SHALL NOT be accepted as the context
for the current transaction.

---

# 18. Expiration Validation

Security contexts MAY contain explicit validity periods.

A receiving domain SHALL validate:

- Issued Time
- Expiration Time
- Current Time
- Allowed Clock Skew
- Context Status

Expired context SHALL NOT be used for security-sensitive decisions.

---

# 19. Version Validation

Security contexts that represent mutable authoritative state SHOULD
include a state or version identifier.

Representative examples include:

- Entitlement Version
- Policy Version
- Decision Version
- State Version

The receiving domain SHALL verify that the version is applicable to the
current transaction.

---

# 20. Integrity Validation

Security context SHALL be protected against unauthorized modification.

Representative mechanisms include:

- Digital Signature
- Message Authentication Code
- Secure Channel
- Integrity-protected Protocol Message
- Server-side State Lookup

The implementation MAY select an appropriate mechanism according to
deployment requirements.

---

# 21. Identifier Validation

Identifiers SHALL be validated before security-sensitive use.

Representative identifiers include:

- Subject Identifier
- Transaction Identifier
- Authentication Identifier
- Entitlement Identifier
- Policy Identifier
- Decision Identifier
- Event Identifier

An unknown, malformed, or inconsistent identifier SHALL result in
rejection or defined error handling.

---

# 22. Context Consistency

A receiving domain SHOULD verify logical consistency among related
context elements.

Representative consistency checks include:

- Subject ↔ Authentication
- Subject ↔ Entitlement
- Entitlement ↔ Transaction
- Policy ↔ Policy Version
- Decision ↔ Policy
- Decision ↔ Resource
- Decision ↔ Action
- Decision ↔ Entitlement
- Execution ↔ Decision
- Audit Event ↔ Transaction

A failed consistency check SHALL prevent unauthorized state transition.

---

# 23. Context Transformation

A downstream domain MAY derive a new context from an upstream context.

For example:

Authentication Context

↓

Authorization Context

The derived context SHALL preserve the security properties required by
the downstream operation.

A derived context SHALL NOT silently change the meaning of the
authoritative upstream context.

---

# 24. Context Minimization

Only required security context SHOULD be propagated.

The protocol SHOULD avoid unnecessary propagation of:

- Sensitive Data
- Unrelated User Data
- Unused Entitlement Attributes
- Unused Policy Attributes
- Unused Audit Information

Context minimization reduces attack surface and limits unintended
authority propagation.

---

# 25. No Implicit Authority Transfer

Propagation of context SHALL NOT automatically transfer authority.

For example:

Authentication Context

does not automatically become

Authorization Context.

Likewise:

Authorization Context

does not automatically become

Entitlement State.

Each domain SHALL establish the authority appropriate to its role.

---

# 26. Rejection Conditions

A receiving domain SHOULD reject context when:

- Context is expired
- Context is malformed
- Context is improperly signed
- Context is not transaction-bound
- Subject does not match
- Resource does not match
- Action does not match
- Entitlement does not match
- Policy version does not match
- State version is stale
- Decision is no longer valid
- Required context is missing

---

# 27. Failure Propagation

Context validation failures SHALL propagate as controlled protocol
failures.

Representative results include:

- Reject
- Deny
- Invalid Context
- Expired Context
- Stale State
- Transaction Mismatch
- Integrity Failure
- Reauthentication Required
- Reauthorization Required

A context failure SHALL NOT silently degrade into an implicit allow.

---

# 28. Recovery Context

Recovery operations SHALL preserve the original security context.

Representative recovery references include:

- Original Transaction Identifier
- Original Decision Identifier
- Original Entitlement Identifier
- Failure Identifier
- Recovery Identifier
- Recovery Timestamp

Recovery SHALL remain subject to the required security validation.

---

# 29. Audit Correlation

Audit records SHOULD retain sufficient references to correlate:

Authentication

with

Entitlement

with

Policy Evaluation

with

Authorization

with

Enforcement

with

Consumption

with

Recovery

The Transaction Identifier SHALL serve as the primary logical
correlation key where applicable.

---

# 30. Security Properties

The Security Context Propagation Model provides:

- Explicit context authority
- Controlled context propagation
- Context validation
- Transaction binding
- Subject binding
- Resource binding
- Action binding
- Entitlement binding
- Policy binding
- Expiration validation
- Version validation
- Integrity validation
- Context minimization
- No implicit authority transfer
- Controlled failure propagation
- Recovery preservation
- Audit traceability

---

# 31. Non-Goals

Figure23 does not define:

- A specific cryptographic algorithm
- A specific network protocol
- A specific cloud provider
- A specific database
- A specific programming language
- A specific deployment topology
- A specific user interface

Implementation details MAY be defined elsewhere.

---

# 32. Design Freeze Decision

Figure23 is designated as the security-context propagation reference
model for the NEW-shot2play Protocol Suite Version 2.0.

Future modifications require Design Freeze review.

