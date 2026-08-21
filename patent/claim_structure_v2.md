# NEW-shot2play Technical Specification Version 2.0
# Claim Structure

## 1. 本文書の目的

本書は、NEW-shot2play Technical Specification Version 2.0における
発明概念を、日本国特許出願および将来のPCT出願に適した特許請求項へ
展開するための骨格を定義するものである。

本書は、特許請求の範囲そのものの最終文言ではない。

本書では、

- 独立請求項に含めるべき発明の必須構成
- 従属請求項へ展開可能な構成
- システム、方法、プログラム等への展開
- 実施形態として保持すべき具体例
- 発明を不必要に限定するため避けるべき表現
- 将来の明細書および図面との対応関係

を整理する。

本書はPhase 2の基準文書として使用し、
Phase 3における独立請求項の作成に引き継ぐ。

---

## 2. 発明の基本的な特許概念

本発明の中心的な特許概念は、

「Authenticationによって確認された事実またはAuthentication Resultを、
Authenticationそのものとは独立して管理されるEntitlementと関連付け、
Policyに基づくEvaluationを行い、その結果に基づいてAuthorizationを
決定し、当該Authorization DecisionをEnforcementを介して
Service Executionに適用する情報処理」

である。

本発明は、単なるAuthentication技術ではない。

また、単なるAuthorization技術でもない。

本発明の特徴は、Authentication、Entitlement、Policy Evaluation、
Authorization Evaluation、Authorization Decision、Enforcementおよび
Service Executionを、それぞれ意味的に区別された処理および情報として
構成し、それらを所定の関係に基づいて連携させる点にある。

---

## 3. 基本処理連鎖

本発明の基本的な処理連鎖は、以下のとおりである。

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

ただし、上記は必ずしも各処理が一対一または逐次的に実行されることを
意味しない。

例えば、EntitlementはAuthenticationの結果を契機として生成され得るが、
Authentication Resultそのものと同一の情報である必要はない。

また、EntitlementはAuthenticationとは異なるValidity Periodを有し得る。

Policy Evaluationは、Authentication Result、Entitlement、Policy、
Security Context、Transactionその他のContextを入力として評価し得る。

Authorization Evaluationは、Policy Evaluationその他の条件を考慮して
Authorization Decisionを生成するための評価処理として構成され得る。

Authorization Decisionは、Enforcementを介してService Executionに
適用され得る。

---

## 4. 発明の最小構成

独立請求項における最小構成は、以下の概念を中心として設計する。

### 4.1 Authentication

対象となる主体、端末、装置、サービスその他の対象について、
Authenticationを実行する。

Authenticationの実施方法は限定しない。

Authenticationは、

- QR Code
- One-time Token
- Challenge-Response
- Public Key Cryptography
- WebAuthn
- Passkey
- 生体認証
- PIN
- その他のAuthentication mechanism

を含み得る。

したがって、独立請求項ではQR Codeそのものを必須構成としない。

### 4.2 Authentication Result

Authenticationによって得られた結果をAuthentication Resultとして
扱う。

Authentication Resultは、Authenticationが成功したという事実、
対象識別情報、認証時刻、Authentication Objectその他の情報を
含み得る。

Authentication ResultはEntitlementと同一の情報である必要はない。

### 4.3 Entitlement

Authentication Resultその他の条件に基づいて、対象に関連する
Entitlementを生成、取得、更新、確認または有効化する。

Entitlementは、対象が特定のServiceまたはActionについて有する
権利、資格、条件、状態その他の意味を表す情報として構成する。

EntitlementはAuthentication Resultとは独立した情報オブジェクトとして
管理され得る。

### 4.4 Policy

Policyは、Authentication Result、Entitlementその他のContextを
どのように評価するかを規定する。

Policyは、

- 対象
- Service
- Action
- 時間
- 場所
- Entitlement
- Security Context
- Transaction
- その他の条件

を含み得る。

### 4.5 Policy Evaluation

Policy Evaluationは、Policyに基づいてAuthentication Result、
Entitlementその他の入力条件を評価する。

Policy EvaluationはAuthenticationそのものと同一の処理ではない。

また、Policy EvaluationはAuthorization Decisionそのものでもない。

### 4.6 Authorization Evaluation

Authorization Evaluationは、Policy Evaluationの結果および
その他の必要な条件に基づいて、対象に対して特定のActionを
許可するか否かを評価する。

### 4.7 Authorization Decision

Authorization Evaluationに基づいてAuthorization Decisionを生成する。

Authorization Decisionは少なくとも、

- Permit
- Deny
- Indeterminate

のいずれかを含み得る。

Authorization Decisionは、Policy Evaluationそのものとは区別される。

### 4.8 Enforcement

EnforcementはAuthorization DecisionをService Executionに適用する。

Enforcementは、Permitの場合に許可されたActionのみをService Executionへ
到達させることができる。

Denyの場合にはService Executionを拒否、停止または制限することができる。

Indeterminateの場合には、Policyに従ってDeny、Revalidation、
Recovery、Terminationその他の処理を実行し得る。

### 4.9 Service Execution

Service Executionは、Authorization DecisionおよびEnforcementに
従って実行されるServiceまたはActionである。

Service Execution自体は本発明の中心ではなく、Authorizationの結果が
実際のServiceへ適用される最終段階として位置付ける。

---

## 5. 独立請求項の中心となる構成関係

独立請求項では、個々の構成を列挙するだけではなく、
各構成間の関係を明確にする必要がある。

特に重要なのは以下の関係である。

### 5.1 AuthenticationとEntitlementの関係

Authentication Resultに基づいてEntitlementを生成、取得または
有効化できる。

しかし、Authentication ResultとEntitlementとは同一ではない。

### 5.2 AuthenticationとEntitlementのValidityの関係

Authentication ObjectまたはAuthenticationに使用される一時的情報は、
所定の短いValidity Periodを有し得る。

一方、EntitlementはAuthentication Objectとは異なるValidity Periodを
有し得る。

したがって、

Authentication Object Validity
≠
Entitlement Validity

という構成を独立請求項または従属請求項で保護できる構造とする。

### 5.3 EntitlementとPolicy Evaluationの関係

EntitlementはPolicy Evaluationにおける評価対象となり得る。

Policy Evaluationは、単にAuthenticationが成功したか否かだけではなく、
Entitlementの存在、状態、Validityその他の条件を評価できる。

### 5.4 Policy EvaluationとAuthorization Decisionの関係

Policy Evaluationの結果を用いてAuthorization Evaluationを実行し、
Authorization Decisionを生成する。

Policy EvaluationとAuthorization Decisionを同一視しない。

### 5.5 Authorization DecisionとEnforcementの関係

Authorization Decisionは、Enforcementによって実際のService Executionに
適用される。

したがって、Authorization Decisionを生成しただけでは、
Service Executionが自動的に実行されることを必須としない。

### 5.6 EnforcementとService Executionの関係

Enforcementは、Authorization Decisionを実際のService Executionに
反映する境界として機能する。

この構成により、Authorizationの判断とServiceの実行を分離できる。

---

## 6. 独立請求項における発明の本質

独立請求項では、以下の全てを無条件に列挙することよりも、
発明の本質的な関係を明確にすることを優先する。

本発明の中心は、

1. Authenticationを行うこと
2. Authentication Resultを取得すること
3. Authentication Resultとは独立してEntitlementを管理すること
4. Policyに基づいてEntitlementその他の条件を評価すること
5. Authorizationを評価すること
6. Authorization Decisionを生成すること
7. Authorization DecisionをEnforcementに適用すること
8. Enforcementに基づいてService Executionを制御すること

である。

特に3から8までの関係が本発明の重要な部分である。

---

## 7. Authenticationを限定しない方針

独立請求項ではAuthentication mechanismを限定しない。

したがって、

「QR Codeを撮影する」

を必須要件とはしない。

代わりに、

「対象についてAuthenticationを実行しAuthentication Resultを取得する」

という一般化された表現を基本とする。

QR Codeは従属請求項または実施形態で展開する。

---

## 8. QR Codeの位置付け

QR Codeは本発明の重要な実施形態の一つである。

しかし、QR Codeそのものが発明の本質ではない。

QR Codeを使用する実施形態では、

- Web画面等にAuthentication Objectを表示する
- Authentication Objectは所定のValidity Periodを有する
- Smartphone等がAuthentication Objectを取得する
- Authenticationを実行する
- Authentication Resultを生成する
- Authentication ObjectのExpiration後は当該ObjectをAuthenticationに
  使用できない

という構成を採用できる。

---

## 9. Authentication ObjectのValidity

Authentication Objectは時間的Validityを有し得る。

Validity Periodは実装上任意の値とすることができる。

例えば、

- 数秒
- 10秒
- 30秒
- 1分
- 5分
- その他の所定時間

とすることができる。

独立請求項では30秒という数値に限定しない。

30秒は一実施形態として扱う。

---

## 10. Authentication ObjectのExpiration

Authentication ObjectはExpiration条件を有し得る。

Expiration後は、当該Authentication ObjectをAuthenticationに使用することを
禁止または無効化することができる。

Expirationは、

- 時間経過
- One-time Use
- Authentication成功
- Server-side invalidation
- Transaction終了
- その他の条件

によって発生し得る。

---

## 11. AuthenticationのFreshness

AuthenticationについてFreshnessを要求することができる。

Freshnessは、

- Authentication ObjectのValidity
- Timestamp
- Nonce
- Challenge
- One-time Token
- Transaction Identifier
- その他のFreshness mechanism

によって確保できる。

FreshnessはReplay防止と関連付けることができる。

---

## 12. AuthenticationとReplay防止

Authentication ObjectがExpirationした後に再利用されることを防止する。

また、One-time Use、Nonce、Challengeその他の手段によって、
同一Authentication Objectまたは同一Authentication Transactionの
Replayを防止することができる。

これらは独立請求項を過度に限定しないよう、主として従属請求項に展開する。

---

## 13. Entitlementの独立管理

本発明においてEntitlementはAuthentication Resultとは独立した
情報オブジェクトとして管理できる。

例えば、

Authentication Result:
「対象者が認証された」

Entitlement:
「対象者は本日来店したため、ECサービスにおいて10%割引を受ける権利を有する」

というように、Authentication ResultとEntitlementとは異なる意味を持つ。

この意味的分離は、本発明の重要な構成として保護対象とする。

---

## 14. Conditional Entitlement

Entitlementは条件付きで生成または有効化できる。

例えば、

「対象者が所定の場所に来店した」

というAuthenticationまたはその他の確認結果を契機として、

「当該対象者はECサービスにおいて割引を受ける権利を有する」

というEntitlementを生成する。

このEntitlementは、Authenticationが実行されたServiceとは
異なるServiceにおいて利用できる。

---

## 15. Cross-Service Entitlement

Entitlementは、Authenticationを実行したServiceとは異なる
Serviceで利用できる。

例えば、

Service A:
来店Authentication

↓
Entitlement:
来店済み

↓
Service B:
ECサイト

↓
Policy Evaluation

↓
Authorization Decision:
Permit

↓
Enforcement

↓
割引Service Execution

という構成が可能である。

このCross-Service利用は、本発明の重要な実施形態として保護する。

---

## 16. EntitlementのValidity

Entitlementは独自のValidity Periodを有し得る。

EntitlementのValidityはAuthentication ObjectのValidityとは
独立して設定できる。

例えば、

Authentication Object:
30秒

Entitlement:
数時間

という構成が可能である。

Authentication ObjectがExpirationしたことは、
必ずしもEntitlementのExpirationを意味しない。

逆に、EntitlementがExpirationまたはRevokedとなった場合には、
当該EntitlementをAuthorizationの根拠として使用できない構成とできる。

---

## 17. EntitlementのState

EntitlementはStateを有し得る。

例えば、

- Active
- Expired
- Revoked
- Suspended
- Pending
- その他の状態

を定義できる。

Authorization EvaluationではEntitlementのStateを評価できる。

---

## 18. Policy Evaluation

Policy Evaluationでは、少なくとも以下の一つ以上を評価対象とすることが
できる。

- Authentication Result
- Entitlement
- Entitlement State
- Entitlement Validity
- Subject
- Service
- Action
- Time
- Location
- Security Context
- Transaction
- その他のContext

Policy Evaluationは、これらを組み合わせた条件評価として構成できる。

---

## 19. Authorization Evaluation

Authorization Evaluationでは、対象が所定のServiceまたはActionを
実行できるか否かを評価する。

Authorization Evaluationは、Policy Evaluationの結果および
Entitlementその他の条件を利用できる。

Authorization EvaluationはAuthenticationとは異なる処理である。

---

## 20. Authorization Decision

Authorization DecisionはAuthorization Evaluationの結果として生成する。

基本的なDecisionは、

- Permit
- Deny
- Indeterminate

とする。

ただし、実装上は、

- Permit with Conditions
- Deny with Reason
- Revalidation Required
- Recovery Required
- その他のDecision State

を含み得る。

---

## 21. Permit

Permitは、適用される条件の下で所定のActionを実行することが
許可されたことを表す。

PermitはEnforcementに入力され、
Service Executionへ適用され得る。

---

## 22. Deny

Denyは、所定のActionのService Executionを許可しないことを表す。

Denyの場合、EnforcementはService Executionを拒否、停止または
制限することができる。

---

## 23. Indeterminate

必要な情報または条件を確定できない場合、Authorization Evaluationは
Indeterminateを生成し得る。

Indeterminateの場合にはPolicyに基づいて、

- Deny
- Revalidation
- Recovery
- Termination
- Fail-Closed

等を実行できる。

---

## 24. Enforcement

EnforcementはAuthorization DecisionをService Executionに適用する。

Enforcementは、DecisionとService Executionとの間に位置する
独立した処理境界として構成する。

この構成により、

Authorization Decision
と
Service Execution

とを意味的に分離できる。

---

## 25. Service Execution

Service Executionは、許可されたActionを実際に実行する処理である。

Service Executionは、例えば、

- 商品購入
- 割引適用
- コンテンツアクセス
- API呼出し
- データ取得
- 機能実行
- その他のService

を含み得る。

---

## 26. Security Context

Policy EvaluationまたはAuthorization EvaluationではSecurity Contextを
利用できる。

Security Contextは例えば、

- Device
- Network
- Location
- Session
- Risk
- Authentication状態
- Transaction
- その他のSecurity information

を含み得る。

Security ContextはEntitlementと組み合わせて評価できる。

---

## 27. Transaction

AuthenticationからService Executionまでの処理は、Transactionとして
管理できる。

Transactionには、

- Transaction Identifier
- Subject
- Authentication Result
- Entitlement
- Policy Evaluation
- Authorization Decision
- Enforcement
- Service Execution
- Result

等を関連付けることができる。

---

## 28. Object State

各Protocol ObjectはStateを有し得る。

例えば、

Authentication Object:
- Active
- Expired
- Used
- Invalid

Entitlement:
- Active
- Expired
- Revoked
- Suspended

Authorization Decision:
- Permit
- Deny
- Indeterminate
- Expired
- Revoked

等のStateを定義できる。

---

## 29. StateとAuthorizationの分離

Object StateそのものをAuthorization Decisionと同一視しない。

例えば、

EntitlementがActiveであることは、
必ずしも全てのService ExecutionがPermitされることを意味しない。

Policy EvaluationおよびAuthorization Evaluationによって、
他の条件と組み合わせてAuthorization Decisionを生成できる。

---

## 30. Revocation

EntitlementまたはAuthorizationに対してRevocationを実行できる。

Revocation後は、適用されるPolicyに従って当該Entitlementまたは
Authorizationを利用できないようにすることができる。

RevocationはExpirationとは異なる状態変更として扱うことができる。

---

## 31. Expiration

ExpirationはValidity Periodの終了その他の条件によって発生する。

Authentication ObjectのExpirationとEntitlementのExpirationとは
独立して管理できる。

この区別を本発明の重要な時間的特徴として保持する。

---

## 32. Revalidation

必要な場合、Authorization EvaluationまたはEnforcementの前後で
Revalidationを実行できる。

例えば、

- Authentication Freshness
- Entitlement Validity
- Entitlement State
- Security Context
- Policy
- Transaction State

を再確認することができる。

---

## 33. Fail-Closed

必要なSecurity Conditionを確定できない場合、
適用されるPolicyに従ってFail-Closed処理を実行できる。

Fail-Closedの場合、未確認の条件をPermitとして扱わない。

ただし、独立請求項ではFail-Closedを必須とするか、
従属請求項へ展開するかをPhase 3で検討する。

---

## 34. 発明の主要な差別化軸

特許請求項では、以下の差別化軸を重視する。

### 34.1 AuthenticationとEntitlementの分離

Authentication ResultとEntitlementを同一視せず、
独立した情報として管理する。

### 34.2 異なるValidity

Authentication ObjectとEntitlementが異なるValidity Periodを
持ち得る。

### 34.3 Cross-Service利用

Authenticationに関連して生成されたEntitlementを、
Authenticationとは異なるServiceで利用できる。

### 34.4 Policy-driven Authorization

Authenticationの成功そのものを最終的なアクセス許可とせず、
Entitlementその他の条件をPolicyに基づいて評価する。

### 34.5 AuthorizationとEnforcementの分離

Authorization DecisionとService Executionとの間に
Enforcementを設ける。

### 34.6 Service Executionへの適用

Authorization Decisionを実際のService Executionに結び付ける。

---

## 35. 独立請求項候補A：情報処理システム

第一候補は、複数の処理機能を有する情報処理システムとして構成する。

基本構成は、

1. Authentication処理部
2. Authentication Result生成または取得部
3. Entitlement管理部
4. Policy Evaluation部
5. Authorization Evaluation部
6. Authorization Decision生成部
7. Enforcement部
8. Service Execution部

である。

これらの処理部は、必ずしも物理的に別々の装置またはプログラムで
ある必要はない。

---

## 36. 独立請求項候補B：情報処理方法

第二候補は情報処理方法として構成する。

基本ステップは、

1. 対象についてAuthenticationを実行する
2. Authentication Resultを取得する
3. Authentication Resultに関連するEntitlementを管理する
4. Policyに基づいてEntitlementその他の条件を評価する
5. Authorizationを評価する
6. Authorization Decisionを生成する
7. Authorization DecisionをEnforcementに適用する
8. Service Executionを制御する

である。

---

## 37. 独立請求項候補C：プログラム

第三候補として、コンピュータを上記情報処理方法として機能させる
プログラムを構成する。

プログラム請求項は、最終的な日本国出願実務およびPCT展開を考慮して
Phase 3以降で文言を調整する。

---

## 38. 独立請求項候補D：記録媒体

必要に応じて、上記プログラムを記録した非一時的なコンピュータ読取可能
記録媒体等への展開を検討する。

具体的な請求項化は出願実務を考慮してPhase 3以降で決定する。

---

## 39. 独立請求項で優先する構成

独立請求項では、原則として以下の順序で重要性を評価する。

### 最重要

Authentication ResultとEntitlementとの意味的・管理上の分離

### 最重要

EntitlementをPolicy EvaluationおよびAuthorizationへ利用する構造

### 最重要

Authorization DecisionをEnforcementを介してService Executionへ
適用する構造

### 重要

Authentication ObjectとEntitlementのValidity/Expirationの分離

### 重要

Cross-Service Entitlement

### 重要

Conditional Entitlement

### 補強

Security Context

Transaction

Object State

Revalidation

Revocation

Replay/Freshness

---

## 40. 独立請求項から外す候補

以下は、発明を過度に限定する可能性があるため、
原則として独立請求項では必須としない。

- QR Code
- Smartphone
- 30秒
- WebAuthn
- Passkey
- Secure Enclave
- 特定のCloud Provider
- AWS
- 特定のDatabase
- 特定のFrontend Framework
- 特定の通信プロトコル
- 特定のAPI
- 特定の暗号アルゴリズム

これらは実施形態または従属請求項候補として保持する。

---

## 41. 従属請求項候補群A：Authentication

Authenticationに関して、以下を従属請求項へ展開できる。

1. QR Codeを利用する構成
2. 一時的Authentication Objectを利用する構成
3. Authentication ObjectにValidity Periodを設定する構成
4. Authentication ObjectにExpirationを設定する構成
5. One-time Use
6. Nonce
7. Challenge
8. Timestamp
9. Replay防止
10. Public Key Cryptography
11. WebAuthn
12. Passkey
13. Smartphoneを利用する構成

---

## 42. 従属請求項候補群B：Authentication Validity

以下を独立した従属構成として展開できる。

1. Authentication Objectが所定時間後にExpirationする
2. Authentication ObjectがOne-time Useである
3. Authentication ObjectのExpiration後にAuthenticationを拒否する
4. Authentication Freshnessを確認する
5. Authentication ResultにTimestampを関連付ける
6. Authentication ObjectとAuthentication Transactionを関連付ける

30秒という値は一実施形態として記載し、
請求項では「所定のValidity Period」等に一般化する。

---

## 43. 従属請求項候補群C：Entitlement

1. Authentication Resultに基づいてEntitlementを生成する
2. Entitlementを保存する
3. Entitlementを更新する
4. Entitlementを有効化する
5. Entitlementを停止する
6. EntitlementをRevocationする
7. EntitlementにStateを付与する
8. EntitlementにValidity Periodを設定する
9. EntitlementにExpirationを設定する
10. Authentication Objectとは異なるValidity Periodを設定する

---

## 44. 従属請求項候補群D：Conditional Entitlement

1. 所定条件が成立した場合にEntitlementを生成する
2. Authentication成功をEntitlement生成条件とする
3. LocationをEntitlement生成条件とする
4. TimeをEntitlement生成条件とする
5. TransactionをEntitlement生成条件とする
6. 複数条件を組み合わせる
7. 条件成立後に別ServiceでEntitlementを利用する

---

## 45. 従属請求項候補群E：Cross-Service

1. Authentication ServiceとAuthorization Serviceを異なるServiceとする
2. Authenticationに関連して生成されたEntitlementを別Serviceへ提供する
3. 別ServiceのPolicy EvaluationでEntitlementを評価する
4. 別ServiceのAuthorization Decisionを生成する
5. 別ServiceのService ExecutionへEnforcementを適用する

---

## 46. 従属請求項候補群F：Policy Evaluation

1. Entitlementを評価する
2. Entitlement Stateを評価する
3. Entitlement Validityを評価する
4. Authentication Resultを評価する
5. Timeを評価する
6. Locationを評価する
7. Security Contextを評価する
8. Transactionを評価する
9. 複数条件を組み合わせる
10. Conditional Policyを評価する

---

## 47. 従属請求項候補群G：Authorization

1. Authorization Evaluationを実行する
2. Permitを生成する
3. Denyを生成する
4. Indeterminateを生成する
5. Permit with Conditionsを生成する
6. Revalidation Requiredを生成する
7. Policyに基づいてIndeterminateを処理する

---

## 48. 従属請求項候補群H：Enforcement

1. Authorization DecisionをEnforcementへ入力する
2. Permitの場合にService Executionを許可する
3. Denyの場合にService Executionを拒否する
4. Indeterminateの場合に再評価する
5. Enforcementで条件を再確認する
6. EnforcementでEntitlement Stateを確認する
7. EnforcementでEntitlement Validityを確認する
8. EnforcementでRevocationを確認する

---

## 49. 従属請求項候補群I：Security

1. Security Contextを評価する
2. Device Contextを評価する
3. Location Contextを評価する
4. Network Contextを評価する
5. Risk Contextを評価する
6. Authentication Freshnessを評価する
7. Replayを検出する
8. Fail-Closedを実行する
9. Revalidationを実行する

---

## 50. 従属請求項候補群J：State

1. Authentication Object State
2. Entitlement State
3. Authorization Decision State
4. Expired State
5. Revoked State
6. Suspended State
7. State Transition
8. State変更をAuditする
9. State変更をTransactionへ関連付ける

---

## 51. 従属請求項候補群K：Transaction

1. Transaction Identifierを生成する
2. Authentication ResultをTransactionへ関連付ける
3. EntitlementをTransactionへ関連付ける
4. Policy EvaluationをTransactionへ関連付ける
5. Authorization DecisionをTransactionへ関連付ける
6. EnforcementをTransactionへ関連付ける
7. Service ExecutionをTransactionへ関連付ける
8. Transaction状態を管理する

---

## 52. 従属請求項候補群L：Evidence

Evidenceは処理結果または状態を示す情報として構成できる。

例えば、

- Authentication Evidence
- Entitlement Evidence
- Policy Evaluation Evidence
- Authorization Evidence
- Enforcement Evidence
- Service Execution Evidence

を生成または保存できる。

ただしEvidenceそのものを発明の中心とせず、
必要に応じて従属請求項または明細書で展開する。

---

## 53. 従属請求項候補群M：Audit

Audit情報として、

- Authentication
- Entitlement
- Policy Evaluation
- Authorization Decision
- Enforcement
- Service Execution
- State Transition
- Error
- Revocation
- Expiration

等の履歴を記録できる。

AuditはProtocol Objectまたは処理そのものとは区別する。

---

## 54. 実施形態として重要なScenario A

Authenticationにより対象を確認する。

Authenticationに使用されるQR Code等のAuthentication Objectは
短いValidity Periodを有する。

Authenticationが成功するとAuthentication Resultを生成する。

Authentication Resultに基づいてEntitlementを生成する。

EntitlementはAuthentication Objectより長いValidity Periodを有する。

別ServiceにおいてEntitlementをPolicy Evaluationする。

Authorization DecisionとしてPermitを生成する。

EnforcementによってService ExecutionへPermitを適用する。

---

## 55. 実施形態として重要なScenario B

Authentication ObjectがExpirationした後も、
対応するEntitlementがActiveである。

そのため、Authentication Objectを再利用する必要なく、
既に発生したEntitlementを別のServiceで利用できる。

この構成は、

Authenticationの一時性
と
Entitlementの持続性

を明確に分離する実施形態である。

---

## 56. 実施形態として重要なScenario C

EntitlementがExpirationまたはRevocationした場合、
Authentication Resultが有効であっても、
当該Entitlementを根拠としてService ExecutionをPermitしない。

この構成により、

Authentication Validity
と
Entitlement Validity

とを独立して管理できる。

---

## 57. 実施形態として重要なScenario D

対象が実店舗へ来店したことをAuthentication等により確認する。

その確認結果に基づいて、

「来店済み」

というEntitlementを生成する。

当該EntitlementはEC ServiceにおいてPolicy Evaluationされる。

Policyが、

「来店済みEntitlementを有する対象には10%割引を許可する」

と定めている場合、

Authorization Decision = Permit

を生成する。

EnforcementはPermitを割引処理に適用し、
Service Executionとして割引を実行する。

---

## 58. Scenario Dの特許上の意味

Scenario Dは単なる「来店者割引」そのものを発明とするものではない。

重要なのは、

1. Authentication等により事実を確認する
2. その確認結果からEntitlementを生成する
3. EntitlementをAuthenticationとは独立して管理する
4. 異なるServiceでEntitlementを評価する
5. Policyに基づいてAuthorization Decisionを生成する
6. Enforcementを介して別Serviceの処理を許可する

という情報処理構造である。

---

## 59. 独立請求項の抽象化レベル

独立請求項は、特定の業務用途に限定しない。

したがって、

「来店」

「割引」

「EC」

「店舗」

等を必須要件とはしない。

これらは実施形態として明細書に記載し、
発明の一般化された構造を請求項で保護する。

---

## 60. 独立請求項における時間概念

時間概念は二層以上に分離して扱う。

第一の時間概念はAuthentication ObjectまたはAuthenticationに
関連するValidityである。

第二の時間概念はEntitlementのValidityである。

さらに必要に応じて、

- Policy Validity
- Authorization Decision Validity
- Session Validity
- Transaction Validity

等を追加できる。

この多層的なValidity構造は、Phase 3で請求項への組込み方を検討する。

---

## 61. Authentication ValidityとEntitlement Validityの独立性

本発明では、

Authentication ObjectがExpirationすること

と

EntitlementがExpirationすること

とは別の状態変更として扱うことができる。

例えば、

t0:
Authentication Object生成

t1:
Authentication成功

t2:
Entitlement生成

t3:
Authentication Object Expiration

t4:
Entitlement Active

t5:
Entitlement Expiration

という時間関係を構成できる。

この関係は、本発明の重要な実施形態として明細書および図面へ展開する。

---

## 62. 発明を狭めないための表現

Phase 3の請求項では、以下のような表現を慎重に使用する。

「QR Codeを撮影して」

ではなく、

「一時的なAuthentication Objectを取得して」

「30秒後に失効する」

ではなく、

「所定のValidity Periodが経過した場合にExpirationする」

「来店した」

ではなく、

「所定の条件の成立を示すAuthentication Result」

「10%割引」

ではなく、

「所定のService Actionに関するEntitlement」

等へ一般化する。

---

## 63. 独立請求項の候補構造

独立請求項候補の基本構造は以下とする。

### 構成A

対象についてAuthenticationを実行し、
Authentication Resultを取得する。

### 構成B

Authentication Resultに関連するEntitlementを、
Authentication Resultとは独立した情報として管理する。

### 構成C

Policyに基づいてEntitlementおよびその他のContextを評価する。

### 構成D

評価結果に基づいてAuthorization Decisionを生成する。

### 構成E

Authorization DecisionをEnforcementに適用する。

### 構成F

Enforcementに基づいてService Executionを制御する。

### 構成G

Authentication ObjectのValidityとEntitlementのValidityを
独立して設定または管理可能とする。

ただし、Gを独立請求項の必須構成とするか、
従属請求項へ配置するかはPhase 3で最終判断する。

---

## 64. 独立請求項候補の優先順位

現時点では以下の優先順位とする。

### Candidate 1

Entitlementを中心とする情報処理システム

### Candidate 2

Entitlementを中心とする情報処理方法

### Candidate 3

上記方法をコンピュータに実行させるプログラム

### Candidate 4

プログラムを記録した記録媒体

Candidate 1およびCandidate 2を中心として、
Candidate 3およびCandidate 4を展開する。

---

## 65. Claim 1における必須構成候補

現時点のClaim 1候補は以下の概念とする。

1. Authenticationを実行する手段
2. Authentication Resultを取得する手段
3. Authentication Resultとは独立したEntitlementを管理する手段
4. Policyに基づいてEntitlementを評価する手段
5. Authorizationを評価する手段
6. Authorization Decisionを生成する手段
7. Authorization DecisionをEnforcementに適用する手段
8. Enforcementに基づいてService Executionを制御する手段

この段階では、最終的な法的文言にはしない。

---

## 66. Claim 1候補に対する時間要件

時間要件については二案を維持する。

### Option A

Claim 1にはAuthenticationとEntitlementの独立性を中心に置き、
Validityの具体的関係は従属請求項とする。

### Option B

Claim 1に、

「Authenticationに関連する第一のValidityと、
Entitlementに関連する第二のValidityとを独立して管理する」

という構成を含める。

Phase 3では、先行技術との差異および発明の本質を比較し、
Option A/Bを決定する。

---

## 67. QR Codeに関する従属請求項候補

例えば以下のような構成を候補とする。

「前記Authentication ObjectがQR Codeとして表示される」

「前記QR Codeが所定のValidity Periodを有する」

「前記QR CodeがExpiration後にAuthenticationに使用できない」

「前記QR CodeがOne-time Useである」

ただし、最終文言では日本国特許実務に適した表現へ修正する。

---

## 68. 30秒に関する扱い

30秒は発明の必須要件ではない。

30秒は、

「Authentication ObjectのValidity Periodを30秒とする一実施形態」

として明細書に記載できる。

請求項では、

「所定の時間」

「所定のValidity Period」

「所定条件の成立」

等の一般化された表現を基本とする。

---

## 69. WebAuthnに関する扱い

WebAuthnはAuthentication mechanismの一実施形態である。

したがって、

Authentication
↓
Authentication Result

という構造を維持したまま、
WebAuthnを利用できる。

WebAuthn自体を発明の必須要件とはしない。

---

## 70. Public Key Cryptographyに関する扱い

Public Key CryptographyはAuthenticationの実施形態として利用できる。

例えば、

- Smartphone側に秘密鍵
- Server側に公開鍵
- Challenge-Response
- Authentication Result

という構成を採用できる。

ただし、独立請求項では特定の鍵保管方式に限定しない。

---

## 71. Secure Enclave / TEEに関する扱い

Secure Enclave、TEEその他のHardware Security mechanismは、
Authenticationの安全性を補強する実施形態として記載できる。

しかし、これらを独立請求項の必須構成としない。

---

## 72. 先行技術との差異として重点検討する事項

Phase 3では、少なくとも以下の先行技術との関係を検討する。

- Password Authentication
- Session Authentication
- OAuth
- OAuth Device Authorization
- FIDO Cross-Device Authentication
- Passkey
- QR Login
- Access Token
- Capability Token
- Entitlement-based Access Control
- Attribute-based Access Control
- Role-based Access Control
- Policy-based Access Control

ただし、本書では先行技術の法的評価を確定しない。

---

## 73. 先行技術との差異の基本仮説

現時点での差異仮説は、

「AuthenticationそのものをAuthorizationの根拠とするのではなく、
Authentication Resultとは独立してEntitlementを生成または管理し、
当該EntitlementをPolicy Evaluationの対象としてAuthorizationを決定し、
そのDecisionをEnforcementを介してService Executionへ適用する」

点にある。

さらに、

「Authentication Objectの短いValidityとEntitlementの異なるValidityを
独立して管理できる」

点を補助的な差異軸とする。

この差異仮説はPhase 3で先行技術調査結果に基づいて再評価する。

---

## 74. 発明の業務用途による限定を避ける

本発明は、

- 小売
- EC
- 医療
- 金融
- 教育
- 会員サービス
- イベント
- 入退場
- クーポン
- ポイント
- コンテンツ
- API
- B2Bサービス

等に適用可能である。

これらは発明の用途例であり、
請求項の中心構成を特定業務へ限定しない。

---

## 75. 発明の技術分野の一般化

本発明は、

「認証された主体に対するアクセス制御」

に限定せず、

「Authenticationにより得られた情報を起点として、
独立したEntitlementを生成・管理し、
Policy Evaluationに基づいてAuthorizationを決定し、
Service Executionへ適用する情報処理」

として記載する。

---

## 76. 特許請求項における「独立性」の意味

本書でいうAuthentication ResultとEntitlementの独立性は、
必ずしも物理的なDatabaseが異なることを意味しない。

同一のDatabase、同一のServer、同一のProcess内で管理されてもよい。

重要なのは、

- 情報の意味
- Lifecycle
- Validity
- State
- Processing role
- Authorization上の役割

が区別されることである。

---

## 77. Validityの独立性の意味

Authentication ValidityとEntitlement Validityの独立性は、
単に異なる数値を設定することだけを意味しない。

それぞれについて、

- Validity Period
- Expiration
- State
- Revocation
- Revalidation

を独立して管理できることを含む。

---

## 78. AuthorizationとAuthenticationの非同一性

本発明では、

Authentication Success
≠
Authorization Permit

とする。

Authenticationは対象または事実を確認する処理であり、
AuthorizationはPolicyおよびEntitlementその他の条件に基づいて
Service Actionの実行可否を決定する処理である。

この非同一性を請求項および明細書の中心的な論理として維持する。

---

## 79. EntitlementとAuthorizationの非同一性

同様に、

Entitlement
≠
Authorization Decision

とする。

Entitlementは権利または資格等を表す情報であり、
Authorization Decisionは特定のServiceまたはActionに対する
処理可否を示すDecisionである。

EntitlementはPolicy EvaluationによってAuthorization Decisionの
入力条件となり得る。

---

## 80. PolicyとAuthorization Decisionの非同一性

Policyは評価ルールであり、
Authorization Decisionは当該Policyに基づく個別のDecisionである。

したがって、

Policy
≠
Authorization Decision

とする。

---

## 81. Authorization DecisionとEnforcementの非同一性

Authorization DecisionはDecisionであり、
EnforcementはそのDecisionをService Executionへ適用する処理である。

したがって、

Authorization Decision
≠
Enforcement

とする。

---

## 82. EnforcementとService Executionの非同一性

EnforcementはService Executionの実行可否または実行条件を制御する。

したがって、

Enforcement
≠
Service Execution

とする。

---

## 83. 本発明の階層構造

本発明の論理階層は、

Authentication
↓
Entitlement
↓
Policy Evaluation
↓
Authorization
↓
Enforcement
↓
Service Execution

として整理できる。

ただし、Authentication Result、Authorization Evaluation、
Authorization Decision、Security Context、Transaction等を
各層間の情報および処理として配置する。

---

## 84. 請求項で保護すべき中心的関係

請求項では個々の名称よりも以下の関係を重視する。

Authentication Result
→
Entitlement

Entitlement
→
Policy Evaluation

Policy Evaluation
→
Authorization Evaluation

Authorization Evaluation
→
Authorization Decision

Authorization Decision
→
Enforcement

Enforcement
→
Service Execution

さらに、

Authentication Validity
と
Entitlement Validity

を独立して管理できる。

---

## 85. Phase 3で決定すべき事項

Phase 3では以下を決定する。

1. Claim 1の最終構成
2. System Claimの文言
3. Method Claimの文言
4. Program Claimの文言
5. Record Medium Claimの採否
6. Authentication/Entitlement分離をClaim 1に入れるか
7. Validity分離をClaim 1に入れるか
8. Cross-ServiceをClaim 1に入れるか
9. Conditional Entitlementを従属請求項とするか
10. Policy Evaluationの範囲
11. Authorization Evaluationの範囲
12. Enforcementの必須性
13. Permit/Deny/Indeterminateの配置
14. Revalidationの配置
15. Revocationの配置
16. Replay/Freshnessの配置
17. Security Contextの配置
18. Transactionの配置
19. Stateの配置
20. Evidence/Auditの配置

---

## 86. Phase 2における暫定Claim Architecture

現時点では以下の構造を暫定採用する。

### Independent Claim 1

情報処理システム

中心：
Authentication Result
+
独立Entitlement
+
Policy Evaluation
+
Authorization
+
Enforcement
+
Service Execution

### Independent Claim 2

情報処理方法

Claim 1に対応する処理ステップ。

### Independent Claim 3

プログラム

Claim 2の処理をコンピュータに実行させるプログラム。

### Independent Claim 4

記録媒体

必要に応じてClaim 3に対応する記録媒体。

---

## 87. 従属請求項の概略構成

従属請求項は以下のグループへ整理する。

Group A:
Authentication

Group B:
Authentication Object Validity

Group C:
Replay/Freshness

Group D:
Entitlement

Group E:
Conditional Entitlement

Group F:
Cross-Service

Group G:
Entitlement Validity

Group H:
Policy Evaluation

Group I:
Authorization Decision

Group J:
Enforcement

Group K:
Security Context

Group L:
Transaction

Group M:
State

Group N:
Expiration / Revocation

Group O:
Revalidation / Fail-Closed

Group P:
Evidence / Audit

---

## 88. Claim Dependency Strategy

従属請求項では、一つの請求項に過度に多くの条件を詰め込まない。

例えば、

Authentication Object Validity

と

Entitlement Validity

と

Cross-Service

と

QR Code

を一つの従属請求項に同時に詰め込むのではなく、
それぞれを独立した請求項群として展開できる構造を維持する。

これにより、先行技術との関係に応じて請求項を補正しやすくする。

---

## 89. Fallback Structure

独立請求項が先行技術との関係で狭める必要が生じた場合に備え、
以下のFallbackを準備する。

### Fallback 1

Authentication ResultとEntitlementの分離

### Fallback 2

EntitlementのPolicy Evaluation

### Fallback 3

Cross-Service Entitlement

### Fallback 4

Authentication ValidityとEntitlement Validityの独立管理

### Fallback 5

Authorization DecisionとEnforcementの分離

### Fallback 6

Conditional Entitlement

### Fallback 7

Revalidation / Revocation / Expiration

---

## 90. 発明の核心に対するFallback優先順位

Fallbackの優先順位は、

1. Entitlementの独立管理
2. Policy Evaluation
3. Authorization Decision
4. Enforcement
5. Service Executionへの適用
6. AuthenticationとEntitlementのValidity分離
7. Cross-Service
8. Conditional Entitlement
9. Security Context
10. Transaction / State

とする。

ただし、この優先順位は先行技術調査後に変更可能とする。

---

## 91. Claim Construction上の注意

「認証されたユーザ」

という一つの状態だけを請求項の中心にしない。

Authentication Result、Entitlement、Policy Evaluation、
Authorization Decision、Enforcementを区別して記載する。

また、

「アクセスを許可する」

という抽象的な結果だけではなく、

Authorization Decisionを生成し、
そのDecisionをEnforcementに適用し、
Service Executionを制御する

という処理関係を明確にする。

---

## 92. 物理構成への過度な限定を避ける

Server、Client、Smartphone、Web Browser等の物理構成は、
実施形態として記載できる。

独立請求項では、これらの具体的配置に過度に限定しない。

処理が複数装置に分散して実行されてもよい。

また、複数のService間でEntitlementが共有または参照されてもよい。

---

## 93. Distributed Architecture

本発明はDistributed Architectureにも適用できる。

例えば、

Authentication Service
Entitlement Service
Policy Service
Authorization Service
Enforcement Service
Business Service

を異なるServerまたはServiceとして構成できる。

また、これらを一つのServerに実装することもできる。

---

## 94. API Architecture

各処理間はAPIを介して連携できる。

例えば、

Authentication API
Entitlement API
Policy API
Authorization API
Enforcement API
Service API

等を構成できる。

ただし、特定のAPI形式を独立請求項の必須構成とはしない。

---

## 95. Data Object Architecture

本発明では、少なくとも以下の情報オブジェクトを区別できる。

Authentication Object
Authentication Result
Entitlement Object
Policy Object
Authorization Evaluation
Authorization Decision
Enforcement State
Service Execution Result

これらは物理的なデータ形式を限定しない。

---

## 96. Authentication ObjectとEntitlement Object

Authentication ObjectはAuthenticationのために使用される。

Entitlement Objectは権利、資格、条件その他のEntitlementを表す。

両者は、

- Lifecycle
- Validity
- Expiration
- State
- Processing purpose

の少なくとも一つを異ならせることができる。

---

## 97. Service Execution Result

Service Executionの結果を取得または保存できる。

例えば、

- Success
- Failure
- Partial Success
- Rejected
- Cancelled

等を含み得る。

Service Execution ResultはAuthorization Decisionとは区別する。

---

## 98. Error Handling

ErrorはAuthorization Decisionとは区別する。

例えば、

Authentication Error
Entitlement Error
Policy Error
Authorization Error
Enforcement Error
Service Execution Error

をそれぞれ管理できる。

Error発生時にはPolicyに基づいて、

- Deny
- Revalidation
- Retry
- Recovery
- Termination
- Fail-Closed

等を実行できる。

---

## 99. EvidenceとAudit

EvidenceおよびAuditは、
Authentication、Entitlement、Policy Evaluation、Authorization、
EnforcementおよびService Executionの処理結果または履歴を記録する。

ただし、EvidenceおよびAuditをProtocol Objectそのものと
同一視しない。

これらは請求項の中心ではなく、必要に応じて従属請求項および
明細書で保護する。

---

## 100. Phase 2 Final Architecture

Phase 2の現時点における最終的な請求項骨格は以下とする。

### Core

Authentication
→
Authentication Result
→
Entitlement
→
Policy Evaluation
→
Authorization Evaluation
→
Authorization Decision
→
Enforcement
→
Service Execution

### Key Separation

Authentication Result
≠
Entitlement

Entitlement
≠
Authorization Decision

Authorization Decision
≠
Enforcement

Enforcement
≠
Service Execution

### Key Temporal Separation

Authentication Object Validity
≠
Entitlement Validity

### Key Application

EntitlementをAuthenticationとは異なるServiceで利用可能

### Key Decision Model

Policy Evaluation
→
Authorization Evaluation
→
Authorization Decision

### Key Execution Model

Authorization Decision
→
Enforcement
→
Service Execution

---

## 101. Phase 2 Final Statement

本書における請求項骨格は、Version 2.0の発明を
「QR CodeによるAuthentication」
として限定するものではない。

本発明の特許上の中心候補は、

「Authenticationによって得られたAuthentication Resultと独立して
Entitlementを管理し、当該EntitlementをPolicyに基づいて評価し、
Authorization Decisionを生成し、そのDecisionをEnforcementを介して
Service Executionへ適用する情報処理」

にある。

さらに、

「Authenticationに関連する一時的なValidityと、
Entitlementに関連する独立したValidityを異なるLifecycleとして
管理できる」

ことを重要な補強構成として位置付ける。

QR Code、30秒、WebAuthn、Smartphone、来店、割引等は
実施形態および従属請求項候補として扱い、
発明の中心をそれらに限定しない。

Phase 3では、本書を基礎として、
実際の独立請求項および従属請求項の文言を作成する。

