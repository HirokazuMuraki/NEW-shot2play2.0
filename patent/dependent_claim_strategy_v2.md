
# NEW-shot2play Technical Specification Version 2.0

# Dependent Claim Strategy

## Phase 4 — 従属請求項展開・Claim Dependency Strategy

## 1. 本文書の目的

本書は、NEW-shot2play Technical Specification Version 2.0に基づく特許出願において、Phase 3で作成した請求項案を基礎として、従属請求項の構成、請求項間の従属関係、Fallback Structureおよび各技術的特徴の配置を整理することを目的とする。

本書は最終的な特許請求の範囲そのものではなく、特許請求項を最終確定するための設計文書として位置付ける。

本発明の中心的な処理構造は、以下のとおりである。

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

ただし、Authentication ResultとEntitlementは同一の情報である必要はなく、Authenticationに使用されるAuthentication ObjectのValidityとEntitlementのValidityも同一である必要はない。

---

## 2. Phase 4の基本方針

Phase 4では、単純に技術的特徴を一つずつ従属請求項に追加するのではなく、先行技術との関係において複数のFallbackを形成できるように請求項を構成する。

特に以下を重視する。

1. 発明の中心構造を複数の請求項から保護できるようにする。
2. Claim 1を過度に限定しない。
3. Authentication方式を特定方式に固定しない。
4. QR Codeを具体的実施形態として保持する。
5. Authentication Objectの短時間Validityを独立した技術的特徴として保持する。
6. EntitlementのValidityをAuthentication ObjectのValidityから分離する。
7. Conditional Entitlementを独立したFallbackとして保持する。
8. Cross-Service Entitlementを独立したFallbackとして保持する。
9. Policy EvaluationとAuthorization Decisionを混同しない。
10. Authorization DecisionとEnforcementを混同しない。
11. EnforcementとService Executionを混同しない。
12. Revocation、RevalidationおよびFail-ClosedをSecurity Fallbackとして保持する。
13. System、Method、ProgramおよびRecording Mediumの各請求項群を維持する。

---

## 3. Claim Architectureの基本構造

本発明の請求項構造は、概念上、以下の階層とする。

### Core Layer

* Authentication Result
* Entitlement
* Policy Evaluation
* Authorization Decision
* Enforcement
* Service Execution

### Authentication Layer

* Authentication Object
* Validity Period
* Expiration
* Freshness
* Replay Prevention
* QR Code
* Public Key Cryptography
* WebAuthn

### Entitlement Layer

* Entitlement
* Entitlement Validity
* Entitlement Expiration
* Entitlement State
* Conditional Entitlement
* Cross-Service Entitlement
* Revocation

### Authorization Layer

* Policy
* Policy Evaluation
* Authorization Evaluation
* Authorization Decision
* Permit
* Deny
* Indeterminate
* Revalidation
* Fail-Closed

### Context and State Layer

* Security Context
* Transaction
* Object State
* State Transition
* Evidence
* Audit

---

## 4. 独立請求項群

Phase 3で設計した独立請求項候補は、以下の4系統を基本とする。

### Independent Claim 1 — 情報処理システム

情報処理システムとして、本発明の中心的な処理構造を保護する。

Claim 1では、少なくとも以下を中心とする。

* Authentication Resultを取得または生成する処理
* Authentication Resultとは独立して管理され得るEntitlementを取得、生成または確認する処理
* Authentication ResultおよびEntitlementを含む情報に基づくPolicy Evaluation
* Policy Evaluationの結果に基づくAuthorization Decision
* Authorization Decisionに基づくEnforcement
* Enforcementを介したService Executionの制御

### Independent Claim 2 — 情報処理方法

Claim 1と実質的に対応する処理を、情報処理方法として保護する。

### Independent Claim 3 — プログラム

Claim 1またはClaim 2に対応する処理をコンピュータに実行させるプログラムとして保護する。

### Independent Claim 4 — 記録媒体

当該プログラムを記録したコンピュータ読み取り可能な記録媒体として保護する。

---

## 5. Claim 1の中心構成

Claim 1では、以下の関係を最重要構成として維持する。

Authentication Result
+
Entitlement
↓
Policy Evaluation
↓
Authorization Decision
↓
Enforcement
↓
Service Execution

特に、

Authentication Result = Entitlement

としないことが重要である。

Authentication Resultは「Authenticationによって確認された結果」を示す情報であり、Entitlementは「主体、対象、Service、Resource、Transactionその他について成立する権利または資格」を示す情報として扱う。

---

## 6. Claim 1におけるAuthenticationの限定

Claim 1では、Authentication mechanismを特定の方式に限定しない。

Authenticationは、例えば以下を含み得る。

* Password
* One-Time Password
* Public Key Cryptography
* WebAuthn
* Passkey
* QR Code
* Device-based Authentication
* Biometric Authentication
* Multi-Factor Authentication
* その他のAuthentication mechanism

これらはClaim 1の必須限定とはしない。

---

## 7. Claim 1におけるEntitlement

Claim 1では、Authentication Resultとは独立して管理可能なEntitlementを中心的な限定とする。

Entitlementは、例えば、

* Subject
* Resource
* Service
* Action
* Transaction
* Benefit
* Privilege
* Qualification
* Access Right
* Usage Right

その他の対象について成立する権利または資格を示す情報とすることができる。

---

## 8. Claim 1におけるPolicy Evaluation

Policy Evaluationは、Authentication Result、Entitlementおよびその他のContext情報をPolicyに照らして評価する処理として構成する。

Policy EvaluationそのものをAuthorization Decisionと同一視しない。

例えば、

Authentication Result
+
Entitlement
+
Subject
+
Resource
+
Action
+
Security Context
+
Transaction

をPolicy Evaluationへの入力とすることができる。

---

## 9. Claim 1におけるAuthorization Decision

Authorization Decisionは、Policy EvaluationまたはAuthorization Evaluationの結果に基づいて生成される。

Authorization Decisionは、少なくとも以下のいずれかを含むことができる。

* Permit
* Deny
* Indeterminate

Claim 1では、具体的なDecision形式を過度に限定しない。

---

## 10. Claim 1におけるEnforcement

Enforcementは、Authorization Decisionを実際のService Executionに適用する処理として位置付ける。

したがって、

Authorization Decision
↓
Enforcement
↓
Service Execution

という分離を維持する。

Authorization DecisionがPermitであっても、Enforcementにおいて追加条件を確認する実施形態を許容する。

---

## 11. Claim 1におけるService Execution

Service Executionは、Authorization DecisionおよびEnforcementに基づいて制御される保護対象の処理とする。

Service Executionは、例えば、

* 情報取得
* 情報変更
* Resourceへのアクセス
* Transaction実行
* 商品購入
* 割引処理
* サービス提供
* API実行
* データ処理

その他の処理を含むことができる。

特定の業務用途に限定しない。

---

# 12. Authentication Layerの従属請求項

Authentication Layerでは、Authenticationそのものを発明の中心とするのではなく、Authenticationを本発明のAuthorization構造へ接続する具体的な実施形態をFallbackとして保持する。

---

## 13. Authentication Object

Authenticationに使用されるObjectをAuthentication Objectとして管理する構成を従属請求項候補とする。

Authentication Objectは、例えば、

* QR Code
* Token
* Challenge
* One-Time Token
* Authentication Credential
* Authentication Request
* Authentication Session Information

等を含むことができる。

---

## 14. Authentication ObjectのValidity

Authentication Objectには、所定のValidity Periodを設定できる。

Authentication ObjectのValidityは、EntitlementのValidityとは独立して管理できる。

この限定は重要なFallbackとする。

---

## 15. Authentication ObjectのExpiration

Authentication ObjectについてExpirationを設定する構成を従属請求項候補とする。

Expiration後には、当該Authentication ObjectをAuthenticationに使用できない構成とすることができる。

---

## 16. Authentication ValidityとEntitlement Validity

以下の関係を重要なFallbackとして保持する。

Authentication Object Validity
≠
Entitlement Validity

例えば、

Authentication Object Validity = 30秒

Entitlement Validity = 数時間

とすることができる。

ただし、30秒という数値自体を発明の必須構成とはしない。

---

## 17. QR Code

QR CodeをAuthentication Objectとして使用する構成を従属請求項候補とする。

QR Codeは、

* Web画面
* Display
* Terminal
* Mobile Device
* その他の表示装置

に表示することができる。

---

## 18. QR Codeの短時間Validity

QR Codeに所定の短時間Validityを設定する構成を従属請求項候補とする。

例えば、

QR Code生成
↓
所定時間のみ有効
↓
Validity終了
↓
Authenticationへの使用不可

という構成とする。

Validityは30秒に限定しない。

---

## 19. 30秒という数値の扱い

30秒は具体的実施形態として明細書および従属請求項に保持することができる。

ただし、Claim 1では30秒を必須条件としない。

さらに、QR CodeのValidityについても、

* 数秒
* 数十秒
* 数分
* その他の所定期間

を含むように表現する。

---

## 20. Authentication Freshness

Authentication ResultについてFreshness条件を設定する構成を従属請求項候補とする。

例えば、

Authentication Result
↓
Freshness確認
↓
Freshness条件を満たす
↓
Policy Evaluation

という構成とする。

---

## 21. Replay Prevention

Authentication ObjectまたはAuthentication ResultについてOne-Time Useその他のReplay Prevention条件を設定する構成を従属請求項候補とする。

Replay PreventionはValidityとは独立した条件として扱うことができる。

---

# 22. Entitlement Layerの従属請求項

Entitlement Layerは、本発明の特許上のFallbackとして特に重要である。

---

## 23. Entitlementの独立管理

EntitlementをAuthentication Resultとは別個の情報として管理する構成を従属請求項候補とする。

これには、

* 保存
* 取得
* 生成
* 更新
* 有効化
* 停止
* Revocation
* Expiration

等を含むことができる。

---

## 24. Authentication完了後のEntitlement

Authenticationが完了した後にEntitlementを生成または有効化する構成を従属請求項候補とする。

ただし、Entitlementの生成時点をAuthentication完了直後に限定しない。

---

## 25. Authenticationとは異なる時点でのEntitlement

EntitlementをAuthenticationとは異なる時点で生成、更新または確認できる構成をFallbackとして保持する。

これにより、

Authentication
↓
時間経過
↓
Entitlement利用
↓
Authorization

という構成を保護対象とできる。

---

## 26. Entitlement Validity

Entitlementに所定のValidity Periodを設定する構成を従属請求項候補とする。

EntitlementがValidity Periodの範囲外となった場合、当該EntitlementをAuthorization Evaluationにおいて有効なEntitlementとして扱わない構成を含む。

---

## 27. Entitlement Expiration

EntitlementについてExpirationを設定する構成を従属請求項候補とする。

ExpirationとValidity Periodは同一概念として固定しない。

---

## 28. Entitlement State

EntitlementにStateを設定し、Stateに応じてAuthorization Evaluationを制御する構成を従属請求項候補とする。

例えば、

* Active
* Expired
* Revoked
* Suspended
* Pending

等を含むことができる。

---

# 29. Conditional Entitlement

Conditional Entitlementは重要なFallbackとして保持する。

Entitlementを、所定の条件の成立に基づいて生成、付与または有効化する構成を従属請求項候補とする。

例えば、

Authentication
+
所定のContext
+
所定のEvent
↓
Conditional Entitlement

という構成とする。

---

## 30. 来店認証の実施形態

Conditional Entitlementの具体例として、

「来店した」

という事実をAuthenticationその他の処理によって確認し、

「来店済み」というEntitlementを生成または有効化する構成を保持する。

このEntitlementは、Authenticationとは異なるServiceで利用できる。

---

# 31. Cross-Service Entitlement

Entitlementを生成または確認したServiceとは異なるServiceにおいて利用する構成を重要なFallbackとして保持する。

例えば、

Physical Store Service
↓
来店Authentication
↓
「来店済み」Entitlement
↓
EC Service
↓
Policy Evaluation
↓
Authorization Decision
↓
割引処理

という構成である。

---

## 32. Cross-Serviceの技術的意味

Cross-Service構成では、Authenticationそのものを別Serviceへ直接移転することを必須としない。

むしろ、

Authentication Result
↓
Entitlement
↓
別ServiceでEntitlementを評価
↓
Authorization

という情報構造を保護対象とする。

---

# 33. Policy Evaluation Layer

Policy Evaluationに関する従属請求項では、入力情報を追加することでFallbackを形成する。

例えば、

* Authentication Result
* Entitlement
* Subject
* Resource
* Action
* Security Context
* Transaction
* Object State
* Time
* Location
* Device Information
* Service Information

等をPolicy Evaluationの対象とする。

---

## 34. Authorization Evaluation

Policy EvaluationとAuthorization Evaluationを分離する構成を従属請求項候補とする。

Policy EvaluationはPolicyに基づく評価を行い、Authorization EvaluationはAuthorization Decision生成に必要な条件を評価する。

ただし、実施形態によっては両処理を一つの処理部で実装できる。

---

# 35. Authorization Decision

Authorization Decisionを生成する構成を維持する。

Decisionは、

* Permit
* Deny
* Indeterminate

を含むことができる。

---

## 36. Permit

Permitに基づいてService Executionを許可する構成を従属請求項候補とする。

---

## 37. Deny

Denyに基づいてService Executionを拒否する構成を従属請求項候補とする。

---

## 38. Indeterminate

必要な情報または条件を確定できない場合にIndeterminateを生成する構成を従属請求項候補とする。

Indeterminateの場合の処理として、

* 再Authentication
* Revalidation
* 再Evaluation
* Deny
* 処理停止
* 処理終了

等を許容する。

---

# 39. Enforcement Layer

EnforcementをAuthorization Decisionとは独立した処理として構成する。

Enforcementは、

* Service Executionの許可
* Service Executionの拒否
* Actionの制限
* Resourceへのアクセス制限
* Transactionの制限
* その他のDecision適用

を行うことができる。

---

## 40. Service Executionとの分離

Service ExecutionそのものとEnforcementを同一処理としない。

この分離をFallbackとして維持する。

---

# 41. Revocation

Entitlementまたはその他のObjectについてRevocationを実行できる構成を従属請求項候補とする。

RevocationはExpirationとは異なる状態変化として扱う。

---

## 42. Revalidation

Authentication Result、Entitlement、Security Contextその他の情報についてRevalidationを実行する構成を従属請求項候補とする。

Revalidationの結果に応じて、

* Permit
* Deny
* Indeterminate
* 再Authentication

等を生成できる。

---

## 43. Fail-Closed

必要な条件を確定できない場合にService Executionを許可しない構成を従属請求項候補とする。

ただし、Fail-ClosedをClaim 1の必須構成とするかについては、先行技術調査および最終的なClaim Constructionを踏まえて判断する。

---

# 44. Security Context

Policy EvaluationまたはAuthorization EvaluationにSecurity Contextを利用する構成を従属請求項候補とする。

Security Contextには、

* Device
* Location
* Time
* Network
* Session
* Risk
* Service
* Transaction

その他のContext情報を含むことができる。

---

# 45. Transaction

Authentication Result、Entitlement、Policy Evaluation、Authorization DecisionおよびService Executionを一つのTransactionまたは関連Transactionとして管理する構成を従属請求項候補とする。

---

# 46. Object State

Authentication Object、Entitlementその他のProtocol ObjectについてStateを管理する構成を従属請求項候補とする。

StateはAuthorization Decisionとは独立して管理できる。

---

# 47. StateとAuthorizationの分離

ObjectがActiveであること自体をAuthorization Decisionと同一視しない。

例えば、

Entitlement = Active
↓
Policy Evaluation
↓
Authorization Decision = Permit

という構造とする。

したがって、Active StateはPermitそのものではない。

---

# 48. Evidence

Authentication、Entitlement、Policy Evaluation、Authorization Decision、EnforcementおよびService Executionに関するEvidenceを保存または参照する構成を従属請求項候補とする。

---

# 49. Audit

各Protocol処理についてAudit Informationを生成または保存する構成を従属請求項候補とする。

Audit対象には、

* Authentication
* Entitlement
* Policy Evaluation
* Authorization Decision
* Enforcement
* Service Execution

を含めることができる。

---

# 50. Fallback Structure — 第一段階

第一Fallbackは、Claim 1の中心構造を維持したまま、Entitlementの独立性を明確化する構成とする。

```text
Authentication Result
+
Independent Entitlement
↓
Policy Evaluation
↓
Authorization Decision
↓
Enforcement
↓
Service Execution
```

これは本発明の最も重要なFallbackの一つとする。

---

# 51. Fallback Structure — 第二段階

第二Fallbackでは、Authentication ObjectのValidityとEntitlement Validityの分離を追加する。

```text
Authentication Object
    ↓
Short Validity
    ↓
Authentication Result

Independent Entitlement
    ↓
Longer Validity
    ↓
Policy Evaluation
```

これにより、短時間のAuthentication Objectと、より長期間有効なEntitlementを分離して管理する構成を保護できる。

---

# 52. Fallback Structure — 第三段階

第三Fallbackでは、Conditional Entitlementを追加する。

```text
Authentication
+
Condition
↓
Conditional Entitlement
↓
Policy Evaluation
↓
Authorization
```

---

# 53. Fallback Structure — 第四段階

第四FallbackではCross-Serviceを追加する。

```text
Service A
  ↓
Authentication
  ↓
Entitlement
  ↓
Service B
  ↓
Policy Evaluation
  ↓
Authorization
```

---

# 54. Fallback Structure — 第五段階

第五Fallbackでは、Authorization DecisionとEnforcementを分離する。

```text
Policy Evaluation
↓
Authorization Evaluation
↓
Authorization Decision
↓
Enforcement
↓
Service Execution
```

---

# 55. Fallback Structure — 第六段階

第六Fallbackでは、IndeterminateおよびRevalidationを追加する。

```text
Policy Evaluation
↓
Authorization Evaluation
↓
Indeterminate
↓
Revalidation / Re-authentication / Deny
```

---

# 56. Fallback Structure — 第七段階

第七FallbackではRevocationおよびObject Stateを追加する。

```text
Entitlement
↓
State
↓
Validity / Revocation
↓
Policy Evaluation
↓
Authorization
```

---

# 57. Claim Dependencyの基本原則

従属請求項は、一つの請求項に過剰な限定を集中させない。

例えば、

Authentication Object Validity
+
QR Code
+
30秒
+
WebAuthn
+
Secure Enclave
+
Replay Prevention
+
Cross-Service
+
Conditional Entitlement

を一つの従属請求項に同時に詰め込むことは避ける。

各技術的特徴を段階的に配置する。

---

# 58. Authentication系Fallback

Authentication系では、以下の順序を基本とする。

1. Authentication Result
2. Authentication Object
3. Authentication Object Validity
4. Authentication Object Expiration
5. Freshness
6. Replay Prevention
7. QR Code
8. WebAuthn
9. Public Key Cryptography
10. Secure Enclave / TEE

---

# 59. Entitlement系Fallback

Entitlement系では、以下の順序を基本とする。

1. Entitlementの独立管理
2. Entitlement Validity
3. Entitlement Expiration
4. Entitlement State
5. Conditional Entitlement
6. Cross-Service Entitlement
7. Revocation

---

# 60. Authorization系Fallback

Authorization系では、以下を保持する。

1. Policy Evaluation
2. Authorization Evaluation
3. Authorization Decision
4. Permit
5. Deny
6. Indeterminate
7. Revalidation
8. Fail-Closed
9. Enforcement

---

# 61. Context系Fallback

Context系では、

1. Security Context
2. Transaction
3. Object State
4. Time
5. Location
6. Device
7. Service Context

等を段階的に追加できる構造とする。

---

# 62. System / Method / Program間の対応

System請求項で保護する技術的構成は、原則としてMethod請求項へ対応させる。

Method請求項で規定する処理は、Program請求項およびRecording Medium請求項へ対応させる。

したがって、

```text
System
   ↓
Method
   ↓
Program
   ↓
Recording Medium
```

という対応関係を維持する。

---

# 63. Method ClaimのFallback

Method Claimでは、

1. Authentication Result取得
2. Entitlement取得・生成・確認
3. Policy Evaluation
4. Authorization Decision
5. Enforcement
6. Service Execution

の順序を基本構造とする。

各処理について追加条件を従属請求項として展開する。

---

# 64. Program ClaimのFallback

Program Claimでは、Method Claimに対応する処理をコンピュータに実行させることを基本とする。

特定のプログラミング言語、フレームワークまたはAPI形式に限定しない。

---

# 65. Recording Medium ClaimのFallback

Recording Medium Claimでは、前記Programを記録したコンピュータ読み取り可能な記録媒体として構成する。

記録媒体の物理形式を過度に限定しない。

---

# 66. QR CodeのFallback位置

QR Codeは本発明の中心的必須構成とはしない。

QR Codeは、

Authentication Object
↓
Validity
↓
Expiration
↓
Authentication

という具体的実施形態として従属請求項および明細書に展開する。

---

# 67. WebAuthnのFallback位置

WebAuthnはAuthentication mechanismの一実施形態として保持する。

Claim 1では必須としない。

WebAuthnを使用する場合には、

Challenge
+
Public Key
+
Signature
↓
Authentication Result

等の構成を従属請求項候補とする。

---

# 68. Public Key Cryptography

Public Key CryptographyをAuthentication mechanismとして使用する構成を従属請求項候補とする。

ただし、特定の暗号アルゴリズムには限定しない。

---

# 69. Secure Enclave / TEE

秘密鍵その他のCredentialをSecure Enclave、Trusted Execution Environmentまたは同等の保護領域に保存する構成を従属請求項候補とする。

この構成はAuthentication LayerのFallbackとして保持する。

---

# 70. 30秒のFallback位置

30秒という具体的数値は、最も下位のFallbackまたは実施形態に配置する。

基本構造は、

Authentication Object
↓
所定のValidity Period
↓
Validity終了
↓
Authentication Object使用不可

とする。

30秒はその一例とする。

---

# 71. Authentication ValidityとEntitlement ValidityのFallback

この関係は独立した重要なFallbackとする。

```text
Authentication Object Validity
       ↓
      短い
       ↓
Authentication

Entitlement Validity
       ↓
      長い
       ↓
Authorization
```

Authentication ObjectがExpirationした後でも、既に成立したEntitlementをPolicyに基づいて利用できる構成を保持する。

---

# 72. Conditional EntitlementのFallback

Conditional Entitlementは、AuthenticationとEntitlementの関係をさらに具体化するFallbackとして配置する。

例えば、

来店確認
↓
来店済みEntitlement
↓
EC Service
↓
Policy Evaluation
↓
割引Authorization
↓
Enforcement
↓
割引Service Execution

という構成を保持する。

---

# 73. Cross-ServiceのFallback

Cross-Serviceは、Entitlementの独立管理をさらに強化するFallbackとする。

Entitlementを生成したServiceとAuthorizationを行うServiceを異なるServiceとして構成できる。

---

# 74. RevocationのFallback

Revocationは、ValidityおよびExpirationとは異なる状態変化として保持する。

```text
Entitlement
 ├─ Validity
 ├─ Expiration
 └─ Revocation
```

これらを同一概念として請求項に記載しない。

---

# 75. RevalidationのFallback

Revalidationは、AuthenticationまたはEntitlementの状態が変化した場合に再評価を行う構成として保持する。

Revalidationは、

* Authentication Result
* Entitlement
* Security Context
* Object State
* Policy

等を対象とすることができる。

---

# 76. Fail-ClosedのFallback

Fail-ClosedはSecurity系Fallbackとして保持する。

ただし、独立請求項の必須構成とするかは、先行技術および最終Claim Constructionを踏まえて判断する。

---

# 77. Fallback Priority

Fallbackの優先順位は以下を基本とする。

### Priority 1

Authentication ResultとEntitlementの独立性

### Priority 2

Policy Evaluation → Authorization Decision → Enforcement → Service Executionの分離

### Priority 3

Authentication Object ValidityとEntitlement Validityの分離

### Priority 4

Conditional Entitlement

### Priority 5

Cross-Service Entitlement

### Priority 6

Authorization DecisionのPermit / Deny / Indeterminate

### Priority 7

Revocation / Revalidation

### Priority 8

Security Context / Transaction / State

### Priority 9

QR Code / WebAuthn / Public Key Cryptography

### Priority 10

30秒、Secure Enclaveその他の具体的実施形態

---

# 78. 先行技術に対するFallback Strategy

先行技術調査によってClaim 1の一部構成が開示されていることが判明した場合でも、直ちにAuthenticationを追加してClaim 1を限定するのではなく、以下の順序で検討する。

1. Entitlementの独立性を明確化する。
2. Policy Evaluationを明確化する。
3. Authorization Decisionを明確化する。
4. Enforcementを明確化する。
5. Service Executionとの分離を明確化する。
6. Authentication ValidityとEntitlement Validityの分離を追加する。
7. Conditional Entitlementを追加する。
8. Cross-Serviceを追加する。
9. Revocation / Revalidationを追加する。
10. 最後に具体的Authentication方式を検討する。

---

# 79. Authenticationを先に限定しない理由

本発明の技術的価値は、特定のAuthentication方式そのものではなく、

Authentication Result
+
Entitlement
↓
Policy Evaluation
↓
Authorization
↓
Enforcement
↓
Service Execution

という情報処理構造にある。

したがって、先行技術との差異を確保するために、最初からQR CodeやWebAuthnをClaim 1へ追加することは避ける。

---

# 80. EntitlementをFallbackの中心に置く理由

Authentication技術単体では、既存の認証技術との重複が発生しやすい。

一方、本発明ではAuthentication ResultとEntitlementを分離し、Entitlementを独立したObjectとして管理する。

さらに、EntitlementをPolicy Evaluationに入力し、その結果に基づいてAuthorization Decisionを生成する。

この関係をFallbackの中心とする。

---

# 81. Time-based Fallback

時間概念については、少なくとも以下を独立して保持する。

1. Authentication Object Validity
2. Authentication Object Expiration
3. Authentication Result Freshness
4. Entitlement Validity
5. Entitlement Expiration
6. Entitlement Revocation
7. Revalidation Timing
8. Service Execution Timing

これらを単一の「有効期限」という概念に統合しない。

---

# 82. Time-based Fallbackの重要性

特に、

Authentication Object Validity = 短時間

Entitlement Validity = より長時間

という関係は、本発明の具体的実施形態として重要である。

例えば、

QR Code Validity = 30秒

であっても、

来店済みEntitlement Validity = 数時間

とすることができる。

この構造により、Authenticationの一時性とEntitlementの時間的有効性を分離できる。

---

# 83. State-based Fallback

Object Stateについては、

* Active
* Pending
* Suspended
* Expired
* Revoked

等を保持できる。

StateはAuthorization Decisionそのものではない。

StateをPolicy EvaluationまたはAuthorization Evaluationの入力として利用する構成をFallbackとする。

---

# 84. Evidence / Audit Fallback

EvidenceおよびAuditは、発明の中心ではなく、実施上およびSecurity上のFallbackとして保持する。

これらをClaim 1に追加することは原則として避ける。

ただし、特定の先行技術との差異を確保する必要が生じた場合には、従属請求項へ追加できる構造を維持する。

---

# 85. Claim Dependencyの原則

従属請求項は、原則として以下を満たすよう設計する。

1. 上位請求項の構成を全て含む。
2. 一つまたは少数の追加的特徴を加える。
3. Fallbackとして意味のある技術的限定とする。
4. 単なる業務用途だけの限定を避ける。
5. 特定製品名への限定を避ける。
6. 特定APIへの限定を避ける。
7. 特定クラウドサービスへの限定を避ける。
8. 特定の数値への過度な限定を避ける。

---

# 86. Claim Groupの整理

請求項群は以下のグループに整理する。

### Group A — Core Architecture

* Authentication Result
* Entitlement
* Policy Evaluation
* Authorization
* Enforcement
* Service Execution

### Group B — Authentication

* Authentication Object
* Validity
* Expiration
* Freshness
* Replay Prevention

### Group C — Entitlement

* Entitlement Validity
* Expiration
* State
* Conditional Entitlement
* Cross-Service
* Revocation

### Group D — Authorization

* Policy Evaluation
* Authorization Evaluation
* Permit
* Deny
* Indeterminate
* Revalidation

### Group E — Security

* Security Context
* Transaction
* Object State
* Evidence
* Audit
* Fail-Closed

### Group F — Concrete Authentication

* QR Code
* WebAuthn
* Public Key Cryptography
* Secure Enclave / TEE

---

# 87. Claim Group間の関係

Group Aを本発明の中心とする。

Group BおよびGroup Cを中心的Fallbackとする。

Group DをAuthorization制御のFallbackとする。

Group EをSecurityおよびOperational Fallbackとする。

Group Fを具体的実施形態として保持する。

---

# 88. System ClaimのFallback

System Claimでは、

Core Architecture
↓
Authentication Layer
↓
Entitlement Layer
↓
Authorization Layer
↓
Security Layer

の順で限定を追加できる構造を維持する。

---

# 89. Method ClaimのFallback

Method Claimでは、各処理ステップを追加することでSystem Claimと対応するFallbackを形成する。

---

# 90. Program / Recording MediumのFallback

ProgramおよびRecording Mediumについては、Method Claimと対応する処理を保持する。

System Claimとの差異は、実装主体ではなく保護対象となる形式の違いとして整理する。

---

# 91. Claim 1からのFallback経路

Claim 1からは、少なくとも以下のFallback経路を保持する。

```text
Claim 1
  │
  ├─ Entitlement Independent
  │
  ├─ Authentication Validity
  │
  ├─ Entitlement Validity
  │
  ├─ Conditional Entitlement
  │
  ├─ Cross-Service
  │
  ├─ Authorization Decision
  │
  ├─ Enforcement
  │
  ├─ Revalidation
  │
  └─ Security Context
```

---

# 92. Authentication系Fallback経路

```text
Authentication
  │
  ├─ Authentication Object
  │     │
  │     ├─ Validity
  │     ├─ Expiration
  │     └─ One-Time Use
  │
  ├─ Freshness
  │
  ├─ Replay Prevention
  │
  └─ QR Code
         │
         ├─ WebAuthn
         └─ Public Key Cryptography
```

---

# 93. Entitlement系Fallback経路

```text
Entitlement
  │
  ├─ Independent Management
  │
  ├─ Validity
  │
  ├─ Expiration
  │
  ├─ State
  │
  ├─ Conditional
  │
  ├─ Cross-Service
  │
  └─ Revocation
```

---

# 94. Authorization系Fallback経路

```text
Policy
  ↓
Policy Evaluation
  ↓
Authorization Evaluation
  ↓
Authorization Decision
  │
  ├─ Permit
  ├─ Deny
  └─ Indeterminate
       │
       ├─ Revalidation
       ├─ Re-authentication
       └─ Deny
  ↓
Enforcement
  ↓
Service Execution
```

---

# 95. Phase 4で避けるべき構成

以下は、原則として独立請求項の必須構成としない。

* QR Code
* 30秒
* WebAuthn
* Secure Enclave
* 特定スマートフォン
* 特定OS
* 特定クラウド
* AWS
* 特定API
* 特定業務
* ECサイト
* 店舗
* 割引

これらは必要に応じて従属請求項または実施形態に配置する。

---

# 96. 業務用途の扱い

「来店した人にECサイトで割引する」というScenarioは、本発明の重要な実施形態である。

しかし、発明そのものを「来店割引システム」として限定しない。

一般化された構造は、

Event / Authentication
↓
Entitlement
↓
Cross-Service Policy Evaluation
↓
Authorization
↓
Service Execution

である。

---

# 97. 技術的効果との対応

Claim Dependencyは、以下の技術的効果と対応するように構成する。

### AuthenticationとEntitlementの分離

Authenticationの一時性とEntitlementの持続性を分離できる。

### Policy Evaluation

ContextおよびEntitlementに基づく動的なAuthorizationが可能となる。

### AuthorizationとEnforcementの分離

Decision生成と実際のService制御を分離できる。

### Cross-Service

一つのServiceで確認された権利を別ServiceのAuthorizationに利用できる。

### Revocation

Entitlementその他のObjectの状態変化をAuthorizationに反映できる。

---

# 98. Phase 3 Claimsとの対応

本書では、Phase 3で作成した `claims_v2.md` に含まれる請求項候補を基礎とする。

Phase 3のClaim 1～4については、独立請求項候補として維持する。

Phase 3のClaim 5以降については、本書のClaim GroupおよびFallback Priorityに基づいて最終的な従属関係を整理する。

---

# 99. Phase 4での請求項数

請求項数については、本書の段階では固定しない。

理由は、先行技術調査、明細書の開示範囲および最終的な出願戦略によって最適な請求項数が変わるためである。

したがって、現時点では、

* 必須請求項
* 強いFallback
* 補助的Fallback
* 実施形態限定

を区別することを優先する。

---

# 100. 最終Claim Setの考え方

最終的な請求項セットでは、

### Core

Authentication Result
+
Independent Entitlement
+
Policy Evaluation
+
Authorization Decision
+
Enforcement
+
Service Execution

を維持する。

### Primary Fallback

Authentication Validity
+
Entitlement Validity

### Secondary Fallback

Conditional Entitlement
+
Cross-Service

### Additional Fallback

Authorization Decision
+
Permit / Deny / Indeterminate
+
Revalidation

### Security Fallback

Revocation
+
State
+
Security Context
+
Transaction

### Concrete Fallback

QR Code
+
WebAuthn
+
Public Key Cryptography
+
30秒
+
Secure Enclave / TEE

という階層を基本とする。

---

# 101. Phase 4 Final Architecture

Phase 4終了時点でのClaim Architectureは以下を基本形とする。

```text
                 ┌──────────────────────────┐
                 │ Independent Claim 1      │
                 │ Information Processing   │
                 │ System                   │
                 └────────────┬─────────────┘
                              │
             ┌────────────────┼────────────────┐
             │                │                │
             ▼                ▼                ▼
       Authentication     Entitlement     Policy / Auth
             │                │                │
             │                │                ├─ Policy Evaluation
             │                │                ├─ Authorization
             │                │                └─ Decision
             │                │
             │                ├─ Validity
             │                ├─ Conditional
             │                ├─ Cross-Service
             │                └─ Revocation
             │
             ├─ Object
             ├─ Validity
             ├─ Expiration
             ├─ Freshness
             ├─ Replay
             └─ QR / WebAuthn
                              │
                              ▼
                         Enforcement
                              │
                              ▼
                       Service Execution


       Independent Claim 2
              │
              ▼
       Information Processing
             Method


       Independent Claim 3
              │
              ▼
           Program


       Independent Claim 4
              │
              ▼
       Recording Medium
```

---

# 102. Phase 4 Final Principles

Phase 4では、以下を確定原則とする。

1. Authenticationは発明全体の入口であるが、発明の中心そのものではない。
2. Authentication ResultとEntitlementは分離する。
3. Entitlementは独立して管理可能とする。
4. Authentication ObjectのValidityとEntitlement Validityは分離する。
5. Policy EvaluationとAuthorization Decisionは分離する。
6. Authorization DecisionとEnforcementは分離する。
7. EnforcementとService Executionは分離する。
8. Conditional EntitlementをFallbackとして保持する。
9. Cross-Service EntitlementをFallbackとして保持する。
10. RevocationおよびRevalidationをFallbackとして保持する。
11. QR Codeは具体的実施形態として保持する。
12. 30秒は具体例として保持し、発明を30秒に限定しない。
13. WebAuthnおよびPublic Key Cryptographyは具体的Authentication方式として保持する。
14. Secure Enclave / TEEは具体的Security実施形態として保持する。
15. System / Method / Program / Recording Mediumの各請求項群を維持する。
16. Claim 1を先行技術対応のために狭める場合でも、複数のFallback経路を確保する。
17. 最終的な請求項文言は、明細書の開示範囲との整合性を確認した上で確定する。

---

# 103. Phase 5への引継ぎ

Phase 4で整理したClaim Architectureを基礎として、次にPhase 5「明細書作成」へ移行する。

Phase 5では、単にInvention Coreを文章化するのではなく、

* 技術分野
* 背景技術
* 従来技術の問題点
* 発明が解決しようとする課題
* 課題を解決するための手段
* 発明の効果
* 図面の簡単な説明
* 実施形態
* 各Protocol Object
* Authentication
* Entitlement
* Policy Evaluation
* Authorization
* Enforcement
* Service Execution
* 時間的Validity
* Conditional Entitlement
* Cross-Service
* Security Context
* State
* Transaction
* Evidence
* Audit

を体系的に記載する。

特に、Phase 4で確保したFallbackについて、明細書側に十分な開示を設ける。

---

# 104. Phase 5での重要事項

特許請求項に記載する構成については、原則として明細書側にも対応する説明および実施可能な態様を記載する。

したがって、Phase 5ではClaim 1の構成だけでなく、従属請求項候補として保持した技術的特徴についても、必要な範囲で明細書に開示する。

これにより、後の補正、分割、Fallback Claim Construction等に対応できる構造を確保する。

---

# 105. Phase 4 Final Statement

本書により、NEW-shot2play Version 2.0の特許請求項について、独立請求項を中心とする基本Architecture、従属請求項の技術分野別Group、Fallback Structureおよび優先順位を整理した。

本発明の最重要Fallbackは、

Authentication Result
と
Authentication Resultとは独立して管理され得るEntitlement

との分離である。

さらに、

Authentication Object Validity
と
Entitlement Validity

との時間的分離、

Policy Evaluation
と
Authorization Decision

との機能的分離、

Authorization Decision
と
Enforcement

との機能的分離、

Enforcement
と
Service Execution

との機能的分離、

およびConditional Entitlement、Cross-Service Entitlementを組み合わせることで、複数の特許的保護範囲を形成する。

具体的なQR Code、30秒、WebAuthn、Public Key CryptographyおよびSecure Enclave / TEEは、本発明を不必要に限定しないよう、具体的実施形態および下位Fallbackとして保持する。

以上をPhase 4のClaim Dependency Strategyとして確定し、次段階のPhase 5「明細書作成」に引き継ぐ。

