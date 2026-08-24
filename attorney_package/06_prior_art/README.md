
NEW-shot2play Technical Specification Version 2.0
先行技術比較資料
Attorney Review Draft

対象: 日本特許出願
目的: Claim 1の新規性・進歩性検討のための先行技術比較
基礎仕様: NEW-shot2play Technical Specification Version 2.0

1. 本書の目的

本書は、NEW-shot2play Technical Specification Version 2.0について、
日本特許出願前に実施した先行技術検討の結果を整理し、
弁理士による新規性および進歩性の最終評価に供することを目的とする。

本書は、先行技術について法的な結論を示すものではない。

最終的な先行技術の認定、引用文献の選定、
新規性・進歩性その他の特許要件に関する判断は、
弁理士による検討を前提とする。

2. 検討対象

本書の主な検討対象は、Version 2.0のClaim 1である。

特に、以下の技術的関係を重点的に検討する。

Authentication処理
Authentication Resultの生成
Authentication Resultとは独立して管理され得るEntitlement
Authentication ObjectのValidity
EntitlementのValidity
Authentication Object ValidityとEntitlement Validityの独立性
Authentication Result、Entitlementその他のContextを利用したPolicy Evaluation
Policy Evaluationに基づくAuthorization Decision
Authorization Decisionに基づくEnforcement
Enforcementを介したService Execution
上記各処理およびObjectのLifecycle上の分離
3. 重要な評価原則

本発明について、個々の構成要素が既知であることと、
Claim 1全体が先行技術によって開示または容易に想到されることとは
区別して評価する必要がある。

Authentication、Authorization、Policy Evaluation、
Access Control、Credential Validity等の個別技術については、
多数の既知技術が存在すると考えられる。

一方、本発明が重点的に主張するのは、
Authenticationそのものではなく、

Authenticationによって得られるAuthentication Resultと、
その後のService利用に関するEntitlementとを分離し、

それぞれを異なるLifecycleおよびValidityとして管理し、

Authentication Result、Entitlementその他のContextを
Policy Evaluationに入力し、

その評価結果に基づいてAuthorization Decisionを生成し、

Authorization DecisionをEnforcementに適用し、

Enforcementを介してService Executionを制御する、

という一連の技術的構造である。

したがって、先行技術評価では、
単なる「Authentication + Authorization」の存在だけではなく、
Claim 1の構成要素の組合せおよび相互関係について検討する必要がある。

4. 検討した主要な先行技術領域
4.1 FIDO Cross-Device Authentication

FIDO系のCross-Device Authenticationは、
スマートフォン等の別デバイスを利用してWebまたは他の端末における
Authenticationを成立させる技術領域である。

本発明との関係では、Authentication Object、
Challenge-Response、公開鍵暗号、Device間連携等の
Authentication関連技術について既知技術との重複を検討する必要がある。

一方、本発明の中心であるAuthentication ResultとEntitlementとの
独立管理、Entitlement Lifecycle、Policy Evaluationへの入力、
Authorization Decision、EnforcementおよびService Executionとの
一連の関係については、Authentication技術そのものとは
別個に評価する必要がある。

4.2 Apple Passkey

Passkeyは、公開鍵暗号を利用したAuthenticationおよび
Credential管理に関する既知技術である。

本発明における公開鍵暗号、Credential、Authentication等の
構成については、既知技術との関係を慎重に確認する必要がある。

ただし、PasskeyによるAuthenticationの存在だけから、
Authentication ResultとEntitlementの独立したLifecycle管理、
Policy EvaluationおよびAuthorization Decisionを含む
本発明の全体構成が直接導かれるとは限らない。

4.3 Google Password Manager

Google Password Managerは、CredentialおよびPasskey等の
Authentication関連情報を管理する技術領域に属する。

本発明のCredentialおよびAuthentication関連構成の一部について
既知技術との重複を検討する必要がある。

しかし、CredentialまたはAuthentication情報の管理と、
Authentication後に独立したEntitlementを管理して
Service利用権を制御する構成とは、技術的な目的および処理関係を
区別して評価する必要がある。

4.4 Microsoft Authenticator

Microsoft Authenticatorは、Authenticationおよび
Multi-Factor Authentication等を提供する既知技術領域である。

Authentication、Device、Credential、Challenge等に関する
構成は既知技術との重複可能性が高い。

一方、本発明の重点であるEntitlement Objectおよび
その独立Lifecycle、Policy Evaluation、Authorization Decision、
EnforcementおよびService Executionとの関係については、
Authentication技術とは別に評価する必要がある。

4.5 WhatsApp Web

WhatsApp Web等のDevice Linking型サービスでは、
スマートフォン等の既認証DeviceとWeb端末との間で
認証またはDevice Linkingを成立させる構成が知られている。

このような技術は、Device間Authenticationや
Session確立等との関係で参考となる。

ただし、Authenticationによって得られた結果と、
独立したService利用権としてのEntitlementを分離して管理し、
そのEntitlementをPolicy Evaluationの入力として利用する
本発明の構造とは区別して評価する必要がある。

4.6 GitHub Device Flow

GitHub Device Flow等のDevice Authorization型技術では、
別Deviceを利用してAuthorizationまたはAuthenticationを完了させ、
対象Device上でサービス利用を可能にする処理が知られている。

このため、Device間の認証・認可連携という観点では
本発明との関連性が高い。

一方、本発明では、
Authentication ResultそのものとEntitlementとを
異なる情報として扱うことが重要な構成となっている。

また、EntitlementのValidityおよびLifecycleをAuthentication Objectの
Validityとは独立して管理し、
Policy Evaluation、Authorization DecisionおよびEnforcementを
経由してService Executionを制御する構成については、
各文献の具体的開示内容を確認する必要がある。

4.7 OAuth 2.0 Device Authorization Grant

OAuth 2.0 Device Authorization Grantは、
ユーザーが別Device等を利用してAuthorizationを完了させ、
Client DeviceにAuthorization結果を提供する技術である。

本技術領域は、Device Authorization、
Access Token、Scope、Authorization Server等との関係で
本発明と比較する必要がある。

ただし、OAuthにおけるTokenやScope等と、
本発明におけるEntitlement Objectを同一視することはできない。

本発明では、Entitlementを独立した情報対象として管理し、
Authentication Resultとは異なるLifecycleを持たせることができ、
さらにPolicy Evaluation、Authorization Decision、
EnforcementおよびService Executionへ連続的に利用する。

この点について、先行技術におけるToken、Scope、Grant等が
本発明のClaim 1の各構成をどこまで開示しているかを
個別に確認する必要がある。

5. 総合的な先行技術評価

現時点の検討では、Claim 1を構成する個々の技術要素については、
既知技術が存在する可能性が高い。

特に、

Authentication
Public Key Authentication
Device Authentication
Authorization
Policy Evaluation
Access Control
Token / Scope
Validity / Expiration
Revocation
Service Access Control

等は、それぞれ既知の技術領域である。

したがって、本発明の特許性を評価する際には、
個々の技術要素そのものではなく、
それらがどのような関係で組み合わされているかを
重点的に検討する必要がある。

6. 本発明において特に確認すべき構成

弁理士には、特に以下の構成について
先行技術との比較をお願いしたい。

6.1 Authentication ResultとEntitlementの分離

Authenticationによって得られるAuthentication Resultと、
Service利用に関するEntitlementとを別個の情報対象として扱う。

6.2 Authentication Object ValidityとEntitlement Validity

Authentication Objectの短時間Validityと、
EntitlementのValidityとを独立して管理可能とする。

例えばAuthentication ObjectのValidityが短時間で終了しても、
既に生成されたEntitlementのValidityが当然に終了するとは限らない。

6.3 Entitlementの独立Lifecycle

Entitlementについて、Authenticationとは異なるLifecycleを設定し、

発行
有効化
利用
更新
Suspension
Revocation
Expiration

等を独立して管理可能とする。

6.4 Policy Evaluationへの入力

Authentication Resultだけではなく、
Entitlementおよびその他のContextをPolicy Evaluationに入力する。

これにより、Authenticationが成立したか否かだけではなく、
「現在どのようなService利用権を有しているか」を
Authorization判断に利用できる。

6.5 Authorization Decision

Policy Evaluation等の結果から、
Permit、DenyまたはIndeterminate等のAuthorization Decisionを生成する。

6.6 Enforcement

Authorization DecisionとService Executionとの間に
Enforcementを設ける。

これにより、Authorization Decisionそのものと、
実際のService Executionとを分離する。

6.7 Service Execution

Enforcementの結果に基づいて、
実際のService Executionを制御する。

7. Claim 1全体としての評価

Claim 1については、
単純なAuthenticationまたはAuthorization技術として評価するのではなく、

Authentication

↓

Authentication Result

↓

Entitlement

↓

Policy Evaluation

↓

Authorization Decision

↓

Enforcement

↓

Service Execution

という処理関係として評価することが重要である。

さらに、

Authentication Object Validity

と

Entitlement Validity

を独立して管理可能とする時間的構造も、
Claim 1または関連請求項との関係で確認する必要がある。

8. 新規性についての発明者側整理

現時点では、発明者側として
「Claim 1の全構成が単一の先行技術文献に開示されていない」
ことを期待することはできるが、
法的な新規性判断を断定するものではない。

特に確認すべき点は、

Authentication Result
Entitlement
両者の独立性
Authentication Object Validity
Entitlement Validity
Policy Evaluation
Authorization Decision
Enforcement
Service Execution

のすべてが、Claim 1に規定された関係で
単一文献に開示されているか否かである。

9. 進歩性についての発明者側整理

進歩性については、個々の要素が既知であることだけでなく、
先行技術からClaim 1の構成をどのように組み合わせる動機付けが存在するか、
また、その組合せによってどのような技術的効果が得られるかを
検討する必要がある。

本発明では、

Authenticationの成立

と

Service利用権の成立

とを分離することで、

Authentication Objectの短時間Validityと、
Entitlementのより長いValidityとを
異なるLifecycleとして管理できる。

さらに、そのEntitlementをPolicy Evaluationに利用することで、
Authenticationの結果だけでは判断できない
Service利用条件をAuthorizationに反映できる。

この点について、先行技術の組合せから
当業者が容易に想到できるかを弁理士に評価してもらう。

10. 発明者側からの重要な注意事項

本資料における「既知」「重複可能性」等の記載は、
発明者側による技術的な予備評価である。

これらをもって、各文献がClaim 1を開示している、
またはClaim 1が特許性を欠くと判断してはならない。

最終的な判断は、各先行技術文献の具体的記載、
出願日・公開日その他の法的事項を踏まえて
弁理士が行うものとする。

11. 弁理士への確認事項

以下について、最終的な専門的評価を求める。

Claim 1の新規性
Claim 1の進歩性
Authentication ResultとEntitlementの分離の評価
Authentication Object ValidityとEntitlement Validityの独立性の評価
EntitlementをPolicy Evaluationへ入力する構成の評価
Authorization DecisionとEnforcementの分離の評価
EnforcementとService Executionの関係の評価
上記構成の組合せによる技術的効果の評価
Claim 1のScopeが広すぎるか否か
Claim 1の構成を維持したまま、より強い請求項構成が可能か
12. 結論

現時点の先行技術検討から、
Version 2.0の個々の構成要素には既知技術が存在する可能性がある。

しかし、本発明の評価対象は個々の要素ではなく、

Authentication ResultとEntitlementの分離、

Authentication Object ValidityとEntitlement Validityの独立管理、

Entitlementを含むContextによるPolicy Evaluation、

Authorization Decision、

Enforcement、

Service Execution

という一連の技術的関係である。

したがって、現段階ではVersion 2.0の発明概念を変更するのではなく、
本資料を弁理士に提供し、日本特許出願における新規性・進歩性および
最適なClaim Scopeについて専門的判断を受けることが適切である。

End of Document
