# NEW-shot2play Technical Specification Version 2.0
# Normative Requirements

## 1. Document Status

This document defines the normative requirements of the NEW-shot2play
Technical Specification Version 2.0.

This document specifies the protocol model, protocol objects, processing
requirements, state requirements, security requirements, and service
execution requirements applicable to a conforming implementation.

Version 2.0 defines a layered processing model in which Authentication,
Entitlement, Policy Evaluation, Authorization Evaluation, Authorization
Decision, Enforcement, and Service Execution are distinct protocol
functions.

A conforming implementation SHALL preserve the semantic distinction
between these functions.

A conforming implementation SHALL implement the requirements expressed
using the normative terms defined in Section 2.

The requirements of this document apply to protocol implementations,
protocol components, protocol objects, and processing operations that
claim conformance to Version 2.0.

This document does not require a particular programming language,
database, operating system, network topology, deployment platform, or
implementation architecture unless such requirement is expressly
specified.

An implementation MAY use additional internal mechanisms provided that
such mechanisms do not violate the normative requirements of this
document.

An implementation MAY combine multiple internal components provided that
the externally observable protocol semantics and required processing
distinctions are preserved.

A conforming implementation SHALL NOT claim successful processing of a
protocol operation solely because an underlying transport, interface,
message exchange, or internal operation completed successfully.

A conforming implementation SHALL determine the result of each required
processing stage according to the applicable protocol requirements and
Policy.

---

## 2. Normative Interpretation

The key words "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", and "MAY" in
this document are to be interpreted as normative requirements.

"SHALL" identifies a mandatory requirement.

"SHALL NOT" identifies a mandatory prohibition.

"SHOULD" identifies a recommended requirement that may be omitted only
when a documented implementation reason exists.

"SHOULD NOT" identifies a recommended prohibition that may be violated
only when a documented implementation reason exists.

"MAY" identifies an optional behavior.

Where this document states that a condition "cannot be established",
the implementation SHALL treat the condition as not established for the
purpose of dependent processing unless the applicable Policy explicitly
defines another processing state.

Where multiple normative requirements apply to the same processing
operation, all applicable requirements SHALL be satisfied.

A later processing stage SHALL NOT be used to silently replace a
mandatory requirement of an earlier processing stage.

An implementation SHALL preserve the semantic meaning of protocol
objects even where multiple objects are represented internally by a
single implementation structure.

An implementation SHALL preserve the distinction between a protocol
object and information describing that object.

An implementation SHALL preserve the distinction between current state
and historical information.

An implementation SHALL preserve the distinction between an evaluation
result and a subsequent Authorization Decision.

An implementation SHALL preserve the distinction between an
Authorization Decision and the Enforcement operation based upon that
Decision.

An implementation SHALL preserve the distinction between Enforcement
and Service Execution.

---

## 3. Fundamental Processing Model

Version 2.0 SHALL use the following logical processing model:

Authentication
        ↓
Entitlement
        ↓
Policy Evaluation
        ↓
Authorization Evaluation
        ↓
Authorization Decision
        ↓
Enforcement
        ↓
Service Execution
        ↓
Execution Result

The processing model defines logical responsibilities and semantic
relationships. It does not require each stage to be implemented as a
separate physical service or software component.

Authentication SHALL establish the applicable authenticated identity,
credential state, authentication result, and required Security Context
before dependent processing proceeds.

Authentication success SHALL NOT by itself constitute Entitlement.

Authentication success SHALL NOT by itself constitute Authorization.

Entitlement SHALL identify the applicable right, qualification,
condition, scope, or other authorization-relevant relationship of the
authenticated subject.

Entitlement SHALL NOT by itself constitute Authorization.

Policy Evaluation SHALL determine the applicable Policy requirements
and their effect on dependent processing.

Policy Evaluation SHALL NOT by itself constitute an Authorization
Decision unless the applicable protocol explicitly defines a separate
Decision creation step and all required conditions for that Decision
are satisfied.

Authorization Evaluation SHALL evaluate whether the required conditions
for the requested operation are satisfied.

Authorization Evaluation SHALL produce an Evaluation Result.

An Evaluation Result SHALL remain distinguishable from an Authorization
Decision.

An Authorization Decision SHALL establish the result that governs
whether and under what conditions Enforcement may proceed.

Enforcement SHALL apply the applicable Authorization Decision and
required conditions before Service Execution.

Service Execution SHALL NOT occur unless all mandatory preconditions
specified by the applicable Policy, Authorization Decision, Enforcement
requirements, and Service Profile have been satisfied.

Service Execution SHALL produce or establish an applicable Execution
Result.

An Execution Result SHALL remain distinguishable from the Authorization
Decision that permitted the execution.

Successful completion of Service Execution SHALL NOT retroactively
establish Authorization.

A failed Service Execution SHALL NOT retroactively invalidate the
Authorization Decision unless the applicable protocol or Policy
requires such invalidation.

An implementation SHALL NOT skip a required processing stage merely
because a preceding or subsequent stage appears to imply the same
result.

An implementation MAY optimize processing where the optimization
preserves the semantic requirements of all applicable stages.

Where a required condition cannot be established, the applicable
Policy SHALL determine the resulting processing state.

Where fail-closed behavior applies, inability to establish a required
condition SHALL NOT result in unauthorized Service Execution.

---

## 4. Transaction Requirements

A Transaction SHALL represent a logically correlated unit of protocol
processing.

A Transaction SHALL have a Transaction Identifier when Transaction
correlation is required by the applicable protocol.

A Transaction Identifier SHALL uniquely identify the Transaction within
the applicable identifier namespace and validity scope.

A Transaction MAY contain or reference:

- Authentication processing
- Entitlement processing
- Policy Evaluation
- Authorization Evaluation
- Authorization Decision
- Enforcement
- Service Execution
- Execution Result
- Security Context
- Evidence
- Audit
- Related protocol objects
- Processing state
- Dependency information

Correlation of protocol objects with a Transaction SHALL NOT eliminate
the semantic identity of those objects.

A Transaction SHALL have a defined processing state.

A Transaction state SHALL represent the current state of the
Transaction and SHALL NOT be interpreted as an implicit statement that
all correlated objects remain valid.

A Transaction MAY transition between states according to the applicable
protocol rules.

A Transaction state transition SHALL be valid according to the current
Transaction state and applicable dependencies.

A Transaction SHALL NOT be considered successfully completed merely
because one or more operations associated with the Transaction have
completed.

A Transaction completion state SHALL be established according to the
applicable completion requirements.

A Transaction MAY be terminated before normal completion.

A terminated Transaction SHALL NOT automatically authorize subsequent
processing.

An expired Transaction SHALL NOT be treated as an active Transaction.

A Transaction MAY be associated with a Session.

Session continuity SHALL NOT by itself establish continued validity of
Authentication, Entitlement, Policy Evaluation, Authorization Decision,
Enforcement, or Service Execution.

A change to a Transaction state MAY require revalidation of dependent
processing.

Where required Transaction state or dependency information cannot be
established, dependent processing SHALL follow the applicable Policy.

---

## 5. Identity and Credential Requirements

An implementation SHALL maintain the semantic distinction between an
Identity and the Credential or authentication mechanism used to establish
that Identity.

An Identity SHALL represent the subject applicable to protocol
processing.

A Credential SHALL represent information, an authenticator, or a
cryptographic mechanism used to establish or validate an Identity.

Credential possession SHALL NOT by itself establish the current
validity of the associated Identity.

Credential state SHALL be evaluated where required by the applicable
Authentication requirements.

A Credential MAY have:

- An identifier
- A version
- An activation state
- An expiration state
- A revocation state
- A suspension state
- A validity period
- A Security Context
- Associated cryptographic key material

An implementation SHALL NOT treat an expired, revoked, suspended, or
otherwise invalid Credential as valid where current Credential validity
is required.

Where a Credential state change affects Authentication validity,
dependent processing SHALL apply the applicable revalidation or
termination requirements.

An implementation SHALL protect Credential-related security information
according to the applicable Security Requirements.

Private cryptographic key material SHALL NOT be exposed through a
protocol object where such exposure would violate the applicable
security requirements.

An implementation MAY use hardware-backed or isolated key protection
mechanisms.

The use of a particular key protection mechanism SHALL NOT remove the
requirement to establish the applicable Authentication result.

---

## 6. Authentication Requirements

Authentication SHALL establish whether the claimed Identity has been
successfully authenticated according to the applicable authentication
mechanism.

An Authentication operation SHALL produce an Authentication Result when
the protocol requires an explicit result object.

An Authentication Result SHALL identify or otherwise establish the
applicable authentication state.

Authentication processing MAY include:

- Identity identification
- Credential validation
- Challenge validation
- Proof verification
- Freshness validation
- Replay detection
- Security Context establishment
- Authentication assurance determination
- Credential state validation

An Authentication Result SHALL NOT be interpreted as permanent validity.

Authentication validity SHALL be limited by the applicable scope,
lifetime, freshness, Credential state, Security Context, and Policy.

Authentication SHALL NOT be treated as Authorization.

Authentication SHALL NOT be treated as Entitlement.

Authentication success SHALL NOT bypass required Entitlement evaluation.

Authentication success SHALL NOT bypass required Policy Evaluation.

Authentication success SHALL NOT bypass required Authorization
Evaluation.

Authentication success SHALL NOT bypass required Enforcement.

Where re-authentication is required, dependent processing SHALL NOT
continue as though the previous Authentication Result remained
unconditionally valid.

A re-authentication operation SHALL produce a new Authentication Result
or another protocol-defined result establishing the outcome.

A failed or incomplete re-authentication SHALL NOT be treated as
successful Authentication.

A Security Context change MAY require Authentication revalidation.

A Credential state change MAY require Authentication revalidation.

Where Authentication validity, freshness, assurance, or Security Context
requirements cannot be established, the applicable Policy SHALL
determine the resulting processing state.

Where fail-closed behavior applies, inability to establish required
Authentication validity SHALL NOT result in unauthorized Service
Execution.

---

## 7. Entitlement Requirements

An Entitlement SHALL represent an applicable right, qualification,
condition, relationship, or other authorization-relevant property of a
subject.

An Entitlement SHALL have a defined scope.

An Entitlement MAY include:

- Entitlement Identifier
- Subject Identifier
- Scope
- Conditions
- Activation state
- Expiration
- Revocation state
- Suspension state
- Version
- Consumption state
- Dependency information

An implementation SHALL validate Entitlement validity before using an
Entitlement where current validity is required.

An expired Entitlement SHALL NOT be treated as valid.

A revoked Entitlement SHALL NOT be treated as valid.

A suspended Entitlement SHALL NOT be treated as active.

An inactive Entitlement SHALL NOT be treated as active.

An Entitlement SHALL NOT automatically constitute Authorization.

An Entitlement SHALL NOT bypass required Policy Evaluation.

An Entitlement SHALL NOT bypass required Authorization Evaluation.

An Entitlement SHALL NOT bypass required Enforcement.

Where an Entitlement contains conditions, the applicable conditions
SHALL be evaluated before dependent processing proceeds.

Where an Entitlement contains dependencies, those dependencies SHALL be
validated according to the applicable protocol requirements.

An Entitlement MAY be single-use.

Where an Entitlement is defined as single-use, the implementation SHALL
prevent reuse after successful consumption.

Consumption of a single-use Entitlement SHALL be processed according to
the applicable atomicity and concurrency requirements.

An Entitlement MAY be renewable.

Renewal SHALL NOT automatically establish continued Authorization.

A change to Entitlement state MAY require revalidation of dependent
Policy Evaluation, Authorization Evaluation, Authorization Decisions,
Enforcement, or Service Execution.

Where required Entitlement validity, scope, condition, dependency, or
consumption state cannot be established, the applicable Policy SHALL
determine the resulting processing state.

Where fail-closed behavior applies, inability to establish required
Entitlement validity SHALL NOT result in unauthorized Service
Execution.

---

## 8. Policy Requirements

A Policy SHALL define or establish the rules applicable to protocol
processing.

A Policy SHALL have a Policy Identifier when Policy identification is
required.

A Policy MAY have:

- Policy Version
- Activation state
- Effective period
- Scope
- Applicability conditions
- Precedence
- Dependencies
- Conflict rules
- Administrative authority
- Security requirements

A Policy SHALL be evaluated for applicability where multiple Policies
may apply.

Policy selection SHALL remain distinguishable from Policy Evaluation.

Policy Evaluation SHALL determine the effect of the applicable Policy
on the requested processing.

Policy Evaluation SHALL NOT automatically constitute Authorization.

A Policy SHALL NOT be treated as current where its validity period,
activation state, Version, or other required state makes it invalid.

An implementation SHALL NOT silently use a historical Policy as the
current Policy where current Policy state is required.

Where multiple Policies apply, the implementation SHALL apply the
applicable precedence, combination, inheritance, or conflict rules.

Where Policy conflict rules are defined, the implementation SHALL apply
those rules before dependent processing proceeds.

A Policy change MAY require revalidation of:

- Entitlement
- Policy Evaluation
- Authorization Evaluation
- Authorization Decision
- Enforcement
- Service Execution
- Transaction state

Policy changes SHALL NOT silently alter the security meaning of an
already established object where such alteration is prohibited by the
applicable Policy or Version requirements.

Policy Evaluation MAY produce:

- Permit
- Deny
- Indeterminate
- Not Applicable
- Revalidation Required
- Another Policy-defined result

A Policy Evaluation Result SHALL remain distinguishable from an
Authorization Decision.

Where Policy applicability, state, Version, dependency, or Evaluation
validity cannot be established, the applicable Policy SHALL determine
the resulting processing state.

Where fail-closed behavior applies, inability to establish required
Policy validity SHALL NOT result in unauthorized Service Execution.


# 9. Authorization Evaluation Requirements

Authorization Evaluation is the protocol processing stage in which the
applicable authorization requirements are evaluated using the applicable
Authentication, Entitlement, Policy, Security Context, Transaction, and
Service Profile information.

Authorization Evaluation SHALL be performed before an Authorization
Decision is established where the applicable Service Profile or Policy
requires Authorization Evaluation.

Authorization Evaluation SHALL remain semantically distinct from:

- Authentication
- Entitlement
- Policy Evaluation
- Authorization Decision
- Enforcement
- Service Execution

Authorization Evaluation SHALL NOT itself constitute Service Execution.

## 9.1 Authorization Evaluation

An Authorization Evaluation SHALL determine whether the currently
applicable authorization requirements can be satisfied.

The evaluation SHALL use the current protocol state required by the
applicable Policy.

An implementation SHALL NOT establish an Authorization Decision from
incomplete evaluation input where the applicable Policy requires the
missing input.

## 9.2 Required Evaluation Inputs

Where applicable, Authorization Evaluation SHALL consider:

- Authentication state
- Authentication assurance
- Entitlement state
- Entitlement scope
- Entitlement conditions
- Applicable Policy
- Policy version
- Security Context
- Transaction state
- Service Profile
- Required capabilities
- Required dependencies
- Required freshness
- Required validity periods

The applicable Policy MAY define additional evaluation inputs.

## 9.3 Evaluation Context

Authorization Evaluation SHALL be associated with the processing context
under which the evaluation is performed.

The evaluation context SHALL identify, directly or indirectly, the
protocol state relevant to the evaluation.

An implementation SHALL NOT use unrelated or stale protocol state as
current authorization input where current state is required.

## 9.4 Evaluation Result

Authorization Evaluation MAY produce:

- Permit
- Deny
- Indeterminate
- Revalidation Required
- Not Applicable

The applicable Policy MAY define additional evaluation results.

An Evaluation Result SHALL remain distinguishable from an Authorization
Decision.

A Permit Evaluation Result SHALL NOT by itself constitute Service
Execution.

## 9.5 Permit Evaluation

A Permit result MAY be produced where all required authorization
conditions have been satisfied.

A Permit result SHALL be based on the applicable Policy and current
evaluation inputs.

A Permit result SHALL NOT authorize processing outside the applicable
scope.

## 9.6 Deny Evaluation

A Deny result SHALL indicate that the applicable authorization
requirements are not satisfied.

A Deny result SHALL NOT be interpreted as an Authorization Decision unless
the applicable protocol establishes the corresponding Decision.

## 9.7 Indeterminate Evaluation

An Indeterminate result MAY be produced where the applicable authorization
requirements cannot be conclusively evaluated.

Indeterminate processing MAY result from:

- Missing required input
- Invalid input
- Unavailable dependency
- Conflicting state
- Invalid Security Context
- Expired state
- Revoked state
- Integrity failure
- Policy uncertainty
- Processing failure

Where fail-closed behavior applies, Indeterminate processing SHALL NOT
result in unauthorized Service Execution.

## 9.8 Revalidation Required

Authorization Evaluation MAY require revalidation where the current
validity of required authorization input cannot be established.

Revalidation MAY be required because of:

- State change
- Policy change
- Entitlement change
- Authentication change
- Security Context change
- Dependency change
- Version change
- Expiration
- Revocation
- Suspension
- Required freshness
- Integrity failure

Where revalidation is required, dependent processing SHALL follow the
applicable Policy.

## 9.9 Evaluation Scope

Authorization Evaluation SHALL be performed within a defined scope.

The scope MAY include:

- Subject
- Entitlement
- Resource
- Action
- Service
- Transaction
- Service Profile
- Time
- Location
- Security Context
- Other Policy-defined conditions

An Evaluation Result SHALL NOT be interpreted outside its applicable
scope.

## 9.10 Evaluation Freshness

Where freshness is required, Authorization Evaluation SHALL use
information satisfying the applicable freshness requirements.

Historical information MAY be retained for Evidence or Audit.

Historical information SHALL NOT automatically be treated as current
authorization state.

## 9.11 Evaluation Dependencies

Authorization Evaluation MAY depend on other protocol objects or state.

Where a required dependency is invalid, unavailable, revoked, expired, or
otherwise unacceptable, the applicable Policy SHALL determine the
resulting processing state.

An implementation SHALL NOT silently ignore a required dependency.

## 9.12 Evaluation Consistency

Distributed or component-based implementations SHALL preserve the semantic
meaning of an Authorization Evaluation across processing components.

An implementation SHALL NOT transform an Evaluation Result into a
different semantic result solely because the result was transmitted
between components.

## 9.13 Evaluation Identifier

An Authorization Evaluation MAY have a unique identifier.

Where an identifier is provided, it SHOULD remain stable for the
identified evaluation instance.

An Evaluation Identifier SHALL NOT be reused for an unrelated evaluation.

## 9.14 Evaluation Correlation

Authorization Evaluation MAY be correlated with:

- Transaction
- Authentication
- Entitlement
- Policy Evaluation
- Authorization Decision
- Enforcement
- Service Execution
- Evidence
- Audit

Correlation SHALL NOT eliminate the semantic distinction between the
correlated objects.

## 9.15 Evaluation Completion

Authorization Evaluation SHALL be considered complete only when the
applicable evaluation processing has reached a defined result state.

Completion of Authorization Evaluation SHALL NOT by itself constitute
Enforcement or Service Execution.

## 9.16 Evaluation Failure

Where Authorization Evaluation fails, the applicable Policy SHALL
determine the resulting processing state.

Evaluation failure MAY result in:

- Deny
- Indeterminate
- Revalidation Required
- Transaction termination
- Service Execution prevention
- Recovery processing

Where fail-closed behavior applies, evaluation failure SHALL NOT result
in unauthorized Service Execution.

## 9.17 Evaluation Evidence

A protocol implementation MAY generate Evidence describing an
Authorization Evaluation.

Such Evidence MAY include:

- Evaluation Identifier
- Transaction Identifier
- Subject reference
- Entitlement reference
- Policy Identifier
- Policy Version
- Service Profile
- Evaluation Result
- Evaluation timestamp
- Security Context
- Dependency state
- Processing component

Evaluation Evidence SHALL remain distinguishable from the Evaluation
itself and from any resulting Authorization Decision.

## 9.18 Evaluation Audit

A protocol implementation MAY generate Audit information describing
Authorization Evaluation processing.

Audit information MAY include:

- Evaluation creation
- Evaluation completion
- Evaluation result
- Evaluation failure
- Revalidation
- Dependency processing
- Security events
- Processing timestamp

Audit information SHALL remain distinguishable from Authorization
Evaluation and from the Authorization Decision.

## 9.19 Evaluation Conformance

A conforming implementation SHALL apply the requirements defined by the
applicable Policy and Service Profile when performing Authorization
Evaluation.

An implementation SHALL NOT:

- Bypass required Authorization Evaluation.
- Use unrelated evaluation input.
- Treat stale state as current where current state is required.
- Treat Evidence as Authorization.
- Treat Audit as Authorization.
- Treat an Evaluation Result as Service Execution.
- Ignore required dependencies.
- Ignore required freshness.
- Ignore required Security Context validation.

Where a required Authorization Evaluation condition cannot be established,
the applicable Policy SHALL determine the resulting processing state.

Where fail-closed behavior applies, inability to establish a required
Authorization Evaluation condition SHALL NOT result in unauthorized
Service Execution.

# 10. Authorization Decision Requirements

An Authorization Decision is the protocol object or result that
establishes the authorization outcome applicable to subsequent
Enforcement.

An Authorization Decision SHALL be established only according to the
applicable Policy and the applicable Authorization Evaluation
requirements.

An Authorization Decision SHALL remain semantically distinct from:

- Authentication
- Entitlement
- Policy Evaluation
- Authorization Evaluation
- Enforcement
- Service Execution
- Execution Result
- Evidence
- Audit

## 10.1 Decision Creation

An Authorization Decision MAY be created following Authorization
Evaluation.

Where the applicable Policy requires a completed Authorization
Evaluation, a Decision SHALL NOT be created without that evaluation.

## 10.2 Decision States

An Authorization Decision MAY have the following states:

- Permit
- Deny
- Indeterminate
- Revalidation Required
- Suspended
- Revoked
- Expired
- Invalid

The applicable Policy MAY define additional states.

## 10.3 Permit Decision

A Permit Decision SHALL indicate that the applicable authorization
requirements have been satisfied for the defined scope and conditions.

A Permit Decision SHALL NOT constitute Enforcement.

A Permit Decision SHALL NOT constitute Service Execution.

## 10.4 Deny Decision

A Deny Decision SHALL indicate that the applicable authorization
requirements do not permit the requested operation.

A Deny Decision SHALL NOT be interpreted as evidence that Service
Execution occurred.

## 10.5 Indeterminate Decision

An Indeterminate Decision MAY be established where the applicable
authorization outcome cannot be conclusively determined.

Where fail-closed behavior applies, an Indeterminate Decision SHALL NOT
authorize Service Execution.

## 10.6 Revalidation Required Decision

A Decision MAY indicate that revalidation is required before continued
dependent processing.

A Revalidation Required Decision SHALL NOT be treated as a Permit
Decision.

## 10.7 Decision Scope

An Authorization Decision SHALL have a defined scope.

The scope MAY include:

- Subject
- Resource
- Action
- Service
- Transaction
- Service Profile
- Time
- Location
- Entitlement
- Security Context

A Decision SHALL NOT be applied outside its defined scope.

## 10.8 Decision Conditions

A Decision MAY contain conditions that SHALL be satisfied before or
during Enforcement.

Conditions MAY include:

- Time conditions
- Location conditions
- Transaction conditions
- Entitlement conditions
- Security Context conditions
- Dependency conditions
- Service conditions

Where a mandatory condition is no longer satisfied, dependent
Enforcement SHALL follow the applicable Policy.

## 10.9 Decision Obligations

A Decision MAY establish obligations applicable to Enforcement.

An obligation SHALL remain distinguishable from the Decision itself.

Failure to satisfy a mandatory obligation SHALL be handled according to
the applicable Policy.

## 10.10 Decision Validity

An Authorization Decision SHALL be considered valid only while all
mandatory validity requirements remain satisfied.

Validity MAY depend on:

- Decision state
- Scope
- Conditions
- Obligations
- Version
- Policy state
- Entitlement state
- Authentication state
- Security Context
- Dependency state
- Freshness
- Revocation state
- Suspension state
- Expiration state

## 10.11 Decision Freshness

Where current authorization state is required, a historical Decision SHALL
NOT automatically be treated as current.

The applicable Policy SHALL define any required freshness period.

## 10.12 Decision Revalidation

A Decision MAY require revalidation.

Revalidation MAY be triggered by:

- Policy change
- Entitlement change
- Authentication change
- Security Context change
- Dependency change
- Version change
- Scope change
- Expiration
- Revocation
- Suspension
- Required freshness

Where revalidation is required, Enforcement SHALL follow the applicable
Policy before permitting continued Service Execution.

## 10.13 Decision Invalidation

A Decision SHALL become invalid where a mandatory validity condition is
no longer satisfied and the applicable Policy requires invalidation.

Invalidation MAY be caused by:

- Revocation
- Expiration
- Suspension
- Policy change
- Entitlement change
- Authentication invalidation
- Security Context invalidation
- Dependency invalidation
- Integrity failure

An invalid Decision SHALL NOT authorize dependent Service Execution.

## 10.14 Decision Revocation

An Authorization Decision MAY be revoked by an authorized protocol or
administrative operation.

Revocation SHALL apply according to the applicable Policy.

A revoked Decision SHALL NOT be treated as a valid Permit Decision.

## 10.15 Decision Suspension

A Decision MAY be suspended.

A suspended Decision SHALL NOT be used for dependent processing where the
applicable Policy requires an active Decision.

Suspension MAY be temporary.

Resumption SHALL require satisfaction of the applicable revalidation and
Policy requirements.

## 10.16 Decision Version

An Authorization Decision MAY contain a Version.

Where Version is required, dependent processing SHALL validate the
Decision Version according to the applicable Policy.

A Version change SHALL NOT silently alter the security meaning of an
existing Decision.

## 10.17 Decision Binding

A Decision MAY be bound to:

- Subject
- Resource
- Action
- Entitlement
- Policy
- Transaction
- Service Profile
- Security Context
- Version

A bound Decision SHALL NOT be reused outside its defined binding.

## 10.18 Decision Dependencies

A Decision MAY depend on other protocol objects.

Where a mandatory dependency becomes invalid, dependent Enforcement SHALL
follow the applicable Policy.

Decision dependency SHALL remain distinguishable from the Decision itself.

## 10.19 Decision Reuse

A Decision MAY be reused only where the applicable Policy permits reuse.

Reuse SHALL preserve:

- Scope
- Conditions
- Obligations
- Validity
- Freshness
- Version
- Dependencies
- Security Context

A Decision SHALL NOT be reused merely because it previously produced a
Permit result.

## 10.20 Decision and Enforcement

Enforcement SHALL use an applicable Authorization Decision where the
Service Profile or Policy requires one.

Enforcement SHALL validate the Decision before applying it where required
by the applicable Policy.

A Decision SHALL NOT be treated as evidence that Enforcement occurred.

## 10.21 Decision and Service Execution

Service Execution SHALL NOT occur solely because an Authorization
Decision exists.

Required Enforcement SHALL be completed before Service Execution where
the applicable Service Profile or Policy requires Enforcement.

## 10.22 Decision Evidence

A protocol implementation MAY generate Evidence describing an
Authorization Decision.

Such Evidence MAY include:

- Decision Identifier
- Decision state
- Decision scope
- Decision conditions
- Decision obligations
- Policy Identifier
- Policy Version
- Transaction Identifier
- Security Context
- Decision timestamp
- Validity state
- Processing result

Decision Evidence SHALL remain distinguishable from the Decision itself.

## 10.23 Decision Audit

Audit information MAY record:

- Decision creation
- Decision update
- Decision validation
- Decision invalidation
- Decision revocation
- Decision suspension
- Decision revalidation
- Decision use
- Decision failure

Audit information SHALL remain distinguishable from the Authorization
Decision.

## 10.24 Decision Conformance

A conforming implementation SHALL preserve the semantic distinction
between Authorization Evaluation, Authorization Decision, Enforcement,
and Service Execution.

An implementation SHALL NOT:

- Treat Entitlement possession as Authorization.
- Treat Policy Evaluation as automatic Authorization.
- Treat an Evaluation Result as a permanent Decision.
- Treat an expired Decision as valid.
- Treat a revoked Decision as valid.
- Treat a suspended Decision as active.
- Ignore Decision scope.
- Ignore mandatory Decision conditions.
- Ignore mandatory Decision obligations.
- Ignore required Decision dependencies.
- Reuse a Decision outside its defined scope.
- Bypass required Enforcement.
- Treat a Permit Decision as successful Service Execution.

Where required Decision validity cannot be established, the applicable
Policy SHALL determine the resulting processing state.

Where fail-closed behavior applies, inability to establish required
Authorization validity SHALL NOT result in unauthorized Service Execution.

# 11. Enforcement Requirements

Enforcement is the protocol processing stage in which an applicable
Authorization Decision is applied to the requested operation.

Enforcement SHALL remain semantically distinct from:

- Authorization Evaluation
- Authorization Decision
- Service Execution
- Execution Result

## 11.1 Enforcement Function

Enforcement SHALL apply the applicable Authorization Decision according to
its scope, conditions, obligations, and Policy requirements.

## 11.2 Enforcement Input

Enforcement MAY require:

- Authorization Decision
- Transaction
- Service Profile
- Security Context
- Entitlement
- Policy
- Dependency state
- Current object state

Required inputs SHALL be validated according to the applicable Policy.

## 11.3 Enforcement Validation

Before permitting Service Execution, Enforcement SHALL validate the
applicable Authorization Decision where required.

Validation MAY include:

- Decision state
- Decision scope
- Decision validity
- Decision freshness
- Decision version
- Decision conditions
- Decision obligations
- Dependency state
- Security Context
- Transaction state

## 11.4 Permit Enforcement

Permit Enforcement MAY establish that the requested operation is
permitted to proceed to Service Execution.

Permit Enforcement SHALL NOT itself constitute Service Execution.

## 11.5 Deny Enforcement

Deny Enforcement SHALL prevent Service Execution where the applicable
Policy requires denial.

A Deny Enforcement result SHALL remain distinguishable from Service
Execution.

## 11.6 Indeterminate Enforcement

Indeterminate Enforcement MAY occur where required Enforcement information
cannot be established.

Where fail-closed behavior applies, Indeterminate Enforcement SHALL NOT
permit unauthorized Service Execution.

## 11.7 Enforcement Revalidation

Enforcement MAY require revalidation before permitting or continuing
Service Execution.

Revalidation MAY be required because of:

- Decision change
- Policy change
- Entitlement change
- Authentication change
- Security Context change
- Dependency change
- Transaction state change
- Expiration
- Revocation
- Suspension

## 11.8 Enforcement Scope

Enforcement SHALL apply only within the scope of the applicable Decision.

An implementation SHALL NOT extend Enforcement beyond that scope without
establishing the additional authorization requirements.

## 11.9 Enforcement Conditions

Mandatory Decision conditions SHALL be satisfied before dependent Service
Execution.

Where a condition becomes unsatisfied during processing, the applicable
Policy SHALL determine whether Enforcement is suspended, terminated,
revalidated, or otherwise changed.

## 11.10 Enforcement Obligations

Mandatory Decision obligations SHALL be applied according to the
applicable Policy.

Failure to satisfy a mandatory obligation SHALL NOT be silently ignored.

## 11.11 Enforcement Restrictions

Enforcement MAY restrict:

- Resource
- Action
- Service
- Time
- Location
- Transaction
- Data
- Processing capability

Restrictions SHALL remain consistent with the applicable Decision.

## 11.12 Enforcement State

An Enforcement operation MAY have a state including:

- Pending
- Permitted
- Denied
- Indeterminate
- Suspended
- Terminated
- Completed
- Failed

The applicable Policy MAY define additional states.

## 11.13 Enforcement Transition

Enforcement state transitions SHALL follow the applicable protocol and
Policy requirements.

An implementation SHALL NOT silently transition from Denied or
Indeterminate to Permitted without satisfying the required conditions.

## 11.14 Enforcement Suspension

Enforcement MAY be suspended where required conditions temporarily cannot
be established.

Resumption SHALL require satisfaction of the applicable Policy.

## 11.15 Enforcement Termination

Enforcement MAY be terminated because of:

- Decision revocation
- Decision expiration
- Policy change
- Entitlement invalidation
- Authentication invalidation
- Security Context invalidation
- Dependency failure
- Transaction termination
- Administrative action
- Security failure

A terminated Enforcement operation SHALL NOT authorize continued Service
Execution.

## 11.16 Enforcement Failure

Where Enforcement fails, the applicable Policy SHALL determine the
resulting processing state.

Enforcement failure MAY result in:

- Deny
- Indeterminate
- Revalidation Required
- Suspension
- Termination
- Recovery processing
- Service Execution prevention

## 11.17 Enforcement Error

An Enforcement Error SHALL remain distinguishable from:

- Authorization Decision
- Enforcement state
- Service Execution
- Execution Result

An Error SHALL NOT silently create a Permit Enforcement state.

## 11.18 Enforcement Recovery

Recovery MAY be performed after an Enforcement failure.

Recovery SHALL apply the applicable Policy.

Recovery SHALL NOT bypass required Authorization Evaluation or Decision
validation.

## 11.19 Enforcement Retry

An Enforcement operation MAY be retried where the applicable Policy
permits retry.

Retry processing SHALL NOT create unauthorized duplicate Service
Execution.

## 11.20 Enforcement Idempotency

Where an Enforcement operation is defined as idempotent, repeated
processing SHALL preserve the semantic result of the operation.

An implementation SHALL define or apply the applicable idempotency
requirements for retryable Enforcement.

## 11.21 Enforcement Concurrency

Concurrent Enforcement operations SHALL preserve the applicable
authorization and state requirements.

An implementation SHALL prevent race conditions from producing an
unauthorized Service Execution.

## 11.22 Enforcement Atomicity

Where atomic Enforcement is required, the implementation SHALL preserve
the required atomic relationship between authorization validation and
Enforcement state.

Partial processing SHALL follow the applicable Policy.

## 11.23 Enforcement Evidence and Audit

A protocol implementation MAY generate Evidence and Audit information
describing Enforcement.

Such information MAY include:

- Decision Identifier
- Enforcement Identifier
- Transaction Identifier
- Enforcement state
- Validation result
- Processing timestamp
- Security Context
- Result
- Failure reason

Evidence and Audit SHALL remain distinguishable from Enforcement and from
the Authorization Decision.

## 11.24 Enforcement Conformance

A conforming implementation SHALL apply Enforcement according to the
applicable Authorization Decision and Policy.

An implementation SHALL NOT:

- Treat an Authorization Decision as completed Enforcement.
- Bypass required Decision validation.
- Ignore mandatory conditions.
- Ignore mandatory obligations.
- Continue Enforcement after required invalidation.
- Use stale authorization state where current state is required.
- Permit Service Execution after required Enforcement failure.
- Treat Evidence or Audit as Enforcement.

Where fail-closed behavior applies, Enforcement failure SHALL NOT result
in unauthorized Service Execution.

# 12. Service Execution Requirements

Service Execution is the protocol stage in which the requested Service
operation is actually performed.

Service Execution SHALL occur only after all required Authentication,
Entitlement, Policy Evaluation, Authorization Evaluation, Authorization
Decision, and Enforcement requirements have been satisfied.

The applicable Service Profile SHALL determine which processing stages
are mandatory.

## 12.1 Execution Preconditions

Before Service Execution begins, all mandatory preconditions SHALL be
satisfied.

Mandatory preconditions MAY include:

- Valid Authentication
- Valid Entitlement
- Applicable Policy
- Completed Policy Evaluation
- Applicable Authorization Decision
- Completed Enforcement
- Valid Security Context
- Valid Transaction
- Valid dependencies
- Required freshness
- Required Service Profile conditions

## 12.2 Execution Authorization

Service Execution SHALL NOT be interpreted as authorization.

Authorization SHALL be established before execution where required.

An implementation SHALL NOT infer authorization solely from the fact that
Service Execution was requested.

## 12.3 Execution Scope

Service Execution SHALL remain within the scope established by the
applicable Authorization Decision and Service Profile.

An implementation SHALL NOT extend execution beyond the authorized scope.

## 12.4 Execution Conditions

Mandatory execution conditions SHALL be evaluated according to the
applicable Policy.

Where a mandatory condition ceases to be satisfied during execution, the
applicable Policy SHALL determine whether execution:

- Continues
- Is suspended
- Is revalidated
- Is cancelled
- Is terminated

## 12.5 Execution Obligations

Execution obligations established by the applicable Decision or Service
Profile SHALL be applied according to the applicable Policy.

## 12.6 Execution Restrictions

Service Execution MAY be restricted by:

- Resource
- Action
- Time
- Location
- Transaction
- Data
- Capability
- Security Context
- Policy conditions

## 12.7 Execution State

A Service Execution MAY have the following states:

- Pending
- Starting
- Running
- Suspended
- Completed
- Cancelled
- Terminated
- Failed

The applicable Service Profile MAY define additional states.

## 12.8 Execution State Transition

Execution state transitions SHALL preserve the applicable authorization,
Policy, and Security Context requirements.

## 12.9 Execution Start

Execution SHALL be considered started only when the Service operation has
actually entered its execution stage.

An Authorization Decision or Enforcement result SHALL NOT be treated as
evidence that execution has started.

## 12.10 Execution Continuation

Continued Service Execution MAY require continuing validity of:

- Authorization Decision
- Entitlement
- Authentication
- Policy
- Security Context
- Dependencies
- Transaction

Where the applicable Policy requires continued validation, the
implementation SHALL perform the required validation.

## 12.11 Authorization Change During Execution

Where the applicable Authorization Decision changes during execution,
the applicable Policy SHALL determine the resulting execution state.

A revoked or invalidated authorization SHALL NOT silently remain valid for
continued execution where continued authorization is required.

## 12.12 Entitlement Change During Execution

Where a required Entitlement changes state during execution, the
applicable Policy SHALL determine the resulting processing state.

## 12.13 Authentication Change During Execution

Where Authentication validity or assurance changes during execution, the
applicable Policy SHALL determine whether re-authentication,
revalidation, suspension, or termination is required.

## 12.14 Policy Change During Execution

A Policy change MAY require re-evaluation or revalidation during Service
Execution.

The applicable Policy SHALL define the effect of such a change.

## 12.15 Security Context Change During Execution

A required Security Context change SHALL be processed according to the
applicable Policy.

Where the required Security Context can no longer be established,
dependent execution SHALL follow the applicable Policy.

## 12.16 Dependency Change During Execution

Where a mandatory dependency becomes invalid or unavailable during
execution, the applicable Policy SHALL determine the resulting execution
state.

## 12.17 Execution Suspension

Service Execution MAY be suspended.

Suspension SHALL preserve the semantic distinction between:

- Authorization
- Enforcement
- Execution
- Execution Result

## 12.18 Execution Resumption

Resumption SHALL occur only where permitted by the applicable Policy.

Required authorization and security conditions SHALL be revalidated where
required.

## 12.19 Execution Cancellation

Service Execution MAY be cancelled by an authorized operation or
according to the applicable Policy.

Cancellation SHALL produce or establish the applicable execution state and
result.

## 12.20 Execution Termination

Service Execution SHALL terminate where required by:

- Authorization invalidation
- Policy
- Entitlement invalidation
- Authentication invalidation
- Security Context invalidation
- Dependency failure
- Transaction termination
- Administrative action
- Security failure

## 12.21 Execution Failure

An Execution Failure SHALL remain distinguishable from:

- Authorization Decision
- Enforcement
- Execution Result
- Evidence
- Audit

Execution failure SHALL be processed according to the applicable Policy.

## 12.22 Execution Error

An Execution Error MAY describe an error occurring during Service
Execution.

An Error SHALL NOT be interpreted as a Permit or Deny Authorization
Decision.

## 12.23 Execution Result

Completion, failure, cancellation, or termination of Service Execution
SHALL produce or establish the applicable Execution Result where required.

An Execution Result SHALL remain distinguishable from Service Execution
itself.

## 12.24 Execution Result Validation

Where required, the Execution Result SHALL be validated before being
accepted as the result of the Service operation.

## 12.25 Partial Execution

Where execution completes only partially, the applicable Policy SHALL
determine the resulting state and Execution Result.

Partial execution SHALL NOT be interpreted as full business success unless
the applicable Service Profile defines it as such.

## 12.26 Execution Timeout

An execution MAY terminate because of timeout.

Timeout processing SHALL follow the applicable Policy.

## 12.27 Execution Idempotency

Where Service Execution is defined as idempotent, repeated execution
requests SHALL preserve the applicable idempotency semantics.

An implementation SHALL prevent retries from producing unauthorized
duplicate effects.

## 12.28 Execution Concurrency

Concurrent Service Execution SHALL preserve authorization scope and
Policy requirements.

Race conditions SHALL NOT result in unauthorized Service Execution.

## 12.29 Execution Completion

A completed Service Execution SHALL remain distinguishable from:

- Authorization Decision
- Enforcement
- Transaction
- Execution Result
- Evidence
- Audit

Completion SHALL establish the applicable execution state.

## 12.30 Service Execution Conformance

A conforming implementation SHALL execute a Service only when all
mandatory conditions defined by the applicable Service Profile and Policy
have been satisfied.

An implementation SHALL NOT:

- Treat Authentication success as sufficient for Service Execution.
- Treat Entitlement possession as sufficient for Service Execution.
- Treat Policy Evaluation success as sufficient for Service Execution.
- Treat Authorization Evaluation as completed Enforcement.
- Treat an Authorization Decision as completed Service Execution.
- Bypass required Enforcement.
- Ignore mandatory execution conditions.
- Ignore mandatory execution obligations.
- Execute outside the authorized scope.
- Treat historical state as current state.
- Treat Evidence as proof that execution occurred.
- Treat Audit as proof that execution succeeded.

Where a required Service Execution condition cannot be established, the
applicable Policy SHALL determine the resulting processing state.

Where fail-closed behavior applies, inability to establish a required
Service Execution condition SHALL NOT result in unauthorized Service
Execution.


# 13. Execution Result Requirements

An Execution Result represents the protocol result associated with a
Service Execution.

An Execution Result SHALL remain semantically distinct from:

- Authorization Evaluation
- Authorization Decision
- Enforcement
- Service Execution
- Transaction
- Evidence
- Audit
- Error

An Execution Result SHALL describe the result of Service Execution and
SHALL NOT itself constitute authorization.

## 13.1 Execution Result Creation

An Execution Result SHALL be created or established when the applicable
Service Execution reaches a result state requiring a result.

An implementation MAY create intermediate result information during
processing where permitted by the applicable Service Profile.

## 13.2 Execution Result Identifier

An Execution Result MAY have a unique identifier.

Where an identifier is provided, it SHALL identify the corresponding
Execution Result instance.

An Execution Result Identifier SHALL NOT be reused for an unrelated
execution result.

## 13.3 Execution Result State

An Execution Result MAY have a state including:

- Success
- Failure
- Partial
- Cancelled
- Terminated
- Indeterminate

The applicable Service Profile MAY define additional result states.

## 13.4 Execution Result Attributes

An Execution Result MAY contain:

- Execution Identifier
- Transaction Identifier
- Result Identifier
- Result State
- Result timestamp
- Service Profile
- Service Identifier
- Processing status
- Business result
- Error reference
- Failure reason
- Completion information

The applicable Service Profile MAY define additional attributes.

## 13.5 Execution Result Validity

An Execution Result SHALL be valid only where it corresponds to the
identified Service Execution.

An implementation SHALL NOT associate an Execution Result with an
unrelated Execution Identifier or Transaction Identifier.

## 13.6 Execution Result and Authorization

An Execution Result SHALL NOT be interpreted as proof that the Service
Execution was authorized.

Authorization SHALL be established through the applicable Authorization
Decision and Enforcement processing.

## 13.7 Execution Result and Business Success

A successful Execution Result MAY indicate successful completion of the
technical Service Execution.

Technical execution success SHALL NOT automatically constitute business
success unless the applicable Service Profile defines that relationship.

## 13.8 Partial Result

Where Service Execution produces only a partial result, the Execution
Result SHALL identify or otherwise establish the partial nature of the
result where required.

Partial processing SHALL NOT be represented as complete processing where
doing so would alter the semantic meaning of the Service operation.

## 13.9 Failed Result

A failed Execution Result SHALL indicate that the applicable Service
Execution did not complete successfully.

Failure information MAY identify:

- Failure category
- Failure reason
- Processing stage
- Error reference
- Recovery state

## 13.10 Indeterminate Result

An Indeterminate Execution Result MAY be produced where the final
execution state cannot be conclusively established.

An Indeterminate Result SHALL NOT be interpreted as successful Service
Execution.

## 13.11 Result Consistency

The Execution Result SHALL remain consistent with the final known state
of the corresponding Service Execution.

An implementation SHALL NOT report successful completion where the
execution state establishes failure or termination.

## 13.12 Result Evidence

An Execution Result MAY generate Evidence.

Evidence MAY include:

- Result Identifier
- Execution Identifier
- Transaction Identifier
- Result State
- Result timestamp
- Processing component
- Validation result

Evidence SHALL remain distinguishable from the Execution Result.

## 13.13 Result Audit

Audit information MAY record:

- Result creation
- Result validation
- Result completion
- Result failure
- Result correction
- Result correlation

Audit information SHALL remain distinguishable from the Execution Result.

## 13.14 Result Conformance

A conforming implementation SHALL preserve the semantic distinction
between Service Execution and its Execution Result.

An implementation SHALL NOT:

- Treat an Authorization Decision as an Execution Result.
- Treat Enforcement as an Execution Result.
- Treat an Execution Result as authorization.
- Treat an Indeterminate Result as success.
- Treat a historical Result as current execution state.
- Associate a Result with an unrelated Execution.

Where the applicable Service Profile requires a valid Execution Result,
failure to establish that result SHALL be handled according to the
applicable Policy.

# 14. Enforcement Result Requirements

An Enforcement Result represents the result of applying Enforcement to an
Authorization Decision.

An Enforcement Result SHALL remain distinct from:

- Authorization Evaluation
- Authorization Decision
- Enforcement
- Service Execution
- Execution Result

## 14.1 Enforcement Result Creation

An Enforcement Result MAY be created when Enforcement reaches a defined
result state.

Where the applicable Service Profile requires an Enforcement Result,
the result SHALL be established before dependent processing continues.

## 14.2 Enforcement Result States

An Enforcement Result MAY include:

- Permitted
- Denied
- Indeterminate
- Revalidation Required
- Failed
- Suspended
- Terminated

The applicable Policy MAY define additional states.

## 14.3 Enforcement Result Attributes

An Enforcement Result MAY contain:

- Enforcement Identifier
- Decision Identifier
- Transaction Identifier
- Result State
- Validation result
- Processing timestamp
- Failure reason
- Security Context
- Processing component

## 14.4 Enforcement Result and Execution

A Permit Enforcement Result MAY establish that Service Execution may
proceed where all other required conditions are satisfied.

A Permit Enforcement Result SHALL NOT itself constitute Service Execution.

A Deny, Failed, or Indeterminate Enforcement Result SHALL NOT permit
Service Execution where fail-closed behavior applies.

## 14.5 Enforcement Result Validation

Where required, the Enforcement Result SHALL be validated before it is
used to establish the next processing stage.

Validation MAY include:

- Result state
- Decision reference
- Transaction reference
- Scope
- Conditions
- Obligations
- Security Context
- Freshness

## 14.6 Enforcement Failure Result

Where Enforcement fails, the resulting Enforcement Result SHALL reflect
the applicable failure state.

Failure SHALL NOT silently become a Permit result.

## 14.7 Enforcement Result Evidence

Evidence MAY describe the Enforcement Result.

Evidence MAY include:

- Enforcement Identifier
- Decision Identifier
- Result State
- Validation result
- Transaction Identifier
- Timestamp
- Failure reason

## 14.8 Enforcement Result Audit

Audit information MAY record:

- Enforcement Result creation
- Enforcement Result validation
- Enforcement failure
- Enforcement retry
- Enforcement recovery
- Enforcement completion

Audit information SHALL remain distinguishable from the Enforcement
Result.

## 14.9 Enforcement Result Conformance

An implementation SHALL preserve the distinction between:

- Authorization Decision
- Enforcement
- Enforcement Result
- Service Execution

An implementation SHALL NOT treat a Permit Enforcement Result as proof
that Service Execution occurred.

# 15. Evidence Requirements

Evidence is information generated or retained to demonstrate, describe,
or support verification of protocol processing, state, decisions,
operations, or results.

Evidence SHALL remain semantically distinct from the object, operation, or
result that it describes.

## 15.1 Evidence Purpose

Evidence MAY be generated for:

- Authentication
- Entitlement
- Policy Evaluation
- Authorization Evaluation
- Authorization Decision
- Enforcement
- Service Execution
- Execution Result
- Transaction
- State Transition
- Dependency processing
- Security processing
- Administrative processing
- Conformance verification

The applicable Policy or Service Profile MAY require particular Evidence.

## 15.2 Evidence Identifier

Evidence MAY have a unique identifier.

Where provided, the identifier SHALL identify the Evidence instance.

An Evidence Identifier SHALL NOT be reused for unrelated Evidence.

## 15.3 Evidence Content

Evidence MAY contain:

- Object Identifier
- Transaction Identifier
- Decision Identifier
- Execution Identifier
- Processing stage
- Processing result
- Timestamp
- Version
- Security Context
- Processing component
- Dependency state
- Validation result
- Failure information

The applicable Policy MAY define additional Evidence content.

## 15.4 Evidence Generation

Evidence MAY be generated:

- During processing
- After processing
- Upon state transition
- Upon failure
- Upon recovery
- Upon administrative action
- Upon completion

Evidence generation SHALL follow the applicable Policy.

## 15.5 Evidence Integrity

Where Evidence is required to be trustworthy, appropriate integrity
protection SHALL be applied.

Integrity protection MAY include:

- Cryptographic protection
- Digital signature
- Integrity hash
- Protected storage
- Access control
- Tamper detection

## 15.6 Evidence Authenticity

Where authenticity is required, the origin or provenance of Evidence
SHALL be established according to the applicable Policy.

## 15.7 Evidence Freshness

Evidence MAY contain timestamps or other information establishing its
freshness.

Evidence describing historical processing SHALL remain distinguishable
from current protocol state.

## 15.8 Evidence Correlation

Evidence MAY reference:

- Authentication
- Entitlement
- Policy
- Authorization Decision
- Enforcement
- Service Execution
- Execution Result
- Transaction
- State Transition

Correlation SHALL NOT eliminate the identity of the Evidence or the
referenced object.

## 15.9 Evidence Retention

Evidence MAY be retained according to:

- Policy
- Legal requirements
- Compliance requirements
- Service Profile
- Operational requirements

Retention SHALL NOT alter the semantic meaning of the Evidence.

## 15.10 Evidence Protection

Required Evidence SHALL be protected against unauthorized modification
or deletion according to the applicable Policy.

## 15.11 Evidence Access Control

Access to Evidence MAY require authorization.

Evidence access control SHALL NOT be interpreted as authorization of the
underlying Service operation.

## 15.12 Evidence Redaction

Evidence MAY be redacted where required by:

- Privacy requirements
- Security requirements
- Policy
- Legal requirements

Redaction SHALL NOT falsely represent the underlying protocol event.

## 15.13 Evidence References

Evidence MAY reference other Evidence or protocol objects.

References SHALL remain distinguishable from the referenced objects.

## 15.14 Evidence of Authentication

Authentication Evidence MAY describe:

- Authentication attempt
- Authentication method
- Authentication result
- Authentication assurance
- Security Context
- Timestamp
- Credential reference

Authentication Evidence SHALL NOT itself constitute Authentication.

## 15.15 Evidence of Entitlement

Entitlement Evidence MAY describe:

- Entitlement creation
- Entitlement activation
- Entitlement validation
- Entitlement consumption
- Entitlement expiration
- Entitlement revocation

Entitlement Evidence SHALL NOT itself constitute Entitlement.

## 15.16 Evidence of Policy Evaluation

Policy Evaluation Evidence MAY describe:

- Policy Identifier
- Policy Version
- Evaluation inputs
- Evaluation result
- Evaluation timestamp
- Processing component

Policy Evaluation Evidence SHALL remain distinct from the Policy and
Evaluation Result.

## 15.17 Evidence of Authorization

Authorization Evidence MAY describe:

- Authorization Evaluation
- Authorization Decision
- Decision state
- Decision scope
- Decision conditions
- Decision validity

Authorization Evidence SHALL NOT itself constitute Authorization.

## 15.18 Evidence of Enforcement

Enforcement Evidence MAY describe:

- Enforcement operation
- Decision reference
- Enforcement result
- Enforcement state
- Validation result
- Failure state

Enforcement Evidence SHALL NOT itself constitute Enforcement.

## 15.19 Evidence of Service Execution

Execution Evidence MAY describe:

- Execution Identifier
- Execution state
- Execution start
- Execution completion
- Execution failure
- Execution termination
- Execution timestamp

Execution Evidence SHALL NOT itself constitute Service Execution.

## 15.20 Evidence of Execution Result

Evidence MAY describe:

- Result Identifier
- Result state
- Completion state
- Failure state
- Processing timestamp

Such Evidence SHALL remain distinguishable from the Execution Result.

## 15.21 State Transition Evidence

Evidence MAY describe a State Transition.

Such Evidence MAY include:

- Previous state
- New state
- Transition trigger
- Transition timestamp
- Object Identifier
- Transaction Identifier

State Transition Evidence SHALL remain distinct from the State Transition.

## 15.22 Dependency Evidence

Evidence MAY describe:

- Dependency Identifier
- Dependency state
- Dependency validation
- Dependency failure
- Dependency transition

Dependency Evidence SHALL remain distinct from the dependency itself.

## 15.23 Security Evidence

Security Evidence MAY describe:

- Integrity validation
- Replay detection
- Freshness validation
- Key validation
- Security Context validation
- Security failure
- Security recovery

Security Evidence SHALL remain distinguishable from the security control
or operation it describes.

## 15.24 Evidence Conformance

A conforming implementation SHALL preserve the semantic distinction
between Evidence and the protocol object or operation being evidenced.

An implementation SHALL NOT:

- Treat Evidence as Authorization.
- Treat Evidence as Enforcement.
- Treat Evidence as Service Execution.
- Treat historical Evidence as current state.
- Use Evidence correlation to eliminate object identity.
- Treat Evidence generation as proof that the underlying operation
  succeeded.

Where Evidence is mandatory, failure to generate, protect, or retain the
required Evidence SHALL be handled according to the applicable Policy.

# 16. Audit Requirements

Audit information records protocol processing history for operational,
security, compliance, or investigative purposes.

Audit information SHALL remain semantically distinct from the protocol
objects and operations being recorded.

## 16.1 Audit Purpose

Audit information MAY record:

- Authentication
- Entitlement
- Policy Evaluation
- Authorization Evaluation
- Authorization Decision
- Enforcement
- Service Execution
- Execution Result
- Transaction
- State Transition
- Dependency processing
- Security events
- Administrative actions
- Errors
- Failures
- Recovery

## 16.2 Audit Identifier

Audit information MAY have a unique identifier.

An Audit Identifier SHALL identify the corresponding audit record.

## 16.3 Audit Content

Audit information MAY include:

- Event type
- Object Identifier
- Transaction Identifier
- Decision Identifier
- Execution Identifier
- Processing component
- Processing stage
- State
- Result
- Timestamp
- Version
- Security Context
- Failure reason

The applicable Policy MAY define additional Audit content.

## 16.4 Audit Integrity

Where Audit integrity is required, appropriate protection SHALL be
applied.

Protection MAY include:

- Integrity controls
- Append-only storage
- Digital signatures
- Access control
- Tamper detection

## 16.5 Audit Immutability

Where immutability is required, Audit information SHALL NOT be modified
in a manner that obscures or alters the recorded event.

Corrections MAY be recorded as additional Audit information.

## 16.6 Audit Correlation

Audit information MAY reference protocol objects and operations.

Correlation SHALL NOT eliminate the distinction between the Audit record
and the referenced object.

## 16.7 Audit and Evidence

Evidence and Audit MAY describe the same protocol event.

Evidence and Audit SHALL remain distinct information types even where they
reference the same event.

## 16.8 Audit Historical State

Audit information MAY preserve historical state.

Historical Audit information SHALL NOT automatically be treated as current
protocol state.

## 16.9 Audit Access

Access to Audit information MAY require authorization according to the
applicable Policy.

Audit access authorization SHALL remain distinct from the authorization
of the underlying Service operation.

## 16.10 Audit Retention

Audit information MAY be retained according to:

- Policy
- Compliance requirements
- Legal requirements
- Security requirements
- Operational requirements

## 16.11 Audit Security

Audit information SHALL be protected according to the applicable
security requirements where it contains security-sensitive information.

## 16.12 Audit Failure

Where required Audit information cannot be generated or protected, the
applicable Policy SHALL determine the resulting processing state.

Where fail-closed behavior applies, failure of a mandatory Audit control
SHALL NOT result in unauthorized Service Execution.

## 16.13 Audit Conformance

A conforming implementation SHALL preserve the semantic distinction
between Audit information and the protocol operation being recorded.

An implementation SHALL NOT:

- Treat Audit as Authorization.
- Treat Audit as Enforcement.
- Treat Audit as Service Execution.
- Treat historical Audit information as current state.
- Treat existence of an Audit record as proof of successful Service
  Execution.
- Use Audit correlation to eliminate object identity.

# 17. State Transition Requirements

A State Transition represents a defined change from one valid protocol
state to another.

State Transition processing SHALL preserve the identity and semantic
meaning of the object whose state changes.

## 17.1 State Transition Definition

A State Transition MAY occur for:

- Authentication
- Entitlement
- Policy
- Authorization Decision
- Enforcement
- Service Execution
- Execution Result
- Transaction
- Security Context
- Dependency

## 17.2 Transition Identifier

A State Transition MAY have a unique identifier.

The identifier SHALL identify the corresponding transition instance.

## 17.3 Previous State

Where applicable, a State Transition SHALL identify or otherwise
establish the previous state.

## 17.4 New State

A State Transition SHALL establish the resulting state according to the
applicable protocol rules.

## 17.5 Transition Validity

A State Transition SHALL be valid only where:

- The previous state is valid.
- The transition trigger is valid.
- The resulting state is permitted.
- Required dependencies are satisfied.
- Applicable Policy requirements are satisfied.

## 17.6 Transition Trigger

A State Transition MAY be triggered by:

- Protocol operation
- Policy
- Administrative Action
- Time
- Expiration
- Revocation
- Validation result
- Security event
- Dependency change
- Transaction event
- Service event

## 17.7 Transition Ordering

Where ordering is significant, State Transitions SHALL be processed in an
order consistent with the applicable protocol and Policy.

An implementation SHALL NOT apply a later transition as though an earlier
required transition had already occurred.

## 17.8 Transition Dependency

A State Transition MAY depend on another protocol object or state.

Where a required dependency is invalid, the applicable Policy SHALL
determine whether the transition is denied, delayed, revalidated, or
otherwise processed.

## 17.9 Transition Consistency

Distributed processing SHALL preserve the semantic consistency of State
Transitions.

Conflicting state information SHALL be resolved according to the
applicable protocol and Policy.

## 17.10 Transition Invalidation

A pending State Transition MAY become invalid due to:

- Object state change
- Policy change
- Dependency change
- Security Context change
- Version change
- Transaction termination

An invalid transition SHALL NOT be applied as though it remained valid.

## 17.11 Transition Evidence

Evidence MAY describe a State Transition.

Such Evidence MAY include:

- Transition Identifier
- Object Identifier
- Previous state
- New state
- Trigger
- Timestamp
- Transaction Identifier
- Processing component

## 17.12 Transition Audit

Audit information MAY record:

- Transition initiation
- Transition validation
- Transition completion
- Transition failure
- Transition rejection

Audit information SHALL remain distinguishable from the State
Transition.

## 17.13 Transition Conformance

A conforming implementation SHALL preserve the semantic distinction
between an object state and the State Transition that changes that state.

An implementation SHALL NOT:

- Treat a State Transition as an Authorization Decision.
- Treat a historical state as current state.
- Apply an invalid transition.
- Ignore required transition dependencies.
- Bypass required validation.
- Treat transition Evidence as the transition itself.
- Treat transition Audit as proof that the transition succeeded.


# 18. Dependency Relationship Requirements

## 18.1 Dependency Relationship

A protocol object MAY depend on another protocol object, processing
condition, Security Context, Policy, or state.

A Dependency Relationship SHALL identify the condition under which the
dependent object or processing operation relies upon the dependency.

A Dependency Relationship SHALL NOT by itself constitute an
Authorization Decision.

## 18.2 Dependency Identifier

A Dependency Relationship MAY have a Dependency Identifier.

Where a Dependency Identifier is used, it SHALL uniquely identify the
Dependency Relationship within the applicable namespace.

A Dependency Identifier SHALL remain distinguishable from the identifiers
of the objects participating in the dependency.

## 18.3 Dependency Types

A Dependency Relationship MAY represent:

- Object dependency
- State dependency
- Policy dependency
- Authorization dependency
- Enforcement dependency
- Security Context dependency
- Transaction dependency
- Service dependency
- Version dependency
- Validation dependency

The applicable protocol SHALL define the semantic meaning of each
dependency type that it uses.

## 18.4 Dependency Direction

A Dependency Relationship SHALL identify, explicitly or by defined
protocol semantics, the dependent object and the dependency upon which
the dependent object relies.

An implementation SHALL NOT reverse a defined dependency relationship
without applying the applicable protocol rules.

## 18.5 Dependency Validity

A dependency SHALL be considered valid only when the conditions required
by the applicable protocol have been established.

Dependency validity MAY depend upon:

- Object existence
- Object state
- Object Version
- Policy state
- Security Context
- Authorization state
- Transaction state
- Freshness
- Revocation state
- Other defined conditions

## 18.6 Dependency Evaluation

Where dependent processing requires dependency evaluation, the
implementation SHALL evaluate the dependency before continuing the
dependent processing.

A successful evaluation of one dependency SHALL NOT be interpreted as
proof that all other required dependencies are satisfied.

## 18.7 Dependency Invalidation

A dependency MAY become invalid as a result of:

- State change
- Revocation
- Expiration
- Suspension
- Version change
- Policy change
- Security Context change
- Transaction termination
- Explicit invalidation
- Other defined protocol events

Where a dependency becomes invalid, dependent processing SHALL follow
the applicable Policy.

## 18.8 Dependency Propagation

Where invalidity of a dependency is required to propagate to dependent
objects or processing operations, the implementation SHALL apply the
defined propagation rules.

Dependency propagation SHALL NOT create an Authorization Decision that
did not otherwise exist.

## 18.9 Dependency Revalidation

A dependency MAY require revalidation before dependent processing
continues.

Where revalidation is required, the implementation SHALL use the
current information required by the applicable protocol.

Historical validation information SHALL NOT automatically establish
current dependency validity.

## 18.10 Dependency Failure

Where a required dependency cannot be established, the applicable
Policy SHALL determine the resulting processing state.

The resulting state MAY include:

- Deny
- Indeterminate
- Revalidation Required
- Processing suspension
- Transaction termination
- Service Execution prevention
- Other defined recovery action

## 18.11 Dependency Chain

A protocol MAY define a chain of dependencies in which one dependency
depends upon another dependency.

An implementation SHALL evaluate all dependencies required for the
dependent processing operation.

A dependency chain SHALL NOT be treated as satisfied merely because the
first dependency in the chain is valid.

## 18.12 Dependency Cycle

An implementation SHALL detect or otherwise prevent an undefined
dependency cycle where such a cycle could prevent determination of the
required processing state.

A dependency cycle SHALL NOT be interpreted as implicit satisfaction of
the affected dependencies.

## 18.13 Dependency and Authorization

Dependency satisfaction SHALL be treated as an input to Authorization
Evaluation where the applicable Policy requires it.

Dependency satisfaction SHALL NOT itself constitute authorization.

## 18.14 Dependency and Enforcement

Where Enforcement depends upon an Authorization Decision or other
required dependency, Enforcement SHALL verify the required dependency
before permitting the applicable Service Execution.

Historical dependency state SHALL NOT automatically authorize current
Enforcement.

## 18.15 Dependency Evidence

An implementation MAY generate Evidence describing dependency
evaluation.

Such Evidence MAY include:

- Dependency Identifier
- Object Identifier
- Dependency state
- Validation result
- Timestamp
- Transaction Identifier
- Processing component
- Result

Dependency Evidence SHALL remain distinguishable from the dependency
relationship and from the objects participating in that relationship.

## 18.16 Dependency Conformance

A conforming implementation SHALL preserve the semantic distinction
between:

- Dependency
- Dependent Object
- Dependency State
- Dependency Evaluation
- Authorization Decision
- Enforcement
- Service Execution

An implementation SHALL NOT:

- Treat dependency existence as authorization.
- Ignore a required dependency.
- Treat an invalid dependency as valid.
- Bypass required dependency evaluation.
- Treat historical dependency state as current state.
- Treat dependency Evidence as the dependency itself.

# 19. Security Context Requirements

## 19.1 Security Context

A Security Context represents security-relevant conditions applicable to
a protocol operation, object, Transaction, or Service Execution.

A Security Context MAY include information relating to:

- Authentication
- Authentication assurance
- Credential state
- Device state
- Security environment
- Session state
- Transaction state
- Policy requirements
- Other defined security conditions

## 19.2 Security Context Identifier

A Security Context MAY have a Security Context Identifier.

Where used, the identifier SHALL uniquely identify the applicable
Security Context within its defined namespace.

The Security Context Identifier SHALL remain distinguishable from the
Authentication Result, Authorization Decision, Transaction Identifier,
and other protocol object identifiers.

## 19.3 Security Context State

A Security Context MAY have a state.

The state MAY include:

- Valid
- Invalid
- Expired
- Revoked
- Suspended
- Revalidation Required
- Unknown
- Other defined state

An implementation SHALL NOT treat an unknown or invalid Security Context
as valid where validity is required.

## 19.4 Security Context Establishment

A Security Context SHALL be established only through the applicable
protocol processing.

Successful Authentication MAY contribute to establishment of a Security
Context but SHALL NOT necessarily establish every required security
condition.

## 19.5 Security Context Change

A Security Context MAY change as a result of:

- Re-authentication
- Credential change
- Device state change
- Policy change
- Entitlement change
- Transaction state change
- Administrative action
- Security event
- Other defined event

A Security Context change MAY require re-evaluation of dependent
processing.

## 19.6 Security Context Propagation

Where a Security Context is required by a dependent component, the
applicable Security Context SHALL be propagated or made available in
accordance with the protocol.

Propagation SHALL preserve the semantic identity and security meaning of
the Security Context.

## 19.7 Security Context Integrity

A Security Context SHALL be protected against unauthorized modification.

Where integrity validation is required, an implementation SHALL validate
the Security Context before relying upon security-sensitive information
contained within it.

## 19.8 Security Context Freshness

Where current Security Context information is required, the
implementation SHALL determine whether the available Security Context
satisfies the applicable freshness requirements.

Historical Security Context information SHALL NOT automatically establish
current security state.

## 19.9 Security Context Dependency

A Security Context MAY depend upon another protocol object or processing
condition.

Where such dependency exists, the applicable dependency requirements
SHALL apply.

## 19.10 Security Context and Authorization

A Security Context MAY be an input to Authorization Evaluation.

A valid Security Context SHALL NOT itself constitute an Authorization
Decision unless the applicable Policy explicitly defines such semantics.

## 19.11 Security Context Failure

Where a required Security Context cannot be established or validated,
the applicable Policy SHALL determine the resulting processing state.

Where fail-closed behavior applies, failure to establish a required
Security Context SHALL NOT result in unauthorized Service Execution.

## 19.12 Security Context Evidence

An implementation MAY generate Evidence describing Security Context
establishment, validation, or change.

Such Evidence MAY include:

- Security Context Identifier
- Validation result
- State
- Timestamp
- Transaction Identifier
- Authentication reference
- Processing component
- Result

Security Context Evidence SHALL remain distinguishable from the Security
Context itself.

## 19.13 Security Context Conformance

A conforming implementation SHALL preserve the semantic distinction
between:

- Security Context
- Authentication
- Entitlement
- Policy Evaluation
- Authorization Evaluation
- Authorization Decision
- Enforcement
- Service Execution

An implementation SHALL NOT:

- Treat Security Context existence as authorization.
- Treat historical Security Context state as current state.
- Ignore required Security Context validation.
- Bypass required Security Context revalidation.
- Treat Security Context Evidence as the Security Context itself.

# 20. Object State and Lifecycle Requirements

## 20.1 Object State

A protocol object MAY have a defined lifecycle and state.

The state SHALL represent the semantic condition of the object as defined
by the applicable protocol.

## 20.2 Object Existence

Object existence SHALL remain distinguishable from object validity.

The existence of an object SHALL NOT by itself establish that the object
is valid, active, current, or usable.

## 20.3 Object State Validation

Where an object's state is required for processing, the implementation
SHALL validate the applicable state before relying upon it.

An implementation SHALL NOT assume that a previously observed state
remains valid indefinitely.

## 20.4 Object State Freshness

Where current state is required, the implementation SHALL apply the
applicable freshness requirements.

Historical state MAY be retained for Evidence, Audit, compliance,
investigation, recovery, or dispute resolution.

Historical state SHALL NOT automatically be treated as current state.

## 20.5 Object Lifecycle

An object lifecycle MAY include:

- Creation
- Activation
- Suspension
- Modification
- Revocation
- Expiration
- Invalidation
- Termination
- Completion
- Deletion

The applicable protocol SHALL define any lifecycle states that are
security-sensitive.

## 20.6 State Transition

A state change SHALL occur only through a valid State Transition.

The requirements of Section 17 SHALL apply to State Transitions.

## 20.7 State Invalidation

An object MAY become invalid without ceasing to exist.

Where an object is invalid, dependent processing SHALL NOT treat the object
as valid.

## 20.8 State Revalidation

An object MAY require revalidation before continued use.

Where revalidation is required, dependent processing SHALL follow the
applicable Policy.

## 20.9 Object Revocation

Where an object is revoked, the applicable protocol SHALL determine the
effect of revocation on dependent objects and processing operations.

Revocation SHALL NOT be ignored merely because a dependent Transaction or
Session remains active.

## 20.10 Object Suspension

A suspended object SHALL NOT be treated as active where active state is
required.

Suspension MAY permit subsequent reactivation or revalidation according
to the applicable protocol.

## 20.11 Object Expiration

An expired object SHALL NOT be treated as current or valid where current
or valid state is required.

Expiration processing MAY require revalidation, replacement, renewal,
or termination of dependent processing.

## 20.12 Object Version

An object MAY have a Version.

Where Version affects security semantics, the applicable Version SHALL be
validated before dependent processing continues.

## 20.13 Object Identity

Object identity SHALL remain stable and distinguishable from object
state.

A state change SHALL NOT silently create a new object identity unless the
applicable protocol explicitly defines such behavior.

## 20.14 Object Lifecycle and Authorization

Object lifecycle state MAY affect Authorization Evaluation.

An object lifecycle event SHALL NOT by itself create an Authorization
Decision unless explicitly defined by the applicable Policy.

## 20.15 Object Lifecycle Evidence

An implementation MAY generate Evidence describing lifecycle events.

Such Evidence MAY include:

- Object Identifier
- Previous state
- New state
- Transition Identifier
- Timestamp
- Transaction Identifier
- Processing component
- Result

Lifecycle Evidence SHALL remain distinguishable from the object and from
the lifecycle event itself.

## 20.16 Object Lifecycle Conformance

A conforming implementation SHALL preserve the semantic distinction
between:

- Object Identity
- Object Existence
- Object State
- State Transition
- Object Version
- Evidence
- Audit

An implementation SHALL NOT:

- Treat object existence as proof of validity.
- Treat historical state as current state.
- Apply an undefined lifecycle transition.
- Ignore required state validation.
- Treat lifecycle Evidence as the lifecycle event itself.

# 21. Transaction Requirements

## 21.1 Transaction

A Transaction represents a defined unit of protocol processing that MAY
correlate multiple protocol operations and objects.

A Transaction SHALL remain distinguishable from the objects and
operations that it correlates.

## 21.2 Transaction Identifier

A Transaction SHALL have a Transaction Identifier where transaction
correlation is required.

The Transaction Identifier SHALL uniquely identify the applicable
Transaction within its namespace.

## 21.3 Transaction Creation

A Transaction SHALL be created only through defined protocol processing.

Transaction creation SHALL NOT itself constitute Authentication,
Entitlement, Policy Evaluation, Authorization, Enforcement, or Service
Execution.

## 21.4 Transaction Scope

A Transaction MAY define the scope of the operations and objects
associated with it.

Objects SHALL NOT be associated with a Transaction solely because they
share a common identifier value unless the protocol defines such
association.

## 21.5 Transaction State

A Transaction MAY have states including:

- Created
- Active
- Suspended
- Terminated
- Completed
- Expired
- Failed
- Other defined state

The applicable protocol SHALL define the semantic meaning of each state
that it uses.

## 21.6 Transaction State Validation

Where current Transaction state is required, the implementation SHALL
validate that state before continuing dependent processing.

A historical Transaction state SHALL NOT automatically establish current
Transaction state.

## 21.7 Transaction State Transition

A Transaction state change SHALL comply with the State Transition
requirements of Section 17.

Invalid Transaction state transitions SHALL NOT be treated as valid
processing.

## 21.8 Transaction Dependencies

A Transaction MAY depend upon:

- Authentication
- Entitlement
- Policy
- Authorization Decision
- Enforcement
- Security Context
- Other protocol objects

Transaction existence SHALL NOT satisfy those dependencies.

## 21.9 Transaction and Authentication

A Transaction MAY correlate Authentication processing.

Transaction continuity SHALL NOT by itself establish current
Authentication validity.

Where current Authentication is required, the applicable Authentication
requirements SHALL apply.

## 21.10 Transaction and Entitlement

A Transaction MAY correlate Entitlement processing.

Transaction continuity SHALL NOT by itself establish current Entitlement
validity.

Where Entitlement state changes, dependent processing SHALL follow the
applicable Policy.

## 21.11 Transaction and Policy

A Transaction MAY identify the Policy applicable to its processing.

A historical Policy state SHALL NOT automatically remain applicable where
the protocol requires current Policy state.

## 21.12 Transaction and Authorization Decision

A Transaction MAY identify an Authorization Decision.

Transaction existence SHALL NOT constitute an Authorization Decision.

A Decision SHALL remain subject to its own validity, scope, dependency,
and lifecycle requirements.

## 21.13 Transaction and Enforcement

A Transaction MAY correlate Enforcement operations.

Enforcement SHALL remain distinguishable from the Transaction.

Transaction continuity SHALL NOT automatically authorize continued
Enforcement.

## 21.14 Transaction and Service Execution

A Transaction MAY correlate Service Execution.

Transaction existence SHALL NOT constitute successful Service Execution.

Service Execution SHALL occur only after the applicable Authorization and
Enforcement requirements have been satisfied.

## 21.15 Transaction and Execution Result

A Transaction MAY identify an Execution Result.

An Execution Result SHALL remain distinguishable from the Transaction and
from the Service Execution that produced it.

## 21.16 Transaction Termination

A Transaction MAY be terminated because of:

- Completion
- Failure
- Expiration
- Cancellation
- Revocation
- Security event
- Administrative action
- Other defined condition

Where Transaction termination affects dependent processing, the applicable
Policy SHALL determine the resulting state.

## 21.17 Transaction Expiration

An expired Transaction SHALL NOT be treated as active.

Where continued processing requires a new or revalidated Transaction, the
implementation SHALL apply the applicable protocol requirements.

## 21.18 Transaction Revalidation

A Transaction MAY require revalidation during its lifecycle.

Revalidation MAY be triggered by:

- State change
- Policy change
- Security Context change
- Authentication change
- Entitlement change
- Authorization change
- Dependency change
- Expiration
- Other defined condition

Where revalidation is required, dependent processing SHALL NOT continue
solely because the Transaction remains active.

## 21.19 Transaction Evidence

An implementation MAY generate Evidence describing Transaction
processing.

Such Evidence MAY include:

- Transaction Identifier
- State
- State Transition
- Correlated Object Identifier
- Authorization Decision Identifier
- Execution Identifier
- Timestamp
- Processing component
- Result

Transaction Evidence SHALL remain distinguishable from the Transaction.

## 21.20 Transaction Audit

Audit information MAY record the processing history of a Transaction.

Audit information SHALL remain distinguishable from the Transaction and
from the operations recorded by the Audit information.

## 21.21 Transaction Conformance

A conforming implementation SHALL preserve the semantic distinction
between:

- Transaction
- Authentication
- Entitlement
- Policy Evaluation
- Authorization Evaluation
- Authorization Decision
- Enforcement
- Service Execution
- Execution Result
- Evidence
- Audit

An implementation SHALL NOT:

- Treat Transaction existence as authorization.
- Treat Transaction continuity as permanent Authentication validity.
- Treat Transaction continuity as permanent Entitlement validity.
- Treat Transaction continuity as permanent Policy validity.
- Treat Transaction continuity as permanent Authorization validity.
- Treat Transaction continuity as permanent Enforcement validity.
- Treat Transaction continuity as proof of Service Execution success.
- Treat historical Transaction state as current state.
- Bypass required Transaction validation.
- Bypass required Authorization Evaluation because a Transaction exists.
- Bypass required Enforcement because a Transaction exists.

# 22. Error and Failure Handling Requirements

## 22.1 Error

An Error represents an abnormal processing condition identified by the
applicable protocol.

An Error SHALL remain distinguishable from:

- Failure
- Authorization Decision
- Execution Result
- Evidence
- Audit

## 22.2 Error Classification

Errors MAY be classified as:

- Input error
- Authentication error
- Entitlement error
- Policy error
- Authorization error
- Enforcement error
- Service Execution error
- Security error
- Dependency error
- Interoperability error
- Version error
- Internal processing error
- Other defined error

## 22.3 Error Processing

Where an Error occurs, the applicable Policy SHALL determine the
resulting processing state.

An Error SHALL NOT automatically constitute Deny, Permit, or
Authorization.

## 22.4 Failure

A Failure represents unsuccessful completion or inability to complete a
required processing operation.

A Failure SHALL remain distinguishable from the Error that may have
caused it.

## 22.5 Failure Propagation

A Failure MAY propagate to dependent processing where required by the
applicable protocol.

Failure propagation SHALL preserve the semantic distinction between the
original Failure and the resulting processing state.

## 22.6 Fail-Closed Processing

Where fail-closed behavior is required by the applicable Policy, an
unresolved required security or authorization condition SHALL NOT result
in unauthorized Service Execution.

Fail-closed behavior MAY result in:

- Deny
- Indeterminate
- Revalidation Required
- Processing termination
- Service Execution prevention
- Other defined safe state

## 22.7 Recovery Processing

A protocol MAY define recovery processing following an Error or Failure.

Recovery SHALL NOT silently establish an Authorization Decision that was
not otherwise established according to the applicable protocol.

## 22.8 Retry Processing

A protocol MAY permit retry processing.

A retry SHALL NOT bypass required validation, Authorization Evaluation,
Enforcement, or Security Context checks.

Where an operation is non-idempotent, retry processing SHALL apply the
applicable idempotency requirements.

## 22.9 Revalidation After Failure

An Error or Failure MAY require revalidation before dependent processing
continues.

Where revalidation is required, the implementation SHALL apply the
current validation requirements.

## 22.10 Error Evidence and Audit

An implementation MAY generate Evidence describing an Error or Failure.

Audit information MAY record the corresponding processing event.

Error Evidence and Audit SHALL remain distinguishable from the Error,
Failure, and protocol operation that they describe.

## 22.11 Exceptional Processing

Exceptional processing SHALL preserve the semantic distinction between:

- Authentication
- Entitlement
- Policy Evaluation
- Authorization Evaluation
- Authorization Decision
- Enforcement
- Service Execution
- Execution Result
- Evidence
- Audit
- Error
- Failure
- Recovery

An implementation SHALL NOT use an Error, Failure, or Recovery result to
silently create an Authorization Decision that was not otherwise
established according to the applicable protocol.

# 23. Security and Integrity Requirements

## 23.1 Security Requirements

A conforming implementation SHALL protect protocol objects and processing
operations against unauthorized modification, substitution, replay,
impersonation, and other security threats addressed by the applicable
protocol.

## 23.2 Object Integrity

Where integrity protection is required, the implementation SHALL verify
the integrity of the applicable protocol object before relying upon its
security-sensitive contents.

## 23.3 State Integrity

Security-sensitive object state SHALL be protected against unauthorized
modification.

A state value SHALL NOT be accepted solely because it is syntactically
valid.

## 23.4 Processing Integrity

Security-sensitive processing SHALL preserve the relationship between
validated inputs and the resulting processing operation.

An implementation SHALL NOT replace a validated object, state, Policy,
or Decision with an unrelated object while retaining the original
validation result.

## 23.5 Authorization Integrity

An Authorization Decision SHALL be protected against unauthorized
modification.

Where Decision integrity cannot be established, the applicable Policy
SHALL determine the resulting processing state.

## 23.6 Enforcement Integrity

Enforcement SHALL apply only to the Authorization Decision and conditions
established for the applicable processing operation.

An implementation SHALL NOT substitute an unrelated Decision during
Enforcement.

## 23.7 Security Failure Handling

Where a required security control fails, the applicable Policy SHALL
determine the resulting processing state.

Where fail-closed behavior applies, the failure SHALL NOT result in
unauthorized Service Execution.

## 23.8 Security Evidence

An implementation MAY generate Evidence describing security validation,
integrity verification, or security failure.

Such Evidence SHALL remain distinguishable from the protected object and
from the security operation itself.

## 23.9 Security Conformance

A conforming implementation SHALL preserve the semantic distinction
between security controls and the protocol objects or decisions that
those controls protect.

An implementation SHALL NOT treat successful security logging as proof
that authorization was granted.

# 24. Replay, Freshness, and Key Protection Requirements

## 24.1 Replay Protection

A conforming implementation SHALL prevent reuse of a protocol object or
security message where the applicable protocol defines the object or
message as single-use.

## 24.2 Replay Detection

An implementation MAY detect replay by evaluating:

- Object Identifier
- Transaction Identifier
- Nonce
- Timestamp
- Expiration
- Consumption state
- Sequence information
- Other defined freshness information

## 24.3 One-Time-Use Objects

Where an object is defined as one-time-use, successful consumption SHALL
prevent unauthorized reuse of that object.

A consumed one-time-use object SHALL NOT be treated as available for a
subsequent operation.

## 24.4 Freshness

Where freshness is required, the implementation SHALL determine whether
the applicable object or state satisfies the defined freshness
requirements.

Historical validity SHALL NOT automatically establish current
freshness.

## 24.5 Expiration

An expired security-sensitive object SHALL NOT be accepted where current
validity is required.

## 24.6 Freshness and Authorization

Where freshness is a required Authorization input, failure to establish
freshness SHALL be handled according to the applicable Policy.

Freshness validation SHALL NOT itself constitute an Authorization
Decision.

## 24.7 Key Protection

Cryptographic keys used to protect protocol security SHALL be protected
against unauthorized disclosure, modification, and misuse.

## 24.8 Key Lifecycle

Where applicable, key lifecycle processing SHALL include:

- Generation
- Activation
- Use
- Rotation
- Revocation
- Expiration
- Destruction

The applicable protocol SHALL define security requirements for each
lifecycle stage that it uses.

## 24.9 Key State Change

A key state change MAY require revalidation or invalidation of dependent
objects.

Dependent processing SHALL follow the applicable Policy.

## 24.10 Replay and Security Evidence

An implementation MAY generate Evidence describing replay detection,
freshness validation, expiration processing, or key validation.

Such Evidence SHALL remain distinguishable from the protected object and
security operation.

# 25. Versioning and Evolution Requirements

## 25.1 Version Identification

A protocol, protocol object, Policy, or other defined protocol artifact
MAY have a Version.

Where Version is security-sensitive, the applicable Version SHALL be
identified and validated before dependent processing continues.

## 25.2 Protocol Version

A protocol Version SHALL identify the normative semantics applicable to
the implementation or protocol interaction.

## 25.3 Object Version

An object Version MAY identify the semantic or structural version of the
object.

An implementation SHALL NOT assume compatibility between object
Versions unless such compatibility is defined.

## 25.4 Policy Version

A Policy Version SHALL identify the Policy semantics applicable to
evaluation.

A historical Policy Version SHALL NOT automatically be treated as the
current Policy Version.

## 25.5 Decision Version

An Authorization Decision MAY identify the Version of the Policy or
Decision model under which the Decision was established.

Where Version affects validity, the applicable Version SHALL be
validated.

## 25.6 Version Compatibility

Where two protocol artifacts interact, the implementation SHALL apply
the applicable compatibility requirements.

Incompatible Versions SHALL NOT be silently treated as compatible.

## 25.7 Version Migration

A protocol MAY define migration between Versions.

Migration SHALL preserve required security semantics or explicitly
invalidate objects whose semantics can no longer be preserved.

## 25.8 Version Deprecation

A deprecated Version MAY remain usable for a defined compatibility
period.

A deprecated Version SHALL NOT automatically remain valid after the
applicable deprecation conditions have been satisfied.

## 25.9 Version Invalidation

A Version MAY be invalidated by Policy, administrative action, security
event, or other defined condition.

Objects dependent upon an invalid Version SHALL follow the applicable
validation or invalidation requirements.

## 25.10 Version Change and Dependency

A Version change MAY require revalidation of dependent:

- Authentication
- Entitlement
- Policy
- Authorization Decision
- Enforcement
- Service Execution
- Transaction

Where required revalidation cannot be completed, dependent processing
SHALL follow the applicable Policy.

## 25.11 Version Evidence and Audit

An implementation MAY generate Evidence describing Version changes.

Such Evidence MAY include:

- Previous Version
- New Version
- Object Identifier
- Transaction Identifier
- Change timestamp
- Change reason
- Validation result

Audit information MAY record the Version change.

Version Evidence and Audit SHALL remain distinguishable from the Version
and from the Version change itself.

# 26. Interoperability and Interface Requirements

## 26.1 Interface Definition

A protocol interface SHALL define the operations, inputs, outputs, and
security requirements necessary for interoperable processing.

## 26.2 Interface Input Validation

An implementation SHALL validate required interface inputs before
performing the corresponding protocol operation.

Invalid input SHALL NOT be interpreted as successful protocol processing.

## 26.3 Interface Output

An interface output SHALL identify the result of the applicable operation
according to the protocol semantics.

Successful message delivery SHALL NOT by itself constitute successful
completion of the requested operation.

## 26.4 Object Reference

Where an interface references a protocol object, the reference SHALL
identify the intended object according to the applicable namespace and
identity rules.

## 26.5 Message Correlation

Messages MAY contain identifiers used to correlate requests, responses,
Transactions, Decisions, or other protocol objects.

Correlation SHALL NOT eliminate the semantic distinction between the
correlated objects.

## 26.6 State Exchange

Where state is exchanged between components, the receiving component
SHALL apply the applicable validation requirements before relying upon
that state.

## 26.7 Version Negotiation

Where Version negotiation is supported, the negotiated Version SHALL be
validated before security-sensitive processing continues.

## 26.8 Interoperability Failure

Where interoperability processing cannot establish required semantics,
the applicable Policy SHALL determine the resulting processing state.

## 26.9 Security Context Exchange

Where Security Context is exchanged between components, its integrity,
identity, and applicability SHALL be validated according to the
applicable protocol.

## 26.10 Interoperability Evidence

An implementation MAY generate Evidence describing:

- Message processing
- Interface invocation
- Validation
- State exchange
- Version negotiation
- Security Context exchange
- Processing result

Interoperability Evidence SHALL remain distinguishable from the
interface operation and message that it describes.

## 26.11 Interoperability Conformance

A conforming implementation SHALL NOT:

- Treat successful Message delivery as proof of authorization.
- Treat Interface availability as proof of authorization.
- Bypass required input validation.
- Ignore incompatible Version requirements.
- Treat correlation as object identity.
- Treat interoperability success as Service Execution success.

# 27. Service Profile Requirements

## 27.1 Service Profile

A Service Profile defines the security and processing requirements
applicable to a particular Service or Service class.

A Service Profile MAY specify:

- Required Authentication
- Required Entitlement
- Applicable Policy
- Required Authorization
- Required Enforcement
- Required Security Context
- Required Version
- Other defined requirements

## 27.2 Service Profile Identifier

A Service Profile MAY have a Service Profile Identifier.

Where used, the identifier SHALL uniquely identify the applicable
Service Profile.

## 27.3 Service Profile Selection

Service Profile selection SHALL identify the Profile applicable to the
requested Service Execution.

Service Profile selection SHALL NOT itself constitute Authorization.

## 27.4 Service Requirements

A conforming implementation SHALL apply the requirements defined by the
applicable Service Profile.

## 27.5 Required Authentication

Where a Service Profile requires Authentication, the implementation SHALL
establish the required Authentication state before permitting dependent
processing.

Authentication success SHALL NOT be treated as satisfaction of all
Service Profile requirements.

## 27.6 Required Entitlement

Where a Service Profile requires Entitlement, the implementation SHALL
establish the required Entitlement validity and scope before permitting
dependent processing.

Entitlement possession SHALL NOT be treated as satisfaction of all
Service Profile requirements.

## 27.7 Required Policy

Where a Service Profile identifies an applicable Policy, the
implementation SHALL apply that Policy to the applicable processing.

## 27.8 Required Authorization

Where Authorization is required by a Service Profile, the implementation
SHALL perform the required Authorization Evaluation and establish the
applicable Authorization Decision before Service Execution.

## 27.9 Required Enforcement

Where Enforcement is required by a Service Profile, the implementation
SHALL apply the required Enforcement before Service Execution.

## 27.10 Required Security Context

Where a Security Context is required, the implementation SHALL establish
and validate the required Security Context before dependent processing.

## 27.11 Service Profile Evaluation

Service Profile evaluation MAY produce a processing result indicating
whether the required conditions have been established.

A Service Profile Evaluation Result SHALL NOT automatically constitute an
Authorization Decision unless explicitly defined by the applicable
Policy.

## 27.12 Service Profile Evidence and Audit

An implementation MAY generate Evidence describing Service Profile
selection and evaluation.

Audit information MAY record:

- Service Profile selection
- Requirement validation
- Policy selection
- Authorization processing
- Enforcement processing
- Service Execution processing

Service Profile Evidence and Audit SHALL remain distinguishable from the
Service Profile and from the protocol operations that they describe.

## 27.13 Service Profile Conformance

A conforming implementation SHALL NOT:

- Treat Service Profile selection as authorization.
- Treat Capability identification as authorization.
- Treat Authentication success as satisfaction of all Service
  requirements.
- Treat Entitlement possession as satisfaction of all Service
  requirements.
- Bypass required Policy Evaluation.
- Bypass required Authorization Evaluation.
- Bypass required Enforcement.
- Execute the Service without satisfying required conditions.

Where a required Service Profile condition cannot be established, the
applicable Policy SHALL determine the resulting processing state.

Where fail-closed behavior applies, inability to establish a required
Service Profile condition SHALL NOT result in unauthorized Service
Execution.

# 28. Administrative and Operational Control Requirements

## 28.1 Administrative Action

An Administrative Action is an operation performed by an authorized
administrative actor or system that changes protocol configuration,
objects, Policies, or security state.

An Administrative Action SHALL remain distinguishable from the object or
state that it changes.

## 28.2 Administrative Authority

An Administrative Action SHALL be performed only where the applicable
administrative authority has been established.

Receipt of a command through an administrative interface SHALL NOT by
itself establish administrative authority.

## 28.3 Administrative Policy

Administrative processing SHALL comply with the applicable Policy and
Security Context.

## 28.4 Administrative Modification

Where an Administrative Action modifies a security-sensitive object,
dependent processing SHALL apply the applicable validation,
revalidation, invalidation, or termination requirements.

## 28.5 Revocation

An authorized Administrative Action MAY revoke:

- Authentication credentials
- Entitlements
- Policies
- Authorization Decisions
- Enforcement state
- Security Contexts
- Other defined objects

Revocation SHALL take effect according to the applicable protocol and
Policy.

## 28.6 Suspension

An authorized Administrative Action MAY suspend a security-sensitive
object.

A suspended object SHALL NOT be treated as active where active state is
required.

## 28.7 Administrative Override

Where an administrative override is supported, the protocol SHALL define
the authority, scope, conditions, and audit requirements applicable to
the override.

An override SHALL NOT silently eliminate required security controls
unless such behavior is explicitly defined by the applicable Policy.

## 28.8 Emergency Administrative Action

A protocol MAY define emergency administrative actions.

Emergency processing SHALL remain subject to defined authority and
security requirements.

## 28.9 Administrative Evidence and Audit

An implementation SHOULD generate Evidence for security-sensitive
Administrative Actions.

Audit information SHOULD record:

- Administrative actor or system
- Administrative Action
- Target object
- Previous state
- New state
- Timestamp
- Result
- Reason where defined

## 28.10 Administrative Conformance

An implementation SHALL NOT:

- Treat receipt of an administrative command as proof of authority.
- Ignore required administrative Policy.
- Modify a security-sensitive object without required validation.
- Treat an administrative override as unrestricted authorization.
- Bypass required dependency processing.
- Suppress required Evidence or Audit information.

# 29. Conformance Requirements

## 29.1 Conforming Implementation

A conforming implementation SHALL implement the normative requirements
applicable to the protocol functions, objects, and processing operations
that it supports.

## 29.2 Semantic Conformance

Conformance SHALL preserve the semantic distinction between:

- Authentication
- Entitlement
- Policy Evaluation
- Authorization Evaluation
- Authorization Decision
- Enforcement
- Service Execution
- Execution Result
- Evidence
- Audit
- Transaction
- State Transition
- Dependency
- Security Context
- Error
- Failure

## 29.3 Processing Order Conformance

Where the protocol defines required processing dependencies, an
implementation SHALL satisfy those dependencies before performing the
dependent operation.

An implementation SHALL NOT bypass a required processing stage merely
because another stage has completed successfully.

## 29.4 Result Conformance

A processing result SHALL remain distinguishable from the operation that
produced it.

A successful result from one processing stage SHALL NOT automatically
constitute successful completion of a subsequent stage.

## 29.5 State Conformance

A conforming implementation SHALL preserve the validity and lifecycle
requirements applicable to protocol object state.

Historical state SHALL remain distinguishable from current state.

## 29.6 Dependency Conformance

A conforming implementation SHALL evaluate required dependencies before
dependent processing.

Dependency existence SHALL NOT be interpreted as dependency validity.

## 29.7 Security Conformance

A conforming implementation SHALL apply the security and integrity
requirements applicable to the supported protocol operations.

Where a required security condition cannot be established, the
applicable Policy SHALL determine the resulting processing state.

## 29.8 Evidence and Audit Conformance

Where Evidence or Audit is required by the applicable protocol or Policy,
the implementation SHALL generate and protect the required information
according to the applicable requirements.

Evidence and Audit SHALL remain distinguishable from the protocol objects
and operations that they describe.

## 29.9 Fail-Closed Conformance

Where fail-closed behavior applies, an implementation SHALL NOT permit
unauthorized Service Execution when a required security, authorization,
dependency, validation, or integrity condition cannot be established.

## 29.10 Implementation Variability

An implementation MAY use different internal mechanisms, storage models,
processing components, or deployment architectures provided that the
resulting behavior conforms to the normative semantics of this
specification.

## 29.11 Conformance Verification

Conformance MAY be demonstrated through:

- Functional testing
- Security testing
- Interoperability testing
- Evidence inspection
- Audit inspection
- State transition testing
- Failure handling testing
- Other defined verification methods

## 29.12 Conformance Evidence

An implementation MAY generate Conformance Evidence describing:

- Processing stage
- Object Identifier
- Transaction Identifier
- Decision Identifier
- Validation result
- State
- Dependency state
- Security Context
- Processing component
- Result

Conformance Evidence SHALL remain distinguishable from the protocol
objects, processing operations, and Decisions that it describes.

## 29.13 Final Conformance Requirement

An implementation SHALL NOT claim conformance solely because it supports
the protocol interfaces or successfully exchanges protocol messages.

Conformance SHALL be determined by compliance with the applicable
normative semantics and processing requirements defined by this
specification.


# 30. Protocol Processing and Semantic Separation Requirements

## 30.1 Processing Layer Separation

A conforming implementation SHALL preserve the semantic separation
between the processing stages defined by this specification.

The processing stages SHALL remain distinguishable as applicable:

- Authentication
- Entitlement
- Policy Evaluation
- Authorization Evaluation
- Authorization Decision
- Enforcement
- Service Execution

An implementation SHALL NOT combine two or more processing stages in a
manner that changes the normative meaning of the resulting processing
state.

An implementation MAY implement multiple processing stages within the
same software component provided that the semantic requirements of each
stage remain preserved.

## 30.2 Processing Order

Where the applicable Service Profile requires multiple processing stages,
the implementation SHALL process those stages according to the dependency
and ordering requirements established by the applicable Policy.

An implementation SHALL NOT bypass a required processing stage solely
because a preceding stage produced a successful result.

In particular, successful Authentication SHALL NOT by itself establish
Authorization.

Successful Entitlement evaluation SHALL NOT by itself establish
Authorization.

Successful Policy Evaluation SHALL NOT by itself establish an
Authorization Decision unless the applicable protocol explicitly defines
such behavior.

## 30.3 Processing Preconditions

Each processing stage SHALL evaluate the inputs required by the applicable
protocol and Policy before producing a result.

Required inputs MAY include:

- Object state
- Object Version
- Security Context
- Transaction Context
- Dependency state
- Freshness information
- Validity information
- Applicable Policy
- Service Profile requirements
- Previous processing results

An implementation SHALL NOT assume that an input remains valid merely
because the input was previously validated.

## 30.4 Result Separation

Processing results SHALL remain distinguishable from the processing
operation that produced them.

The following objects SHALL remain semantically distinguishable:

- Authentication Result
- Entitlement Result
- Policy Evaluation Result
- Authorization Evaluation Result
- Authorization Decision
- Enforcement Result
- Execution Result

An implementation SHALL NOT treat the existence of one result object as
proof that another required result object exists.

## 30.5 Historical Results

A historical processing result MAY be retained for:

- Evidence
- Audit
- Compliance
- Investigation
- Recovery
- Dispute resolution

A historical result SHALL NOT automatically be treated as a current
result where current state is required.

Where the applicable Policy requires current validation, the implementation
SHALL establish current validity before dependent processing continues.

## 30.6 Revalidation

A processing stage MAY require revalidation when relevant state,
dependencies, security conditions, or Policy requirements change.

Revalidation MAY be triggered by:

- Expiration
- Revocation
- Suspension
- Version change
- Security Context change
- Dependency change
- Policy change
- Transaction state change
- Required freshness threshold
- Detected integrity failure
- Recovery processing

Where revalidation is required, dependent processing SHALL NOT continue
as though the previous validation remained unconditionally valid.

## 30.7 Fail-Closed Processing

Where the applicable Policy requires fail-closed behavior, inability to
establish a required security or authorization condition SHALL NOT result
in unauthorized Service Execution.

A fail-closed condition SHALL NOT be interpreted as a Permit Decision.

An implementation SHALL preserve the distinction between:

- Deny
- Indeterminate
- Revalidation Required
- Error
- Failure
- Recovery Required

unless the applicable Policy explicitly defines their processing
relationship.

## 30.8 Implementation Variability

An implementation MAY vary in:

- Component architecture
- Storage architecture
- Message transport
- Internal data structures
- Processing location
- Processing concurrency
- Caching strategy

provided that such implementation variability does not alter the
normative semantics of the protocol.

## 30.9 Semantic Preservation

An implementation SHALL preserve the semantic identity, state, validity,
scope, dependency, and security meaning of protocol objects when those
objects are transferred, cached, reconstructed, or processed by another
component.

An implementation SHALL NOT use implementation convenience as a reason
to eliminate a semantic distinction required by this specification.

## 30.10 Processing Conformance

A conforming implementation SHALL be capable of demonstrating that each
required processing stage was performed according to the applicable
normative requirements.

Such demonstration MAY be provided through:

- Protocol state
- Processing results
- Evidence
- Audit information
- Conformance testing
- Other defined verification mechanisms

---

# 31. State, Dependency, and Validity Consistency Requirements

## 31.1 Current State

Where current state is required, an implementation SHALL establish the
current state of the relevant object before performing dependent
processing.

An implementation SHALL NOT infer current validity solely from the
existence of an object identifier.

## 31.2 State and Existence

The existence of a protocol object SHALL remain distinguishable from the
state of that object.

An object MAY exist while being:

- Inactive
- Suspended
- Revoked
- Expired
- Invalid
- Pending
- Terminated

An implementation SHALL NOT treat object existence as proof of active
validity.

## 31.3 State Transition Validity

A State Transition SHALL be valid only when the transition is permitted by
the applicable protocol and Policy.

An implementation SHALL validate required:

- Previous state
- New state
- Trigger
- Authority
- Dependencies
- Security Context
- Version
- Preconditions

before applying a security-sensitive State Transition.

## 31.4 Dependency Validity

A dependent object SHALL NOT be treated as valid solely because its own
internal state appears valid.

Where an object's validity depends on another object, the applicable
dependency SHALL be established before dependent processing proceeds.

## 31.5 Dependency Invalidation

Where a dependency becomes invalid, revoked, expired, suspended,
inconsistent, or otherwise unacceptable, dependent processing SHALL
follow the applicable Policy.

The implementation MAY:

- Revalidate the dependency
- Revalidate the dependent object
- Invalidate the dependent object
- Suspend dependent processing
- Terminate dependent processing
- Produce an applicable Error or Failure

## 31.6 Dependency Propagation

Where Policy requires dependency invalidation to propagate, the
implementation SHALL apply that propagation consistently to all affected
objects and processing stages.

Dependency propagation SHALL NOT silently create an Authorization
Decision.

## 31.7 Dependency Chain

Where multiple dependencies form a dependency chain, the validity of the
chain SHALL be established according to the applicable Policy before
dependent processing is permitted.

A valid leaf object SHALL NOT by itself establish validity of the complete
dependency chain.

## 31.8 Dependency Revalidation

A dependency change MAY require revalidation of affected:

- Entitlements
- Policy Evaluations
- Authorization Evaluations
- Authorization Decisions
- Enforcement operations
- Service Executions
- Transactions

Where revalidation is required, dependent processing SHALL follow the
applicable revalidation requirements.

## 31.9 Distributed Consistency

Where the same protocol object is processed by multiple components, those
components SHALL preserve the object's identifier, state, Version, and
applicable security meaning.

An implementation SHALL NOT create conflicting interpretations of a
security-sensitive object without applying the applicable consistency
rules.

## 31.10 Consistency Failure

Where required state or dependency consistency cannot be established,
the implementation SHALL follow the applicable Policy.

Where fail-closed behavior applies, consistency failure SHALL NOT result
in unauthorized Service Execution.

---

# 32. Security Context and Processing Context Requirements

## 32.1 Security Context

A Security Context MAY be associated with one or more processing stages.

A Security Context MAY include information describing:

- Authentication state
- Assurance level
- Credential state
- Device or execution environment
- Relevant security attributes
- Transaction context
- Applicable security conditions
- Freshness information

## 32.2 Security Context Validity

A Security Context SHALL be considered valid only when its required
attributes and state satisfy the applicable Policy.

An implementation SHALL NOT treat the existence of a Security Context as
proof that all security requirements are satisfied.

## 32.3 Security Context Change

A change to a security-sensitive Security Context MAY require
revalidation of dependent processing.

Such change MAY include:

- Authentication state change
- Credential state change
- Assurance change
- Device state change
- Security attribute change
- Policy-required freshness change

## 32.4 Security Context Propagation

Where a Security Context is propagated between components, the receiving
component SHALL preserve the security semantics required by the
applicable protocol.

A receiving component SHALL NOT assume that a propagated Security Context
is current solely because it was received from another component.

## 32.5 Security Context Dependency

Where an Authorization Decision, Enforcement operation, or Service
Execution depends on a Security Context, the applicable Security Context
requirements SHALL be satisfied before the dependent operation proceeds.

## 32.6 Processing Context

A Processing Context MAY associate related protocol objects and
operations.

A Processing Context MAY include:

- Transaction Identifier
- Object Identifier
- Processing stage
- Security Context Identifier
- Policy Identifier
- Decision Identifier
- Execution Identifier
- Version information
- Dependency information

## 32.7 Processing Context Integrity

Processing Context information SHALL remain integrity-protected to the
extent required by the applicable Policy.

An implementation SHALL NOT permit unauthorized modification of
security-sensitive Processing Context information.

## 32.8 Processing Context Freshness

Where freshness is required, the implementation SHALL validate the
freshness of the Processing Context before dependent processing proceeds.

Stale Processing Context SHALL NOT automatically be treated as current.

## 32.9 Context Correlation

Correlation identifiers MAY be used to associate related processing
operations.

Correlation SHALL NOT eliminate the semantic distinction between the
objects or operations being correlated.

## 32.10 Context Failure

Where required Security Context or Processing Context information cannot
be established, the applicable Policy SHALL determine the resulting
processing state.

Where fail-closed behavior applies, inability to establish required
context SHALL NOT result in unauthorized Service Execution.

---

# 33. Transaction and Session Processing Requirements

## 33.1 Transaction Association

A Transaction MAY associate multiple protocol processing stages and
objects.

A Transaction MAY identify:

- Authentication
- Entitlement
- Policy Evaluation
- Authorization Evaluation
- Authorization Decision
- Enforcement
- Service Execution
- Execution Result
- Evidence
- Audit
- Dependencies

Transaction correlation SHALL NOT eliminate the semantic distinction
between the Transaction and the objects associated with it.

## 33.2 Transaction State

A Transaction SHALL have a distinguishable state where transaction state
is required by the applicable protocol.

Transaction state MAY include:

- Created
- Pending
- Processing
- Suspended
- Failed
- Cancelled
- Terminated
- Completed

The applicable protocol MAY define additional states.

## 33.3 Transaction State Validation

An implementation SHALL validate the current Transaction state before
performing a state-dependent operation.

An implementation SHALL NOT assume that an active Transaction remains
valid indefinitely.

## 33.4 Transaction Continuity

Transaction continuity MAY allow related processing to remain associated
with the same Transaction.

Transaction continuity SHALL NOT automatically establish:

- Current Authentication validity
- Current Entitlement validity
- Current Policy validity
- Current Authorization validity
- Current Enforcement validity

## 33.5 Session Continuity

A Session MAY provide continuity across multiple protocol operations.

Session continuity SHALL remain distinguishable from Authentication.

An active Session SHALL NOT automatically constitute permanent
Authentication validity.

## 33.6 Session Termination

Where a Session is terminated, dependent processing SHALL follow the
applicable Policy.

Termination MAY require:

- Re-authentication
- Entitlement re-evaluation
- Policy re-evaluation
- Authorization re-evaluation
- Enforcement termination
- Service Execution termination

## 33.7 Transaction Suspension

A Transaction MAY be suspended where required conditions cannot currently
be satisfied.

Suspension SHALL NOT be treated as successful authorization.

Upon resumption, the implementation SHALL apply any required
revalidation before continuing dependent processing.

## 33.8 Transaction Cancellation

A Transaction MAY be cancelled according to the applicable protocol and
Policy.

Cancellation SHALL produce the applicable state and processing result.

Cancellation SHALL NOT be treated as successful Service Execution.

## 33.9 Transaction Termination

A terminated Transaction SHALL NOT automatically authorize subsequent
processing.

Where processing is resumed through a new Transaction, the applicable
protocol requirements SHALL apply to the new Transaction.

## 33.10 Transaction Completion

A completed Transaction SHALL remain distinguishable from:

- Authorization Decision
- Enforcement
- Service Execution
- Execution Result
- Evidence
- Audit

Transaction completion SHALL NOT by itself establish business success.

## 33.11 Transaction Evidence and Audit

A Transaction MAY generate Evidence describing:

- Transaction creation
- Transaction state
- State transitions
- Correlated objects
- Processing stages
- Authorization Decisions
- Enforcement operations
- Service Execution
- Termination
- Completion

Audit information MAY record the processing history of the Transaction.

Evidence and Audit SHALL remain distinguishable from the Transaction
itself.

## 33.12 Transaction Conformance

A conforming implementation SHALL preserve Transaction identity,
lifecycle state, dependency relationships, and correlation semantics.

An implementation SHALL NOT:

- Treat Transaction existence as authorization.
- Treat Transaction continuity as permanent Authentication validity.
- Treat historical Transaction state as current state.
- Reuse an unrelated Transaction Identifier as the current Transaction.
- Bypass required validation because a Transaction remains active.
- Treat Transaction completion as proof of business success.

Where required Transaction validity cannot be established, the applicable
Policy SHALL determine the resulting processing state.

---

# 34. Error, Failure, Recovery, and Exceptional Processing Requirements

## 34.1 Error Definition

An Error SHALL represent an unsuccessful or exceptional processing
condition as defined by the applicable protocol.

An Error SHALL remain distinguishable from:

- Deny
- Indeterminate
- Failure
- Recovery
- Authorization Decision
- Execution Result

## 34.2 Error Classification

An implementation MAY classify Errors according to:

- Input validation
- Authentication
- Entitlement
- Policy
- Authorization
- Enforcement
- Service Execution
- Integrity
- Dependency
- Version
- Interoperability
- Administrative processing
- Internal processing

The classification SHALL NOT alter the normative meaning of the Error
unless defined by the applicable Policy.

## 34.3 Error Processing

An Error SHALL be processed according to the applicable protocol and
Policy.

An implementation SHALL NOT silently ignore an Error that affects a
required security or authorization condition.

## 34.4 Error Propagation

Where an Error affects dependent processing, the implementation SHALL
apply the applicable propagation requirements.

Error propagation SHALL NOT automatically create an Authorization
Decision.

## 34.5 Failure

A Failure SHALL remain distinguishable from an Error.

A Failure MAY indicate that a required processing operation could not
complete successfully.

Failure MAY result from:

- Validation failure
- Dependency failure
- Security failure
- Processing failure
- Enforcement failure
- Service Execution failure
- Recovery failure

## 34.6 Fail-Closed Exceptional Processing

Where fail-closed behavior applies, an Error or Failure affecting a
required security condition SHALL NOT result in unauthorized Service
Execution.

An implementation SHALL NOT convert an unresolved Error or Failure into a
Permit Decision solely to continue processing.

## 34.7 Recovery Processing

A protocol implementation MAY enter Recovery processing following an
Error or Failure.

Recovery MAY include:

- Retry
- Revalidation
- Re-authentication
- Dependency reconstruction
- State reconstruction
- Transaction restart
- Termination

Recovery SHALL follow the applicable Policy.

## 34.8 Retry Processing

A retry MAY be permitted where the applicable Policy allows retry.

A retry SHALL NOT bypass requirements that applied to the original
processing operation.

Where the relevant security state may have changed, revalidation SHALL
be performed before retrying dependent processing.

## 34.9 Recovery and Authorization

Recovery SHALL NOT by itself establish Authorization.

Where recovery causes required inputs or security conditions to change,
the applicable Authorization Evaluation and Authorization Decision
requirements SHALL apply.

## 34.10 Recovery and Service Execution

Recovery SHALL NOT silently continue Service Execution when the
conditions required for continued execution are no longer satisfied.

Where fail-closed behavior applies, unsuccessful recovery SHALL NOT result
in unauthorized Service Execution.

## 34.11 Exceptional Processing

Exceptional processing SHALL preserve the semantic distinction between
the normal processing stages and the exceptional state.

An implementation SHALL NOT treat an Error, Failure, or Recovery state as
though it were a successful processing result.

## 34.12 Exceptional State Evidence

An implementation MAY generate Evidence describing:

- Error
- Failure
- Recovery
- Retry
- Revalidation
- Processing state
- Transaction state
- Security Context
- Result

Audit information MAY record exceptional processing.

Evidence and Audit SHALL remain distinguishable from the Error, Failure,
Recovery operation, and affected protocol objects.

## 34.13 Exceptional Processing Conformance

A conforming implementation SHALL handle Errors, Failures, and Recovery
according to the applicable normative requirements.

An implementation SHALL NOT:

- Treat an Error as Authorization.
- Treat a Failure as Authorization.
- Treat Recovery as successful Authorization.
- Bypass required revalidation during Recovery.
- Ignore required security failures.
- Continue unauthorized Service Execution following an applicable
  fail-closed condition.

Where the applicable Policy cannot establish a permitted recovery path,
the implementation SHALL follow the defined failure behavior.


# 35. Cross-Layer Processing Requirements

## 35.1 Cross-Layer Processing Model

A conforming implementation SHALL preserve the defined processing
relationship among:

- Authentication
- Entitlement
- Policy Evaluation
- Authorization Evaluation
- Authorization Decision
- Enforcement
- Service Execution

The processing layers MAY be implemented by separate components.

The separation of components SHALL NOT eliminate the semantic distinction
between the processing layers.

## 35.2 Processing Order

Where the applicable Policy defines an ordering requirement, an
implementation SHALL perform the required processing stages in the
specified order.

An implementation SHALL NOT treat a later processing stage as having
implicitly completed an earlier required stage.

## 35.3 Authentication Dependency

Where Authentication is required, dependent processing SHALL NOT proceed
as though Authentication were valid unless the applicable Authentication
requirements have been satisfied.

Authentication success SHALL NOT by itself establish:

- Entitlement
- Policy applicability
- Authorization
- Enforcement permission
- Service Execution permission

## 35.4 Entitlement Dependency

Where Entitlement is required, dependent Authorization processing SHALL
use an Entitlement whose validity, scope, condition, and state satisfy the
applicable requirements.

Entitlement possession SHALL NOT by itself establish Authorization.

## 35.5 Policy Dependency

Where Policy Evaluation is required, Authorization processing SHALL
respect the applicable Policy Evaluation result.

An implementation SHALL NOT bypass required Policy Evaluation by directly
creating an Authorization Decision.

## 35.6 Authorization Dependency

Where Authorization Evaluation is required, Enforcement SHALL use the
applicable Authorization Evaluation and Authorization Decision semantics.

An implementation SHALL NOT treat a Permit result as permission to
bypass required Enforcement controls.

## 35.7 Enforcement Dependency

Where Enforcement is required, Service Execution SHALL NOT be considered
authorized solely because an Authorization Decision exists.

The applicable Enforcement requirements SHALL be satisfied before
Service Execution is permitted.

## 35.8 Cross-Layer Failure

Failure of a required processing layer SHALL cause dependent processing to
follow the applicable Policy and failure-handling requirements.

A downstream layer SHALL NOT silently reinterpret an upstream failure as
successful processing.

## 35.9 Cross-Layer State

State changes in one processing layer MAY invalidate assumptions made by
another processing layer.

Where such invalidation is defined, dependent processing SHALL perform
the required validation or revalidation.

## 35.10 Cross-Layer Conformance

A conforming implementation SHALL preserve the semantic boundaries among
all defined processing layers.

An implementation SHALL NOT collapse multiple security decisions into a
single result where doing so would change the normative meaning of the
defined processing model.

# 36. Decision Boundary and Permission Semantics Requirements

## 36.1 Decision Boundary

An Authorization Decision SHALL represent a defined authorization
outcome within its applicable scope.

The existence of a Decision SHALL NOT be interpreted as permission
outside that scope.

## 36.2 Permission Scope

A Permit Decision SHALL identify or be associated with the conditions,
scope, and dependencies under which the permission applies.

An implementation SHALL NOT extend a Permit Decision beyond its defined
scope.

## 36.3 Conditional Permission

Where a Decision is conditional, all required conditions SHALL be
satisfied before the associated operation is permitted.

Failure to satisfy a required condition SHALL NOT be interpreted as
unconditional permission.

## 36.4 Obligations

Where an Authorization Decision contains or references obligations,
Enforcement SHALL apply the obligations before or during the associated
Service Execution as required by the applicable Policy.

## 36.5 Restrictions

Where a Decision contains restrictions, dependent Enforcement SHALL
respect those restrictions.

An implementation SHALL NOT remove or weaken a Decision restriction
without an applicable authorized operation permitting that change.

## 36.6 Decision Expiration

A Decision SHALL NOT be treated as valid after its defined expiration
unless the applicable protocol explicitly permits renewal or
revalidation.

## 36.7 Decision Revocation

A revoked Decision SHALL NOT be used to authorize dependent processing.

Revocation processing SHALL apply to dependent Enforcement and Service
Execution where required by the applicable Policy.

## 36.8 Decision Suspension

A suspended Decision SHALL NOT be treated as an active Permit unless the
applicable Policy explicitly permits such use.

## 36.9 Decision Invalidation

Where a Decision becomes invalid because of a change in a required
dependency, the Decision SHALL NOT continue to authorize dependent
processing.

## 36.10 Historical Decision

Historical Decision information MAY be retained for:

- Evidence
- Audit
- Compliance
- Investigation
- Dispute resolution

Historical Decision information SHALL NOT automatically be treated as a
current authorization state.

# 37. Processing Continuity and State Change Requirements

## 37.1 Continuity

Session or Transaction continuity MAY preserve processing context.

Continuity SHALL NOT by itself establish that all previously satisfied
security requirements remain satisfied.

## 37.2 State Reassessment

Where a required security object changes state, dependent processing
SHALL reassess the affected requirements where required by the applicable
Policy.

## 37.3 Authentication State Change

A change in Authentication validity, assurance, credential state, or
Security Context MAY require re-authentication or revalidation.

Dependent processing SHALL follow the applicable requirements.

## 37.4 Entitlement State Change

A change in Entitlement validity, scope, condition, consumption state,
suspension, revocation, or expiration MAY require Authorization
re-evaluation.

Where such re-evaluation is required, dependent processing SHALL NOT
continue solely because the Transaction remains active.

## 37.5 Policy State Change

A Policy change MAY affect:

- Policy applicability
- Authorization Evaluation
- Authorization Decisions
- Enforcement
- Service Execution

Where required, affected processing SHALL be revalidated according to
the applicable Policy.

## 37.6 Authorization State Change

A change in Authorization Decision state SHALL be propagated to dependent
Enforcement where required.

A previously issued Permit SHALL NOT automatically remain effective after
the Decision has become invalid, revoked, suspended, or expired.

## 37.7 Enforcement State Change

An Enforcement state change MAY suspend, terminate, or otherwise affect
Service Execution.

Service Execution SHALL follow the applicable Enforcement state.

## 37.8 Dependency State Change

A change in a required dependency MAY invalidate dependent objects or
processing results.

The implementation SHALL apply the applicable dependency and
revalidation requirements.

## 37.9 State Change During Execution

Where a security-relevant state changes during Service Execution, the
applicable Policy SHALL determine whether execution:

- Continues
- Is revalidated
- Is suspended
- Is terminated
- Produces an exceptional result

## 37.10 Continuity Conformance

An implementation SHALL NOT use continuity as a mechanism to bypass
required current-state validation.

# 38. Processing Result Semantics Requirements

## 38.1 Result Distinction

The following SHALL remain semantically distinguishable:

- Evaluation Result
- Authorization Decision
- Enforcement Result
- Execution Result
- Error
- Failure
- Recovery Result

## 38.2 Evaluation Result

An Evaluation Result SHALL describe the outcome of the applicable
evaluation operation.

An Evaluation Result SHALL NOT automatically be treated as successful
Enforcement or Service Execution.

## 38.3 Authorization Decision Result

An Authorization Decision SHALL represent the defined authorization
outcome.

It SHALL NOT be treated as evidence that Enforcement or Service
Execution has successfully completed.

## 38.4 Enforcement Result

An Enforcement Result SHALL describe the outcome of the applicable
Enforcement operation.

A successful Enforcement Result SHALL NOT automatically establish that
the Service Execution itself was successful.

## 38.5 Execution Result

An Execution Result SHALL describe the outcome of Service Execution.

An Execution Result SHALL NOT retroactively establish that the preceding
Authorization Decision was valid.

## 38.6 Error Result

An Error SHALL describe an error condition according to the applicable
error classification.

An Error SHALL NOT be interpreted as a Permit Decision unless the
applicable protocol explicitly defines such semantics.

## 38.7 Failure Result

A Failure SHALL describe unsuccessful or prevented processing.

A Failure SHALL NOT be interpreted as successful Authorization,
Enforcement, or Service Execution.

## 38.8 Recovery Result

A Recovery Result SHALL describe the outcome of recovery processing.

Successful recovery SHALL NOT automatically restore previously valid
security state where revalidation is required.

## 38.9 Result Correlation

Results MAY reference the objects and operations to which they relate.

Correlation SHALL NOT eliminate the semantic distinction between a Result
and the object or operation that produced it.

## 38.10 Result Conformance

A conforming implementation SHALL preserve the defined semantics of each
Result type.

# 39. Security Decision Integrity Requirements

## 39.1 Decision Integrity

Authorization Decisions SHALL be protected against unauthorized
modification.

Where integrity protection is required, a modified Decision SHALL NOT be
accepted as an authentic Decision.

## 39.2 Decision Authenticity

Where the protocol requires identification of the component that creates
or validates a Decision, the implementation SHALL verify the required
authenticity information.

## 39.3 Decision Binding Integrity

Where a Decision is bound to an Entitlement, Policy, Security Context,
Transaction, Service Profile, or other dependency, the binding SHALL be
protected against unauthorized alteration.

## 39.4 Decision Replay

A Decision SHALL NOT be reused outside its defined validity, scope,
freshness, or consumption requirements.

## 39.5 Enforcement Integrity

Enforcement SHALL verify the integrity of the Decision and required
inputs before relying upon them where such validation is required.

## 39.6 Execution Authorization Integrity

Service Execution SHALL rely only on authorization information that
satisfies the applicable integrity and validity requirements.

## 39.7 State Integrity

Security-relevant state SHALL be protected against unauthorized
modification where the applicable Policy requires such protection.

## 39.8 Cross-Component Integrity

Where security objects cross component boundaries, the receiving
component SHALL apply the required integrity and validity checks before
using those objects.

## 39.9 Integrity Failure

Where a required integrity check fails, dependent processing SHALL
follow the applicable failure Policy.

Where fail-closed behavior applies, integrity failure SHALL NOT result in
unauthorized Service Execution.

## 39.10 Security Integrity Conformance

A conforming implementation SHALL preserve the integrity and semantic
identity of security-relevant protocol objects throughout processing.

# 40. Final Normative Processing Requirements

## 40.1 Normative Applicability

A conforming implementation SHALL apply the normative requirements that
are applicable to the protocol operation, object, Policy, Service Profile,
Security Context, and processing state.

## 40.2 Required Validation

An implementation SHALL perform all validation required before a
dependent operation is permitted.

## 40.3 Required Separation

An implementation SHALL preserve the semantic distinction between:

- Authentication
- Entitlement
- Policy Evaluation
- Authorization Evaluation
- Authorization Decision
- Enforcement
- Service Execution
- Execution Result
- Evidence
- Audit
- Error
- Failure
- Recovery

## 40.4 No Implicit Authorization

An implementation SHALL NOT infer Authorization solely from:

- Authentication success
- Entitlement possession
- Policy selection
- Policy Evaluation success
- Service Profile selection
- Capability identification
- Session continuity
- Transaction continuity
- Interface availability
- Message delivery
- Evidence generation
- Audit generation

## 40.5 No Implicit Execution

An Authorization Decision SHALL NOT be treated as proof that Service
Execution has occurred or succeeded.

Enforcement SHALL remain a distinct processing stage where required.

## 40.6 Current-State Requirement

Historical information SHALL NOT automatically be treated as current
security state.

Where current state is required, the implementation SHALL establish
current validity according to the applicable requirements.

## 40.7 Dependency Requirement

Where a required dependency cannot be established as valid, dependent
processing SHALL follow the applicable Policy.

## 40.8 Fail-Closed Requirement

Where fail-closed behavior applies, inability to establish a required
security condition SHALL NOT result in unauthorized Service Execution.

## 40.9 Semantic Preservation

An implementation SHALL NOT combine, rename, substitute, or reinterpret
protocol objects or processing stages in a manner that changes their
normative security meaning.

## 40.10 Conformance

A conforming implementation SHALL satisfy the applicable normative
requirements of this specification.

Successful message exchange, interface compatibility, or implementation
of individual protocol functions SHALL NOT by itself establish
conformance.

## 40.11 Normative Completeness

The normative processing model SHALL be interpreted as an integrated
system.

Individual requirements SHALL NOT be interpreted in isolation where
doing so would contradict another applicable normative requirement.

## 40.12 Final Security Condition

Where the applicable Policy requires Authentication, Entitlement, Policy
Evaluation, Authorization Evaluation, Authorization Decision,
Enforcement, and Service Execution controls, all required conditions
SHALL be satisfied before the protected Service Execution is permitted.

Where any mandatory condition cannot be established, the implementation
SHALL follow the applicable denial, indeterminate, revalidation,
termination, recovery, or fail-closed behavior.
