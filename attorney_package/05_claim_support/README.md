
NEW-shot2play Technical Specification Version 2.0
Claim Support Package
Attorney Review Draft

対象: 日本特許出願
目的: 請求項と明細書・図面・技術資料とのSupport関係の確認
位置付け: 弁理士レビュー用資料

1. 本書の目的

本ディレクトリには、Version 2.0の請求項について、明細書、図面および技術資料によるSupport関係を確認するための資料を格納する。

本資料は請求項そのものを変更するものではない。

現行Version 2.0のClaim Architectureを維持した上で、弁理士が各Claim Elementの技術的Supportを確認できるようにすることを目的とする。

2. 主要Support資料

主要なSupport資料は以下である。

patent/specification_v2.md
patent/invention_core_v2.md
patent/claims_v2.md
patent/dependent_claim_strategy_v2.md
protocol/object_model.md
protocol/normative_requirements.md
design/canonical_object_graph.md
figures/

日本特許出願用に再構成した資料については、以下を参照する。

attorney_package/02_application_documents/specification_v2_attorney_review.md
attorney_package/02_application_documents/claims_v2_attorney_review.md
attorney_package/03_drawings/drawing_support_matrix_v2.md
3. Support確認の基本原則

Support確認では、単に同一の単語が存在するかではなく、Claim Elementとして記載された技術的構成が明細書その他の技術資料によって実質的に説明されているかを確認する。

特に以下を重視する。

構成要件そのもののSupport
構成要件間の技術的関係のSupport
独立したLifecycleのSupport
Validityの関係のSupport
処理順序または処理関係のSupport
実施形態によるSupport
図面によるSupport
4. Claim 1の重点確認事項

Claim 1について特に重要なSupport対象は以下である。

Authentication processing
Authentication Result processing
Entitlement processing
Policy Evaluation processing
Authorization processing
Authorization Decision
Enforcement
Service Execution
Authentication ResultとEntitlementの分離
Authentication Object ValidityとEntitlement Validityの独立性
Policy Evaluationへの複数Context入力
Authorization DecisionとEnforcementの分離
EnforcementによるService Execution制御
5. 評価方法

各Claim Elementについて、以下の観点で確認する。

STRONG

明細書および関連技術資料に明確なSupportが存在する。

SUPPORTED

技術的Supportは存在するが、弁理士による日本特許出願向け表現の確認が望ましい。

ATTORNEY REVIEW

技術内容としてはSupportされるが、請求項表現または明細書記載との対応について弁理士による最終確認が必要。

ATTENTION

Supportの明確性について追加確認が望ましい。

この分類は法的判断ではなく、発明者側の技術レビュー上の分類である。

6. Claim Support Matrix

詳細なClaim Support Matrixは、本ディレクトリの

claim1_support_matrix_v2.md

に記載する。

7. 本書の位置付け

本資料は、Version 2.0のClaim Scopeを変更するためのものではない。

弁理士が現行Claim Architectureを維持するか、または日本出願向けに修正するかを判断するための技術的検討資料である。

End of Document
