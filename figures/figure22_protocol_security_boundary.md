# Figure22 Protocol Security Boundary

## Document Information

| Item | Value |
|---|---|
| Figure ID | Figure22 |
| Title | Protocol Security Boundary |
| Version | NEW-shot2play Protocol Suite Version 2.0 |
| Revision | Design Definition Version 2.0 |
| Status | Design Definition |
| Purpose | Trust-domain and security-boundary model for authentication, authorization, enforcement, entitlement, and audit |

---

# 1. Purpose

Figure22 defines the security boundaries and trust relationships of the
NEW-shot2play Protocol Suite Version 2.0.

The model separates the principal protocol domains into explicit trust
boundaries.

The primary domains are:

- Client Domain
- Authentication Domain
- Authorization Domain
- Enforcement Domain
- Entitlement Domain
- Audit Domain

Each domain SHALL operate within its defined security responsibility.

---

# 2. Security Boundary Principle

The protocol SHALL NOT rely on a single undifferentiated trusted
environment.

Instead, security responsibilities SHALL be separated between domains.

The representative trust structure is:

Client

        ↓

Authentication Domain

        ↓

Authorization Domain

        ↓

Enforcement Domain

        ↓

Entitlement Domain

        ↓

Audit Domain

The direction of this flow represents protocol dependency and does not
by itself imply unrestricted trust.

---

# 3. Client Domain

The Client Domain represents the user-facing or requesting endpoint.

Representative components include:

- Browser
- Smartphone
- Client Application
- Authentication Interface
- Protected Service Request

The Client Domain SHALL be treated as an untrusted or partially trusted
execution environment.

Client-side state SHALL NOT be considered authoritative for:

- Authorization
- Entitlement State
- Consumption State
- Security Policy
- Audit State

---

# 4. Authentication Domain

The Authentication Domain establishes the authentication context.

Representative responsibilities include:

- Challenge Generation
- Authentication Verification
- Public-Key Verification
- Authentication Result
- Authentication Identifier
- Authentication Timestamp

The Authentication Domain SHALL establish evidence that a subject
satisfied the required authentication mechanism.

Authentication SHALL NOT by itself grant authorization to a protected
resource.

---

# 5. Authorization Domain

The Authorization Domain evaluates whether a subject is permitted to
perform a requested action.

Representative inputs include:

- Subject
- Resource
- Action
- Authentication Context
- Entitlement Context
- Policy
- Transaction Context

Representative outputs include:

- Allow
- Deny
- Conditional Allow
- Indeterminate
- Error

Authorization decisions SHALL be generated within the Authorization
Domain.

---

# 6. Enforcement Domain

The Enforcement Domain applies the authorization decision to the
protected operation.

Representative responsibilities include:

- Decision Validation
- Transaction Correlation
- Resource Correlation
- Action Correlation
- Decision Expiration Check
- Policy Condition Enforcement

The Enforcement Domain SHALL NOT create an authorization decision by
itself unless explicitly defined by the protocol architecture.

The Enforcement Domain SHALL consume an authorization result and apply
it to the requested operation.

---

# 7. Entitlement Domain

The Entitlement Domain manages the state of the entitlement or
consumable authorization resource.

Representative responsibilities include:

- Entitlement Creation
- Entitlement Activation
- Reservation
- Consumption
- State Transition
- Exhaustion
- Suspension
- Revocation
- Expiration

The Entitlement Domain SHALL maintain authoritative entitlement state.

Client-side entitlement state SHALL NOT override the authoritative
server-side state.

---

# 8. Audit Domain

The Audit Domain preserves security-relevant lifecycle evidence.

Representative events include:

- Transaction Initiation
- Authentication
- Authorization Request
- Authorization Decision
- Enforcement
- Service Execution
- Consumption
- Entitlement State Change
- Failure
- Recovery
- Compensation
- Reconciliation
- Completion

Audit records SHOULD preserve sufficient information to reconstruct the
logical transaction.

---

# 9. Trust Boundaries

The following boundaries SHALL be treated as explicit trust boundaries:

1. Client ↔ Authentication
2. Authentication ↔ Authorization
3. Authorization ↔ Enforcement
4. Enforcement ↔ Entitlement
5. Entitlement ↔ Audit

Each boundary SHOULD validate the information crossing the boundary.

A receiving domain SHALL NOT blindly trust client-controlled values.

---

# 10. Client-to-Authentication Boundary

The Client-to-Authentication boundary separates user-controlled
execution from authentication verification.

The boundary SHOULD protect:

- Challenge
- Authentication Assertion
- Public-Key Reference
- Authentication Result

The server-side Authentication Domain SHALL determine whether the
authentication evidence is valid.

---

# 11. Authentication-to-Authorization Boundary

The Authentication-to-Authorization boundary transfers the authenticated
subject context.

Representative information includes:

- Subject Identifier
- Authentication Identifier
- Authentication Result
- Authentication Time
- Authentication Method
- Authentication Assurance Context

The Authorization Domain SHALL rely on the verified authentication
context rather than on client-provided identity claims alone.

---

# 12. Authorization-to-Enforcement Boundary

The Authorization-to-Enforcement boundary transfers the authorization
decision.

Representative information includes:

- Decision Identifier
- Transaction Identifier
- Subject
- Resource
- Action
- Decision Result
- Policy Reference
- Entitlement Reference
- Decision Expiration

The Enforcement Domain SHALL validate that the decision applies to the
current transaction and requested operation.

---

# 13. Enforcement-to-Entitlement Boundary

The Enforcement-to-Entitlement boundary controls protected
consumption.

Representative information includes:

- Transaction Identifier
- Entitlement Identifier
- Authorized Action
- Consumption Context
- Decision Identifier

The Entitlement Domain SHALL validate the transaction and entitlement
context before modifying authoritative state.

---

# 14. Entitlement-to-Audit Boundary

The Entitlement-to-Audit boundary transfers state-change evidence.

Representative information includes:

- Entitlement Identifier
- Previous State
- New State
- State Version
- Transaction Identifier
- Consumption Transaction Identifier
- Update Time

The Audit Domain SHALL preserve the resulting lifecycle evidence.

---

# 15. Trust Model

The trust model distinguishes between:

### Trusted Authority

- Authentication Verification
- Authorization Policy
- Authorization Decision
- Entitlement State
- Audit Records

### Untrusted or Partially Trusted Input

- Client Input
- Client-Controlled State
- Client-Provided Resource State
- Client-Provided Entitlement State
- Client-Provided Authorization Result

The protocol SHALL establish authoritative server-side state for all
security-critical decisions.

---

# 16. Security-Critical State

The following state SHALL be authoritative within the server-side
security domains:

- Authentication Result
- Authorization Decision
- Entitlement State
- Consumption State
- Transaction State
- Audit State

Client-side representations MAY be used for display or interaction but
SHALL NOT override authoritative state.

---

# 17. Transaction Correlation

A Transaction Identifier SHALL provide correlation across trust
boundaries.

Representative relationship:

Client Request

        ↓

Authentication Context

        ↓

Authorization Request

        ↓

Authorization Decision

        ↓

Enforcement

        ↓

Consumption Transaction

        ↓

Entitlement State Change

        ↓

Audit Record

The Transaction Identifier SHOULD remain consistent throughout the
logical transaction.

---

# 18. Decision Integrity

Authorization decisions SHALL be protected against unauthorized
modification.

Representative protections include:

- Decision Identifier
- Transaction Binding
- Resource Binding
- Action Binding
- Policy Binding
- Expiration
- Integrity Protection

A modified or mismatched decision SHALL NOT be accepted by the
Enforcement Domain.

---

# 19. Entitlement Integrity

Entitlement state SHALL be protected against unauthorized modification.

Representative protections include:

- Authoritative server state
- Transaction binding
- State versioning
- Idempotency
- Atomic state transition
- Audit trail

A client SHALL NOT be able to directly transition an entitlement from
one authoritative state to another.

---

# 20. Audit Integrity

Audit records SHALL be protected against unauthorized modification.

Representative protections include:

- Append-oriented recording
- Transaction correlation
- Event timestamps
- Event identifiers
- State version references
- Access control

Audit records SHOULD provide sufficient evidence to reconstruct
security-relevant events.

---

# 21. Failure Isolation

A failure in one domain SHALL NOT automatically invalidate the security
assumptions of unrelated domains.

Representative isolation includes:

- Authentication failure → reject authorization
- Authorization failure → reject enforcement
- Enforcement failure → prevent consumption
- Consumption failure → preserve entitlement consistency
- Audit failure → invoke defined audit recovery or reconciliation

Failure handling SHALL preserve transaction correlation.

---

# 22. Recovery Across Trust Boundaries

Recovery operations SHALL preserve the original security context.

A recovery transaction SHOULD reference:

- Original Transaction Identifier
- Failure Identifier
- Recovery Identifier
- Original Entitlement Identifier
- Original Authorization Decision
- Recovery Result

Recovery SHALL NOT bypass the security controls of the affected domain.

---

# 23. Least Privilege

Each domain SHALL operate with only the permissions required to perform
its defined responsibility.

Representative separation:

Authentication

        ↓

establishes authentication evidence

Authorization

        ↓

establishes permission decision

Enforcement

        ↓

applies permission decision

Entitlement

        ↓

maintains authoritative consumption state

Audit

        ↓

preserves lifecycle evidence

No domain SHOULD implicitly acquire authority belonging to another
domain.

---

# 24. Separation of Duties

The protocol architecture SHALL maintain logical separation between:

- Authentication
- Authorization
- Enforcement
- Entitlement Management
- Audit

The same component MAY implement multiple domains in a concrete
deployment, but the logical security responsibilities SHALL remain
distinct.

---

# 25. Security Boundary Enforcement

Every trust boundary SHOULD apply appropriate validation.

Representative validation includes:

- Schema Validation
- Signature or Integrity Validation
- Identifier Validation
- Transaction Validation
- Policy Validation
- Expiration Validation
- State Version Validation
- Authorization Validation

Boundary validation SHALL occur before security-sensitive state is
accepted.

---

# 26. No Implicit Trust Propagation

Successful authentication SHALL NOT imply authorization.

Successful authorization SHALL NOT imply successful execution.

Successful execution SHALL NOT imply successful consumption.

Successful consumption SHALL NOT imply audit completion.

Each security domain SHALL establish its own required state transition.

---

# 27. Global Security Flow

The representative secure flow is:

Client

        ↓

[Trust Boundary]

Authentication

        ↓

[Trust Boundary]

Authorization

        ↓

[Trust Boundary]

Enforcement

        ↓

[Trust Boundary]

Entitlement

        ↓

[Trust Boundary]

Audit

The transaction remains correlated across the entire flow.

---

# 28. Security Properties

The security-boundary model provides:

- Trust-domain separation
- Explicit security boundaries
- Authentication isolation
- Authorization isolation
- Enforcement isolation
- Entitlement integrity
- Audit integrity
- Transaction correlation
- Failure isolation
- Recovery isolation
- Least privilege
- Separation of duties
- No implicit trust propagation
- Authoritative server-side state

---

# 29. Non-Goals

Figure22 does not define:

- A specific network topology
- A specific cloud provider
- A specific database technology
- A specific programming language
- A specific deployment architecture
- A specific user interface
- A specific cryptographic algorithm beyond protocol requirements
- A specific logging product

Implementation details MAY be defined elsewhere.

---

# 30. Design Freeze Decision

Figure22 is designated as the security-boundary reference model for the
NEW-shot2play Protocol Suite Version 2.0.

Future modifications require Design Freeze review.

