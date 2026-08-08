# Figure08 Security Architecture

## Document Information

| Item | Value |
|---|---|
| Figure ID | Figure08 |
| Title | Security Architecture |
| Version | NEW-shot2play Protocol Suite Version 2.0 |
| Revision | Design Definition Version 2.0 |
| Status | Design Definition |
| Purpose | Security boundaries, responsibilities, and control layers |

---

# 1. Purpose

Figure08 defines the security architecture of the
NEW-shot2play Protocol Suite Version 2.0.

The architecture separates security responsibilities across:

- Device Security
- Authentication Security
- Server Security
- Authorization Security

The model also identifies the principal trust boundaries between
the protected device environment and the server environment.

---

# 2. Security Architecture Model

The fundamental security structure is:

Device Security

        ↓

Authentication Security

        ↓

Server Security

        ↓

Authorization Security

Each layer has a distinct security responsibility.

---

# 3. Device Security

## Purpose

Device Security protects credential material used to establish
proof of possession.

## Principal Controls

- Private Key Protection
- Credential Protection
- Device Binding

## Security Responsibility

The private key SHALL remain within the protected device
credential environment.

The private key SHALL NOT be transmitted to the authentication
server.

---

# 4. Private Key Protection

The private key represents the secret credential material used
to generate the authentication signature.

The architecture assumes that the private key is protected by
the device security environment.

Examples of protected environments may include:

- Secure Enclave
- Trusted Execution Environment
- Hardware-backed keystore
- Equivalent protected credential storage

The protocol specification does not require a particular hardware
implementation.

---

# 5. Credential Protection

Credential protection ensures that authentication proof can only
be generated through the registered device credential.

The server maintains verification information rather than the
private key.

Conceptually:

Private Key

        ↓

Signature

        ↓

Server Verification

---

# 6. Device Binding

Device Binding associates the registered credential with the
intended device registration.

The server SHALL maintain sufficient registration information
to determine whether the credential is currently valid.

A revoked registration SHALL NOT be accepted for authentication.

---

# 7. Authentication Security

## Purpose

Authentication Security protects the authentication transaction
against replay, substitution, and unauthorized reuse.

## Principal Controls

- One-Time QR Token
- Challenge
- Signature
- Verification

---

# 8. One-Time QR Token

The One-Time QR Token provides a temporary authentication
transaction reference.

Security properties include:

- Random generation
- Short validity period
- Single-use processing
- Server-side consumption state
- Expiration

The QR token does not itself constitute authentication proof.

---

# 9. Challenge

The challenge provides transaction freshness.

The challenge SHALL be associated with the authentication
transaction and SHALL NOT be reusable across unrelated
authentication transactions.

---

# 10. Signature

The registered device generates a cryptographic signature using
the protected private key.

The signature provides proof of possession of the registered
credential.

---

# 11. Server Security

## Purpose

Server Security maintains the authoritative transaction and
registration state.

## Principal Controls

- Token State
- Registration State
- Verification State

---

# 12. Token State

The server SHALL maintain sufficient state to determine whether
a one-time authentication token is:

- Valid
- Expired
- Consumed
- Invalid

A consumed token SHALL NOT be accepted again.

---

# 13. Registration State

The server SHALL maintain the registration state associated with
the credential.

Principal states include:

- Registered
- Active
- Revoked
- Terminated

Authentication SHALL require an acceptable registration state.

---

# 14. Verification State

The server evaluates the authentication response.

Verification SHALL include the applicable:

- Token validation
- Challenge validation
- Credential association
- Signature verification
- Registration state validation

---

# 15. Authorization Security

## Purpose

Authorization Security determines what an authenticated subject
is permitted to perform.

Authentication and authorization are separate security functions.

## Principal Controls

- Entitlement
- Policy Evaluation
- Authorization Decision

---

# 16. Entitlement

Entitlement represents the rights, attributes, or permissions
available to the authenticated subject within the applicable
service context.

Authentication does not automatically imply a particular
entitlement.

---

# 17. Policy Evaluation

Policy Evaluation determines whether the applicable entitlement
and contextual conditions satisfy the authorization policy.

Potential inputs include:

- Authenticated identity
- Entitlement
- Service context
- Transaction context
- Policy conditions

---

# 18. Authorization Decision

The authorization layer produces the final decision.

Conceptually:

Authenticated

        ↓

Entitlement

        ↓

Policy Evaluation

        ↓

Authorization Decision

The authorization decision SHALL be independent from the
cryptographic authentication proof itself.

---

# 19. Trust Boundaries

The architecture identifies two principal security environments.

## Device Trust Boundary

The protected device environment contains:

- Private Key
- Device Credential Secret Material

The private key remains within this boundary.

## Server Trust Boundary

The server environment contains:

- Public Verification Information
- Token State
- Registration State
- Verification State
- Authorization Context

The server does not require possession of the device private key.

---

# 20. Cross-Boundary Data

Information crossing the device/server boundary may include:

- One-Time QR Token
- Challenge
- Authentication Response
- Signature
- Authentication Result

Private credential material SHALL NOT cross the trust boundary.

---

# 21. Security Separation

The architecture separates four security responsibilities:

| Layer | Primary Responsibility |
|---|---|
| Device Security | Protect credential secret material |
| Authentication Security | Prove possession and transaction freshness |
| Server Security | Maintain authoritative state and verify responses |
| Authorization Security | Determine permitted actions |

This separation prevents authentication state from being confused
with authorization state.

---

# 22. Version 2.0 Extension

Version 2.0 extends the authentication security architecture
toward entitlement-aware authorization.

The extended processing model is:

Authentication

        ↓

Entitlement

        ↓

Policy Evaluation

        ↓

Authorization

        ↓

Service

The extension permits the authentication result to become a
controlled input to subsequent authorization processing.

---

# 23. Security Principles

The architecture is based on the following principles:

1. Private credential material remains device-protected.
2. Authentication transactions are temporary.
3. Authentication tokens are single-use.
4. Authentication responses are transaction-bound.
5. Registration state is authoritative.
6. Authentication and authorization are separate functions.
7. Authorization is policy-controlled.
8. Terminal security states cannot be silently reversed.

---

# 24. Non-Goals

Figure08 does not define:

- Specific hardware security implementation
- Specific cryptographic algorithm selection
- Detailed transport protocol
- Detailed authorization policy syntax
- User interface design

Those elements are defined elsewhere in the specification.

---

# 25. SVG Design Requirements

The SVG representation SHALL include:

- Device Security
- Authentication Security
- Server Security
- Authorization Security
- Device Trust Boundary
- Server Trust Boundary
- Cross-boundary data flow
- Version 2.0 authorization extension

The trust boundary SHALL be visually distinguishable from ordinary
processing relationships.

Security responsibilities SHALL be represented as architectural
layers rather than as individual protocol messages.

No label or explanatory text SHALL extend outside its containing
visual region.

---

# 26. Design Freeze Decision

Figure08 is designated as the security architecture reference
model for NEW-shot2play Protocol Suite Version 2.0.

Future modifications require Design Freeze review.

