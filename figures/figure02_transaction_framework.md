# Figure02 Transaction Framework

## Document Information

| Item | Value |
|---|---|
| Figure ID | Figure02 |
| Title | Transaction Framework |
| Version | NEW-shot2play Protocol Suite Version 2.0 |
| Revision | Design Freeze Version 2.0 Revision 1.1 |
| Status | Design Definition |
| Purpose | Runtime transaction processing representation |

---

# 1. Purpose

Figure02 defines the runtime transaction framework of the
NEW-shot2play Protocol Suite Version 2.0.

While Figure01 defines the static architecture layers,
Figure02 defines the processing sequence executed during
a service access transaction.

The purpose of this figure is to clarify the relationship
between:

- Authentication Event
- Entitlement Evaluation
- Policy Decision
- Authorization Result
- Service Execution

---

# 2. Transaction Processing Principle

The fundamental transaction flow is:

Authentication Event

        ↓

Authentication Context Creation

        ↓

Entitlement Evaluation

        ↓

Policy Evaluation

        ↓

Authorization Decision

        ↓

Service Execution

        ↓

Audit Record

---

# 3. Transaction Object Model

A Version 2.0 transaction consists of the following
logical objects.

---

## 3.1 Authentication Context

### Purpose

Represents the result and assurance information generated
by authentication processing.

### Object

Authentication Context

### Contains

- Identity information
- Authentication result
- Trust level
- Authentication timestamp

---

## 3.2 Entitlement Context

### Purpose

Represents rights, qualifications, and usage conditions
associated with the authenticated entity.

### Object

Entitlement Context

### Contains

- Entitlement identifier
- Applicable rights
- Validity period
- Usage conditions

---

## 3.3 Policy Context

### Purpose

Represents the rules and conditions evaluated for
authorization.

### Object

Policy Context

### Contains

- Policy identifier
- Evaluation conditions
- Decision parameters

---

## 3.4 Authorization Result

### Purpose

Represents the final permission decision.

### Object

Authorization Result

### Result Types

- Permit
- Deny
- Conditional Permit

---

# 4. Runtime Sequence

The runtime sequence is:

Request Initiation

        ↓

Authentication Processing

        ↓

Authentication Context Generated

        ↓

Entitlement Lookup

        ↓

Policy Evaluation

        ↓

Authorization Decision

        ↓

Service Response

        ↓

Audit Logging

---

# 5. Conditional Authorization Model

Version 2.0 introduces conditional authorization.

Authorization is not determined only by identity.

The decision may depend on:

- Authentication result
- Entitlement state
- Policy conditions
- External evidence

---

# 6. Scenario D Representation

Example:

Customer visits a physical store.

The visit authentication event creates evidence.

The customer later accesses an EC service.

The authorization decision evaluates the previous visit
condition and applies a discount permission.

Processing model:

Store Visit Authentication

        ↓

Visit Evidence Creation

        ↓

Entitlement Update

        ↓

EC Authentication

        ↓

Policy Evaluation

        ↓

Discount Authorization

        ↓

EC Service Access

---

# 7. Patent Boundary Candidate

Figure02 represents the following innovation areas:

- Authentication Event linked entitlement
- Conditional authorization
- Cross-service entitlement reuse
- Evidence-based policy evaluation

---

# 8. Non-Goals

Figure02 does not define:

- Cryptographic algorithms
- Detailed message formats
- User interface behavior
- Specific application implementation

These are defined in protocol specification sections.

---

# 9. SVG Design Requirements

The SVG representation SHALL include:

- Transaction timeline
- Processing objects
- Decision points
- Conditional authorization flow
- Scenario D example
- Innovation boundary indication

---

# 10. Design Freeze Decision

Figure02 is approved as the transaction processing
reference model for NEW-shot2play Protocol Suite
Version 2.0.

Future modifications require Design Freeze review.

