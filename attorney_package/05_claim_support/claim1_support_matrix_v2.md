
NEW-shot2play Technical Specification Version 2.0
Claim 1 Support Matrix
Attorney Review Draft

対象: 日本特許出願
目的: Claim 1の各構成要件について、明細書・技術資料・図面によるSupportを確認する
基礎Claim: patent/claims_v2.md
関連明細書: patent/specification_v2.md

1. 本書の目的

本書は、Version 2.0のClaim 1について、各構成要件が明細書その他の技術資料によってSupportされているかを確認するための発明者側レビュー資料である。

本書はClaim 1そのものを変更するものではない。

また、本書におけるSupport評価は法的判断ではなく、弁理士による最終確認のための技術的整理である。

2. Claim 1の基本構造

Claim 1は、概念的には以下の処理構造を有する。

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

各処理段階は意味的に区別される。

また、Authentication ObjectのValidityとEntitlementのValidityは、必要に応じて独立して管理することができる。

3. Claim 1 Element 1
Authentication processing unit
Claim Element

Authenticationを実行するAuthentication processing unit。

Specification Support

patent/specification_v2.md

Section 7: Authentication
Section 8: Authentication Object
Section 33: 基本処理モデル
Section 35: QR Codeを利用する実施形態
Section 36: 公開鍵暗号を利用する実施形態
Technical Support

Authentication処理、Authentication ObjectおよびそのValidityについて明示的に記載されている。

Figure Support

Canonical FiguresのAuthentication関連部分により全体構造を説明可能。

Preliminary Evaluation

STRONG

Attorney Review

Authentication processing unitとしての法的表現について最終確認を求める。

4. Claim 1 Element 2
Authentication Result processing unit
Claim Element

Authentication processingの結果としてAuthentication Resultを生成または処理する構成。

Specification Support

patent/specification_v2.md

Section 5: 課題を解決するための手段
Section 6: 発明の基本構成
Section 15: Authentication ResultとEntitlementの独立性
Section 19: Policy Evaluation
Section 33: 基本処理モデル
Technical Support

Authentication ResultをAuthentication後の独立した情報として扱い、Entitlementその他のContextとともに後段のPolicy Evaluationに利用する構成が記載されている。

Figure Support

Authentication and Entitlement Relationshipを示すCanonical FigureによりSupport可能。

Preliminary Evaluation

STRONG

Attorney Review

Authentication Resultをどのような情報としてClaim上定義するかについて最終確認を求める。

5. Claim 1 Element 3
Entitlement processing unit
Claim Element

Authentication Resultとは独立して管理され得るEntitlementを処理する構成。

Specification Support

patent/specification_v2.md

Section 14: Entitlement
Section 15: Authentication ResultとEntitlementの独立性
Section 16: Conditional Entitlement
Section 17: Cross-Service Entitlement
Section 33: 基本処理モデル
Technical Support

EntitlementはAuthentication Resultそのものではなく、独立した情報として管理可能であることが明記されている。

Additional Support

patent/invention_core_v2.md

Authentication ResultとEntitlementの独立性、EntitlementのLifecycleおよびValidityについて記載されている。

design/canonical_object_graph.md

Authentication ResultとEntitlementを別個のObjectとして扱う構造が記載されている。

Figure Support

Authentication and Entitlement RelationshipおよびEntitlement Object / LifecycleのCanonical FigureによりSupport可能。

Preliminary Evaluation

STRONG

Attorney Review

Entitlementの法的表現および「独立して管理され得る」という表現について最終確認を求める。

6. Claim 1 Element 4
Policy Evaluation processing unit
Claim Element

Authentication Result、Entitlementその他のContext情報の少なくとも一つに基づいてPolicy Evaluationを実行する構成。

Specification Support

patent/specification_v2.md

Section 18: Policy
Section 19: Policy Evaluation
Section 27: Security Context
Section 33: 基本処理モデル
Section 37: 分散システムへの適用
Section 41: 本発明の一般化
Technical Support

Policy Evaluationの入力としてAuthentication Result、Entitlement、Subject、Resource、Action、Security Contextその他の情報を利用する構成が明示されている。

Figure Support

Context and Policy EvaluationのCanonical FigureによりSupport可能。

Preliminary Evaluation

STRONG

Attorney Review

Policy Evaluation processing unitの記載範囲および入力情報の表現について確認を求める。

7. Claim 1 Element 5
Authorization processing unit
Claim Element

Policy Evaluationその他の情報に基づいてAuthorizationを評価し、Authorization Decisionに至る構成。

Specification Support

patent/specification_v2.md

Section 20: Authorization Evaluation
Section 21: Authorization Decision
Section 22: Permit
Section 23: Deny
Section 24: Indeterminate
Technical Support

Policy Evaluation結果その他の情報に基づいてAuthorization Evaluationを実行し、Authorization Decisionを生成する構造が記載されている。

Figure Support

Authorization Decision and CapabilityのCanonical FigureによりSupport可能。

Preliminary Evaluation

SUPPORTED

Attorney Review

Authorization processing unitとAuthorization Evaluationの関係について、請求項上の表現を弁理士に確認していただきたい。

8. Claim 1 Element 6
Authorization Decision
Claim Element

Authorization EvaluationまたはPolicy Evaluation等の結果に基づいてAuthorization Decisionを生成する構成。

Specification Support

patent/specification_v2.md

Section 20: Authorization Evaluation
Section 21: Authorization Decision
Section 22: Permit
Section 23: Deny
Section 24: Indeterminate
Technical Support

Authorization DecisionをPolicyまたはPolicy Evaluationそのものとは異なる情報として扱う構造が明示されている。

Figure Support

Authorization Decision and CapabilityのCanonical FigureによりSupport可能。

Preliminary Evaluation

STRONG

Attorney Review

Permit、Deny、IndeterminateをClaim 1でどこまで限定するかは弁理士判断に委ねる。

9. Claim 1 Element 7
Enforcement processing unit
Claim Element

Authorization Decisionに基づいてEnforcementを実行する構成。

Specification Support

patent/specification_v2.md

Section 25: Enforcement
Section 33: 基本処理モデル
Section 39: 発明の効果
Section 41: 本発明の一般化
Technical Support

Authorization DecisionとEnforcementを別個の処理として管理し、Enforcementによって実際のService Executionを制御する構成が明記されている。

Figure Support

Authorization Decision and CapabilityおよびAuthorization State / RevocationのCanonical FigureによりSupport可能。

Preliminary Evaluation

STRONG

Attorney Review

Enforcementを単なる実装上の処理ではなくClaim Elementとして扱うことの法的評価を求める。

10. Claim 1 Element 8
Service Execution processing unit
Claim Element

Enforcementに基づいて保護対象のService Executionを制御する構成。

Specification Support

patent/specification_v2.md

Section 26: Service Execution
Section 33: 基本処理モデル
Section 34: 来店認証とECサービスの実施形態
Section 39: 発明の効果
Technical Support

Service ExecutionをAuthorization DecisionおよびEnforcementを経て制御する構成が記載されている。

Figure Support

Authorization Decision and CapabilityのCanonical FigureによりSupport可能。

Preliminary Evaluation

STRONG

Attorney Review

Service Execution processing unitの範囲について最終確認を求める。

11. Claim 1 Element 9
Authentication ResultとEntitlementの独立性
Claim Element

Authentication Resultとは独立して管理され得るEntitlement。

Specification Support

patent/specification_v2.md

Section 11: Authentication ValidityとEntitlement Validity
Section 14: Entitlement
Section 15: Authentication ResultとEntitlementの独立性
Section 38: 時間的有効性の多層構造
Technical Support

Authentication ResultとEntitlementを同一Objectとして扱う必要はなく、異なる情報として管理可能であることが記載されている。

Design Support

design/canonical_object_graph.md

Authentication ResultとEntitlementを別個のLogical Objectとして扱う構造が明示されている。

Preliminary Evaluation

STRONG

Attorney Review

本要素はVersion 2.0の中心的な技術的特徴の一つであるため、Claim Scope上の重要性を確認する。

12. Claim 1 Element 10
Authentication Object ValidityとEntitlement Validity
Claim Element

Authentication ObjectのValidityとEntitlementのValidityを異なるLifecycleとして管理可能とする構成。

Specification Support

patent/specification_v2.md

Section 9: Authentication ObjectのValidity
Section 11: Authentication ValidityとEntitlement Validity
Section 38: 時間的有効性の多層構造
Technical Support

Authentication ObjectにはValidity Periodを設定でき、Entitlementには独立したValidityを設定できる。

両者は同一のLifecycleとして扱う必要がない。

Important Note

30秒等の具体的な時間は一実施例にすぎない。

本構成の技術的意味は、Authentication ObjectのValidityとEntitlementのValidityを独立して管理できることにある。

Preliminary Evaluation

STRONG

Attorney Review

本構成を独立Claimまたは従属Claimのどちらで保護するかについて弁理士判断を求める。

13. Claim 1 Element 11
Context情報
Claim Element

Authentication Result、Entitlementその他のContext情報をPolicy Evaluationに利用する構成。

Specification Support

patent/specification_v2.md

Section 18: Policy
Section 19: Policy Evaluation
Section 27: Security Context
Section 28: Transaction
Section 33: 基本処理モデル
Technical Support

複数の情報をContextとして評価し、Authorization Decisionを生成する構成が記載されている。

Preliminary Evaluation

STRONG

Attorney Review

Context情報の具体的範囲をClaim上どこまで限定するか確認を求める。

14. Claim 1 Element 12
Revocation / Revalidation
Claim Element

Entitlementその他のObjectについてRevocationまたはRevalidationを行い、AuthorizationおよびService Executionを制御する構成。

Specification Support

patent/specification_v2.md

Section 30: Revocation
Section 31: Revalidation
Section 32: Fail-Closed
Technical Support

Object StateおよびValidityを確認し、必要な条件が成立しない場合にDenyまたはIndeterminateとして処理する構成が記載されている。

Figure Support

Authorization State / RevocationのCanonical FigureによりSupport可能。

Preliminary Evaluation

SUPPORTED

Attorney Review

Claim 1の必須要件とするか、従属Claimで保護するかについて弁理士判断を求める。

15. Claim 1 Element 13
QR Code
Claim Element

Authentication ObjectまたはAuthentication処理にQR Codeを利用する構成。

Specification Support

patent/specification_v2.md

Section 35: QR Codeを利用する実施形態
Technical Support

QR Codeを利用したAuthentication Objectの提示およびAuthentication処理について記載されている。

Preliminary Evaluation

SUPPORTED

Attorney Review

QR CodeをClaim 1の必須構成とするか、従属Claimまたは実施形態として扱うか確認を求める。

16. Claim 1 Element 14
公開鍵暗号
Claim Element

Authenticationに公開鍵暗号方式を利用する構成。

Specification Support

patent/specification_v2.md

Section 36: 公開鍵暗号を利用する実施形態
Technical Support

公開鍵暗号を利用したAuthenticationについて記載されている。

Preliminary Evaluation

SUPPORTED

Attorney Review

公開鍵暗号をClaim Scopeに含める範囲について最終判断を求める。

17. Overall Support Evaluation

Claim 1の中心的構造については、明細書および技術資料に相当するSupportが存在する。

特に以下の構成については明確なSupportが確認できる。

Authentication ResultとEntitlementの分離
Authentication Object ValidityとEntitlement Validityの独立性
EntitlementをPolicy Evaluationへ入力する構造
Policy Evaluation
Authorization Decision
Enforcement
Service Execution

これらを一連の処理構造として構成することについても、Version 2.0の明細書および技術資料に記載されている。

18. 特許性上の重点Support

本発明の特許性を検討する際には、単一の構成要素だけではなく、以下の組合せを重視することが望ましい。

Authentication Result
+
Entitlement
+
独立したLifecycle
+
Policy Evaluation
+
Authorization Decision
+
Enforcement
+
Service Execution

さらに、

Authentication Object Validity
と
Entitlement Validity

を独立して管理可能とする関係も重要である。

19. 弁理士への確認事項

以下について最終確認をお願いしたい。

Claim 1の各Elementについて日本特許法上のSupport要件を満たすか。
Claim 1の構成要件間の技術的関係についてSupportが十分か。
Authentication ResultとEntitlementの独立性をClaim上どのように表現することが適切か。
Authentication Object ValidityとEntitlement Validityの独立性をどのClaimに配置するか。
Policy Evaluation、Authorization Decision、EnforcementおよびService Executionの関係をClaim Scopeとして維持することが適切か。
QR Codeおよび公開鍵暗号を独立Claimまたは従属Claimで扱うことが適切か。
Revocation / RevalidationをどのClaimに配置することが適切か。
20. 結論

発明者側の技術レビューでは、Claim 1の中心的な構成について重大なSupport欠落は確認されていない。

一方、日本特許出願におけるSupport要件、明確性、Claim Scopeおよび法的な記載要件については弁理士による最終確認が必要である。

本資料では、現行Version 2.0の技術構造およびClaim Architectureを変更しない。

End of Document
