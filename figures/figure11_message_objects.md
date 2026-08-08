# Figure11 Message Objects

## Document Information

| Item | Value |
|---|---|
| Figure ID | Figure11 |
| Title | Message Objects |
| Version | NEW-shot2play Protocol Suite Version 2.0 |
| Revision | Design Definition Version 2.0 |
| Status | Design Definition |
| Purpose | Definition of protocol messages exchanged between protocol participants |

---

# 1. Purpose

Figure11 defines the principal message objects exchanged during
the NEW-shot2play Protocol Suite Version 2.0 lifecycle.

The figure distinguishes protocol messages from the underlying
Protocol Data Objects defined in Figure10.

A message represents a protocol exchange.

A data object represents the information carried, referenced, or
maintained by the protocol.

---

# 2. Message Object Model

The principal message sequence is:

Registration Request

        ↓

Registration Response

        ↓

Authentication Request

        ↓

Authentication Response

        ↓

Entitlement Request

        ↓

Entitlement Response

        ↓

Authorization Request

        ↓

Authorization Decision

The actual implementation MAY combine or optimize individual
messages provided that the logical protocol responsibilities
remain equivalent.

---

# 3. Registration Request

The Registration Request initiates registration of a device and
authentication credential.

## Primary Responsibilities

- Identify the subject
- Identify the device
- Provide credential registration information
- Provide the public verification key
- Establish registration context

## Representative Content

- Registration Request Identifier
- User Identifier
- Device Identifier
- Credential Identifier
- Public Key
- Registration Metadata

## Security Requirements

The Registration Request SHALL NOT contain the private key.

The request SHALL be associated with a server-controlled
registration transaction.

---

# 4. Registration Response

The Registration Response confirms the result of a registration
request.

## Primary Responsibilities

- Identify registration result
- Provide Registration Identifier
- Establish registration state
- Return applicable registration metadata

## Representative Content

- Registration Transaction Identifier
- Registration Identifier
- Registration State
- Registration Result
- Server Timestamp

## Possible Results

- Registered
- Rejected
- Failed

---

# 5. Authentication Request

The Authentication Request initiates an authentication transaction.

## Primary Responsibilities

- Identify the authentication transaction
- Provide or reference the QR authentication context
- Establish challenge information
- Establish transaction expiration

## Representative Content

- Authentication Transaction Identifier
- Registration Identifier
- QR Token Identifier
- Challenge
- Creation Time
- Expiration Time

## Security Requirements

The Authentication Request SHALL be associated with a unique
authentication transaction.

The transaction SHALL have an explicit validity period.

---

# 6. Authentication Response

The Authentication Response provides proof associated with the
authentication transaction.

## Primary Responsibilities

- Reference the authentication transaction
- Provide cryptographic proof
- Bind the proof to the transaction challenge
- Return authentication result information

## Representative Content

- Authentication Transaction Identifier
- Challenge Reference
- Credential Identifier
- Cryptographic Signature
- Client Timestamp
- Authentication Result

## Security Requirements

The response SHALL be verified against the registered public key.

The private key SHALL remain within the protected device
environment.

---

# 7. Entitlement Request

The Entitlement Request determines which capabilities are available
to the authenticated subject.

## Primary Responsibilities

- Reference authenticated identity
- Identify requested service context
- Request applicable entitlement information

## Representative Content

- Entitlement Request Identifier
- Authentication Transaction Identifier
- Subject Identifier
- Service Identifier
- Requested Capability

## Version 2.0 Role

The Entitlement Request introduces an explicit authorization
processing step following successful authentication.

Authentication success SHALL NOT by itself imply unrestricted
entitlement.

---

# 8. Entitlement Response

The Entitlement Response provides the capability information
applicable to the authenticated subject.

## Primary Responsibilities

- Identify applicable entitlement
- Identify capability
- Identify scope
- Identify entitlement state

## Representative Content

- Entitlement Identifier
- Subject Identifier
- Capability
- Scope
- Effective Time
- Expiration Time
- Entitlement State

## Possible Results

- Entitled
- Not Entitled
- Expired
- Revoked

---

# 9. Authorization Request

The Authorization Request asks whether a specific action is
permitted.

## Primary Responsibilities

- Identify the authenticated subject
- Identify requested action
- Identify target resource
- Reference entitlement
- Provide authorization context

## Representative Content

- Authorization Request Identifier
- Authentication Transaction Identifier
- Subject Identifier
- Entitlement Identifier
- Requested Action
- Target Resource
- Context Information

## Version 2.0 Role

The Authorization Request separates authentication from the
decision to permit a specific service action.

---

# 10. Authorization Decision

The Authorization Decision provides the result of policy evaluation.

## Primary Responsibilities

- Identify authorization request
- Reference applicable entitlement
- Reference applicable policy
- Provide authorization result
- Establish decision timestamp

## Representative Content

- Authorization Identifier
- Authorization Request Identifier
- Entitlement Identifier
- Policy Identifier
- Authorization Decision
- Decision Timestamp

## Possible Results

- Allowed
- Denied

---

# 11. Message-to-Object Relationship

Messages carry or reference the Protocol Data Objects defined
in Figure10.

| Message | Primary Data Object |
|---|---|
| Registration Request | Registration Object |
| Registration Response | Registration Object |
| Authentication Request | Authentication Object |
| Authentication Response | Authentication Object |
| Entitlement Request | Entitlement Object |
| Entitlement Response | Entitlement Object |
| Authorization Request | Authorization Object |
| Authorization Decision | Authorization Object |

A message SHALL NOT be interpreted as the persistent data object
it references.

---

# 12. Message Flow

The logical Version 2.0 message flow is:

Registration Request

        ↓

Registration Response

        ↓

Authentication Request

        ↓

Authentication Response

        ↓

Authentication Verification

        ↓

Entitlement Request

        ↓

Entitlement Response

        ↓

Authorization Request

        ↓

Policy Evaluation

        ↓

Authorization Decision

        ↓

Service Action

---

# 13. Authentication Message Boundary

Authentication messages establish identity assurance.

The authentication boundary consists of:

- Authentication Request
- Authentication Response
- Challenge
- Cryptographic Proof
- Verification Result

Successful completion produces an authenticated identity context.

---

# 14. Authorization Message Boundary

Authorization messages establish permission to perform a service
action.

The authorization boundary consists of:

- Entitlement Request
- Entitlement Response
- Authorization Request
- Policy Evaluation
- Authorization Decision

This boundary occurs after authentication verification.

---

# 15. Message State

Each message transaction SHALL be associated with an applicable
state.

## Registration

Requested

        ↓

Processed

        ↓

Registered / Rejected

## Authentication

Requested

        ↓

Challenge Issued

        ↓

Response Received

        ↓

Verified / Rejected

## Entitlement

Requested

        ↓

Evaluated

        ↓

Returned

## Authorization

Requested

        ↓

Evaluated

        ↓

Allowed / Denied

---

# 16. Message Security Properties

The protocol messages SHALL support the following properties:

## Freshness

Authentication messages SHALL be bound to a current transaction
and challenge.

## Integrity

Security-sensitive message content SHALL be protected against
unauthorized modification.

## Authenticity

Authentication responses SHALL provide cryptographic proof
associated with the registered credential.

## State Validation

Messages SHALL be validated against authoritative server-side
transaction state.

## Authorization Separation

Authentication messages SHALL NOT be treated as authorization
decisions.

---

# 17. Message Error Handling

A message MAY result in an error when:

- Transaction is unknown
- Transaction is expired
- Token is already consumed
- Challenge is invalid
- Signature verification fails
- Registration is revoked
- Entitlement is unavailable
- Policy evaluation fails
- Requested action is not authorized

The implementation SHALL return a deterministic protocol result
for each applicable failure condition.

---

# 18. Version 2.0 Extension

Version 2.0 extends the message model beyond authentication.

The extended sequence is:

Authentication Request

        ↓

Authentication Response

        ↓

Authenticated Identity

        ↓

Entitlement Request

        ↓

Entitlement Response

        ↓

Authorization Request

        ↓

Authorization Decision

        ↓

Service Action

This establishes an explicit message boundary between
authentication and authorization.

---

# 19. Non-Goals

Figure11 does not define:

- HTTP method selection
- URI structure
- JSON serialization syntax
- Transport encryption configuration
- API gateway configuration
- Network routing
- User interface behavior

Those implementation details are defined separately.

---

# 20. SVG Design Requirements

The SVG representation SHALL include:

- Registration Request / Response
- Authentication Request / Response
- Entitlement Request / Response
- Authorization Request
- Authorization Decision
- Message-to-object relationship
- Version 2.0 authorization extension

Request and response messages SHALL be visually distinguishable.

The logical sequence SHALL be clear.

No message label or connector SHALL extend outside its containing
visual region.

---

# 21. Design Freeze Decision

Figure11 is designated as the message object reference model for
NEW-shot2play Protocol Suite Version 2.0.

Future modifications require Design Freeze review.

