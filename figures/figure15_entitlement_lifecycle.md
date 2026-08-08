# Figure15 Entitlement Lifecycle

## Document Information

| Item | Value |
|---|---|
| Figure ID | Figure15 |
| Title | Entitlement Lifecycle |
| Version | NEW-shot2play Protocol Suite Version 2.0 |
| Revision | Design Definition Version 2.0 |
| Status | Design Definition |
| Purpose | Definition of the lifecycle of an Entitlement from acquisition through expiration or revocation |

---

# 1. Purpose

Figure15 defines the lifecycle of an Entitlement in the
NEW-shot2play Protocol Suite Version 2.0.

An Entitlement is an independent object representing a user's
eligibility, capability, or permission to use a specified service
or resource.

The Entitlement lifecycle is independent from the Authentication
lifecycle.

Authentication establishes the identity of the subject.

Entitlement processing determines whether the subject possesses
the required right for the requested service.

---

# 2. Fundamental Model

The fundamental Entitlement lifecycle is:

Acquire

        ↓

Hold

        ↓

Use

        ↓

Consume

        ↓

Expire

An Entitlement MAY also be:

Hold

        ↓

Revoke

An Entitlement that has been revoked SHALL NOT be used for an
authorization Allow decision.

---

# 3. Entitlement Object

Each Entitlement SHALL be represented as an independent logical
object.

Representative attributes include:

- Entitlement Identifier
- Subject Identifier
- Capability
- Resource
- Service
- Scope
- Acquisition Time
- Effective Time
- Expiration Time
- Quantity
- Usage Count
- State
- Revocation Status
- Transaction Reference

The Entitlement Object SHALL remain logically distinguishable from
the Authentication Object.

---

# 4. Acquire State

The Acquire state represents creation or acquisition of an
Entitlement.

An Entitlement MAY be acquired as a result of:

- Successful transaction
- Visit evidence
- Purchase
- Attendance
- Coupon issuance
- Point award
- External business event
- Other Service Profile defined event

The acquisition event SHALL establish the initial ownership or
eligibility relationship.

---

# 5. Hold State

The Hold state represents an Entitlement that has been successfully
acquired and remains available for use.

A held Entitlement MAY contain:

- Validity period
- Scope
- Quantity
- Usage restrictions
- Applicable Policy
- Service restrictions

The Hold state does not itself imply that the Entitlement has been
consumed.

---

# 6. Use State

The Use state represents evaluation or application of an Entitlement
for a requested service action.

The authorization process MAY verify:

- Subject ownership
- Entitlement state
- Scope
- Resource
- Service
- Effective time
- Expiration time
- Revocation status
- Usage conditions

A successful Use evaluation MAY result in service authorization.

---

# 7. Consume State

The Consume state represents use of an Entitlement that is defined
as consumable.

Examples include:

- Coupon
- Ticket
- One-time benefit
- Attendance right
- Limited-use entitlement
- Point balance

Consumption SHALL update the authoritative Entitlement state.

For a single-use Entitlement, successful consumption SHALL prevent
the same Entitlement from being consumed again.

---

# 8. Expire State

The Expire state represents an Entitlement that is no longer valid
because its defined validity period has ended.

Expiration MAY be determined by:

- Expiration Time
- Validity Period
- Service Policy
- Business Rule

An expired Entitlement SHALL NOT satisfy an authorization condition
requiring an active Entitlement.

---

# 9. Revoke State

An Entitlement MAY be revoked before its normal expiration.

Revocation MAY occur because of:

- Security Incident
- Administrative Action
- User Request
- Business Rule
- Fraud Detection
- Service Termination
- Entitlement Cancellation

A revoked Entitlement SHALL NOT satisfy an authorization condition
requiring an active Entitlement.

---

# 10. Entitlement State Separation

The Entitlement lifecycle SHALL be logically separated from
authentication state.

Authentication states represent identity verification.

Entitlement states represent eligibility or capability.

The logical relationship is:

Authentication

        ↓

Authenticated Identity

        +

Entitlement State

        ↓

Policy Evaluation

        ↓

Authorization Decision

Authentication success SHALL NOT automatically transition an
Entitlement into a usable state.

---

# 11. Entitlement and Authorization

Authorization MAY depend on the current state of one or more
Entitlements.

For example:

Entitlement A = Active

        AND

Entitlement B = Active

        ↓

Policy Evaluation

        ↓

Authorization

        ↓

Allow

If a required Entitlement is expired, revoked, unavailable, or
otherwise invalid, the authorization decision SHALL be Deny.

---

# 12. Multiple Entitlements

A Service Profile MAY require multiple Entitlements.

Representative logical conditions include:

A

        ↓

Service X

or:

A AND B

        ↓

Service X

or:

A OR B

        ↓

Service X

The Policy layer SHALL determine the applicable logical relationship.

---

# 13. Quantity-Based Entitlement

An Entitlement MAY contain a quantity or remaining usage count.

Example:

10 Uses

        ↓

Use

        ↓

9 Uses

        ↓

Use

        ↓

8 Uses

The authoritative remaining quantity SHALL be updated after each
successful consumption.

An Entitlement with zero remaining quantity SHALL NOT satisfy a
condition requiring an available usage.

---

# 14. Time-Based Entitlement

An Entitlement MAY contain an effective period.

The logical validity condition is:

Current Time >= Effective Time

        AND

Current Time < Expiration Time

An Entitlement outside the effective period SHALL NOT be treated as
active unless an applicable policy explicitly defines another valid
state.

---

# 15. Revocation and Expiration

Revocation and expiration are distinct lifecycle conditions.

Expiration occurs as a consequence of the defined validity period.

Revocation occurs as an explicit invalidation event.

Both conditions SHALL prevent an invalid Entitlement from producing
an authorization Allow decision.

---

# 16. Entitlement Evidence

An Entitlement acquisition SHOULD be associated with verifiable
evidence.

Representative evidence includes:

- Transaction Identifier
- Acquisition Event
- Time
- Service Profile
- Subject
- Source Event
- Evidence Identifier

The evidence provides a traceable basis for the existence of the
Entitlement.

---

# 17. Entitlement Consumption Evidence

When an Entitlement is consumed, the consumption event SHOULD be
recorded.

Representative information includes:

- Entitlement Identifier
- Subject Identifier
- Consumption Transaction
- Consumption Time
- Resource
- Service
- Quantity Before
- Quantity After
- Result

This information supports auditability and dispute investigation.

---

# 18. State Transition Rules

The following transitions SHALL be supported:

Acquire → Hold

Hold → Use

Use → Hold

Use → Consume

Hold → Expire

Hold → Revoke

An implementation MAY define additional intermediate states where
required by a Service Profile.

An invalid transition SHALL be rejected.

---

# 19. State Transition Integrity

Each state transition SHALL be associated with the Entitlement
Identifier and the transaction or event that caused the transition.

The system SHALL prevent unauthorized modification of the
authoritative Entitlement state.

A state transition SHALL be processed atomically where required to
prevent duplicate consumption or inconsistent entitlement state.

---

# 20. Concurrent Consumption

Where an Entitlement is consumable, concurrent requests SHALL NOT
result in multiple successful consumptions of the same unavailable
quantity.

The authoritative Entitlement state SHALL be updated using an
atomic or otherwise concurrency-safe mechanism.

This requirement is particularly important for:

- One-time coupons
- Tickets
- Limited-use rights
- Point balances
- Quantity-based benefits

---

# 21. Authorization Interaction

The logical authorization sequence is:

Authenticated Identity

        ↓

Entitlement Lookup

        ↓

State Validation

        ↓

Revocation Check

        ↓

Expiration Check

        ↓

Policy Evaluation

        ↓

Authorization Decision

The authorization engine SHALL use the authoritative Entitlement
state.

---

# 22. Version 2.0 Example

A representative service scenario is:

Visit Evidence

        ↓

Acquire Entitlement

        ↓

Hold Discount Entitlement

        ↓

EC Authorization Request

        ↓

Policy Evaluation

        ↓

Allow

        ↓

Consume Discount Entitlement

        ↓

Entitlement Completed

If the Entitlement is already consumed, expired, or revoked, the
same authorization request SHALL result in Deny.

---

# 23. Security Properties

The Entitlement lifecycle provides:

- Separation of identity and eligibility
- Explicit entitlement ownership
- State-based authorization
- Expiration enforcement
- Revocation enforcement
- Controlled consumption
- Quantity management
- Concurrent-consumption protection
- Evidence traceability
- Auditability

---

# 24. Non-Goals

Figure15 does not define:

- A specific database implementation
- A specific policy language
- A specific transaction database
- A specific API framework
- A specific business rule set
- A specific user interface

Implementation-specific details MAY be defined elsewhere in the
technical specification.

---

# 25. SVG Design Requirements

The SVG representation SHALL include:

- Entitlement Object
- Acquire
- Hold
- Use
- Consume
- Expire
- Revoke
- State transition rules
- Authorization interaction
- Multiple-entitlement example
- Quantity-based example
- Version 2.0 example

The normal lifecycle SHALL be visually distinct from invalidation
paths such as Expire and Revoke.

All labels, descriptions, arrows, and connectors SHALL remain within
their containing visual regions.

---

# 26. Design Freeze Decision

Figure15 is designated as the Entitlement Lifecycle reference model
for NEW-shot2play Protocol Suite Version 2.0.

Future modifications require Design Freeze review.

