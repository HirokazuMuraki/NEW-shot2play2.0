# NEW-shot2play Technical Specification Version 2.0
# 日本特許出願用 請求項ドラフト
# Attorney Review Draft

**対象:** 日本特許出願  
**位置付け:** 弁理士レビュー用技術原稿  
**基礎文書:** `patent/claims_v2.md`  
**関連明細書:** `attorney_package/02_application_documents/specification_v2_attorney_review.md`

> 本書は、NEW-shot2play Technical Specification Version 2.0における請求項初稿を基礎として、日本特許出願に向けて弁理士が検討するための技術原稿である。正式な出願用請求項の法的表現、請求項数、従属関係、Claim Scopeその他については、弁理士による最終的な判断および修正を前提とする。

---

# 1. 本書の目的

本書は、NEW-shot2play Technical Specification Version 2.0に基づいて作成された現行請求項案を、弁理士による日本特許出願前レビューに供することを目的とする。

本書では、発明者側でこれまで検討してきた技術的構成およびClaim Architectureを維持する。

特に、独立請求項において、

- Authenticationそのものを発明の中心としないこと
- Authentication ResultとEntitlementとの分離
- Authentication Object ValidityとEntitlement Validityとの独立性
- Policy Evaluation
- Authorization Decision
- Enforcement
- Service Execution

という構成関係を維持する。

本書は正式な出願書類ではなく、弁理士による特許性、明確性、サポート要件、実施可能要件およびClaim Scopeの最終検討のための技術原稿である。

---

# 2. 発明の名称

情報処理システム、情報処理方法、プログラム及び記録媒体

---

# 3. Claim Architecture

本発明の基本的な処理関係は、以下のとおりである。

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

ここで、Authentication ResultとEntitlementは同一の情報として扱われる必要はない。

また、Authentication ObjectのValidityとEntitlementのValidityとは同一である必要はなく、それぞれ独立した時間的条件として管理することができる。

---

# 4. 独立請求項

## 【請求項1】

Authenticationを実行するAuthentication処理部と、

前記Authenticationの結果を示すAuthentication Resultを取得するAuthentication Result処理部と、

前記Authentication Resultとは独立して管理され得るEntitlementを取得、生成または確認するEntitlement処理部と、

前記Authentication Resultおよび前記Entitlementを含む一以上の情報とPolicyとに基づいてPolicy Evaluationを実行するPolicy Evaluation処理部と、

前記Policy Evaluationの結果に基づいてAuthorization Decisionを生成するAuthorization処理部と、

前記Authorization Decisionに基づいてEnforcementを実行するEnforcement処理部と、

前記Enforcementに基づいて保護対象のService Executionを制御するService Execution処理部と、

を備え、

前記Authentication Resultと前記Entitlementとを別個の情報として取り扱い、

前記EntitlementのValidityを前記Authenticationに使用されるAuthentication ObjectのValidityとは独立して管理可能とし、

前記Authorization Decisionに基づいて前記Service Executionの実行を制御する、

情報処理システム。

---

# 5. システムに係る従属請求項

## 【請求項2】

前記Authentication Objectが所定のValidity Periodを有し、

前記Validity Periodの経過後に前記Authentication Objectを使用したAuthenticationを無効として扱う、

請求項1に記載の情報処理システム。

## 【請求項3】

前記Authentication ObjectがExpirationを有し、

前記Authentication ObjectがExpirationした場合に、当該Authentication Objectを使用したAuthenticationを拒否する、

請求項1または2に記載の情報処理システム。

## 【請求項4】

前記Authentication ObjectのValidity Periodが、前記EntitlementのValidity Periodとは異なる、

請求項1から3のいずれか一項に記載の情報処理システム。

## 【請求項5】

前記Authentication ObjectのValidity Periodが、前記EntitlementのValidity Periodより短い、

請求項4に記載の情報処理システム。

## 【請求項6】

前記Authentication ObjectがQR Codeを含む、

請求項1から5のいずれか一項に記載の情報処理システム。

## 【請求項7】

前記QR Codeが、Web画面上に表示される、

請求項6に記載の情報処理システム。

## 【請求項8】

前記QR Codeが、所定のValidity Periodの間のみAuthenticationに使用可能である、

請求項6または7に記載の情報処理システム。

## 【請求項9】

前記Validity Periodが、QR Codeの生成時点から所定時間として設定される、

請求項8に記載の情報処理システム。

## 【請求項10】

前記Validity Periodが30秒である、

請求項9に記載の情報処理システム。

> **技術側注記:** 30秒はValidity Periodの一具体例であり、本発明のValidity Periodそのものを30秒に限定する趣旨ではない。

## 【請求項11】

前記Authentication ObjectがOne-time Use条件を有し、

前記Authentication ObjectがAuthenticationに使用された後、同一のAuthentication Objectを使用した後続のAuthenticationを無効として扱う、

請求項1から10のいずれか一項に記載の情報処理システム。

## 【請求項12】

前記Entitlementが、所定の条件が成立した場合に生成または有効化される、

請求項1から11のいずれか一項に記載の情報処理システム。

## 【請求項13】

前記所定の条件が、所定のAuthenticationの成功、所定の場所への到達、所定の取引の成立、所定のServiceの利用、所定の時間条件または所定のObject Stateの成立の一以上を含む、

請求項12に記載の情報処理システム。

## 【請求項14】

前記Entitlementが、前記Entitlementを生成または確認したServiceとは異なるServiceにおけるAuthorizationのために使用される、

請求項1から13のいずれか一項に記載の情報処理システム。

## 【請求項15】

前記Entitlementが、前記Authenticationの完了後においても、前記Authenticationとは独立して保存、更新、停止、失効または再有効化される、

請求項1から14のいずれか一項に記載の情報処理システム。

## 【請求項16】

前記Entitlementが所定のValidity Periodを有し、

前記Entitlementが前記Validity Periodの範囲外にある場合に、前記Policy Evaluationまたは前記Authorization Evaluationにおいて当該Entitlementを有効なEntitlementとして扱わない、

請求項1から15のいずれか一項に記載の情報処理システム。

## 【請求項17】

前記Entitlementが、前記Validity Periodの終了とは独立してRevocationされ得る、

請求項16に記載の情報処理システム。

## 【請求項18】

前記Policy Evaluationが、前記Authentication Result、前記Entitlement、Subject、Resource、Action、Security Context、TransactionまたはObject Stateの一以上を入力として実行される、

請求項1から17のいずれか一項に記載の情報処理システム。

## 【請求項19】

前記Authorization Decisionが、Permit、DenyまたはIndeterminateのいずれかを示す、

請求項1から18のいずれか一項に記載の情報処理システム。

## 【請求項20】

前記Indeterminateが生成された場合に、再Authentication、Revalidation、再Evaluation、Deny、処理停止または処理終了の一以上を実行する、

請求項19に記載の情報処理システム。

## 【請求項21】

前記Enforcementが、前記Authorization DecisionをService Executionに適用することにより、前記Service Executionの許可、拒否、制限、停止または終了の一以上を実行する、

請求項1から20のいずれか一項に記載の情報処理システム。

## 【請求項22】

前記Service Executionが、情報取得、情報変更、Resourceへのアクセス、商品またはServiceの提供、決済、割引、特典の付与またはAPI処理の一以上を含む、

請求項1から21のいずれか一項に記載の情報処理システム。

## 【請求項23】

前記AuthenticationについてFreshness条件を設定し、

前記Authentication Resultが前記Freshness条件を満たさない場合に、再Authentication、Revalidation、DenyまたはIndeterminateの一以上を実行する、

請求項1から22のいずれか一項に記載の情報処理システム。

## 【請求項24】

前記ObjectがCreated、Active、Suspended、Expired、Revoked、Consumed、CompletedまたはFailedの一以上のStateを有する、

請求項1から23のいずれか一項に記載の情報処理システム。

## 【請求項25】

前記ObjectのStateがAuthorization Decisionとは独立して管理され、前記Stateが前記Policy EvaluationまたはAuthorization Evaluationの入力情報として使用される、

請求項24に記載の情報処理システム。

## 【請求項26】

前記Security Contextが、主体、装置、通信、Session、Transaction、Authentication状態、Entitlement状態またはObject Stateの一以上を含む、

請求項1から25のいずれか一項に記載の情報処理システム。

## 【請求項27】

前記Transactionが、Authentication Result、Entitlement、Authorization DecisionまたはService Execution Resultの一以上を関連付ける、

請求項1から26のいずれか一項に記載の情報処理システム。

## 【請求項28】

前記Authenticationが公開鍵暗号方式によって実行される、

請求項1から27のいずれか一項に記載の情報処理システム。

## 【請求項29】

前記AuthenticationがWebAuthnまたはPasskeyを用いて実行される、

請求項28に記載の情報処理システム。

## 【請求項30】

前記Authenticationに使用される秘密鍵が、Secure Enclave、Trusted Execution Environmentまたはこれらに相当する保護領域に保持される、

請求項28または29に記載の情報処理システム。

---

# 6. 情報処理方法に係る請求項

## 【請求項31】

情報処理システムが、

Authenticationを実行するステップと、

前記Authenticationの結果を示すAuthentication Resultを取得するステップと、

前記Authentication Resultとは独立して管理され得るEntitlementを取得、生成または確認するステップと、

前記Authentication Resultおよび前記Entitlementを含む一以上の情報とPolicyとに基づいてPolicy Evaluationを実行するステップと、

前記Policy Evaluationの結果に基づいてAuthorization Decisionを生成するステップと、

前記Authorization Decisionに基づいてEnforcementを実行するステップと、

前記Enforcementに基づいて保護対象のService Executionを制御するステップと、

を含み、

前記Authentication Resultと前記Entitlementとを別個の情報として取り扱い、

前記EntitlementのValidityを、前記Authenticationに使用されるAuthentication ObjectのValidityとは独立して管理可能とする、

情報処理方法。

## 【請求項32】

前記Authentication Objectに所定のValidity Periodを設定し、前記Validity Periodの経過後に当該Authentication ObjectをAuthenticationに使用できないものとして扱う、

請求項31に記載の情報処理方法。

## 【請求項33】

前記Authentication ObjectのValidity Periodと前記EntitlementのValidity Periodとを異なる期間として管理する、

請求項31または32に記載の情報処理方法。

## 【請求項34】

前記Authenticationによって確認された結果に基づいてEntitlementを生成または有効化し、前記Entitlementを前記Authenticationとは異なるServiceにおけるAuthorizationのために使用する、

請求項31から33のいずれか一項に記載の情報処理方法。

## 【請求項35】

前記EntitlementのValidityが終了した場合、または前記EntitlementがRevocationされた場合に、前記Policy EvaluationまたはAuthorization Evaluationにおいて当該Entitlementを有効なEntitlementとして扱わない、

請求項31から34のいずれか一項に記載の情報処理方法。

---

# 7. プログラムに係る請求項

## 【請求項36】

コンピュータに、

Authenticationを実行する処理と、

前記Authenticationの結果を示すAuthentication Resultを取得する処理と、

前記Authentication Resultとは独立して管理され得るEntitlementを取得、生成または確認する処理と、

前記Authentication Resultおよび前記Entitlementを含む一以上の情報とPolicyとに基づいてPolicy Evaluationを実行する処理と、

前記Policy Evaluationの結果に基づいてAuthorization Decisionを生成する処理と、

前記Authorization Decisionに基づいてEnforcementを実行する処理と、

前記Enforcementに基づいて保護対象のService Executionを制御する処理と、

を実行させ、

前記Authentication Resultと前記Entitlementとを別個の情報として取り扱わせ、

前記EntitlementのValidityを、前記Authenticationに使用されるAuthentication ObjectのValidityとは独立して管理可能とする、

プログラム。

---

# 8. 記録媒体に係る請求項

## 【請求項37】

請求項36に記載のプログラムを記録した、

コンピュータ読み取り可能な記録媒体。

---

# 9. Claim 1のFallback候補

Claim 1については、先行技術および日本出願実務における弁理士レビューの結果に応じて、以下のFallback構造を保持する。

## Fallback A

Authentication ResultとEntitlementの独立性をより強く限定する。

## Fallback B

Authentication ObjectのValidityとEntitlementのValidityとの時間的独立性を追加する。

## Fallback C

Cross-Service Entitlementを追加する。

## Fallback D

Conditional Entitlementを追加する。

## Fallback E

Policy Evaluationを介してAuthorization Decisionを生成する構成をより具体的に限定する。

## Fallback F

Authorization DecisionとEnforcementとを分離する構成を追加する。

## Fallback G

Enforcementを介してService Executionを制御する構成を追加する。

> **弁理士向け注記:** 上記Fallbackは、発明者側があらかじめClaim Scopeを固定するものではなく、先行技術および日本特許実務に応じて選択・再構成するための候補である。

---

# 10. Claim 1で避けるべき限定

Claim 1では、発明の技術的核心を不必要に狭めることを避ける。

原則として、以下をClaim 1の必須構成として限定しない。

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

これらは、必要に応じて従属請求項または明細書の実施形態として扱う。

---

# 11. 本発明における時間概念

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

# 12. Scenario D

一実施形態として、あるServiceにおいてAuthenticationを行った主体についてEntitlementを生成または有効化し、当該Entitlementを別のEC ServiceにおけるAuthorizationの条件として使用する。

例えば、来店したことをAuthenticationまたはその他の確認処理によって確認し、その結果に基づいて「来店済み」というEntitlementを生成する。

EC Serviceにおいて、Policy Evaluationは当該Entitlementを評価し、所定の商品または取引についてAuthorization Decisionを生成する。

Permitの場合、Enforcementによって割引その他の特典をService Executionへ適用する。

この場合、来店時に使用されたAuthentication ObjectのValidityが30秒であったとしても、「来店済み」というEntitlementを数時間または所定期間有効とすることができる。

したがって、Authenticationの一時性とEntitlementの時間的有効性を分離することができる。

---

# 13. Claim 1の重要構成関係

本発明のClaim Architectureにおいて、特に重要な構成関係は以下である。

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

ただし、Authentication ResultとEntitlementは同一Objectである必要はない。

また、Authentication ObjectのValidityとEntitlementのValidityも同一である必要はない。

この分離により、Authenticationを行ったという事実と、その後のServiceにおいて現在有効な権利または資格を有することとを、別々の情報処理対象として管理できる。

---

# 14. Claimと発明の核心との関係

Claim 1の中心的構成は、Authentication方式そのものではない。

発明の中心は、

1. Authenticationを実行すること
2. Authentication Resultを取得すること
3. Authentication Resultとは独立して管理され得るEntitlementを取得、生成または確認すること
4. Authentication ResultおよびEntitlementを含む情報をPolicy Evaluationに利用すること
5. Policy Evaluationの結果に基づいてAuthorization Decisionを生成すること
6. Authorization DecisionをEnforcementに適用すること
7. Enforcementを介してService Executionを制御すること
8. Authentication ObjectのValidityとEntitlementのValidityを独立して管理可能とすること

という一連の技術的関係である。

---

# 15. Claim Support上の主要確認事項

## Claim 1

以下の構成は、明細書ドラフトに対応する説明を有する。

- Authentication
- Authentication Result
- Entitlement
- Authentication ResultとEntitlementの独立性
- Authentication Object
- Authentication Object Validity
- Entitlement Validity
- Policy Evaluation
- Authorization Evaluation
- Authorization Decision
- Enforcement
- Service Execution

## Claim 2～5

以下の技術内容を支持する。

- Authentication Object Validity Period
- Authentication Object Expiration
- Authentication Object ValidityとEntitlement Validityとの相違
- Authentication Object ValidityがEntitlement Validityより短い構成

## Claim 6～10

以下のQR Code実施形態を支持する。

- QR Code
- Web画面表示
- QR CodeのValidity Period
- QR Code生成時点からのValidity
- 30秒という具体例

> 30秒は具体的な実施例または従属Claim上の限定であり、Claim 1の発明全体を30秒に限定するものではない。

## Claim 11

One-time UseおよびReplay防止。

## Claim 12～15

Conditional Entitlement、Cross-Service EntitlementおよびEntitlementの独立Lifecycle。

## Claim 16～17

Entitlement ValidityおよびRevocation。

## Claim 18～23

Policy Evaluation、Authorization Decision、Permit、Deny、Indeterminate、Enforcement、Service ExecutionおよびAuthentication Freshness。

## Claim 24～27

Object State、StateとAuthorizationとの分離、Security ContextおよびTransaction。

## Claim 28～30

公開鍵暗号、WebAuthn、Passkey、Secure Enclave、Trusted Execution Environment等による具体的Authentication実施形態。

## Claim 31～35

情報処理方法としての展開。

## Claim 36

プログラムとしての展開。

## Claim 37

コンピュータ読み取り可能な記録媒体としての展開。

---

# 16. Claim間の構造

現行Claim Architectureは以下の構造を採用している。

### 独立Claim

- Claim 1 — 情報処理システム
- Claim 31 — 情報処理方法
- Claim 36 — プログラム
- Claim 37 — 記録媒体

### システム従属Claim

Claim 2～30

### 方法従属Claim

Claim 32～35

この構造については、最終的な請求項数、従属関係および日本出願上の適切な構成について弁理士による判断を求める。

---

# 17. Claim Scopeに関する基本方針

本原稿では、発明者側の技術的意図として、Claim 1を特定のAuthentication方式に限定しない。

特に、

- QR Code
- 30秒
- WebAuthn
- Passkey
- 公開鍵暗号
- Secure Enclave

等は、発明の具体的実施形態またはFallbackとして保持する。

一方、発明の核心である、

Authentication Result
と
Entitlement

との分離、および、

Authentication Object Validity
と
Entitlement Validity

との独立性は、Claim 1における重要な構成関係として維持する。

---

# 18. 弁理士による最終検討事項

以下について、弁理士による最終判断を求める。

1. Claim 1の新規性
2. Claim 1の進歩性
3. Claim 1の先行技術との境界
4. Claim 1のClaim Scope
5. Authentication ResultとEntitlementとの分離の法的表現
6. Authentication Object ValidityとEntitlement Validityとの独立性の法的表現
7. Policy EvaluationとAuthorization Decisionとの関係
8. Authorization DecisionとEnforcementとの関係
9. EnforcementとService Executionとの関係
10. Claim 2～30の従属関係
11. Claim 31～35の方法Claimとしての構成
12. Claim 36のプログラムClaimとしての構成
13. Claim 37の記録媒体Claimとしての構成
14. Claim間の冗長性
15. 各従属ClaimのSupport
16. 明確性
17. サポート要件
18. 実施可能要件
19. 新規事項の有無
20. 日本出願における請求項数および構成の最適化
21. 先行技術に応じたFallback選択
22. 必要に応じたClaimの統合、分割、限定または再構成

---

# 19. 本ドラフトの位置付け

本書は、NEW-shot2play Technical Specification Version 2.0の現行Claim Architectureを基礎として、日本特許出願に向けて弁理士が検討するための技術原稿である。

本書は正式な出願用請求項ではない。

正式な出願に際しては、弁理士による先行技術調査、特許性判断、明確性、サポート要件、実施可能要件、新規事項、Claim Scopeその他の専門的検討を経て最終化する。

現行Version 2.0の技術的内容および発明者側のClaim Architectureを尊重し、発明者側では必要以上にClaim Scopeを狭めないことを基本方針とする。

また、30秒、QR Code、WebAuthn、Passkey、Secure Enclave等の具体的構成は、発明全体を限定するものではなく、従属Claimまたは具体的実施形態として位置付ける。

---

# 20. 基礎文書との関係

本書の基礎となる文書は以下である。

- `patent/claims_v2.md`
- `patent/specification_v2.md`
- `patent/claim_structure_v2.md`
- `patent/dependent_claim_strategy_v2.md`
- `patent/invention_core_v2.md`

本書作成に伴い、上記Version 2.0基礎文書は変更しない。

---

# 21. 弁理士への重要メッセージ

本発明について発明者側が最も重要と考えているのは、Authentication方式そのものではない。

重要なのは、

「Authenticationによって確認された結果」

と

「その後のService利用に関する権利または資格」

を別個の情報処理対象として扱い、

そのEntitlementを独立したLifecycleで管理し、

Authentication Result、Entitlementその他のContextをPolicy Evaluationに入力し、

その評価結果に基づいてAuthorization Decisionを生成し、

Authorization DecisionをEnforcementに適用し、

Enforcementを介して実際のService Executionを制御する、

という一連の技術的構造である。

さらに、Authentication Objectの短時間Validityと、Entitlementのより長いValidityとを独立して管理可能とする点も重要な構成関係である。

この技術的関係を維持した上で、日本特許出願として最適なClaim Scopeおよび請求項構成について最終判断を求める。

