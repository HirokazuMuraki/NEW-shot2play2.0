# Figure33 Authorization Decision Binding to Service Execution

## Document Information

| Item | Value |
|---|---|
| Figure ID | Figure33 |
| Title | Authorization Decision Binding to Service Execution |
| Version | NEW-shot2play Protocol Suite Version 2.0 |
| Revision | Design Definition Version 2.0 |
| Status | Design Definition |
| Purpose | Definition of the binding relationship between an Authorization Decision and protected Service Execution |

---

# 1. Purpose

Figure33 defines how an Authorization Decision is bound to the
execution of a protected service operation.

The model establishes that an Authorization Decision is not merely
an advisory result.

Once an Authorization Decision has been established, the decision
SHALL be associated with the specific authorization request,
subject, resource, service, action, and applicable authorization
state for which the decision was generated.

The protected service SHALL execute the requested operation only
when the decision presented at the enforcement boundary satisfies
the required binding conditions.

---

# 2. Fundamental Binding Model

The fundamental relationship is:

Authorization Request

        ↓

Authorization Evaluation

        ↓

Authorization Decision

        ↓

Decision Binding

        ↓

Enforcement Boundary

        ↓

Protected Service Execution

The decision SHALL remain logically associated with the request
throughout the transition from authorization evaluation to service
execution.

---

# 3. Authorization Request

The Authorization Request identifies the operation for which
authorization is requested.

Representative attributes include:

- Authorization Request Identifier
- Subject Identifier
- Action
- Resource
- Service Identifier
- Entitlement Reference
- Transaction Reference
- Request Timestamp
- Authorization Context Reference

The Authorization Request establishes the scope of the operation
being evaluated.

---

# 4. Authorization Decision

The Authorization Decision represents the result of the
authorization evaluation.

Representative decision attributes include:

- Decision Identifier
- Decision Result
- Subject Identifier
- Action
- Resource
- Service Identifier
- Decision Timestamp
- Policy Reference
- Entitlement Reference
- Authorization Context Reference
- Decision Validity
- Decision Expiration
- Transaction Reference

The Decision SHALL be distinguishable from the Authorization Request
while remaining traceably associated with it.

---

# 5. Decision Binding

Decision Binding associates the Authorization Decision with the
operation for which the decision was produced.

The binding SHALL establish sufficient correspondence between the
decision and the execution request.

Representative binding elements include:

- Decision Identifier
- Authorization Request Identifier
- Subject Identifier
- Action
- Resource
- Service Identifier
- Entitlement Reference
- Policy Reference
- Transaction Reference
- Decision Validity

A service SHALL NOT treat an unrelated Authorization Decision as
satisfying the authorization requirement for the current request.

---

# 6. Subject Binding

The Authorization Decision SHALL be bound to the authenticated
subject for which the decision was evaluated.

If the subject associated with the presented decision does not match
the subject associated with the current service request, the decision
SHALL NOT authorize the operation.

Subject mismatch SHALL result in Deny or another explicitly
fail-closed outcome.

---

# 7. Action Binding

The Authorization Decision SHALL identify the action that was
authorized.

Examples include:

- Read
- Write
- Purchase
- Redeem
- Transfer
- Execute
- Access

A decision authorizing one action SHALL NOT automatically authorize a
different action.

Action mismatch SHALL prevent the corresponding service execution.

---

# 8. Resource Binding

The Authorization Decision SHALL identify the resource or protected
object to which the decision applies.

A decision concerning one resource SHALL NOT automatically authorize
access to another resource.

Resource mismatch SHALL result in Deny or another explicitly
fail-closed outcome.

---

# 9. Service Binding

The Authorization Decision SHALL identify the service or service
boundary for which the decision was produced.

A decision issued for one protected service SHALL NOT automatically
be accepted by another service unless the applicable authorization
model explicitly permits such delegation or propagation.

Service mismatch SHALL prevent unauthorized service execution.

---

# 10. Entitlement Binding

Where an Entitlement is required for authorization, the decision
SHALL retain a traceable relationship to the Entitlement state used
during evaluation.

If the applicable Entitlement becomes invalid before execution, the
service SHALL apply the applicable invalidation and fail-closed
rules.

A stale or revoked Entitlement SHALL NOT be treated as an active
authorization basis merely because an earlier decision existed.

---

# 11. Policy Binding

The Authorization Decision SHALL identify or otherwise remain
traceable to the Policy under which the decision was produced.

Representative policy references include:

- Policy Identifier
- Policy Version
- Policy Revision
- Policy Evaluation Reference

Where Policy validity is required at the enforcement boundary, the
service SHALL verify that the decision remains valid under the
applicable Policy state.

A decision based on an obsolete or invalid Policy SHALL NOT be used
to authorize an operation where current Policy validation is
required.

---

# 12. Temporal Binding

The Authorization Decision MAY contain validity information.

Representative temporal attributes include:

- Decision Timestamp
- Effective Time
- Expiration Time
- Validity Period
- Maximum Decision Age

The service SHALL reject a decision that has expired when expiration
is an applicable authorization condition.

A decision SHALL NOT be considered permanently valid merely because
it was previously issued.

---

# 13. Transaction Binding

The Authorization Decision MAY be bound to a Transaction Identifier.

The Transaction Identifier provides traceability between:

- Authorization Request
- Authorization Evaluation
- Authorization Decision
- Service Action
- Entitlement State Update

Where transaction binding is required, a mismatch SHALL prevent the
decision from authorizing the current operation.

---

# 14. Enforcement Boundary

The Enforcement Boundary is the point at which the Authorization
Decision is converted into an operational permission.

The enforcement process SHALL verify the applicable binding
conditions before allowing protected execution.

Representative enforcement checks include:

- Decision exists
- Decision result is Allow
- Subject matches
- Action matches
- Resource matches
- Service matches
- Decision remains valid
- Required Entitlement remains valid
- Required Policy remains valid
- Required transaction relationship remains valid

Only after the applicable checks succeed SHALL protected service
execution proceed.

---

# 15. Protected Service Execution

Protected Service Execution represents the actual operation performed
by the protected service.

Examples include:

- Reading protected data
- Writing protected data
- Executing a transaction
- Redeeming an entitlement
- Performing a purchase
- Accessing a restricted function
- Executing a privileged operation

The service SHALL NOT execute the protected operation when a required
decision binding condition fails.

---

# 16. Decision Reuse

An Authorization Decision MAY be reused only when the applicable
authorization model explicitly permits reuse.

Permitted reuse SHALL remain subject to:

- Subject binding
- Action binding
- Resource binding
- Service binding
- Entitlement validity
- Policy validity
- Decision validity
- Transaction requirements
- Context requirements

A decision SHALL NOT be reused outside its authorized scope merely
because the decision result is Allow.

---

# 17. Decision Substitution Prevention

The enforcement boundary SHALL prevent substitution of an
Authorization Decision belonging to another request, subject,
resource, action, service, transaction, or authorization context.

Decision substitution includes:

- Using another user's decision
- Using a decision for another resource
- Using a decision for another action
- Using a decision for another service
- Using an expired decision
- Using a revoked authorization basis
- Using a decision produced under an invalid Policy

Decision substitution SHALL result in Deny or another explicitly
fail-closed outcome.

---

# 18. Fail-Closed Enforcement

If the enforcement boundary cannot establish the required decision
binding, the protected service SHALL fail closed.

The fundamental rule is:

Required Decision Binding Valid

        ↓

Protected Execution Permitted

Conversely:

Required Decision Binding Invalid

        ↓

Protected Execution Denied

The service SHALL NOT assume that an unverified decision is valid.

---

# 19. Decision-to-Execution Continuity

The logical continuity is:

Authorization Request

        ↓

Authorization Evaluation

        ↓

Authorization Decision

        ↓

Decision Binding

        ↓

Enforcement Verification

        ↓

Protected Service Execution

The identity, authorization scope, and applicable security state
represented by the decision SHALL remain consistent across these
stages.

---

# 20. Distributed Service Considerations

In a distributed deployment, an Authorization Decision MAY be
generated by one component and enforced by another.

The receiving service SHALL verify the decision binding required by
the applicable protocol.

The receiving service SHALL NOT rely solely on the fact that another
component previously produced an Allow result.

Where security state has changed between decision generation and
execution, the applicable invalidation and fail-closed rules SHALL
take precedence.

---

# 21. Relationship to Earlier Figures

Figure33 is complementary to:

- Figure25 — Decision Binding and Service Execution
- Figure26 — Authorization Scope Propagation
- Figure31 — Authorization Invalidation, Fail-Closed Enforcement and Controlled Recovery
- Figure32 — Authorization Decision Dependency and Precedence Model

Figure33 focuses specifically on the binding and enforcement
relationship between an Authorization Decision and the protected
service operation.

---

# 22. Security Invariant

The fundamental security invariant represented by Figure33 is:

"An Authorization Decision SHALL authorize protected execution only
when the decision remains bound to the operation and security state
for which it was established."

A decision that cannot be established as applicable to the current
execution SHALL NOT produce protected execution.

---

# 23. Design Principle

Figure33 establishes the following design principle:

**Authorization produces a decision.**

**Decision Binding establishes applicability.**

**Enforcement verifies applicability.**

**Protected Service Execution occurs only after successful
verification.**

This separation prevents an Allow result from becoming an
unrestricted or transferable permission.

---

# 24. Summary

Figure33 defines the binding relationship between an Authorization
Decision and protected Service Execution.

The model establishes that:

1. The Authorization Request defines the requested operation.
2. Authorization Evaluation produces an Authorization Decision.
3. Decision Binding associates the decision with the operation.
4. Subject, action, resource, and service relationships SHALL remain
   consistent.
5. Entitlement and Policy references SHALL remain traceable.
6. Temporal and transaction conditions MAY constrain decision
   validity.
7. The Enforcement Boundary SHALL verify required binding conditions.
8. Protected Service Execution SHALL occur only after successful
   enforcement verification.
9. Decision substitution SHALL be prevented.
10. Invalid or unverifiable binding SHALL result in Deny or
    Fail-Closed behavior.
11. The same principle applies when decision generation and execution
    occur across distributed services.

