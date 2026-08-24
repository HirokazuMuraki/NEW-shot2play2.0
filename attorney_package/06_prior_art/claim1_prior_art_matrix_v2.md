
NEW-shot2play Technical Specification Version 2.0
Claim 1 Element-by-Element Prior Art Matrix
Attorney Review Draft

対象: 日本特許出願
目的: Claim 1の新規性・進歩性検討
基礎請求項: patent/claims_v2.md
関連明細書: attorney_package/02_application_documents/specification_v2_attorney_review.md

1. 本資料の目的

本資料は、Version 2.0のClaim 1を構成要素ごとに分解し、
これまで検討した主要な先行技術領域との関係を整理するものである。

本資料は法的な先行技術判断ではない。

「既知技術との関係が強い部分」と
「本発明の組合せとして評価すべき部分」を分離し、
弁理士による新規性・進歩性判断を容易にすることを目的とする。

2. Claim 1の評価単位

Claim 1の中心的な処理関係は、概念的には以下のように整理できる。

Authentication
→ Authentication Result
→ Entitlement
→ Policy Evaluation
→ Authorization Decision
→ Enforcement
→ Service Execution

これに加えて、

Authentication Object Validity
と
Entitlement Validity

を異なるValidityとして管理可能とする点が重要な評価対象となる。

3. Element 1 — Authentication Processing Unit
Claim Element

Authenticationを実行するAuthentication processing unit。

技術的意味

SubjectまたはDevice等についてAuthenticationを実行し、
Authentication Object等に基づいてAuthenticationを成立させる。

Prior Artとの関係

FIDO Cross-Device Authentication、Passkey、
Microsoft Authenticator、Device Flow等、
Authenticationそのものは既知技術領域である。

新規性上の評価

Authentication単独では、既知技術との重複可能性が高い。

したがって、Claim 1全体の特許性をAuthentication単独に依存させるべきではない。

進歩性上の評価

Authentication処理自体ではなく、
Authentication後にどのような情報を生成し、
その情報をどのようにEntitlementおよびAuthorizationへ接続するかが
重要となる。

弁理士確認事項

Authentication processing unitの具体的構成について、
Claim 1に必要な限定範囲を確認する。

4. Element 2 — Authentication Result Processing Unit
Claim Element

Authenticationの結果としてAuthentication Resultを生成または処理する
Authentication Result processing unit。

技術的意味

Authenticationが成立したという結果を、
後続処理で利用可能な独立した情報として扱う。

Prior Artとの関係

Authentication ResultまたはAuthentication statusに相当する情報は、
一般的なAuthenticationシステムに存在し得る。

新規性上の評価

Authentication Resultという概念だけでは、
必ずしも新規性を強く主張できない可能性がある。

重要点

本発明では、Authentication Resultを
Entitlementそのものとは扱わない。

この分離が後続のPolicy EvaluationおよびAuthorizationに
つながることが重要である。

弁理士確認事項

Authentication Resultの定義およびEntitlementとの関係を
Claim 1上どの程度明確に限定すべきか確認する。

5. Element 3 — Entitlement Processing Unit
Claim Element

Authentication Resultとは独立して管理され得るEntitlementを処理する
Entitlement processing unit。

技術的意味

Authenticationの成立結果とは別に、
Service利用に関する権利、資格、条件または利用可能性を
Entitlementとして管理する。

Prior Artとの関係

OAuthのScope、Token、Grant、
Access ControlにおけるPermission等、
Service利用権を表す情報自体は既知である。

重要な相違点

本発明では、EntitlementをAuthentication Resultとは
異なるObjectとして管理することができる。

さらにEntitlementは独立したLifecycleを有し得る。

新規性上の評価

単なるPermissionまたはScopeの存在ではなく、
Authentication Resultとの分離およびLifecycleの独立性を含めて評価する必要がある。

進歩性上の評価

Authenticationの成立とService利用権の管理を分離することで、
Authenticationの短時間Validityとは異なる期間で
Service利用権を管理できる構造につながる。

弁理士確認事項

Entitlementの「独立性」をClaim 1の重要な限定として
どのように表現するか確認する。

6. Element 4 — Authentication Object Validity
Claim Element

Authentication ObjectについてValidity、Expirationその他の
時間的有効性を管理する構成。

技術的意味

Authenticationに利用されるObjectを短時間のみ有効とすることができる。

Prior Artとの関係

Token Expiration、Nonce、Challenge Timeout等、
Authentication関連Objectの時間的Validityは既知技術である。

新規性上の評価

Authentication ObjectのValidity単独では、
新規性を主張することは困難となる可能性がある。

重要点

本発明では、Authentication Object Validityを
Entitlement Validityと同一視しない。

弁理士確認事項

Validityそのものではなく、
複数のValidityを独立管理する構造との関係で評価する。

7. Element 5 — Entitlement Validity
Claim Element

EntitlementについてValidityまたはExpirationを管理する構成。

技術的意味

Entitlementを一定期間または一定条件のもとで有効とする。

Prior Artとの関係

Token、Grant、Subscription、License、
Access Permission等にValidityまたはExpirationを設定する技術は既知である。

重要点

本発明ではEntitlement Validityを
Authentication Object Validityから独立して管理できる。

弁理士確認事項

単なるEntitlement Expirationではなく、
Authentication Object Validityとの関係を含めて評価する。

8. Element 6 — Validity Independence
Claim Element

Authentication ObjectのValidityとEntitlementのValidityとを
異なるLifecycleまたはValidityとして管理可能とする構成。

技術的意味

Authentication Objectが短時間で失効しても、
既に生成されたEntitlementが必ずしも同時に失効する必要がない。

Prior Artとの関係

各種TokenやSessionに個別のExpirationを設定する技術は既知である。

重要な評価点

本発明では、異なる種類のObjectについて
異なるValidityを持たせるだけではなく、

Authentication
と
Service利用権

との間に異なる時間的Lifecycleを設ける。

新規性・進歩性上の評価

この構成はClaim 1または関連従属請求項の
重要な評価ポイントとなる可能性がある。

弁理士確認事項

この構成を独立請求項に含めることのメリットと、
Claim Scopeへの影響を評価する。

9. Element 7 — Policy Evaluation Processing Unit
Claim Element

Authentication Result、Entitlementおよびその他のContext情報の
少なくとも一つに基づいてPolicy Evaluationを実行する処理部。

技術的意味

Authenticationの成功だけではなく、
Entitlementおよびその他のContextを考慮して
Service利用条件を評価する。

Prior Artとの関係

Policy Engine、ABAC、RBAC、Access Control等における
Policy Evaluationは既知技術である。

新規性上の評価

Policy Evaluation単独では既知技術との重複可能性が高い。

重要点

本発明では、Policy Evaluationの入力として
Authentication ResultだけでなくEntitlementを利用する。

進歩性上の評価

Authentication ResultとEntitlementを分離した上で、
両者をPolicy Evaluationへ入力する関係について評価する必要がある。

弁理士確認事項

既知Policy Engineが本発明の入力関係およびObject分離を
開示しているか確認する。

10. Element 8 — Authorization Evaluation
Claim Element

Policy Evaluationその他の情報に基づいて、
所定のActionまたはService Executionが許可されるべきかを評価する構成。

技術的意味

PolicyおよびContextからAuthorizationの可否を判断する。

Prior Artとの関係

Authorization EvaluationおよびAccess Controlは広く既知である。

評価

この構成単独での新規性は弱い可能性がある。

ただし、前段のEntitlementおよびPolicy Evaluationとの
関係を含めて評価する必要がある。

11. Element 9 — Authorization Decision
Claim Element

Policy EvaluationまたはAuthorization Evaluationの結果に基づいて
Authorization Decisionを生成するAuthorization processing unit。

技術的意味

Permit、Deny、Indeterminate等のDecisionを生成する。

Prior Artとの関係

Access ControlおよびPolicy Decision Pointにおける
Permit / Deny等は既知技術である。

新規性上の評価

Authorization Decision単独では既知技術との重複可能性が高い。

重要点

本発明では、Authorization Decisionが
Entitlementを含むContextのPolicy Evaluation結果として生成され、
さらにEnforcementへ渡される。

12. Element 10 — Enforcement Processing Unit
Claim Element

Authorization Decisionに基づいてEnforcementを実行する
Enforcement processing unit。

技術的意味

DecisionとService Executionとの間に実際の制御段階を設ける。

Prior Artとの関係

Policy Enforcement Point、Access Gateway、
Authorization Middleware等の既知技術と関連する。

重要点

本発明ではAuthorization DecisionとEnforcementを
別個の処理として扱う。

弁理士確認事項

既知のPDP / PEP構成との関係を確認し、
本発明におけるEnforcementの限定が十分な意味を持つか評価する。

13. Element 11 — Service Execution Processing Unit
Claim Element

Enforcementに基づいて保護対象Service Executionを制御する
Service Execution processing unit。

技術的意味

最終的なServiceへのアクセス、情報取得、情報変更、
Resource操作等を実際に実行または拒否する。

Prior Artとの関係

Authorization結果に応じてService Accessを制御する技術は既知である。

評価

Service Execution単独では新規性を主張する部分ではない。

重要なのは、

Authorization Decision
→ Enforcement
→ Service Execution

という処理境界である。

14. Element 12 — Authentication Result / Entitlement Separation
Claim Element

Authentication ResultとEntitlementとを別個の情報として管理する。

Prior Art比較

AuthenticationシステムではAuthentication Resultが生成される。

AuthorizationシステムではPermission、Scope、Token等が使用される。

しかし、これらを本発明のように、

Authentication Result

と

Entitlement

という異なるObjectとして整理し、
独立したLifecycleを持たせ、
Policy Evaluationに入力する構造については
文献ごとの具体的開示を確認する必要がある。

重要度

高

本発明の中心的構成の一つである。

15. Element 13 — Independent Lifecycle
Claim Element

Authentication ResultとEntitlementについて、
異なるLifecycleを設定可能とする。

Prior Art比較

Token、Session、Credential等にLifecycleを設定すること自体は既知である。

しかし、Authentication ObjectのLifecycleと
Service利用権としてのEntitlement Lifecycleを
明確に分離する構造は、個々の文献について具体的に確認する必要がある。

重要度

高

16. Element 14 — Cross-Service Entitlement
Claim Element

Entitlementを異なるServiceにおけるPolicy Evaluationまたは
Authorizationに利用可能とする構成。

Prior Art比較

OAuth Scope、Token、SSO、Federation等には
複数ServiceにわたるAuthorization情報の利用が存在する。

したがって、Cross-Serviceという概念単独では
既知技術との関係が強い可能性がある。

一方、独立Entitlement Objectとして扱い、
各ServiceのPolicy Evaluationへ入力する構造については
具体的な開示内容を確認する必要がある。

重要度

中〜高

17. Element 15 — Conditional Entitlement
Claim Element

条件付きEntitlementをPolicy Evaluationに利用する構成。

Prior Art比較

Conditional Access、ABAC、Policy-based Access Control等には
条件に基づくAuthorizationが存在する。

したがって、Conditional Authorization自体は既知技術である。

本発明では、条件付きEntitlementをAuthentication Resultとは
独立したObjectとして扱える点との組合せが重要である。

重要度

中

18. 主要先行技術との総合比較
技術領域	Authentication	Device Flow	Entitlement / Permission	Policy Evaluation	Authorization	Enforcement	Service Execution	本発明との主要相違点
FIDO Cross-Device Authentication	強	強	限定的	限定的	関連	限定的	関連	Authentication後のEntitlement Lifecycle
Apple Passkey	強	関連	限定的	限定的	関連	限定的	関連	AuthenticationとEntitlementの分離
Google Password Manager	強	関連	限定的	限定的	関連	限定的	関連	Credential中心でありEntitlement中心ではない
Microsoft Authenticator	強	強	限定的	関連	関連	関連	関連	独立Entitlement Lifecycle
WhatsApp Web	強	強	限定的	限定的	関連	関連	強	Device Linking中心
GitHub Device Flow	強	強	関連	関連	強	関連	強	Authentication ResultとEntitlementの独立性
OAuth 2.0 Device Authorization Grant	関連	強	強	関連	強	関連	強	Entitlement Objectと独立Lifecycle
NEW-shot2play V2.0	強	関連	強	強	強	強	強	各Object / Lifecycleを分離した一連の処理構造
19. Claim 1の特許性評価上の重点

発明者側として特に重要と考えるのは、
以下の組合せである。

Authentication Result
Authentication Resultとは独立したEntitlement
Authentication Object Validity
Entitlement Validity
両Validityの独立管理
EntitlementをPolicy Evaluationへ入力
Authorization Decision
Enforcement
Service Execution

これらを単独の技術として評価するのではなく、
一連の処理関係として評価することを弁理士に求める。

20. 新規性評価のための確認表
評価対象	単独既知可能性	Claim 1全体との関係
Authentication	高	中心ではない
Authentication Result	高	Entitlementとの分離が重要
Authentication Object Validity	高	Entitlement Validityとの関係が重要
Entitlement	高	独立Objectとしての扱いが重要
Entitlement Validity	高	Authentication Validityとの独立性が重要
Policy Evaluation	高	Entitlementを入力とする関係が重要
Authorization Decision	高	後段処理との関係が重要
Enforcement	高	Decisionとの分離が重要
Service Execution	高	Enforcementとの境界が重要
全体構成	要検討	最重要
21. 進歩性評価のための確認表
組合せ	発明者側の評価
Authentication + Authorization	既知技術との関係が強い
Authentication + Policy Evaluation	既知技術との関係が強い
Authentication + Entitlement	既知技術との関係を要確認
Authentication Result + Entitlementの独立性	重要
Authentication Validity + Entitlement Validityの独立性	重要
Entitlement + Policy Evaluation	重要
Policy Evaluation + Authorization Decision	既知技術との関係が強い
Authorization Decision + Enforcement	既知技術との関係が強い
Enforcement + Service Execution	既知技術との関係が強い
上記全体の組合せ	最重要評価対象
22. 弁理士に確認を求める事項
Claim 1の全構成を単一先行技術文献が開示しているか。
Authentication ResultとEntitlementの分離が先行技術に開示されているか。
Authentication Object ValidityとEntitlement Validityの独立性が開示されているか。
EntitlementをPolicy Evaluationへ入力する構成が開示されているか。
Authorization DecisionとEnforcementの分離が開示されているか。
上記構成を組み合わせる動機付けが存在するか。
組合せによる技術的効果が評価できるか。
Claim 1を維持したまま、従属請求項によって特許性を補強すべきか。
Claim 1のScopeをどの範囲に設定することが適切か。
日本出願において、どの構成を独立請求項の中核として維持すべきか。
23. 最終整理

現時点では、Claim 1の個々の要素の多くについて
既知技術との関係が強い。

したがって、本発明の特許性については、
個々の要素の新規性だけを問題にするのではなく、

Authentication ResultとEntitlementの分離、

異なるValidityおよびLifecycle、

Entitlementを利用したPolicy Evaluation、

Authorization Decision、

Enforcement、

Service Execution

という構成の組合せについて、
新規性および進歩性を評価する必要がある。

発明者側としては、現時点でVersion 2.0の発明概念を変更せず、
本Matrixを弁理士に提供して専門的な評価を受けることが適切と考える。

End of Document
