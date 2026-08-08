# Figure12 Authentication Message Objects

## Document Information

| Item | Value |
|---|---|
| Figure ID | Figure12 |
| Title | Authentication Message Objects |
| Version | NEW-shot2play Protocol Suite Version 2.0 |
| Revision | Design Definition Version 2.0 |
| Status | Design Definition |
| Purpose | Definition of authentication message objects and their relationships |

---

# 1. Purpose

Figure12 defines the internal structure of the authentication
messages used by the NEW-shot2play Protocol Suite Version 2.0.

The figure identifies the principal information elements exchanged
during an authentication transaction and their relationship to the
authentication transaction state.

The model distinguishes:

- Authentication Request
- QR Token
- Transaction Context
- Challenge
- Authentication Response
- Cryptographic Proof
- Verification Result

---

# 2. Authentication Message Model

The fundamental authentication message model is:

Authentication Request

        ↓

Transaction Context

        ↓

Challenge + QR Token

        ↓

Authentication Response

        ↓

Cryptographic Proof

        ↓

Verification

        ↓

Authentication Result

The elements SHALL remain bound to the same authentication
transaction.

---

# 3. Authentication Request

The Authentication Request establishes the server-side context
for an authentication transaction.

## Primary Responsibilities

- Create or reference an authentication transaction
- Associate the transaction with a registered identity
- Establish a challenge
- Establish transaction expiration
- Establish the QR authentication context

## Representative Elements

- Authentication Transaction Identifier
- Registration Identifier
- Challenge
- QR Token Identifier
- Creation Timestamp
- Expiration Timestamp
- Transaction State

---

# 4. Authentication Transaction Identifier

The Authentication Transaction Identifier uniquely identifies the
authentication transaction.

## Requirements

The identifier SHALL be unique within the applicable protocol
scope.

It SHALL allow the server to associate:

- Authentication Request
- Authentication Response
- Challenge
- QR Token
- Verification Result

with the same transaction.

The identifier SHALL NOT itself be treated as authentication proof.

---

# 5. QR Token

The QR Token represents the short-lived transaction reference
presented for authentication.

## Primary Responsibilities

- Bind the displayed QR authentication context to the transaction
- Identify the applicable authentication transaction
- Support freshness validation
- Prevent reuse after consumption or expiration

## Security Properties

The QR Token SHALL:

- Have a limited validity period
- Be associated with a single authentication transaction
- Become invalid after successful consumption
- Become invalid after expiration
- Be validated against authoritative server-side state

The QR Token SHALL NOT be treated as a substitute for the
cryptographic authentication proof.

---

# 6. Transaction Context

The Transaction Context contains the server-side state necessary
to validate the authentication exchange.

## Representative Elements

- Authentication Transaction Identifier
- Registration Identifier
- Challenge
- QR Token Identifier
- Creation Timestamp
- Expiration Timestamp
- Consumption State
- Authentication State

## Security Role

The Transaction Context prevents a response from being accepted
outside the transaction for which it was created.

---

# 7. Challenge

The Challenge is the transaction-specific value used to bind the
cryptographic response to the current authentication transaction.

## Requirements

The Challenge SHALL:

- Be generated for the authentication transaction
- Be unpredictable
- Be associated with the applicable registration
- Be validated during verification
- Prevent replay of a previously generated response

The Challenge SHALL NOT be reused across independent authentication
transactions.

---

# 8. Authentication Response

The Authentication Response provides the client-side proof
associated with the Authentication Request.

## Primary Responsibilities

- Reference the authentication transaction
- Bind the response to the Challenge
- Identify the registered credential
- Provide cryptographic proof
- Return client-side authentication information

## Representative Elements

- Authentication Transaction Identifier
- Challenge Reference
- Credential Identifier
- Cryptographic Signature
- Client Timestamp
- Response State

---

# 9. Cryptographic Proof

The Cryptographic Proof demonstrates possession of the private key
associated with the registered public key.

## Primary Responsibilities

- Prove possession of the registered credential
- Bind the response to the current Challenge
- Prevent unauthorized credential substitution

## Verification

The server SHALL verify the Cryptographic Proof using the public
key associated with the registered credential.

The private key SHALL remain within the protected client device
environment.

---

# 10. Verification Result

The Verification Result represents the result of server-side
validation of the Authentication Response.

## Verification Conditions

The server SHALL verify, as applicable:

- Transaction existence
- Transaction state
- Transaction expiration
- QR Token validity
- Challenge validity
- Credential registration state
- Cryptographic proof
- Replay / consumption state

## Possible Results

- Verified
- Rejected
- Expired
- Already Consumed
- Invalid Challenge
- Invalid Credential
- Invalid Proof

---

# 11. Authentication Result

The Authentication Result represents the final authentication
outcome.

## Successful Result

A successful authentication establishes an authenticated identity
context.

## Failed Result

A failed authentication SHALL NOT establish an authenticated
identity context.

The result SHALL be associated with the applicable authentication
transaction.

---

# 12. Message Binding

The following elements SHALL remain logically bound:

Authentication Transaction Identifier

        +

QR Token

        +

Challenge

        +

Credential Identifier

        +

Cryptographic Proof

        ↓

Verification Result

The server SHALL reject a response when the elements do not belong
to the same valid authentication transaction.

---

# 13. Authentication Message Lifecycle

The authentication message lifecycle is:

Created

        ↓

Challenge Issued

        ↓

QR Context Presented

        ↓

Authentication Response Received

        ↓

Cryptographic Verification

        ↓

Verified / Rejected

        ↓

Transaction Consumed

A successfully verified transaction SHALL become consumed and
shall not be accepted again.

---

# 14. Freshness Model

Authentication freshness is established through the combination
of:

- Unique Authentication Transaction Identifier
- Short-lived QR Token
- Transaction-specific Challenge
- Transaction State
- One-time Consumption

The server SHALL validate all applicable freshness conditions
before accepting the authentication response.

---

# 15. Replay Protection

Replay protection SHALL be provided by:

1. Transaction-specific Challenge
2. Short-lived QR Token
3. Explicit expiration
4. Server-side transaction state
5. One-time consumption

A previously accepted Authentication Response SHALL NOT be
accepted again.

An expired Authentication Response SHALL NOT be accepted.

A response associated with a different transaction SHALL NOT be
accepted.

---

# 16. Registration Relationship

The Authentication Message Objects reference the registered
credential established by the Registration Object.

The logical relationship is:

Registration Object

        ↓

Registered Credential

        ↓

Authentication Request

        ↓

Authentication Response

        ↓

Cryptographic Verification

The Authentication Message Objects do not create a new credential.

---

# 17. Authentication-to-Authorization Boundary

Successful authentication produces an authenticated identity
context.

It does not itself produce unrestricted authorization.

The Version 2.0 sequence is:

Authentication Request

        ↓

Authentication Response

        ↓

Verification Result

        ↓

Authenticated Identity

        ↓

Entitlement

        ↓

Policy Evaluation

        ↓

Authorization Decision

This boundary SHALL remain explicit.

---

# 18. Server-Side and Client-Side Responsibility

## Server-Side

The server is responsible for:

- Transaction creation
- Challenge generation
- QR Token validation
- Transaction expiration
- Registration lookup
- Public key lookup
- Cryptographic verification
- Replay detection
- Transaction consumption
- Authentication result

## Client-Side

The client is responsible for:

- Reading the QR authentication context
- Accessing the registered credential
- Producing the cryptographic proof
- Returning the Authentication Response

Private key material SHALL remain on the protected client device.

---

# 19. Security Boundary

The Authentication Message Object model defines the following
security boundaries:

## Transaction Boundary

The Authentication Transaction Identifier and server-side state
define the transaction boundary.

## Credential Boundary

The registered public key identifies the credential used for
verification.

## Proof Boundary

The Cryptographic Proof demonstrates possession of the
corresponding private key.

## Authorization Boundary

Authentication success terminates the authentication boundary.
Entitlement and Policy Evaluation occur after authentication.

---

# 20. Version 2.0 Extension

Version 2.0 preserves the core authentication message model while
making the transition to authorization explicit.

The extended model is:

Authentication Request

        ↓

Authentication Response

        ↓

Verification Result

        ↓

Authenticated Identity

        ↓

Entitlement Context

        ↓

Policy Evaluation

        ↓

Authorization Decision

        ↓

Service

The Authentication Message Objects are therefore the source of
identity assurance but not the final authorization decision.

---

# 21. Non-Goals

Figure12 does not define:

- Specific cryptographic algorithms
- Specific signature encoding
- JSON schema
- HTTP method
- URI structure
- TLS configuration
- Database implementation
- User interface layout

Those details are defined elsewhere in the implementation and
protocol specifications.

---

# 22. SVG Design Requirements

The SVG representation SHALL include:

- Authentication Request
- Transaction Context
- QR Token
- Challenge
- Authentication Response
- Cryptographic Proof
- Verification Result
- Authentication-to-Authorization boundary
- Version 2.0 authorization extension

The relationship between transaction state and cryptographic proof
SHALL be visually clear.

The distinction between authentication and authorization SHALL be
explicit.

No text or connector SHALL extend outside its containing visual
region.

---

# 23. Design Freeze Decision

Figure12 is designated as the authentication message object
reference model for NEW-shot2play Protocol Suite Version 2.0.

Future modifications require Design Freeze review.

