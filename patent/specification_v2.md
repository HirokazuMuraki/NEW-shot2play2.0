# NEW-shot2play Technical Specification Version 2.0
# 特許明細書 Version 2.0

## 1. 発明の名称

認証結果と権利情報とを独立して管理し、ポリシー評価に基づいて
認可を決定し、当該認可をサービス実行に適用する情報処理システム、
情報処理方法、プログラムおよび記録媒体

---

## 2. 技術分野

本発明は、情報処理システム、アクセス制御、認証、権利管理、
ポリシー評価、認可制御およびサービス実行制御に関する。

より具体的には、本発明は、Authenticationによって確認された結果と、
当該Authenticationとは独立して管理され得るEntitlementとを用いて
Policy Evaluationを行い、その評価結果に基づいてAuthorization Decision
を生成し、当該Authorization DecisionをEnforcementを介して
Service Executionに適用する情報処理技術に関する。

本発明は、Webサービス、モバイルサービス、ECサービス、会員サービス、
業務システム、分散サービス、APIサービスその他の情報処理サービスに
適用することができる。

---

## 3. 背景技術

従来の情報処理システムでは、利用者が所定のサービスを利用する際に、
利用者をAuthenticationによって確認し、そのAuthenticationの結果に
基づいてサービスへのアクセスを許可する構成が広く用いられている。

例えば、利用者がIDおよびパスワードを入力する方式、ワンタイム
パスワードを使用する方式、QR Codeを利用する方式、公開鍵暗号方式、
WebAuthn等の認証方式によって利用者を確認し、認証が成功した場合に
所定のサービスへのアクセスを許可することができる。

しかしながら、Authenticationによって確認された事実と、
その利用者が現在有する権利または資格と、特定のサービスについて
実際に許可される操作とは、必ずしも同一ではない。

例えば、ある利用者が店舗において本人確認を受けた場合、その
Authentication自体は短時間で終了することができる。

一方、そのAuthenticationによって確認された「来店した」という
事実に基づいて、当該利用者に対して数時間または所定期間有効な
Entitlementを付与することができる。

この場合、Authenticationに使用された一時的な情報が失効したとしても、
それだけでEntitlementまで失効する必要はない。

また、Entitlementの有無だけによってサービス実行を許可すると、
サービス、資源、操作、時間、取引状態その他のContextを考慮した
細かな制御が困難になる。

さらに、あるサービスで成立した事実または権利を、別のサービスに
おけるAuthorizationに利用する場合には、Authenticationと
Authorizationとを同一の処理として扱うことは適切でない。

したがって、Authentication、Entitlement、Policy Evaluation、
AuthorizationおよびService Executionを、それぞれ異なる情報および
処理として扱いながら、これらを所定の関係によって連携させる技術が
求められる。

---

## 4. 発明が解決しようとする課題

本発明の一つの目的は、Authenticationの結果と、Authenticationとは
独立して管理され得るEntitlementとを区別して扱うことである。

本発明の他の目的は、Authentication ObjectのValidityとEntitlementの
Validityとを独立して管理可能とすることである。

本発明の他の目的は、Authentication Result、Entitlement、Policy、
Security Contextその他の情報をPolicy Evaluationに利用し、その結果に
基づいてAuthorization Decisionを生成することである。

本発明の他の目的は、Authorization DecisionとService Executionとの
間にEnforcementを設け、Authorization Decisionを実際のサービス実行に
適用することである。

本発明のさらに他の目的は、Authenticationによって確認された事実を
Authenticationとは異なるサービスにおけるAuthorizationに利用可能と
することである。

---

## 5. 課題を解決するための手段

本発明の一態様に係る情報処理システムは、

Authenticationを実行するAuthentication処理部と、

前記Authenticationの結果としてAuthentication Resultを生成または
取得する処理部と、

前記Authentication Resultとは独立して管理され得るEntitlementを
取得、生成または確認するEntitlement処理部と、

前記Authentication Result、前記Entitlementおよびその他のContext
情報の少なくとも一つに基づいてPolicy Evaluationを実行する
Policy Evaluation処理部と、

前記Policy Evaluationの結果に基づいてAuthorization Decisionを
生成するAuthorization処理部と、

前記Authorization Decisionに基づいてEnforcementを実行する
Enforcement処理部と、

前記Enforcementに基づいて保護対象のService Executionを制御する
Service Execution処理部と、

を備える。

ここで、Authentication ResultとEntitlementとは同一のObjectである
必要はない。

また、Authentication ObjectのValidityとEntitlementのValidityとは
異なる期間として管理することができる。

---

## 6. 発明の基本構成

本発明の基本的な処理連鎖は、概念的に以下のように表すことができる。

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

ただし、各処理は必ずしも一つの装置または一つのサーバに配置される
必要はない。

複数のサーバ、サービス、API、データベース、端末その他の構成要素に
分散して実装することができる。

また、EntitlementはAuthenticationの直後に生成される必要はなく、
Authenticationとは異なる時点、異なる処理または異なるサービスに
おいて生成、取得、更新、停止または確認されてもよい。

---

## 7. Authentication

Authenticationは、主体、利用者、装置、アプリケーションその他の
対象について、所定の確認処理を行うことをいう。

Authenticationの方式は特定の方式に限定されない。

例えば、パスワード、ワンタイムパスワード、QR Code、公開鍵暗号、
WebAuthn、Passkey、生体認証、端末認証、証明書その他の方式を利用
することができる。

本発明において重要なのは、Authenticationの具体的方式そのものでは
なく、Authenticationによって得られるAuthentication Resultを、
Entitlementその他の情報と区別して扱えることである。

---

## 8. Authentication Object

Authenticationに使用される情報をAuthentication Objectとすることが
できる。

Authentication Objectには、識別情報、Token、Challenge、Response、
QR Codeその他の情報を含めることができる。

Authentication Objectは、一時的なObjectとして生成することができる。

Authentication Objectには、必要に応じてValidity Period、Expiration、
One-time Useその他の利用条件を設定することができる。

---

## 9. Authentication ObjectのValidity

Authentication Objectは、所定のValidity Periodを有することができる。

Validity Periodは、Authentication ObjectがAuthenticationに使用可能な
期間を示す。

Validity Periodを経過したAuthentication Objectは、Policyその他の
条件に従ってAuthenticationに使用できないものとして扱うことが
できる。

例えば、Web画面に表示されるQR CodeをAuthentication Objectとして
使用する場合、QR Codeの生成時点から所定時間だけ有効とすることが
できる。

この所定時間は30秒に限定されない。

例えば、数秒、10秒、30秒、1分、5分その他の任意の期間を設定する
ことができる。

---

## 10. Authentication ObjectのExpiration

Authentication ObjectにはExpirationを設定することができる。

Expirationは、Authentication ObjectがAuthenticationに使用できなく
なる時点を示す。

Authentication ObjectがExpirationした場合、当該Objectを使用した
Authenticationを拒否することができる。

Authentication ObjectのExpirationは、EntitlementのExpirationとは
独立して管理することができる。

---

## 11. Authentication ValidityとEntitlement Validity

Authentication ObjectのValidityとEntitlementのValidityとは異なる
概念である。

例えば、Authentication ObjectのValidity Periodを30秒とし、
Authenticationによって確認された事実に基づいて生成されたEntitlement
を数時間有効とすることができる。

この場合、Authentication Objectが30秒後に使用不能となったとしても、
既に生成されたEntitlementのValidityが当然に終了するわけではない。

反対に、Entitlementが失効したとしても、そのことだけによって
過去のAuthentication ObjectのValidityが延長されることもない。

このように、Authenticationに使用される一時的Objectと、
Authenticationによって成立または確認された権利情報とは、
異なるライフサイクルを有することができる。

---

## 12. Authentication Freshness

Authentication ResultにはFreshness条件を設定することができる。

Freshnessは、Authentication Resultが現在のPolicyに照らして十分に
新しいものであるかを示す条件である。

Policyは、Authentication Resultについて所定のFreshnessを要求する
ことができる。

Freshness条件を満たさない場合、再Authentication、再Validation、
Deny、Indeterminateその他の処理を実行することができる。

---

## 13. Replay防止

Authentication ObjectにはReplay防止条件を設定することができる。

例えば、Authentication Objectを一回のみ使用可能とし、Authentication
が成功した後に同一Objectを再使用できないようにすることができる。

Replay防止は、Authentication ObjectのValidityとは別個の条件として
管理することができる。

---

## 14. Entitlement

Entitlementは、主体、対象、サービス、資源、取引その他について、
所定の権利、資格、状態または事実が成立していることを示す情報である。

EntitlementはAuthentication Resultとは異なるObjectとして管理する
ことができる。

EntitlementはAuthenticationの完了時に必ず生成される必要はない。

また、EntitlementはAuthenticationとは異なる時点、異なる処理または
異なるServiceにおいて生成、取得、更新、停止または確認されてもよい。

---

## 15. Authentication ResultとEntitlementの独立性

本発明では、Authentication ResultとEntitlementとを独立した情報として
扱うことができる。

例えば、Authenticationによって本人であることが確認されても、
所定のEntitlementを有していない場合には、特定のService Executionを
許可しないことができる。

反対に、過去のAuthentication等によって成立した事実に基づく
Entitlementを保持している場合、そのEntitlementを現在のPolicy
Evaluationに利用することができる。

---

## 16. Conditional Entitlement

Entitlementは、所定の条件が成立した場合に生成、付与または有効化
することができる。

条件として、Authentication Result、取引結果、位置情報、時刻、
購入履歴、会員状態、サービス利用履歴その他のContextを利用する
ことができる。

例えば、利用者が店舗に来店したことをAuthenticationによって確認し、
その確認結果に基づいて「来店済み」というEntitlementを生成すること
ができる。

---

## 17. Cross-Service Entitlement

あるServiceにおいて生成または確認されたEntitlementを、当該Service
とは異なるServiceにおけるPolicy EvaluationまたはAuthorizationに
利用することができる。

例えば、店舗Serviceにおいて来店を確認し、その結果として生成された
「来店済み」というEntitlementをEC Serviceにおいて取得し、当該
Entitlementに基づいて割引のAuthorization Decisionを生成することが
できる。

この場合、店舗ServiceにおけるAuthenticationとEC Serviceにおける
Authorizationとは同一の処理ではない。

---

## 18. Policy

Policyは、Authentication Result、Entitlement、Subject、Resource、
Action、Security Context、Transaction、時刻その他の情報に基づいて
所定の判断を行うための規則である。

Policyはサービスごとに異なるものとして管理することができる。

---

## 19. Policy Evaluation

Policy Evaluationは、Policyに基づいて入力情報を評価する処理である。

Policy Evaluationの入力には、Authentication Result、Entitlement、
Security Context、Transactionその他の情報を含めることができる。

Policy Evaluationの結果は、Authorization Evaluationまたは
Authorization Decisionの生成に利用することができる。

---

## 20. Authorization Evaluation

Authorization Evaluationは、Policy Evaluationその他の必要な情報に
基づいて、所定のActionまたはService Executionが許可されるべきかを
評価する処理である。

Authorization Evaluationでは、Entitlementの状態、Validity、
Expiration、Revocation、Security Context、Transactionその他の条件を
評価することができる。

---

## 21. Authorization Decision

Authorization Decisionは、Authorization Evaluationの結果として
生成される判断情報である。

Authorization Decisionは、例えばPermit、DenyまたはIndeterminate
を含むことができる。

Authorization DecisionはPolicyそのものでも、Policy Evaluationそのもの
でもなく、Policy Evaluation等によって得られた結果をサービス実行に
適用するための判断情報として管理することができる。

---

## 22. Permit

Permitは、所定のActionまたはService Executionについて、実行を
許可するDecisionである。

Permitが生成された場合でも、Enforcementにおいて追加の条件確認を
行うことができる。

したがって、Permitの生成とService Executionの実行とは同一の処理で
ある必要はない。

---

## 23. Deny

Denyは、所定のActionまたはService Executionについて、実行を
許可しないDecisionである。

Denyが生成された場合、Enforcementは対応するService Executionを
拒否または停止することができる。

---

## 24. Indeterminate

Indeterminateは、必要な情報または条件を確定できず、Permitまたは
Denyを確定できない状態を示すDecisionである。

Indeterminateの場合、Policyに応じて、再Authentication、
Revalidation、再Evaluation、Deny、処理停止その他の処理を実行する
ことができる。

---

## 25. Enforcement

Enforcementは、Authorization Decisionを実際のService Executionに
適用する処理である。

Enforcementは、Service Executionの許可、拒否、停止、条件付き許可、
処理内容変更その他の制御を行うことができる。

Authorization DecisionとEnforcementとは別個の処理として管理する
ことができる。

---

## 26. Service Execution

Service Executionは、保護対象となるサービス処理の実行である。

Service Executionには、情報取得、情報変更、Resourceへのアクセス、
取引処理、購入処理、割引処理その他の処理を含めることができる。

Service Executionは、Authorization DecisionおよびEnforcementを経て
実行可能な状態となる。

---

## 27. Security Context

Policy EvaluationおよびAuthorization Evaluationでは、Security Context
を利用することができる。

Security Contextには、時刻、位置、端末状態、ネットワーク状態、
セッション状態、利用環境その他の情報を含めることができる。

---

## 28. Transaction

Transactionは、Authentication、Entitlement、Authorizationまたは
Service Executionに関連する一連の処理を識別または管理するために
利用することができる。

Transactionには、識別子、時刻、状態、関連Objectその他の情報を
関連付けることができる。

---

## 29. Object State

Authentication Object、Entitlementその他のProtocol Objectには、
Stateを設定することができる。

Stateには、Active、Expired、Revoked、Consumed、Suspendedその他の
状態を含めることができる。

Object StateはAuthorization Decisionそのものとは異なる情報として
管理することができる。

---

## 30. Revocation

Entitlementその他のObjectは、Validity Periodの終了とは独立して
Revocationされ得る。

RevocationされたObjectについては、Policyに従ってAuthorization
Evaluationにおいて無効なObjectとして扱うことができる。

---

## 31. Revalidation

必要に応じて、Authentication Result、Entitlementその他の情報を
再Validationすることができる。

Revalidationが成功しない場合、Policyに従ってDeny、Indeterminate、
再Authenticationその他の処理を実行することができる。

---

## 32. Fail-Closed

必要な条件を確認できない場合、Policyに従ってService Executionを
許可しない構成とすることができる。

例えば、必要なEntitlementのValidityを確認できない場合、
Authorization DecisionをPermitとせず、DenyまたはIndeterminateとして
扱うことができる。

---

## 33. 基本処理モデル

本発明の基本的処理モデルは、

1. Authenticationを実行する。
2. Authentication Resultを取得する。
3. Authentication ObjectのValidity等を確認する。
4. Entitlementを取得、生成または確認する。
5. EntitlementのStateおよびValidityを確認する。
6. Authentication Result、Entitlementその他のContextを用いて
   Policy Evaluationを実行する。
7. Authorization Evaluationを実行する。
8. Authorization Decisionを生成する。
9. Authorization DecisionをEnforcementに適用する。
10. Enforcementに基づいてService Executionを制御する。

という処理を含む。

ただし、上記の処理順序は一実施形態であり、本発明はこれに限定されない。

---

## 34. 来店認証とECサービスの実施形態

利用者が店舗を訪れた際、店舗ServiceにおいてAuthenticationを実行する。

AuthenticationにはQR Codeを利用することができる。

QR Codeは、生成時点から30秒間だけ有効とすることができる。

利用者によるAuthenticationが成功すると、店舗Serviceは
Authentication Resultを取得する。

店舗Serviceは、当該Authentication Resultに基づいて「来店済み」
というEntitlementを生成または有効化することができる。

当該Entitlementは、Authentication Objectとは異なるObjectとして
管理され、例えば数時間有効とすることができる。

その後、利用者がEC Serviceを利用する際、EC Serviceは当該Entitlement
を取得し、Policy Evaluationを実行する。

Policy Evaluationにおいて、当該Entitlementが有効であること、
所定の商品が対象であること、その他の条件が成立していることを
確認する。

条件が成立した場合、Authorization DecisionとしてPermitを生成する。

Enforcementは当該Permitを適用して、EC Serviceにおける割引処理を
Service Executionとして実行可能とする。

この構成によれば、店舗で使用されたAuthentication ObjectのValidity
が30秒であっても、「来店済み」というEntitlementを数時間有効と
することができる。

---

## 35. QR Codeを利用する実施形態

Authentication ObjectとしてQR Codeを利用することができる。

QR Codeには、Authenticationに必要な一時的情報を含めることができる。

QR CodeのValidity Periodは任意に設定することができる。

例えば30秒とすることができるが、本発明は30秒という数値に限定
されない。

QR CodeのValidityが終了した場合、当該QR Codeを利用したAuthentication
を拒否することができる。

また、QR CodeをOne-time Useとして管理することにより、同一QR Codeの
Replayを防止することができる。

---

## 36. 公開鍵暗号を利用する実施形態

Authenticationでは公開鍵暗号方式を利用することができる。

例えば、端末側に秘密鍵を保持し、サーバ側に対応する公開鍵を保持
することができる。

AuthenticationにおいてChallengeおよびResponseを利用し、秘密鍵を
保持する端末であることを確認することができる。

秘密鍵は、Secure Enclave、Trusted Execution Environmentその他の
安全性を備えた領域に保持することができる。

ただし、本発明は特定の鍵保管方式に限定されない。

---

## 37. 分散システムへの適用

Authentication、Entitlement、Policy Evaluation、Authorization、
EnforcementおよびService Executionは、異なるサーバ、サービス、
APIまたはネットワークノードに配置することができる。

例えば、Authentication Service、Entitlement Service、Policy Service、
Authorization ServiceおよびApplication Serviceを別々に配置する
ことができる。

これらのサービス間ではAPI、メッセージ、Tokenその他の情報交換手段
を利用することができる。

---

## 38. 時間的有効性の多層構造

本発明では、時間に関するValidityを複数のObjectおよび処理について
独立して設定することができる。

例えば、

Authentication Object Validity
Authentication Result Freshness
Entitlement Validity
Entitlement Expiration
Transaction Validity
Policy上の時間条件

等を、それぞれ異なる条件として管理することができる。

これにより、Authenticationの短時間ValidityとEntitlementの長時間
Validityとを両立させることができる。

---

## 39. 発明の効果

本発明によれば、AuthenticationとEntitlementとを異なる概念として
管理することができる。

また、Authentication ObjectのValidityとEntitlementのValidityとを
独立して管理することができる。

さらに、Authentication Result、Entitlement、Policyその他のContextを
Policy Evaluationに利用し、その結果としてAuthorization Decisionを
生成することができる。

また、Authorization DecisionとEnforcementとを分離することにより、
Decisionと実際のService Executionとの間に明確な制御境界を設ける
ことができる。

さらに、あるServiceで成立したEntitlementを別のServiceにおける
Authorizationに利用することができる。

---

## 40. 図面の簡単な説明

【図1】本発明の全体的なProtocol Architectureを示す図である。

【図2】Authentication、Entitlement、Policy EvaluationおよびAuthorizationの関係を示す図である。

【図3】Entitlement ObjectおよびそのLifecycleを示す図である。

【図4】Security ContextのAssembly、Policy EvaluationおよびAuthorization Decisionの関係を示す図である。

【図5】Authorization Decisionを保護対象OperationにBindingし、Service Executionへ適用する関係を示す図である。

【図6】Authorization StateのInvalidation、RevocationおよびFail-Closedによる伝播制御の関係を示す図である。

【図7】Distributed AuthorizationにおけるConsistencyの関係を示す図である。

---

## 41. 本発明の一般化

本発明は、QR Codeを利用するAuthenticationに限定されない。

また、特定の認証方式、通信方式、データベース、サーバ構成、
クラウドサービスまたはサービス業種に限定されない。

Authenticationによって確認された結果と、Authenticationとは独立して
管理され得るEntitlementとを用いてPolicy Evaluationを行い、その結果
に基づいてAuthorization Decisionを生成し、当該Authorization Decision
をEnforcementを介してService Executionに適用する構成であれば、
様々な情報処理システムに適用することができる。

---

## 42. 変形例

本発明では、Entitlementを一つのServiceから別のServiceに伝達する
ことができる。

また、複数のEntitlementを同時にPolicy Evaluationに利用することが
できる。

さらに、Authentication Result、Entitlement、Security Contextおよび
Transactionの組合せによって異なるAuthorization Decisionを生成する
ことができる。

Authorization DecisionはPermit、DenyまたはIndeterminate以外の
状態を含むことができる。

Enforcementは、Service Executionの単純な許可または拒否だけでなく、
処理内容の変更、利用可能な機能の限定、価格または条件の変更その他の
制御を行うことができる。

---

## 43. 産業上の利用可能性

本発明は、Webサービス、ECサービス、会員サービス、店舗サービス、
金融関連サービス、業務システム、APIサービス、クラウドサービス、
モバイルサービスその他の情報処理サービスに利用することができる。

また、複数のサービスを横断して権利情報または事実情報を利用する
必要があるシステムに適用することができる。

---

## 44. 明細書と請求項との対応

本明細書に記載されたAuthentication、Authentication Result、
Authentication Object、Entitlement、Policy Evaluation、
Authorization Evaluation、Authorization Decision、Enforcementおよび
Service Executionの構成は、請求項に記載される構成を支持するための
技術的説明として使用することができる。

特に、Authentication ObjectのValidityとEntitlementのValidityとの
独立性、Conditional Entitlement、Cross-Service Entitlement、
Policy EvaluationとAuthorization Decisionとの分離、Authorization
DecisionとEnforcementとの分離およびEnforcementとService Execution
との分離について、本明細書において複数の実施形態を記載する。

---

## 45. 本書の位置付け

本書は、Version 2.0における発明の核心、請求項構造および従属請求項
戦略を基礎として作成した特許明細書ドラフトである。

本書は、特許出願時に必要となる正式な書式、出願人情報、発明者情報、
優先権情報その他の手続事項を含むものではない。

また、実際の出願に際しては、請求項と明細書とのサポート関係、
新規事項の有無、先行技術との関係、実施可能要件その他について
最終的な専門家レビューを行うものとする。

