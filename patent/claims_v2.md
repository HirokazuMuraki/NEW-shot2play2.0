# NEW-shot2play Technical Specification Version 2.0
# Phase 3 — 特許請求の範囲 初稿

## 1. 本文書の目的

本書は、NEW-shot2play Technical Specification Version 2.0に
基づき、特許出願における「特許請求の範囲」の初稿を作成するための
文書である。

本書は、Phase 1で確定した発明の核心およびPhase 2で整理した
Claim Architectureを、実際の請求項構造へ展開することを目的とする。

本書の段階では、最終的な出願書類としての文言を確定するのではなく、
発明の保護範囲、独立請求項の構成、従属請求項によるFallback Structure、
および各構成要件と発明の核心との関係を明確にする。

特に、独立請求項において特定の認証方式、通信方式、装置構成、
数値、業務用途等に不必要に限定されることを避ける。

---

## 2. 発明の名称

情報処理システム、情報処理方法、プログラム及び記録媒体

---

## 3. 請求項設計の基本方針

本発明の請求項は、Authenticationそのものを発明の中心とするのではなく、
Authenticationによって確認された結果と、当該Authenticationとは
独立して管理され得るEntitlementとを用いてPolicy Evaluationを行い、
その評価に基づいてAuthorizationを決定し、そのAuthorization Decision
をEnforcementを介してService Executionへ結び付ける情報処理構造を
中心として構成する。

基本的な処理関係は、

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

である。

ただし、上記の各処理が必ずしも単一の装置または単一のプロセスに
よって実行されることを要しない。

また、Authentication ResultとEntitlementは同一の情報として
扱われる必要はなく、Authenticationの完了後にEntitlementが
生成、確認、更新、停止、失効またはその他の状態変化を受け得る。

---

## 4. 独立請求項における発明の最小構成

本発明の独立請求項における基本的な構成は、少なくとも以下を含む。

1. 対象または主体についてAuthenticationを実行すること。
2. Authenticationの結果を示すAuthentication Resultを取得すること。
3. Authentication Resultとは独立して管理され得るEntitlementを
   取得、生成または確認すること。
4. Authentication ResultおよびEntitlementを含む一以上の情報に基づいて
   Policy Evaluationを実行すること。
5. Policy Evaluationの結果に基づいてAuthorization Decisionを生成すること。
6. Authorization Decisionに基づいてEnforcementを実行すること。
7. Enforcementを介してService Executionを制御すること。

---

## 5. 独立請求項におけるAuthenticationの位置付け

Authenticationは、本発明における一つの情報処理要素である。

Authenticationは、例えば、

- パスワード認証
- 多要素認証
- 公開鍵暗号方式による認証
- 生体認証
- WebAuthn
- Passkey
- QR Codeを利用する認証
- デバイス認証
- トークン認証
- セッション認証
- その他の主体または対象を確認する認証方式

のいずれであってもよい。

したがって、独立請求項では特定のAuthentication mechanismを
必須構成として限定しない。

---

## 6. Authentication Result

Authentication Resultは、Authenticationの結果として生成または
取得される情報である。

Authentication Resultは、例えば、

- Authenticationが成功したこと
- Authenticationが失敗したこと
- Authenticationされた主体
- Authenticationされた対象
- Authenticationの時刻
- Authenticationに使用された認証方式
- Authenticationに関連するIdentifier
- Authenticationの信頼度
- AuthenticationのValidity
- Authenticationの状態

の一以上を示す情報を含むことができる。

Authentication Resultは、Entitlementそのものとは異なる情報として
管理される。

---

## 7. Entitlement

Entitlementは、主体、対象、サービス、資源、取引またはその他の対象に
関連して、所定の権利、資格、条件、属性または利用可能性を示す情報である。

Entitlementは、Authentication Resultとは独立したObjectとして
生成、保存、更新、停止、失効、再有効化またはその他の状態変化を
受けることができる。

Entitlementは、Authenticationの完了時に必ず生成される必要はない。

また、Entitlementは、Authenticationとは異なる時点、異なる処理、
異なるサービス、または異なるシステムによって生成または管理されてもよい。

---

## 8. Authentication ResultとEntitlementの独立性

Authentication Resultは、Authenticationが成立したことを示す情報であり、
Entitlementは、所定の権利または資格が成立していることを示す情報である。

したがって、

Authentication Result = Entitlement

として扱う必要はない。

Authenticationが成功していても、対応するEntitlementが存在しない、
期限切れである、停止されている、失効している、またはPolicy上利用できない
場合がある。

逆に、ある時点で生成されたEntitlementが、その後のAuthentication
とは独立して利用される場合がある。

---

## 9. Authentication ObjectのValidity

Authenticationに使用されるAuthentication Objectは、
必要に応じて所定のValidity Periodを有することができる。

Authentication ObjectのValidity Periodは、EntitlementのValidity Period
とは独立して設定され得る。

Authentication ObjectがValidity Periodを経過した場合、
当該Authentication ObjectをAuthenticationに使用できない構成と
することができる。

例えば、Web画面に表示されたQR CodeがAuthentication Objectとして
使用される場合、QR Codeの生成時点から所定の時間のみAuthentication
に使用可能とすることができる。

---

## 10. Authentication ObjectのExpiration

Authentication ObjectにはExpirationを設定することができる。

Expirationは、Authentication ObjectがAuthenticationに使用可能な
期間の終了を示す。

Authentication ObjectがExpirationした場合、当該Objectを使用した
Authenticationを拒否することができる。

Authentication ObjectのExpirationは、EntitlementのExpirationとは
別個に管理される。

---

## 11. Authentication ValidityとEntitlement Validity

Authentication ObjectのValidityとEntitlementのValidityは、
異なる時間的条件として管理され得る。

例えば、Authentication ObjectのValidity Periodを30秒とし、
Authenticationが成功した結果として生成されたEntitlementを
数時間または数日間有効とすることができる。

したがって、

短時間有効なAuthentication Object
+
Authentication Result
+
長時間有効なEntitlement

という構成が可能である。

Authentication ObjectのValidityが終了したことは、
それ自体によってEntitlementのValidityが終了したことを意味しない。

また、EntitlementのValidityが終了したことは、
過去のAuthentication Resultが無効なAuthenticationであったことを
意味しない。

---

## 12. Authentication Freshness

AuthenticationにはFreshness条件を設定することができる。

Freshnessは、Authentication ResultまたはAuthentication Objectが
現在の処理において十分に新しいものであるかを示す条件である。

Policyは、Authentication Resultについて所定のFreshnessを要求する
ことができる。

Freshness条件が満たされない場合、Policy Evaluationにおいて
再Authentication、再Validation、Deny、Indeterminateまたはその他の
所定の処理を行うことができる。

---

## 13. Replay防止

Authentication Objectは、必要に応じてOne-time Useその他の
Replay防止条件を有することができる。

Authentication Objectが既に使用された場合、同一のObjectを使用した
後続のAuthenticationを拒否することができる。

Replay防止はAuthentication ObjectのValidityとは別個の条件として
設定することができる。

---

## 14. Conditional Entitlement

Entitlementは、所定の条件が成立した場合にのみ生成、付与、
有効化または利用可能とすることができる。

例えば、

- 所定の場所への来店
- 所定のAuthenticationの成功
- 所定の取引の成立
- 所定のサービスの利用
- 所定の期間の経過
- 所定の状態への遷移
- 所定の属性の成立

等を条件としてEntitlementを生成または有効化することができる。

---

## 15. Cross-Service Entitlement

Entitlementは、Entitlementが生成または確認されたServiceとは
異なるServiceにおいて利用することができる。

例えば、あるServiceにおけるAuthenticationまたは行動によって
Entitlementを生成し、当該Entitlementを別のServiceにおける
Authorizationの条件として利用することができる。

これにより、

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
↓
Service Execution

というCross-Service処理が可能となる。

---

## 16. Policy

Policyは、Authentication Result、Entitlement、Subject、
Resource、Action、Security Context、Transaction、時間条件、
Object Stateその他の情報に基づいて、所定のService Executionを
許可、拒否またはその他の状態として処理するための条件を示す。

Policyは、業務用途に応じて定義することができる。

---

## 17. Policy Evaluation

Policy Evaluationは、Policyと一以上の入力情報とを評価する処理である。

入力情報は、例えば、

- Authentication Result
- Entitlement
- Subject
- Resource
- Action
- Security Context
- Transaction
- Object State
- 時間情報
- その他のContext

を含むことができる。

Policy Evaluationは、Authorization Decisionそのものとは異なる
処理として実行される。

---

## 18. Authorization Evaluation

Authorization Evaluationは、Policy Evaluationその他の必要な
情報に基づいて、所定のActionについてAuthorizationを成立させる
ための評価処理である。

Authorization Evaluationは、少なくとも、

- Authentication Result
- Entitlement
- Policy
- Security Context
- Resource
- Action
- Object State

の一以上を考慮することができる。

---

## 19. Authorization Decision

Authorization Decisionは、Authorization Evaluationの結果として
生成されるDecisionである。

Authorization Decisionは、例えば、

- Permit
- Deny
- Indeterminate

のいずれかを示すことができる。

Authorization Decisionは、Policyそのものでも、Policy Evaluationそのもの
でも、Enforcementそのものでもない。

---

## 20. Permit

Permitは、所定のActionまたはService Executionについて、
必要なAuthorization条件が成立したことを示すDecisionである。

Permitが生成された場合でも、Enforcementにおいて追加の条件確認を
行うことができる。

したがって、Permitの生成とService Executionの実行は同一の処理である
必要はない。

---

## 21. Deny

Denyは、所定のActionまたはService Executionについて、
Authorizationが成立しないことを示すDecisionである。

Denyが生成された場合、Enforcementは、対応するService Executionを
拒否、停止、終了またはその他のPolicyに従った処理を行う。

---

## 22. Indeterminate

Indeterminateは、必要な情報または条件を確定できず、
PermitまたはDenyを確定できない状態を示すDecisionである。

Indeterminateの場合、Policyに応じて、

- Deny
- 再Evaluation
- Revalidation
- 再Authentication
- 処理停止
- 処理終了
- その他の所定のRecovery

を実行することができる。

---

## 23. Enforcement

Enforcementは、Authorization DecisionをService Executionに
適用する処理である。

Enforcementは、例えば、

- Service Executionの許可
- Service Executionの拒否
- Actionの制限
- Resourceへのアクセス制限
- 処理の停止
- 処理の終了
- 追加条件の確認

を実行することができる。

EnforcementはAuthorization Decisionそのものとは異なる。

---

## 24. Service Execution

Service Executionは、Authorization DecisionおよびEnforcementに
基づいて実行される保護対象のService処理である。

Service Executionは、例えば、

- 情報の取得
- 情報の変更
- 資源へのアクセス
- 商品またはサービスの提供
- 決済
- 割引
- 特典の付与
- API処理
- その他の業務処理

を含むことができる。

---

## 25. Security Context

Security Contextは、処理主体、装置、通信、Session、Transaction、
認証状態、Entitlement状態、Object Stateその他のSecurityに関する
Context情報を含むことができる。

Policy EvaluationまたはAuthorization Evaluationは、
Security Contextを考慮することができる。

---

## 26. Transaction

Transactionは、一以上のProtocol ProcessingまたはService Processing
を関連付けるための処理単位である。

Transactionは、

- Transaction Identifier
- 開始時刻
- 終了時刻
- Processing State
- Security Context
- Authentication Result
- Entitlement
- Authorization Decision
- Service Execution Result

その他の情報を関連付けることができる。

---

## 27. Object State

Authentication Object、Entitlement、Transactionその他のProtocol Object
は、所定のStateを有することができる。

Stateは、例えば、

- Created
- Active
- Suspended
- Expired
- Revoked
- Consumed
- Completed
- Failed

その他の状態を含むことができる。

---

## 28. StateとAuthorizationの分離

Object Stateは、Authorization Decisionそのものではない。

例えば、EntitlementがActiveであることはAuthorization Decisionを
直接意味するものではなく、Policy EvaluationおよびAuthorization
Evaluationにおける入力情報の一つとなり得る。

同様に、EntitlementがExpiredまたはRevokedである場合、
Policyに従ってAuthorization DecisionとしてDenyまたはその他の
結果が生成され得る。

---

## 29. Revocation

Entitlementその他のObjectは、Validity Periodの終了とは独立して
Revocationされることができる。

RevocationされたObjectは、Policyに従ってAuthorization Evaluation
において無効として扱うことができる。

RevocationはExpirationとは異なる状態変化として管理される。

---

## 30. Revalidation

Policyは、Authentication Result、Entitlement、Security Context、
Object Stateその他の情報についてRevalidationを要求することができる。

Revalidationが要求された場合、対象情報を再確認することができる。

Revalidationが成功しない場合、Policyに従ってDeny、Indeterminate、
処理停止、再Authenticationその他の処理を行うことができる。

---

## 31. Fail-Closed

Security上必要な条件を確立できない場合、Policyに応じて
Fail-Closed処理を適用することができる。

Fail-Closedが要求されている場合、必要なSecurity条件が確立されない
状態で保護対象Service Executionを実行してはならない。

---

# 32. 独立請求項

## 【請求項1】

Authenticationを実行するAuthentication処理部と、

前記Authenticationの結果を示すAuthentication Resultを取得する
Authentication Result処理部と、

前記Authentication Resultとは独立して管理され得るEntitlementを
取得、生成または確認するEntitlement処理部と、

前記Authentication Resultおよび前記Entitlementを含む一以上の
情報とPolicyとに基づいてPolicy Evaluationを実行する
Policy Evaluation処理部と、

前記Policy Evaluationの結果に基づいてAuthorization Decisionを
生成するAuthorization処理部と、

前記Authorization Decisionに基づいてEnforcementを実行する
Enforcement処理部と、

前記Enforcementに基づいて保護対象のService Executionを制御する
Service Execution処理部と、

を備え、

前記Authentication Resultと前記Entitlementとを別個の情報として
取り扱い、

前記EntitlementのValidityを前記Authenticationに使用される
Authentication ObjectのValidityとは独立して管理可能とし、

前記Authorization Decisionに基づいて前記Service Executionの
実行を制御する、

情報処理システム。

---

## 【請求項2】

前記Authentication Objectが所定のValidity Periodを有し、

前記Validity Periodの経過後に前記Authentication Objectを使用した
Authenticationを無効として扱う、

請求項1に記載の情報処理システム。

---

## 【請求項3】

前記Authentication ObjectがExpirationを有し、

前記Authentication ObjectがExpirationした場合に、
当該Authentication Objectを使用したAuthenticationを拒否する、

請求項1または2に記載の情報処理システム。

---

## 【請求項4】

前記Authentication ObjectのValidity Periodが、前記Entitlementの
Validity Periodとは異なる、

請求項1から3のいずれか一項に記載の情報処理システム。

---

## 【請求項5】

前記Authentication ObjectのValidity Periodが、前記Entitlementの
Validity Periodより短い、

請求項4に記載の情報処理システム。

---

## 【請求項6】

前記Authentication ObjectがQR Codeを含む、

請求項1から5のいずれか一項に記載の情報処理システム。

---

## 【請求項7】

前記QR Codeが、Web画面上に表示される、

請求項6に記載の情報処理システム。

---

## 【請求項8】

前記QR Codeが、所定のValidity Periodの間のみAuthenticationに
使用可能である、

請求項6または7に記載の情報処理システム。

---

## 【請求項9】

前記Validity Periodが、QR Codeの生成時点から所定時間として
設定される、

請求項8に記載の情報処理システム。

---

## 【請求項10】

前記Validity Periodが30秒である、

請求項9に記載の情報処理システム。

---

## 【請求項11】

前記Authentication ObjectがOne-time Use条件を有し、

前記Authentication ObjectがAuthenticationに使用された後、
同一のAuthentication Objectを使用した後続のAuthenticationを
無効として扱う、

請求項1から10のいずれか一項に記載の情報処理システム。

---

## 【請求項12】

前記Entitlementが、所定の条件が成立した場合に生成または
有効化される、

請求項1から11のいずれか一項に記載の情報処理システム。

---

## 【請求項13】

前記所定の条件が、所定のAuthenticationの成功、
所定の場所への到達、所定の取引の成立、所定のServiceの利用、
所定の時間条件または所定のObject Stateの成立の一以上を含む、

請求項12に記載の情報処理システム。

---

## 【請求項14】

前記Entitlementが、前記Entitlementを生成または確認したServiceとは
異なるServiceにおけるAuthorizationのために使用される、

請求項1から13のいずれか一項に記載の情報処理システム。

---

## 【請求項15】

前記Entitlementが、前記Authenticationの完了後においても、
前記Authenticationとは独立して保存、更新、停止、失効または
再有効化される、

請求項1から14のいずれか一項に記載の情報処理システム。

---

## 【請求項16】

前記Entitlementが所定のValidity Periodを有し、

前記Entitlementが前記Validity Periodの範囲外にある場合に、
前記Policy Evaluationまたは前記Authorization Evaluationにおいて
当該Entitlementを有効なEntitlementとして扱わない、

請求項1から15のいずれか一項に記載の情報処理システム。

---

## 【請求項17】

前記Entitlementが、前記Validity Periodの終了とは独立して
Revocationされ得る、

請求項16に記載の情報処理システム。

---

## 【請求項18】

前記Policy Evaluationが、前記Authentication Result、
前記Entitlement、Subject、Resource、Action、Security Context、
TransactionまたはObject Stateの一以上を入力として実行される、

請求項1から17のいずれか一項に記載の情報処理システム。

---

## 【請求項19】

前記Authorization Decisionが、Permit、DenyまたはIndeterminateの
いずれかを示す、

請求項1から18のいずれか一項に記載の情報処理システム。

---

## 【請求項20】

前記Indeterminateが生成された場合に、再Authentication、
Revalidation、再Evaluation、Deny、処理停止または処理終了の
一以上を実行する、

請求項19に記載の情報処理システム。

---

## 【請求項21】

前記Enforcementが、前記Authorization DecisionをService Execution
に適用することにより、前記Service Executionの許可、拒否、
制限、停止または終了の一以上を実行する、

請求項1から20のいずれか一項に記載の情報処理システム。

---

## 【請求項22】

前記Service Executionが、情報取得、情報変更、Resourceへのアクセス、
商品またはServiceの提供、決済、割引、特典の付与またはAPI処理の
一以上を含む、

請求項1から21のいずれか一項に記載の情報処理システム。

---

## 【請求項23】

前記AuthenticationについてFreshness条件を設定し、

前記Authentication Resultが前記Freshness条件を満たさない場合に、
再Authentication、Revalidation、DenyまたはIndeterminateの
一以上を実行する、

請求項1から22のいずれか一項に記載の情報処理システム。

---

## 【請求項24】

前記ObjectがCreated、Active、Suspended、Expired、Revoked、
Consumed、CompletedまたはFailedの一以上のStateを有する、

請求項1から23のいずれか一項に記載の情報処理システム。

---

## 【請求項25】

前記ObjectのStateがAuthorization Decisionとは独立して管理され、
前記Stateが前記Policy EvaluationまたはAuthorization Evaluationの
入力情報として使用される、

請求項24に記載の情報処理システム。

---

## 【請求項26】

前記Security Contextが、主体、装置、通信、Session、Transaction、
Authentication状態、Entitlement状態またはObject Stateの
一以上を含む、

請求項1から25のいずれか一項に記載の情報処理システム。

---

## 【請求項27】

前記Transactionが、Authentication Result、Entitlement、
Authorization DecisionまたはService Execution Resultの
一以上を関連付ける、

請求項1から26のいずれか一項に記載の情報処理システム。

---

## 【請求項28】

前記Authenticationが公開鍵暗号方式によって実行される、

請求項1から27のいずれか一項に記載の情報処理システム。

---

## 【請求項29】

前記AuthenticationがWebAuthnまたはPasskeyを用いて実行される、

請求項28に記載の情報処理システム。

---

## 【請求項30】

前記Authenticationに使用される秘密鍵が、Secure Enclave、
Trusted Execution Environmentまたはこれらに相当する保護領域に
保持される、

請求項28または29に記載の情報処理システム。

---

# 33. 情報処理方法

## 【請求項31】

情報処理システムが、

Authenticationを実行するステップと、

前記Authenticationの結果を示すAuthentication Resultを取得する
ステップと、

前記Authentication Resultとは独立して管理され得るEntitlementを
取得、生成または確認するステップと、

前記Authentication Resultおよび前記Entitlementを含む一以上の
情報とPolicyとに基づいてPolicy Evaluationを実行するステップと、

前記Policy Evaluationの結果に基づいてAuthorization Decisionを
生成するステップと、

前記Authorization Decisionに基づいてEnforcementを実行する
ステップと、

前記Enforcementに基づいて保護対象のService Executionを制御する
ステップと、

を含み、

前記Authentication Resultと前記Entitlementとを別個の情報として
取り扱い、

前記EntitlementのValidityを、前記Authenticationに使用される
Authentication ObjectのValidityとは独立して管理可能とする、

情報処理方法。

---

## 【請求項32】

前記Authentication Objectに所定のValidity Periodを設定し、
前記Validity Periodの経過後に当該Authentication Objectを
Authenticationに使用できないものとして扱う、

請求項31に記載の情報処理方法。

---

## 【請求項33】

前記Authentication ObjectのValidity Periodと前記Entitlementの
Validity Periodとを異なる期間として管理する、

請求項31または32に記載の情報処理方法。

---

## 【請求項34】

前記Authenticationによって確認された結果に基づいてEntitlementを
生成または有効化し、前記Entitlementを前記Authenticationとは
異なるServiceにおけるAuthorizationのために使用する、

請求項31から33のいずれか一項に記載の情報処理方法。

---

## 【請求項35】

前記EntitlementのValidityが終了した場合、または前記Entitlementが
Revocationされた場合に、前記Policy Evaluationまたは
Authorization Evaluationにおいて当該Entitlementを有効なEntitlement
として扱わない、

請求項31から34のいずれか一項に記載の情報処理方法。

---

# 34. プログラム

## 【請求項36】

コンピュータに、

Authenticationを実行する処理と、

前記Authenticationの結果を示すAuthentication Resultを取得する
処理と、

前記Authentication Resultとは独立して管理され得るEntitlementを
取得、生成または確認する処理と、

前記Authentication Resultおよび前記Entitlementを含む一以上の
情報とPolicyとに基づいてPolicy Evaluationを実行する処理と、

前記Policy Evaluationの結果に基づいてAuthorization Decisionを
生成する処理と、

前記Authorization Decisionに基づいてEnforcementを実行する処理と、

前記Enforcementに基づいて保護対象のService Executionを制御する
処理と、

を実行させ、

前記Authentication Resultと前記Entitlementとを別個の情報として
取り扱わせ、

前記EntitlementのValidityを、前記Authenticationに使用される
Authentication ObjectのValidityとは独立して管理可能とする、

プログラム。

---

# 35. 記録媒体

## 【請求項37】

請求項36に記載のプログラムを記録した、

コンピュータ読み取り可能な記録媒体。

---

# 36. Claim 1のFallback候補

Claim 1については、先行技術調査およびPhase 3のレビュー結果に応じて、
以下のFallback構造を保持する。

### Fallback A

Authentication ResultとEntitlementの独立性をより強く限定する。

### Fallback B

Authentication ObjectのValidityとEntitlementのValidityとの
時間的独立性を追加する。

### Fallback C

Cross-Service Entitlementを追加する。

### Fallback D

Conditional Entitlementを追加する。

### Fallback E

Policy Evaluationを介してAuthorization Decisionを生成する
構成をより具体的に限定する。

### Fallback F

Authorization DecisionとEnforcementとを分離する構成を追加する。

### Fallback G

Enforcementを介してService Executionを制御する構成を追加する。

---

# 37. Claim 1で避けるべき限定

Claim 1では、原則として以下を必須構成としない。

- QR Code
- 30秒
- WebAuthn
- Passkey
- Secure Enclave
- Trusted Execution Environment
- 特定のスマートフォン
- 特定のWebブラウザ
- 特定のクラウドサービス
- 特定のAPI
- 特定のデータベース
- 特定の業務用途
- 特定の業界
- 特定の通信プロトコル

これらは、必要に応じて従属請求項または明細書の実施形態として
保護範囲に取り込む。

---

# 38. 本発明における時間概念

本発明では、少なくとも以下の時間概念を区別する。

1. Authentication ObjectのValidity
2. Authentication ObjectのExpiration
3. Authentication ResultのFreshness
4. EntitlementのValidity
5. EntitlementのExpiration
6. EntitlementのRevocation時点
7. Transactionの開始および終了
8. Object Stateの変更時点

これらは同一の時間条件として扱われる必要はない。

特に、

Authentication Objectの短時間Validity

と、

Entitlementの長時間Validity

とを独立して管理できることは、本発明の重要な構成関係である。

---

# 39. Scenario D

一実施形態として、あるServiceにおいてAuthenticationを行った
主体についてEntitlementを生成または有効化し、当該Entitlementを
別のEC ServiceにおけるAuthorizationの条件として使用する。

例えば、来店したことをAuthenticationまたはその他の確認処理に
よって確認し、その結果に基づいて「来店済み」というEntitlementを
生成する。

EC Serviceにおいて、Policy Evaluationは当該Entitlementを評価し、
所定の商品または取引についてAuthorization Decisionを生成する。

Permitの場合、Enforcementによって割引その他の特典をService Execution
へ適用する。

この場合、来店時に使用されたAuthentication ObjectのValidityが
30秒であったとしても、「来店済み」というEntitlementを数時間または
所定期間有効とすることができる。

したがって、Authenticationの一時性とEntitlementの時間的有効性を
分離することができる。

---

# 40. Phase 3における最重要構成関係

本発明のClaim Architectureにおいて、特に保護すべき構成関係は
以下である。

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

ただし、Authentication ResultとEntitlementは同一Objectである
必要はなく、Authentication ObjectのValidityとEntitlementのValidity
も同一である必要はない。

この分離により、Authenticationを行ったという事実と、
その後のServiceにおいて現在有効な権利または資格を有することとを
別々の情報処理対象として管理できる。

---

# 41. Phase 3 Final Position

Phase 3の初稿では、Claim 1を情報処理システムとして構成し、
情報処理方法、プログラムおよび記録媒体を別の独立請求項群として
展開した。

Claim 1では、特定のAuthentication mechanismではなく、

Authentication ResultとEntitlementとの分離、

Entitlementの独立管理、

Policy Evaluation、

Authorization Decision、

Enforcement、

Service Execution

の構成関係を中心とした。

また、Authentication ObjectのValidityとEntitlementのValidityを
独立した時間概念として扱う構成をClaim 1に含めた。

QR Code、30秒、WebAuthn、公開鍵暗号、Secure Enclave等は、
発明の具体的実施形態または従属請求項として位置付けた。

今後のPhase 3レビューでは、特に以下を検討する。

1. Claim 1の必須構成が過剰ではないか。
2. Claim 1が発明の核心を十分に捉えているか。
3. Authentication ResultとEntitlementの分離が明確か。
4. Authentication Object ValidityとEntitlement Validityの分離が
   明確か。
5. Policy EvaluationとAuthorization Decisionが混同されていないか。
6. Authorization DecisionとEnforcementが混同されていないか。
7. EnforcementとService Executionが混同されていないか。
8. Cross-Service構成を十分にFallbackとして保持できているか。
9. Conditional Entitlementを適切に従属請求項へ展開できるか。
10. 先行技術との関係でClaim 1をどのFallbackへ縮小できるか。

本書は、Phase 3における請求項文言の検討用初稿であり、
出願前には先行技術調査および日本国特許法上の記載要件、
明確性、サポート要件、実施可能要件等を含む専門的レビューを
行うものとする。

