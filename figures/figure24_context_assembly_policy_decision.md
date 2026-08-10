# Figure24 Context Assembly and Policy Decision Model

## Document Information

| Item | Value |
|---|---|
| Figure ID | Figure24 |
| Title | Context Assembly and Policy Decision Model |
| Version | NEW-shot2play Protocol Suite Version 2.0 |
| Revision | Design Definition Version 2.0 |
| Status | Design Definition |
| Purpose | Define how validated security context is assembled and evaluated to produce an authorization decision |

---

# 1. Purpose

Figure24 defines the logical processing model by which validated
security context is assembled into an authorization evaluation context
and processed by the Policy Evaluation and Authorization Domains.

The model establishes the following principle:

Validated Context

↓

Context Assembly

↓

Policy Evaluation

↓

Authorization Decision

↓

Enforcement

The model does not permit a policy engine to treat unvalidated input as
authoritative security context.

---

# 2. Core Principle

Security-sensitive policy evaluation SHALL operate on a defined and
validated evaluation context.

The evaluation context SHALL be assembled from authoritative or
validated protocol inputs.

Representative inputs include:

- Authentication Context
- Entitlement Context
- Resource Context
- Action Context
- Transaction Context
- Policy Context
- Environmental Context where applicable

Each input SHALL have a defined source and validation requirement.

---

# 3. Processing Stages

The model consists of five logical stages:

1. Context Intake
2. Context Validation
3. Context Assembly
4. Policy Evaluation
5. Authorization Decision

A subsequent Enforcement stage consumes the resulting authorization
decision.

---

# 4. Context Intake

Context Intake receives the protocol inputs associated with the current
request.

Representative inputs include:

- Subject Identifier
- Authentication Result
- Entitlement Identifier
- Entitlement State
- Transaction Identifier
- Resource Identifier
- Action Identifier
- Policy Identifier
- Policy Version

Context Intake SHALL associate received inputs with the current
transaction.

---

# 5. Context Validation

Each received context element SHALL be validated before it is included
in the evaluation context.

Representative validation includes:

- Identifier validation
- Authentication validation
- Entitlement validation
- Transaction binding
- Resource binding
- Action binding
- Expiration validation
- Version validation
- Integrity validation
- Consistency validation

Invalid context SHALL NOT be included in an allow-producing evaluation.

---

# 6. Context Assembly

Context Assembly constructs the normalized evaluation context.

The assembled context MAY include:

- Subject
- Authentication Status
- Authentication Assurance
- Entitlement State
- Entitlement Version
- Transaction
- Resource
- Action
- Policy
- Policy Version
- Evaluation Time
- Applicable Environmental Conditions

Context Assembly SHALL preserve the semantic meaning of authoritative
inputs.

---

# 7. Context Normalization

Context normalization SHALL produce a consistent representation for
policy evaluation.

Normalization MAY include:

- Identifier canonicalization
- Timestamp normalization
- State representation normalization
- Policy version resolution
- Attribute selection
- Default handling where explicitly defined

Normalization SHALL NOT introduce authority that was not present in
the authoritative input.

---

# 8. Missing Context

If a required context element is missing, the Policy Evaluation Domain
SHALL NOT infer an allow result unless an explicit policy defines such
behavior.

Representative missing context includes:

- Missing Authentication
- Missing Entitlement
- Missing Transaction
- Missing Resource
- Missing Action
- Missing Policy
- Missing Required Attribute

The default security behavior SHOULD be controlled rejection or deny.

---

# 9. Stale Context

Context may become stale between creation and evaluation.

Representative stale conditions include:

- Expired Authentication Context
- Expired Entitlement
- Superseded Entitlement Version
- Superseded Policy Version
- Superseded Authorization Context
- Invalid Transaction State

Stale context SHALL NOT be silently treated as current context.

---

# 10. Policy Selection

The Policy Evaluation Domain selects the policy applicable to the
current evaluation.

Policy selection MAY use:

- Resource
- Action
- Subject
- Entitlement
- Transaction
- Environment
- Policy Identifier
- Policy Version

The selected policy SHALL be identifiable and versioned.

---

# 11. Policy Version

The policy version SHALL be part of the evaluation context.

A policy decision SHALL be associated with the exact policy version used
for the evaluation.

This provides deterministic traceability between:

Policy

↓

Evaluation

↓

Authorization Decision

↓

Audit Record

---

# 12. Policy Evaluation

The Policy Evaluation Domain evaluates the assembled context against the
selected policy.

Representative evaluation results include:

- Allow
- Deny
- Conditional Allow
- Require Additional Authentication
- Require Additional Entitlement
- Require Reauthorization
- Invalid Context

The exact result set MAY be implementation-defined, provided that its
security semantics are explicitly defined.

---

# 13. Conditional Policy

A policy MAY produce a conditional result.

Representative conditions include:

- Time Condition
- Location Condition
- Entitlement Condition
- Transaction Condition
- Resource Condition
- Action Condition
- Prior Authentication Condition
- Prior Transaction Condition

A conditional result SHALL identify the conditions that must be
satisfied before enforcement.

---

# 14. Authorization Decision

The Authorization Domain converts the policy evaluation result into an
authorization decision.

The decision SHALL identify:

- Decision Identifier
- Transaction Identifier
- Subject Identifier
- Resource Identifier
- Action Identifier
- Policy Identifier
- Policy Version
- Entitlement Identifier where applicable
- Decision Result
- Decision Expiration
- Decision Version

The decision SHALL be bound to the context from which it was derived.

---

# 15. Decision Binding

The authorization decision SHALL be bound to the request context.

Representative binding relationships include:

Subject

↔ Authentication

Subject

↔ Entitlement

Entitlement

↔ Transaction

Transaction

↔ Resource

Resource

↔ Action

Policy

↔ Policy Version

Decision

↔ Evaluation Context

A failed binding SHALL invalidate the decision.

---

# 16. Decision Freshness

Authorization decisions MAY have a limited validity period.

Decision freshness SHALL be evaluated using:

- Decision Creation Time
- Decision Expiration Time
- Current Time
- Transaction State
- Relevant Entitlement State
- Relevant Policy Version

A stale decision SHALL NOT be enforced.

---

# 17. Decision Result

The authorization decision represents the final security determination
for the current authorization operation.

Representative results:

| Result | Meaning |
|---|---|
| ALLOW | Operation may proceed |
| DENY | Operation shall not proceed |
| CONDITIONAL | Additional conditions required |
| REAUTH | Authentication must be renewed |
| REENTITLE | Entitlement must be established or refreshed |
| REAUTHORIZE | A new authorization decision is required |
| INVALID | Context or decision is invalid |

---

# 18. Enforcement Handoff

The Authorization Domain SHALL provide the resulting decision to the
Enforcement Domain.

The Enforcement Domain SHALL validate:

- Decision Identifier
- Transaction Identifier
- Subject Identifier
- Resource Identifier
- Action Identifier
- Policy Version
- Decision Expiration
- Decision Integrity

Only a valid decision may authorize execution.

---

# 19. Deny Path

A failed validation or policy evaluation MAY produce a deny result.

Representative deny causes include:

- Invalid Authentication
- Missing Entitlement
- Expired Entitlement
- Policy Mismatch
- Resource Mismatch
- Action Mismatch
- Transaction Mismatch
- Stale Context
- Invalid Decision

Deny SHALL prevent the protected operation from being executed.

---

# 20. Conditional Path

A conditional result SHALL NOT be treated as an unconditional allow.

The Enforcement Domain SHALL identify the required condition and
confirm that the condition has been satisfied.

Representative sequence:

Policy Evaluation

↓

Conditional Result

↓

Condition Verification

↓

Authorization Decision

↓

Enforcement

---

# 21. Additional Authentication

A policy MAY require additional authentication.

For example:

Existing Authentication

↓

Policy Evaluation

↓

Additional Assurance Required

↓

Reauthentication

↓

Updated Authentication Context

↓

Reevaluation

The updated context SHALL be validated before reevaluation.

---

# 22. Additional Entitlement

A policy MAY require additional entitlement.

Representative sequence:

Current Entitlement

↓

Policy Evaluation

↓

Additional Entitlement Required

↓

Entitlement Establishment / Update

↓

Updated Entitlement Context

↓

Reevaluation

The entitlement update SHALL be authoritative.

---

# 23. Reauthorization

A policy MAY require a new authorization decision when the relevant
context changes.

Representative triggers include:

- Entitlement State Change
- Policy Version Change
- Resource Change
- Action Change
- Transaction State Change
- Decision Expiration

A previous authorization decision SHALL NOT automatically remain valid
after a material context change.

---

# 24. Evaluation Determinism

For a defined policy version and defined evaluation context, the
evaluation result SHOULD be deterministic.

The evaluation record SHOULD identify:

- Policy Identifier
- Policy Version
- Evaluation Inputs
- Evaluation Time
- Evaluation Result
- Decision Identifier

This enables reproducibility and audit analysis.

---

# 25. Context Minimization

The evaluation context SHOULD contain only information necessary for the
policy being evaluated.

Unnecessary data SHOULD NOT be propagated into the policy engine.

This reduces:

- Attack Surface
- Privacy Exposure
- Unintended Authority
- Evaluation Complexity

---

# 26. Authority Separation

Context Assembly SHALL NOT itself grant authorization.

Policy Evaluation SHALL determine applicable policy conditions.

Authorization SHALL create the enforceable decision.

Enforcement SHALL execute only after decision validation.

Thus:

Context

≠

Policy Decision

≠

Authorization

≠

Execution

---

# 27. Failure Handling

The system SHALL define controlled handling for:

- Context Validation Failure
- Policy Selection Failure
- Policy Evaluation Failure
- Decision Creation Failure
- Decision Validation Failure
- Enforcement Failure

A failure SHALL NOT silently degrade into an allow result.

---

# 28. Audit Trace

The processing lifecycle SHOULD produce correlated audit references.

Representative references include:

- Transaction Identifier
- Evaluation Identifier
- Policy Identifier
- Policy Version
- Decision Identifier
- Entitlement Identifier
- Authentication Identifier
- Event Identifier

These references SHOULD allow reconstruction of the decision path.

---

# 29. Security Properties

The Context Assembly and Policy Decision Model provides:

- Validated evaluation inputs
- Explicit context authority
- Context normalization
- Policy version traceability
- Deterministic evaluation
- Explicit conditional handling
- Authorization decision binding
- Decision freshness
- Authority separation
- Controlled deny behavior
- Reauthentication support
- Reentitlement support
- Reauthorization support
- Audit traceability
- Context minimization

---

# 30. Non-Goals

Figure24 does not define:

- A specific policy language
- A specific policy engine
- A specific cryptographic algorithm
- A specific database
- A specific cloud provider
- A specific programming language
- A specific user interface

Implementation details MAY be defined elsewhere.

---

# 31. Design Freeze Decision

Figure24 is designated as the context assembly and policy decision
reference model for the NEW-shot2play Protocol Suite Version 2.0.

Future modifications require Design Freeze review.

