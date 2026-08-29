# NEW-shot2play Technical Specification Version 2.0
# Invention Summary

## 1. Purpose

本書は、NEW-shot2play Technical Specification Version 2.0における発明の核心、
技術的特徴、保護範囲の考え方を、弁理士との共有を目的として整理した内部資料である。

出願用の「要約書」そのものではなく、請求項・明細書・先行技術評価を横断して
発明の中心思想を確認するための基準文書として使用する。

---

## 2. Invention in One Sentence

本発明は、Authenticationによって確認されたAuthentication Resultと、
当該Authenticationとは独立して管理され得るEntitlementとを分離して扱い、
それらをPolicy Evaluation等の入力としてAuthorization Decisionを生成し、
Enforcementを介してService Executionを制御するとともに、
Authentication ObjectのValidityとEntitlementのValidityを独立して管理する
情報処理技術である。

---

## 3. Core Processing Model

基本的な処理連鎖は以下である。

Authentication
↓
Authentication Result
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

各処理は単一装置・単一サーバに限定されず、複数のService、API、Server、
Database、端末等に分散して実装できる。

---

## 4. Core Technical Concepts

### 4.1 Authentication Result と Entitlement の分離

Authentication Resultは、Authenticationによって確認された結果を示す情報である。

Entitlementは、主体、対象、Service、Resource、Transactionその他について、
所定の権利、資格、状態または事実が成立していることを示す情報である。

両者は同一Objectとして扱う必要がなく、EntitlementはAuthenticationとは
異なる時点、処理、ServiceまたはSystemで生成・取得・更新・停止・失効等を
行うことができる。

### 4.2 Authentication Object Validity と Entitlement Validity の独立性

Authenticationに使用されるAuthentication Objectは短時間だけ有効とし、
Authenticationによって確認された事実に基づくEntitlementは、より長い期間
有効とすることができる。

例えばAuthentication Objectを30秒有効とし、生成されたEntitlementを数時間
有効とする構成が可能である。

30秒という数値自体は本発明の核心ではなく、短時間Authentication Objectと
長時間Entitlementの独立したLifecycleを説明する具体例である。

### 4.3 Policy Evaluation

Policy Evaluationには、Authentication Result、Entitlementだけでなく、

- Subject
- Resource
- Action
- Security Context
- Transaction
- Object State
- 時間情報
- その他Context

を入力として利用できる。

### 4.4 Authorization Decision

Authorization Decisionは、Policy Evaluation / Authorization Evaluation等の
結果として生成されるDecisionであり、例えばPermit、Deny、Indeterminateを示す。

Policyそのもの、Policy Evaluationそのもの、Enforcementそのものとは区別する。

### 4.5 Enforcement

EnforcementはAuthorization Decisionを実際のService Executionに適用する
制御境界である。

Authorization DecisionとService Executionを同一処理として扱うのではなく、
その間にEnforcementを設けることが本構成の重要な特徴の一つである。

---

## 5. Important Fallback Structures

### 5.1 Conditional Entitlement

所定のAuthentication、場所、取引、Service利用、時間、Object State等の
条件が成立した場合にEntitlementを生成・付与・有効化できる。

### 5.2 Cross-Service Entitlement

あるServiceで生成または確認されたEntitlementを、別のServiceにおける
Authorizationの条件として利用できる。

代表例：

店舗Service
→ Authentication
→ 「来店済み」Entitlement
→ EC Service
→ Policy Evaluation
→ 割引Authorization Decision
→ Enforcement
→ Service Execution

### 5.3 Security Context

主体、装置、通信、Session、Transaction、認証状態、Entitlement状態、
Object Stateその他のContextをPolicy Evaluation / Authorization Evaluationに
利用できる。

### 5.4 Transaction

Authentication、Entitlement、Authorization、Service Execution等に関連する
一連のProtocol Processing / Service Processingを関連付ける単位として扱う。

### 5.5 Object State

ObjectはCreated、Active、Suspended、Expired、Revoked、Consumed、Completed、
Failed等のStateを持つことができる。

Object StateはAuthorization Decisionそのものではなく、Policyに基づく
Authorization判断の入力情報として扱うことができる。

---

## 6. Authentication Mechanisms

本発明は特定のAuthentication方式そのものを中心としない。

実装例として、

- Password
- One-time Password
- QR Code
- Public-key cryptography
- WebAuthn
- Passkey
- Biometrics
- Device Authentication
- Certificate

等を利用できる。

QR Code、30秒、WebAuthn、Secure Enclave等は、独立請求項を不必要に
限定するための要件ではなく、従属請求項・実施形態としてFallbackを形成する。

---

## 7. Principal Technical Distinction

本発明で重視するのは、単なるAuthentication、単なるAccess Control、
単なるEntitlement Management、単なるPolicy Engineのいずれかではない。

中心となる技術的構造は、

**Authentication Result**
と
**Entitlement**

を独立した情報として扱い、

**Authentication Object Validity**
と
**Entitlement Validity**

を独立した時間的Lifecycleとして管理し、

その上で、

**Policy Evaluation**
→ **Authorization Decision**
→ **Enforcement**
→ **Service Execution**

という明確な処理境界を形成する点にある。

---

## 8. Representative Use Case

利用者が店舗を訪れた際にAuthenticationを行う。

Authentication Objectとして短時間有効なQR Code等を利用できる。

Authenticationが成功するとAuthentication Resultを取得し、その結果に基づいて
「来店済み」というEntitlementを生成または有効化する。

そのEntitlementはAuthentication Objectとは独立して所定期間保持できる。

後に利用者がEC Serviceを利用した際、EC Serviceは当該Entitlement、Subject、
Resource、Action、Security Context、Transaction、Object State等を用いて
Policy Evaluationを実行する。

その結果としてAuthorization Decisionを生成し、Enforcementを介して
割引等のService Executionを制御する。

---

## 9. Scope Strategy

独立請求項では、特定のAuthentication mechanism、QR Code、30秒等に
不必要に限定しない。

従属請求項では、

- Validity
- Expiration
- QR Code
- One-time Use
- Conditional Entitlement
- Cross-Service Entitlement
- Freshness
- Object State
- Security Context
- Transaction
- Public-key Authentication
- WebAuthn / Passkey
- Secure Enclave / Trusted Execution Environment

等をFallbackとして保持する。

---

## 10. Attorney Review Points

弁理士レビューでは、特に以下を確認する。

1. Authentication ResultとEntitlementの独立性がClaim 1の中心として明確か。
2. Authentication Object ValidityとEntitlement Validityの独立性が十分に記載されているか。
3. Policy Evaluation、Authorization Evaluation、Authorization Decisionの関係が明確か。
4. Authorization DecisionとEnforcementの分離が明確か。
5. EnforcementとService Executionの分離が明確か。
6. Conditional EntitlementをFallbackとして維持できるか。
7. Cross-Service EntitlementをFallbackとして維持できるか。
8. Security Context、Transaction、Object Stateを追加的なPolicy入力として維持できるか。
9. QR Code、30秒、WebAuthn等が独立請求項を不必要に限定していないか。
10. 先行技術との関係でClaim 1の構成要件の組合せおよび相互関係が十分に説明されているか。

---

## 11. Source Position

本Summaryは、Version 2.0の `specification_v2.md`、`claims_v2.md` および
既存の発明核心資料を基礎として整理した内部資料である。
