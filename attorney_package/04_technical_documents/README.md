
NEW-shot2play Technical Specification Version 2.0
技術資料・原本参照一覧
Attorney Review Package

対象: 日本特許出願
目的: 弁理士によるVersion 2.0技術内容の確認
位置付け: 技術資料への参照案内

1. 本書の目的

本書は、NEW-shot2play Technical Specification Version 2.0について、弁理士が発明の技術的構成を確認するための原本資料および関連資料を整理するものである。

本書自体は新たな技術内容を定義するものではない。

Version 2.0の技術内容については、Git repository内の既存のTechnical Specification、ProtocolおよびDesign資料を原本として扱う。

2. Version 2.0 Technical Specification

Version 2.0の特許技術内容については、以下を主要な原本資料とする。

patent/specification_v2.md
patent/invention_core_v2.md
patent/claims_v2.md
patent/claim_structure_v2.md
patent/dependent_claim_strategy_v2.md

これらはVersion 2.0の発明概念、明細書構成、請求項構成および従属Claim戦略を確認するための基礎資料である。

3. Protocol資料

Version 2.0の技術構造および処理モデルを確認するため、以下のProtocol資料を参照する。

protocol/object_model.md
protocol/normative_requirements.md

これらの資料には、Authentication、Entitlement、Policy、Authorization、Enforcement、Service Executionその他の処理およびObject間の意味的分離に関する技術的定義が含まれる。

4. Design資料

Version 2.0のObject間の関係および全体構造を確認するため、以下のDesign資料を参照する。

design/canonical_object_graph.md

本資料は、Authentication Result、Entitlement、Policy Evaluation、Authorization Decisionその他のObjectおよび処理段階の関係を確認する際の主要資料である。

5. Canonical Figures

Version 2.0では、以下のCanonical Figuresを主要な図面資料として扱う。

Overall Protocol Architecture
Authentication and Entitlement Relationship
Entitlement Object / Lifecycle
Context and Policy Evaluation
Authorization Decision and Capability
Authorization State / Revocation
Distributed Authorization Consistency

図面ファイルはrepositoryのfigures/ディレクトリに格納されている。

6. 技術的中心構造

Version 2.0における技術的中心構造は、概念的には以下の処理関係として整理される。

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

ただし、これらは単純な直列処理を意味するものではなく、Security Context、Transaction、Object State、Validity、Revocation、Revalidationその他のContext情報を含めて評価され得る。

7. 特に重要な技術的関係

弁理士による確認に際して、特に以下の関係を確認することが望ましい。

Authentication ResultとEntitlementの意味的分離
Authentication ObjectとEntitlementのLifecycleの分離
Authentication Object ValidityとEntitlement Validityの独立性
EntitlementをPolicy Evaluationの入力として利用する構造
Policy EvaluationとAuthorization Decisionの分離
Authorization DecisionとEnforcementの分離
EnforcementとService Executionの分離
RevocationおよびRevalidationによる状態管理
Fail-Closedによる安全側制御
分散システムにおけるAuthorization状態の整合性
8. 技術資料と出願書類の関係

本ディレクトリの技術資料は、弁理士が発明の技術的内容を理解するための補助資料である。

正式な日本特許出願書類については、

attorney_package/02_application_documents/

に格納された弁理士レビュー用ドラフトを基礎資料とする。

ただし、正式な出願書類としての最終的な構成、記載方法、法的表現および符号等については弁理士の判断によるものとする。

9. 原本管理について

Version 2.0の技術内容について、弁理士提出用資料を作成する際に、複数の独立した技術仕様を新たに作成しない。

原則として、

patent/
protocol/
design/
figures/

をVersion 2.0の技術原本として維持する。

attorney_package/内の資料は、これらの原本を弁理士が確認しやすいように整理した提出用資料である。

10. Version 1.0との関係

Version 1.0については、

attorney_package/07_version1_reference/

を参照する。

Version 1.0とVersion 2.0との技術的関係、公開状況および特許性への影響については、弁理士による最終評価を前提とする。

11. 本書の位置付け

本書は、Version 2.0の技術資料を新たに定義または変更するものではない。

弁理士がVersion 2.0の技術的原本を効率的に確認するための参照資料である。

End of Document
