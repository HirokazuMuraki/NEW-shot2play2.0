# Figure04 Registration Transaction

## Document Information

| Item | Value |
|---|---|
| Figure ID | Figure04 |
| Title | Registration Transaction |
| Version | NEW-shot2play Protocol Suite Version 2.0 |
| Revision | Design Definition Version 2.0 |
| Status | Design Definition |
| Purpose | Initial credential and identity registration model |

---

# 1. Purpose

Figure04 defines the registration transaction used to establish
the relationship between a user identity, authentication device,
credential information, and protocol context.

Registration is the foundation process that enables subsequent
authentication and authorization transactions.

---

# 2. Registration Concept

NEW-shot2play Protocol Suite Version 2.0 separates:

- Identity establishment
- Credential registration
- Device binding
- Entitlement association

The registration process creates a trusted relationship
between the user and the authentication environment.

---

# 3. Registration Processing Model

The fundamental registration flow is:

Registration Request

        ↓

Identity Verification

        ↓

Credential Registration

        ↓

Device Binding

        ↓

Registration Completion

---

# 4. Registration Request

## Purpose

Initiates the registration transaction.

## Contains

- Registration identifier
- User context
- Device information
- Registration timestamp

---

# 5. Identity Verification

## Purpose

Confirms the identity associated with the registration request.

## Responsibilities

- Identity validation
- Initial trust establishment
- Registration authorization

---

# 6. Credential Registration

## Purpose

Registers authentication credentials required for future authentication.

## Responsibilities

- Credential creation
- Public key registration
- Credential metadata storage

## Security Model

Private key material remains within the user's device.

The server stores only the information required
for verification.

---

# 7. Device Binding

## Purpose

Associates the registered credential with an authorized device.

## Responsibilities

- Device identification
- Credential association
- Trust relationship establishment

---

# 8. Registration Completion

## Purpose

Finalizes the registration transaction.

The result creates the authentication foundation
used by subsequent transactions.

---

# 9. Version 2.0 Extension

Version 2.0 extends registration beyond authentication.

The registration context may associate:

Identity

        ↓

Credential

        ↓

Entitlement Context

        ↓

Authorization Capability

---

# 10. Scenario D Registration Example

A user registers a device before using
a visit-based service.

Processing:

User Registration

        ↓

Device Credential Registration

        ↓

Service Relationship Establishment

        ↓

Future Visit Authentication

        ↓

Conditional Service Benefit

---

# 11. Patent Boundary Candidate

Figure04 represents:

- Initial trust establishment
- Credential binding
- Device association
- Future authorization preparation

---

# 12. Non-Goals

Figure04 does not define:

- Authentication runtime exchange
- Authorization decision logic
- Network transport protocol

Those are defined in separate figures.

---

# 13. SVG Design Requirements

The SVG representation SHALL include:

- Registration transaction sequence
- User and device relationship
- Credential binding concept
- Version 2.0 extension area

---

# 14. Design Freeze Decision

Figure04 is approved as the registration reference model
for NEW-shot2play Protocol Suite Version 2.0.

Future modifications require Design Freeze review.

