# Figure03 Transaction Object Model

## Document Information

| Item | Value |
|---|---|
| Figure ID | Figure03 |
| Title | Transaction Object Model |
| Version | NEW-shot2play Protocol Suite Version 2.0 |
| Revision | Design Freeze Version 2.0 Revision 1.1 |
| Status | Design Definition |
| Purpose | Protocol object relationship representation |

---

# 1. Purpose

Figure03 defines the protocol object model used within
the NEW-shot2play Protocol Suite Version 2.0.

Figure01 defines the architecture layers.

Figure02 defines the runtime transaction sequence.

Figure03 defines the logical data objects exchanged,
evaluated, and generated during authorization processing.

---

# 2. Object Processing Model

The fundamental object relationship is:

Authentication Object

        ↓

Entitlement Object

        ↓

Policy Object

        ↓

Authorization Object

        ↓

Service Request Object

---

# 3. Object Relationship Overview

A Version 2.0 transaction consists of:

1. Authentication Object

Identity assurance information.

2. Entitlement Object

Rights, qualifications, and conditions.

3. Policy Object

Evaluation rules and decision conditions.

4. Authorization Object

Final permission result.

5. Service Request Object

Application service execution request.

---

# 4. Authentication Object

## Purpose

Represents the authentication result and trust context.

## Responsibilities

- Identity verification result
- Authentication assurance
- Authentication timestamp
- Session context

## Relationship

Authentication Object provides the trusted input
for entitlement evaluation.

---

# 5. Entitlement Object

## Purpose

Represents rights and qualifications associated with
an authenticated entity.

## Responsibilities

- Entitlement identifier
- Rights information
- Qualification information
- Validity period
- Service scope
- Usage conditions

## Version 2.0 Innovation Area

The Entitlement Object is a core architectural element
introduced in Version 2.0.

It enables authentication results to be transformed
into reusable digital rights.

---

# 6. Policy Object

## Purpose

Represents evaluation rules applied to entitlement.

## Responsibilities

- Policy identifier
- Evaluation conditions
- Required evidence
- Decision criteria

## Example

A policy may evaluate:

- Valid visit evidence
- Time restriction
- Service eligibility

---

# 7. Authorization Object

## Purpose

Represents the final authorization decision.

## Responsibilities

- Decision result
- Authorization scope
- Permission conditions
- Expiration information

## Result Types

- Permit
- Deny
- Conditional Permit

---

# 8. Service Request Object

## Purpose

Represents the requested service operation.

## Responsibilities

- Service identifier
- Requested operation
- Target resource
- Execution context

---

# 9. Scenario D Object Relationship

Example:

Store visit authentication creates evidence.

The evidence generates entitlement.

The entitlement is evaluated by policy.

The authorization object permits EC discount usage.

Processing model:

Authentication Object

        ↓

Visit Evidence

        ↓

Entitlement Object

        ↓

Policy Object

        ↓

Authorization Object

        ↓

EC Service Request

---

# 10. Patent Boundary Candidate

Figure03 represents the following innovation areas:

- Authentication result to entitlement transformation
- Reusable entitlement object
- Policy-driven authorization
- Cross-service rights utilization

---

# 11. Non-Goals

Figure03 does not define:

- Binary data encoding
- Network transport format
- Cryptographic implementation
- API specification

Detailed protocol messages are defined separately.

---

# 12. SVG Design Requirements

The SVG representation SHALL include:

- Object relationship flow
- Object responsibility labels
- Version 2.0 innovation boundary
- Scenario D example
- Clear protocol object hierarchy

---

# 13. Design Freeze Decision

Figure03 is approved as the transaction object
reference model for NEW-shot2play Protocol Suite
Version 2.0.

Future modifications require Design Freeze review.

