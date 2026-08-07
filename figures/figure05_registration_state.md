# Figure05 Registration State Transition

## Document Information

| Item | Value |
|---|---|
| Figure ID | Figure05 |
| Title | Registration State Transition |
| Version | NEW-shot2play Protocol Suite Version 2.0 |
| Revision | Design Definition Version 2.0 |
| Status | Design Definition |
| Purpose | Registration lifecycle state model |

---

# 1. Purpose

Figure05 defines the lifecycle state transition model
of the registration process.

While Figure04 describes the registration transaction flow,
Figure05 defines the persistent states maintained during
registration lifecycle management.

---

# 2. Registration State Model

The registration lifecycle consists of:

- Initial State
- Registration Requested
- Identity Verified
- Credential Registered
- Device Bound
- Registration Completed
- Revoked State

---

# 3. State Transition Flow

The fundamental state transition is:

Initial

        ↓

Registration Requested

        ↓

Identity Verified

        ↓

Credential Registered

        ↓

Device Bound

        ↓

Registration Completed

        ↓

Revoked

---

# 4. Initial State

## Purpose

Represents the condition before registration begins.

## Characteristics

- No registered credential
- No device relationship
- No authorization capability

---

# 5. Registration Requested State

## Purpose

Represents a received and accepted registration request.

## Stored Information

- Registration identifier
- User context
- Request timestamp

---

# 6. Identity Verified State

## Purpose

Represents successful identity verification.

## Characteristics

- Identity trust established
- Registration may continue

---

# 7. Credential Registered State

## Purpose

Represents successful credential creation and registration.

## Characteristics

- Public verification information stored
- Private key remains protected on device

---

# 8. Device Bound State

## Purpose

Represents the association between credential and device.

## Characteristics

- Device relationship established
- Future authentication enabled

---

# 9. Registration Completed State

## Purpose

Represents a fully available authentication environment.

## Characteristics

- Authentication possible
- Entitlement association possible
- Authorization processing available

---

# 10. Revoked State

## Purpose

Represents a terminated registration relationship.

## Causes

- User request
- Device replacement
- Security incident
- Administrative action

---

# 11. Version 2.0 Extension

Version 2.0 extends the lifecycle beyond authentication.

Completed registration may create:

Registration Completed

        ↓

Entitlement Context

        ↓

Authorization Capability

---

# 12. Security Consideration

State transition SHALL prevent:

- Unauthorized credential activation
- Invalid device binding
- Replay registration
- State rollback

---

# 13. Patent Boundary Candidate

Figure05 represents:

- Registration lifecycle management
- Credential state control
- Device trust transition
- Authorization preparation state

---

# 14. Non-Goals

Figure05 does not define:

- Authentication protocol messages
- Runtime authorization decisions
- Transport communication

---

# 15. SVG Design Requirements

The SVG representation SHALL include:

- State transition diagram
- State names
- Transition direction
- Version 2.0 extension area
- Revocation path

---

# 16. Design Freeze Decision

Figure05 is approved as the registration lifecycle
reference model for NEW-shot2play Protocol Suite
Version 2.0.

Future modifications require Design Freeze review.

