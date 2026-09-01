
# NEW-shot2play Technical Specification Version 2.0

# Dependent Claim Strategy

## Phase 4 — 従属請求項展開・Claim Dependency Strategy

## 1. 本文書の目的

本書は、NEW-shot2play Technical Specification Version 2.0に基づく特許出願において、従属請求項の構成、請求項間の従属関係、Fallback Structureおよび各技術的特徴の配置を整理するための設計文書である。

本書は最終的な特許請求の範囲そのものではなく、特許請求項を最終確定するための設計文書として位置付ける。

本発明の中心的な処理構造は、Authentication Result、Entitlement、Policy Evaluation、Authorization Decision、EnforcementおよびService Executionを分離して扱うことである。

## 50. Fallback Structure — 第一段階

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

## 51. Fallback Structure — 第二段階

第二Fallbackでは、Claim 1において既に採用されているAuthentication ObjectのValidityとEntitlement Validityの分離を前提として、これらの時間的有効性をさらに具体化する構成を追加する。

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

Authentication ObjectのValidityとEntitlement Validityとは独立して管理され得るものとして扱い、例えばAuthentication Objectを短時間有効とし、Authenticationによって確認された結果に基づくEntitlementをより長期間有効とする構成を従属請求項上の具体化として保護する。

ここで、Validity分離そのものをClaim 1に対して新たに追加するFallbackとして扱うのではなく、Claim 1における独立性を前提として、その時間的構成をさらに限定するFallbackとして位置付ける。

## 52. Fallback Structure — 第三段階

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

## 53. Fallback Structure — 第四段階

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

## 54. Fallback Structure — 第五段階

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

## 55. Fallback Structure — 第六段階

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

## 56. Fallback Structure — 第七段階

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

## 57. Claim Dependencyの基本原則

従属請求項は、一つの請求項に過剰な限定を集中させない。

各技術的特徴を段階的に配置する。

## 58. Authentication系Fallback

Authentication系では、Authentication Result、Authentication Object、Validity Period、Expiration、Freshness、Replay Prevention、QR Code、WebAuthn、Public Key Cryptography等を段階的に保持する。

## 59. Entitlement系Fallback

Entitlement系では、Entitlementの独立管理、Entitlement Validity、Expiration、State、Conditional Entitlement、Cross-Service Entitlement、Revocationを保持する。

## 60. Authorization系Fallback

Authorization系では、Policy Evaluation、Authorization Evaluation、Authorization Decision、Permit、Deny、Indeterminate、Revalidation、Fail-Closed、Enforcementを保持する。

## 61. Context系Fallback

Context系では、Security Context、Transaction、Object State、Time、Location、Device、Service Context等を段階的に追加できる構造とする。

## 62. System / Method / Program間の対応

System、Method、ProgramおよびRecording Mediumの各請求項群について、対応する処理構成を維持する。

## 63. Method ClaimのFallback

Method Claimでは、Authentication Result取得、Entitlement取得・生成・確認、Policy Evaluation、Authorization Decision、Enforcement、Service Executionを基本構造とする。

## 64. Program ClaimのFallback

Program Claimでは、Method Claimに対応する処理をコンピュータに実行させることを基本とする。

## 65. Recording Medium ClaimのFallback

Recording Medium Claimでは、前記Programを記録したコンピュータ読み取り可能な記録媒体として構成する。

## 66. QR CodeのFallback位置

QR Codeは本発明の中心的必須構成とはせず、Authentication Object、Validity、Expiration、Authenticationという具体的実施形態として保持する。

## 67. WebAuthnのFallback位置

WebAuthnはAuthentication mechanismの一実施形態として保持する。

## 68. Public Key Cryptography

Public Key CryptographyをAuthentication mechanismとして使用する構成を従属請求項候補とする。

## 69. Secure Enclave / TEE

秘密鍵その他のCredentialをSecure Enclave、Trusted Execution Environmentまたは同等の保護領域に保存する構成を従属請求項候補とする。

## 70. 30秒のFallback位置

30秒という具体的数値は、最も下位のFallbackまたは実施形態に配置する。基本構造はAuthentication Objectの所定Validity Periodとし、30秒はその一例とする。

## 71. Authentication ValidityとEntitlement ValidityのFallback

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

## 72. Conditional EntitlementのFallback

Conditional Entitlementは、AuthenticationとEntitlementの関係をさらに具体化するFallbackとして配置する。

## 73. Cross-ServiceのFallback

Cross-Serviceは、Entitlementの独立管理をさらに強化するFallbackとする。

## 74. RevocationのFallback

Revocationは、ValidityおよびExpirationとは異なる状態変化として保持する。

## 75. RevalidationのFallback

Revalidationは、AuthenticationまたはEntitlementの状態が変化した場合に再評価を行う構成として保持する。

## 76. Fail-ClosedのFallback

Fail-ClosedはSecurity系Fallbackとして保持する。

## 77. Fallback Priority

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

## 78. 先行技術に対するFallback Strategy

先行技術調査によってClaim 1の一部構成が開示されていることが判明した場合でも、直ちにAuthenticationを追加してClaim 1を限定するのではなく、Entitlementの独立性、Policy Evaluation、Authorization Decision、Enforcement、Service Executionとの分離、Validity分離、Conditional Entitlement、Cross-Service、Revocation / Revalidation、具体的Authentication方式の順で検討する。

## 79. Authenticationを先に限定しない理由

本発明の技術的価値は、特定のAuthentication方式そのものではなく、Authentication Result、Entitlement、Policy Evaluation、Authorization、EnforcementおよびService Executionという情報処理構造にある。

## 80. EntitlementをFallbackの中心に置く理由

本発明ではAuthentication ResultとEntitlementを分離し、Entitlementを独立したObjectとして管理し、Policy Evaluationに入力してAuthorization Decisionを生成する。この関係をFallbackの中心とする。

## 81. Time-based Fallback

時間概念については、Authentication Object Validity、Authentication Object Expiration、Authentication Result Freshness、Entitlement Validity、Entitlement Expiration、Entitlement Revocation、Revalidation Timing、Service Execution Timingを独立して保持する。

## 82. Time-based Fallbackの重要性

Authentication Object Validityを短時間、Entitlement Validityをより長時間とする関係は、本発明の具体的実施形態として重要である。

## 83. State-based Fallback

Object StateについてActive、Pending、Suspended、Expired、Revoked等を保持できる。StateはAuthorization Decisionそのものではなく、Policy EvaluationまたはAuthorization Evaluationの入力として利用する。

## 84. Evidence / Audit Fallback

EvidenceおよびAuditは、発明の中心ではなく、実施上およびSecurity上のFallbackとして保持する。

## 85. Claim Dependencyの原則

従属請求項は、上位請求項の構成を全て含み、一つまたは少数の追加的特徴を加え、Fallbackとして意味のある技術的限定とするよう設計する。

## 86. Claim Groupの整理

Group A — Core Architecture、Group B — Authentication、Group C — Entitlement、Group D — Authorization、Group E — Security、Group F — Concrete Authenticationに整理する。

## 87. Claim Group間の関係

Group Aを本発明の中心とし、Group BおよびGroup Cを中心的Fallback、Group DをAuthorization制御のFallback、Group EをSecurityおよびOperational Fallback、Group Fを具体的実施形態として保持する。

## 88. System ClaimのFallback

System ClaimではCore Architecture、Authentication Layer、Entitlement Layer、Authorization Layer、Security Layerの順で限定を追加できる構造を維持する。

## 89. Method ClaimのFallback

Method Claimでは、各処理ステップを追加することでSystem Claimと対応するFallbackを形成する。

## 90. Program / Recording MediumのFallback

ProgramおよびRecording Mediumについては、Method Claimと対応する処理を保持する。

## 91. Claim 1からのFallback経路

Claim 1からは、Entitlement Independent、Authentication Validity、Entitlement Validity、Conditional Entitlement、Cross-Service、Authorization Decision、Enforcement、Revalidation、Security Context等のFallback経路を保持する。

## 92. Authentication系Fallback経路

Authentication Object、Validity、Expiration、One-Time Use、Freshness、Replay Prevention、QR Code、WebAuthn、Public Key Cryptography等を段階的に配置する。

## 93. Entitlement系Fallback経路

EntitlementのIndependent Management、Validity、Expiration、State、Conditional、Cross-Service、Revocationを段階的に配置する。

## 94. Authorization系Fallback経路

Policy、Policy Evaluation、Authorization Evaluation、Authorization Decision、Permit、Deny、Indeterminate、Revalidation、Enforcement、Service Executionを段階的に配置する。

## 95. Phase 4で避けるべき構成

QR Code、30秒、WebAuthn、Secure Enclave、特定スマートフォン、特定OS、特定クラウド、特定API、特定業務等を原則として独立請求項の必須構成としない。

## 96. 業務用途の扱い

「来店した人にECサイトで割引する」というScenarioは重要な実施形態であるが、発明そのものを来店割引システムとして限定しない。

## 97. 技術的効果との対応

AuthenticationとEntitlementの分離、Policy Evaluation、AuthorizationとEnforcementの分離、Cross-Service、Revocation等の技術的効果をClaim Dependencyと対応させる。

## 98. Phase 3 Claimsとの対応

Phase 3で作成したclaims_v2.mdを基礎とし、独立請求項候補および従属請求項候補を本書のClaim GroupおよびFallback Priorityに基づいて整理する。

## 99. Phase 4での請求項数

請求項数については、本書の段階では固定せず、先行技術調査、明細書の開示範囲および最終的な出願戦略によって最適な請求項数を判断する。

## 100. 最終Claim Setの考え方

Core、Primary Fallback、Secondary Fallback、Additional Fallback、Security Fallback、Concrete Fallbackという階層を基本とする。

## 101. Phase 4 Final Architecture

Phase 4終了時点では、Independent Claim 1を中心としてAuthentication、Entitlement、Policy / Authorization、Enforcement、Service Executionを配置し、Independent Claim 2、3、4としてMethod、Program、Recording Mediumを対応させる。

## 102. Phase 4 Final Principles

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

## 103. Phase 5への引継ぎ

Phase 4で整理したClaim Architectureを基礎として、次にPhase 5「明細書作成」へ移行する。

## 104. Phase 5での重要事項

特許請求項に記載する構成については、原則として明細書側にも対応する説明および実施可能な態様を記載する。

## 105. Phase 4 Final Statement

本書により、NEW-shot2play Version 2.0の特許請求項について、独立請求項を中心とする基本Architecture、従属請求項の技術分野別Group、Fallback Structureおよび優先順位を整理した。
