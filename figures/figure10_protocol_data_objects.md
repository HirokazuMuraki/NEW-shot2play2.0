# Figure10 Protocol Data Objects

## Document Information

| Item | Value |
|---|---|
| Figure ID | Figure10 |
| Title | Protocol Data Objects |
| Version | NEW-shot2play Protocol Suite Version 2.0 |
| Revision | Design Definition Version 2.0 |
| Status | Design Definition |
| Purpose | Definition and relationship of protocol data objects |

---

# 1. Purpose

Figure10 defines the principal protocol data objects used by the
NEW-shot2play Protocol Suite Version 2.0.

The figure establishes the relationship between:

- Registration Object
- Authentication Object
- Entitlement Object
- Policy Object
- Authorization Object

Each object has a distinct responsibility within the protocol.

---

# 2. Data Object Model

The fundamental data object relationship is:

Registration Object

        ↓

Authentication Object

        ↓

Entitlement Object

        ↓

Policy Object

        ↓

Authorization Object

The objects are related but SHALL NOT be treated as a single
undifferentiated data structure.

---

# 3. Registration Object

The Registration Object represents the persistent association
between a user identity and a registered authentication device.

## Primary Responsibilities

- Identify the registered subject
- Identify the registered device
- Associate the authentication credential
- Maintain registration state
- Support revocation and termination

## Representative Data

- Registration Identifier
- User Identifier
- Device Identifier
- Credential Identifier
- Public Key
- Registration State
- Registration Timestamp
- Revocation Information

## Security Role

The Registration Object provides the authoritative association
between the registered subject and the authentication credential.

Private key material SHALL NOT be stored as part of the server-side
Registration Object.

---

# 4. Authentication Object

The Authentication Object represents a single authentication
transaction.

## Primary Responsibilities

- Identify the authentication transaction
- Bind the transaction to a challenge
- Associate the QR token
- Track transaction state
- Store authentication result

## Representative Data

- Authentication Transaction Identifier
- Registration Identifier
- QR Token Identifier
- Challenge
- Creation Time
- Expiration Time
- Consumption State
- Authentication Result

## Security Role

The Authentication Object provides transaction-specific state and
prevents reuse of completed or expired authentication transactions.

---

# 5. Entitlement Object

The Entitlement Object represents the permissions or service rights
associated with an authenticated subject.

## Primary Responsibilities

- Identify granted capability
- Identify scope of capability
- Associate capability with a subject
- Define capability validity

## Representative Data

- Entitlement Identifier
- Subject Identifier
- Capability
- Scope
- Effective Time
- Expiration Time
- Entitlement State

## Version 2.0 Role

The Entitlement Object is introduced as a distinct authorization
layer in Version 2.0.

Successful authentication does not by itself create unrestricted
entitlement.

---

# 6. Policy Object

The Policy Object represents the rules used to determine whether
an entitlement permits a requested action.

## Primary Responsibilities

- Define authorization conditions
- Define permitted actions
- Define resource scope
- Define contextual constraints

## Representative Data

- Policy Identifier
- Policy Version
- Subject Conditions
- Resource Conditions
- Action Conditions
- Context Conditions
- Policy State

## Version 2.0 Role

The Policy Object separates authorization policy from
authentication state.

---

# 7. Authorization Object

The Authorization Object represents the result of evaluating an
authenticated request against applicable entitlement and policy.

## Primary Responsibilities

- Identify the authorization request
- Reference the authenticated subject
- Reference applicable entitlement
- Reference applicable policy
- Record authorization decision

## Representative Data

- Authorization Identifier
- Authentication Transaction Identifier
- Entitlement Identifier
- Policy Identifier
- Requested Action
- Target Resource
- Authorization Decision
- Decision Timestamp

## Version 2.0 Role

The Authorization Object provides the explicit boundary between
authentication success and permission to perform a service action.

---

# 8. Object Relationships

The principal relationship is:

Registration Object

        ↓

Authentication Object

        ↓

Authenticated Identity

        ↓

Entitlement Object

        ↓

Policy Object

        ↓

Authorization Object

        ↓

Service Action

Authentication establishes identity assurance.

Entitlement and policy determine whether the authenticated subject
is permitted to perform the requested action.

---

# 9. Object Responsibility

| Object | Primary Responsibility |
|---|---|
| Registration Object | Credential and device registration |
| Authentication Object | Authentication transaction state |
| Entitlement Object | Granted capability |
| Policy Object | Authorization rules |
| Authorization Object | Authorization decision |

The responsibilities SHALL remain logically separated.

---

# 10. Object Lifecycle

## Registration Object

Registered

        ↓

Active

        ↓

Revoked

        ↓

Terminated

## Authentication Object

Created

        ↓

Challenge Issued

        ↓

Response Received

        ↓

Verified

        ↓

Consumed

or

Rejected

## Entitlement Object

Created

        ↓

Active

        ↓

Expired / Revoked

## Policy Object

Published

        ↓

Active

        ↓

Superseded / Disabled

## Authorization Object

Requested

        ↓

Evaluated

        ↓

Allowed / Denied

---

# 11. Object Separation Principle

The protocol SHALL maintain separation between:

- Identity registration
- Authentication transaction
- Entitlement
- Policy
- Authorization decision

Authentication data SHALL NOT be used as a substitute for
authorization policy.

Authorization policy SHALL NOT modify authentication credential
material.

---

# 12. Data Flow

The principal data flow is:

Registration Object

        ↓

Authentication Object

        ↓

Authentication Verification

        ↓

Authenticated Identity

        ↓

Entitlement Lookup

        ↓

Policy Evaluation

        ↓

Authorization Object

        ↓

Service Action

---

# 13. Security Boundary

The principal security boundaries are:

## Credential Boundary

Registration Object contains public verification information
and registration state.

Private key material remains outside the server-side object model.

## Transaction Boundary

Authentication Object contains transaction-specific state.

Expired or consumed transactions SHALL NOT be reused.

## Authorization Boundary

Entitlement and Policy Objects determine permitted service actions.

Authorization Object records the resulting decision.

---

# 14. Version 2.0 Extension

Version 2.0 extends the protocol data model by explicitly defining:

- Entitlement Object
- Policy Object
- Authorization Object

This creates a distinct authorization data layer following
authentication.

The extended model is:

Authentication

        ↓

Authenticated Identity

        ↓

Entitlement

        ↓

Policy Evaluation

        ↓

Authorization Decision

        ↓

Service

---

# 15. Non-Goals

Figure10 does not define:

- Physical database schema
- Specific database technology
- Serialization format
- API endpoint definitions
- Cryptographic algorithm parameters
- User interface representation

Those details are defined by the implementation specification
where applicable.

---

# 16. SVG Design Requirements

The SVG representation SHALL include:

- Five principal protocol data objects
- Object responsibility
- Primary object relationships
- Authentication-to-authorization separation
- Version 2.0 authorization extension

The five objects SHALL be visually distinguishable.

The relationship from Registration through Authorization SHALL
be clearly represented.

No text or connector SHALL extend outside its containing visual
region.

---

# 17. Design Freeze Decision

Figure10 is designated as the protocol data object reference model
for NEW-shot2play Protocol Suite Version 2.0.

Future modifications require Design Freeze review.

