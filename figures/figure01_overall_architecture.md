# Figure01 Overall Architecture

## Document Information

| Item | Value |
|---|---|
| Figure ID | Figure01 |
| Title | Overall Architecture |
| Version | NEW-shot2play Protocol Suite Version 2.0 |
| Revision | Design Freeze Version 2.0 Revision 1.1 |
| Status | Design Definition |
| Purpose | Core architectural representation |

---

# 1. Purpose

Figure01 defines the highest-level architecture of the
NEW-shot2play Protocol Suite Version 2.0.

This figure establishes the fundamental separation between:

- Authentication
- Entitlement
- Policy Evaluation
- Authorization
- Service

Version 2.0 introduces a protocol architecture where
authentication establishes identity assurance, while
entitlement and policy processing determine service access.

---

# 2. Architectural Principle

The fundamental processing model is:

Authentication

        ↓

Entitlement

        ↓

Policy Evaluation

        ↓

Authorization

        ↓

Service


Authentication establishes identity assurance.

Entitlement defines rights, qualifications, and usage conditions.

Policy Evaluation determines whether defined conditions
are satisfied.

Authorization generates the final service permission decision.

---

# 3. Core Layer Model

Figure01 defines the following protocol layers.

Identity Layer

        ↓

Credential Layer

        ↓

Authentication Layer

        ↓

Entitlement Layer

        ↓

Policy Layer

        ↓

Authorization Layer

        ↓

Application Layer

        ↓

Service

---

# 4. Layer Definition

## 4.1 Identity Layer

### Purpose

The Identity Layer represents the entity identity
participating in the protocol.

### Primary Object

Identity Object

The Identity Layer provides the foundation of
the trust relationship.

---

## 4.2 Credential Layer

### Purpose

The Credential Layer represents credentials associated
with identity verification.

### Primary Object

Credential Object

The Credential Layer provides trusted information
used during authentication processing.

---

## 4.3 Authentication Layer

### Purpose

The Authentication Layer verifies identity authenticity.

### Primary Object

Authentication Object

The Authentication Layer is based on the
NEW-shot2play Protocol Suite Version 1.0
Baseline Specification.

Version 1.0 provides the authentication foundation
for Version 2.0.

---

## 4.4 Entitlement Layer

### Purpose

The Entitlement Layer defines rights, qualifications,
and service usage permissions.

### Primary Object

Entitlement Object

### Patent Boundary Candidate

Entitlement Object Architecture

The Entitlement Layer is the primary innovation area
introduced by Version 2.0.

---

## 4.5 Policy Layer

### Purpose

The Policy Layer evaluates conditions associated
with entitlement.

### Primary Objects

Policy Object

Policy Evaluation Engine

### Patent Boundary Candidate

Policy Evaluation Engine

The Policy Layer determines whether entitlement
conditions satisfy defined rules.

---

## 4.6 Authorization Layer

### Purpose

The Authorization Layer generates the final service
access decision.

### Primary Object

Authorization Object

### Patent Boundary Candidate

Conditional Authorization

The Authorization Layer converts policy evaluation
results into service permissions.

---

## 4.7 Application Layer

### Purpose

The Application Layer consumes authorization results
and provides application functionality.

---

# 5. Version Boundary

Figure01 defines the relationship between Version 1.0
and Version 2.0.

NEW-shot2play Version 1.0

Authentication Protocol Foundation


================================


NEW-shot2play Version 2.0

Entitlement-Based Authorization Framework


## Version 1.0 Scope

- Identity verification
- Credential processing
- Authentication Protocol


## Version 2.0 Scope

- Entitlement Object
- Policy Evaluation Engine
- Conditional Authorization
- Cross-Service Entitlement Reuse

---

# 6. Object Placement

| Object | Layer |
|---|---|
| Identity Object | Identity Layer |
| Credential Object | Credential Layer |
| Authentication Object | Authentication Layer |
| Entitlement Object | Entitlement Layer |
| Policy Object | Policy Layer |
| Authorization Object | Authorization Layer |
| Service Object | Application Layer |

---

# 7. SVG Design Requirements

The SVG representation SHALL satisfy:

- Vertical layered architecture
- Clear processing direction
- Explicit Version 1.0 and Version 2.0 boundary
- Patent innovation boundary visibility
- Object names displayed in each layer
- Technical review readability

---

# 8. Non-Goals

Figure01 does not define:

- Detailed protocol messages
- Network transport mechanisms
- Application business logic
- User interface behavior

These elements are defined in later specifications.

---

# 9. Related Documents

Related specifications:

- Chapter01 Framework Overview
- Chapter02 Protocol Architecture
- Chapter05 Entitlement Protocol
- Chapter06 Policy Evaluation
- Chapter07 Authorization Protocol

---

# 10. Design Freeze Decision

Figure01 is approved as the primary architecture diagram
for NEW-shot2play Protocol Suite Version 2.0.

Future modifications require Design Freeze review.

