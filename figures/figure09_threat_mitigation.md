# Figure09 Threat Mitigation

## Document Information

| Item | Value |
|---|---|
| Figure ID | Figure09 |
| Title | Threat Mitigation |
| Version | NEW-shot2play Protocol Suite Version 2.0 |
| Revision | Design Definition Version 2.0 |
| Status | Design Definition |
| Purpose | Threat-to-control mapping for authentication and authorization security |

---

# 1. Purpose

Figure09 defines the principal security threats considered by the
NEW-shot2play Protocol Suite Version 2.0 and maps each threat to
the corresponding security control.

The figure establishes a direct relationship among:

- Threat
- Attack Condition
- Security Control
- Security Result

The purpose is to demonstrate how protocol mechanisms mitigate
specific security threats.

---

# 2. Threat Mitigation Model

The fundamental processing model is:

Threat

        ↓

Attack Condition

        ↓

Security Control

        ↓

Security Result

Each threat is evaluated against one or more protocol controls.

---

# 3. QR Token Replay

## Threat

An attacker attempts to reuse a previously observed QR authentication
token.

## Attack Condition

A valid authentication token is captured and presented again.

## Security Controls

- One-Time Token
- Server-Side Consumption State
- Short Validity Period

## Security Result

A consumed or expired token cannot be successfully reused.

---

# 4. Token Expiration Abuse

## Threat

An attacker attempts to use an authentication token after its
validity period has ended.

## Attack Condition

A token remains available after expiration.

## Security Controls

- Short Validity Period
- Server-Side Expiration Validation

## Security Result

Expired authentication transactions are rejected.

---

# 5. Challenge Replay

## Threat

An attacker attempts to reuse a previously issued authentication
challenge.

## Attack Condition

A challenge from one authentication transaction is presented
in another transaction or reused after completion.

## Security Controls

- Transaction-Specific Challenge
- Transaction Binding
- Server-Side State Validation

## Security Result

A challenge cannot be successfully reused outside its intended
transaction context.

---

# 6. Credential Theft

## Threat

An attacker attempts to obtain the private authentication
credential.

## Attack Condition

Private key material is exposed or copied from the device.

## Security Controls

- Device-Protected Private Key
- Hardware-Backed or Protected Credential Environment
- Private Key Non-Transmission

## Security Result

The private key remains within the protected device environment.

---

# 7. Signature Forgery

## Threat

An attacker attempts to submit an invalid or forged authentication
signature.

## Attack Condition

A response is submitted without possession of the registered
private key.

## Security Controls

- Cryptographic Signature
- Server-Side Signature Verification
- Credential Association

## Security Result

Invalid signatures fail authentication verification.

---

# 8. Revoked Device

## Threat

A previously registered device attempts authentication after its
registration has been revoked or terminated.

## Attack Condition

A credential associated with a revoked registration is presented.

## Security Controls

- Registration State
- Revocation
- Registration Validation

## Security Result

Revoked or terminated registrations cannot establish authentication.

---

# 9. Transaction Tampering

## Threat

An attacker attempts to alter authentication transaction data.

## Attack Condition

Transaction-related data is modified before verification.

## Security Controls

- Transaction Binding
- Challenge
- Cryptographic Signature
- Server-Side Validation

## Security Result

Modified transaction data fails applicable validation.

---

# 10. Unauthorized Action

## Threat

An authenticated subject attempts to perform an action that is
not permitted.

## Attack Condition

Authentication succeeds but the requested action exceeds the
subject's permitted entitlement.

## Security Controls

- Entitlement
- Policy Evaluation
- Authorization Decision

## Security Result

Authentication alone does not grant unauthorized permissions.

The requested action is permitted only when the authorization
policy allows it.

---

# 11. Threat-to-Control Mapping

| Threat | Primary Security Control | Result |
|---|---|---|
| QR Token Replay | One-Time Token | Replay rejected |
| Token Expiration Abuse | Short Validity Period | Expired token rejected |
| Challenge Replay | Transaction Binding | Reuse rejected |
| Credential Theft | Device-Protected Private Key | Secret remains protected |
| Signature Forgery | Cryptographic Verification | Invalid response rejected |
| Revoked Device | Registration State | Revoked credential rejected |
| Transaction Tampering | Signature + State Validation | Modified transaction rejected |
| Unauthorized Action | Entitlement + Policy | Unauthorized action denied |

---

# 12. Defense-in-Depth

The protocol does not depend on a single security control.

The authentication security model combines:

- Temporary transaction state
- One-time token usage
- Challenge freshness
- Device-protected credentials
- Cryptographic verification
- Registration state
- Server-side authoritative state

The authorization security model adds:

- Entitlement
- Policy Evaluation
- Authorization Decision

The combination provides defense-in-depth across the
authentication and authorization lifecycle.

---

# 13. Security Control Relationships

The principal relationships are:

QR Token Replay

        ↓

One-Time Token

        ↓

Consumption State


Challenge Replay

        ↓

Transaction Binding

        ↓

Freshness Validation


Credential Theft

        ↓

Device-Protected Private Key

        ↓

Proof of Possession


Signature Forgery

        ↓

Cryptographic Verification

        ↓

Authentication Failure


Revoked Device

        ↓

Registration State

        ↓

Authentication Denial


Unauthorized Action

        ↓

Entitlement

        ↓

Policy Evaluation

        ↓

Authorization Decision

---

# 14. Residual Security Principle

No single protocol mechanism is assumed to eliminate every threat.

Security is achieved through the combined operation of:

- Device security
- Transaction security
- Cryptographic verification
- Server-side state
- Registration control
- Authorization policy

A failure of one control SHALL NOT silently convert an invalid
authentication or authorization request into a valid request.

---

# 15. Version 2.0 Extension

Version 2.0 explicitly extends threat mitigation beyond
authentication into authorization.

The extended model is:

Authentication Threats

        ↓

Authentication Controls

        ↓

Authenticated Identity

        ↓

Entitlement

        ↓

Policy Evaluation

        ↓

Authorization Controls

        ↓

Authorized Service Action

This separation ensures that successful authentication does not
automatically imply unrestricted service access.

---

# 16. Security Boundary

Threat mitigation is applied across the principal security
boundaries:

## Device Boundary

Protects:

- Private Key
- Credential Secret Material

## Authentication Boundary

Protects:

- Token
- Challenge
- Transaction State
- Signature Verification

## Server Boundary

Protects:

- Registration State
- Token State
- Verification State

## Authorization Boundary

Protects:

- Entitlement
- Policy
- Authorization Decision

---

# 17. Non-Goals

Figure09 does not define:

- Detailed threat modeling methodology
- Specific cryptographic algorithm selection
- Network firewall architecture
- Operating system security configuration
- User interface security controls

Those elements are defined elsewhere where applicable.

---

# 18. SVG Design Requirements

The SVG representation SHALL include:

- Threat column
- Attack Condition column
- Security Control column
- Security Result column
- Clear mapping arrows
- Version 2.0 authorization extension

The threat-to-control relationship SHALL be visually dominant.

Threats and controls SHALL be distinguishable without relying
solely on color.

No text or arrow SHALL extend outside its containing visual region.

---

# 19. Design Freeze Decision

Figure09 is designated as the threat mitigation reference model
for NEW-shot2play Protocol Suite Version 2.0.

Future modifications require Design Freeze review.

