# NEW-shot2play Technical Specification Version 2.0
# 特許性検討事項・弁理士共有メモ

**Document Type:** Patentability Review / Attorney Discussion Memo  
**Target:** Japan Patent Application  
**Version:** 1.0  
**Status:** Attorney Review Draft  
**Related Specification:** NEW-shot2play Technical Specification Version 2.0

---

# 1. 本文書の目的

本書は、NEW-shot2play Technical Specification Version 2.0について、
日本における特許出願を行うにあたり、これまで実施した技術仕様レビュー、
請求項レビュー、明細書サポートレビューおよび先行技術との比較検討の結果を
整理し、弁理士との検討・協議に使用することを目的とする。

本書は特許出願書類そのものではなく、出願方針および特許性についての
技術側からの検討資料である。

最終的な特許性判断、請求項の法的表現、先行技術評価および出願書類の
確定については、弁理士による検討を前提とする。

---

# 2. Version 2.0の位置付け

NEW-shot2play Technical Specification Version 2.0は、
Version 1.0の単なる改訂版ではなく、新たな発明概念および
新たな特許出願を目的として構成した技術仕様である。

Version 1.0は、Version 2.0を設計する際の前段階の仕様・技術的基礎として
位置付ける。

Version 2.0では、従来のAuthentication中心の構成から、
Authentication後のサービス利用権および利用制御を含む
多段階の処理モデルへ発明概念を拡張している。

---

# 3. 発明の核心

Version 2.0の基本処理モデルは以下である。

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

本発明において特に重要な点は、
Authentication ResultとEntitlementを同一の情報として扱うのではなく、
別個の情報として取り扱える点にある。

さらに、EntitlementのValidityを、
Authenticationに使用されるAuthentication ObjectのValidityとは
独立して管理可能とする。

したがって、本発明の中心的な技術関係は、

Authentication Object Validity
≠
Entitlement Validity

である。

Authentication ObjectのValidityが終了した場合でも、
それによって直ちにEntitlementそのもののValidityを終了させる必要はなく、
逆にEntitlementの失効等によってAuthentication Objectそのものを
無効化する必要もない。

このように、Authenticationとサービス利用権のLifecycleを
分離して管理できることがVersion 2.0の重要な特徴である。

---

# 4. 独立請求項

現在のVersion 2.0では、以下の3つの独立請求項を中心としている。

## 4.1 Claim 1 — System

Claim 1は、以下の処理構成を含む情報処理システムとして構成している。

- Authentication
- Authentication Result取得
- Entitlement取得・生成・確認
- Policy Evaluation
- Authorization Decision
- Enforcement
- Service Execution

さらに、

- Authentication ResultとEntitlementの分離
- Authentication Object ValidityとEntitlement Validityの独立管理
- Authorization Decisionに基づくService Execution制御

を規定している。

## 4.2 Claim 31 — Method

Claim 31は、Claim 1の技術的処理関係を情報処理方法として構成する。

## 4.3 Claim 36 — Program

Claim 36は、同じ発明概念をプログラムとして構成する。

---

# 5. 特許性上、特に重視している構成

以下をVersion 2.0の重要な発明要素としている。

## 5.1 Authentication ResultとEntitlementの分離

Authenticationが成功したことを示すAuthentication Resultと、
サービスを利用する権利を示すEntitlementを別個に扱う。

Authentication Resultが存在することと、
特定Serviceを利用するEntitlementが存在することを同一視しない。

---

## 5.2 Authentication Object ValidityとEntitlement Validityの分離

Authenticationに使用されるAuthentication Objectには、
独自のValidity PeriodまたはExpiration等を設定できる。

一方、Entitlementについても独自のValidityまたはLifecycleを設定できる。

両者のValidityは必ずしも一致する必要がない。

---

## 5.3 Policy Evaluation

Authentication ResultおよびEntitlementだけでなく、
Subject、Resource、Contextその他の情報とPolicyを利用して
Policy Evaluationを実行する。

---

## 5.4 Authorization Decision

Policy Evaluationの結果に基づいてAuthorization Decisionを生成する。

Authorization DecisionはService Executionそのものとは別の
論理的処理結果として扱う。

---

## 5.5 Enforcement

Authorization DecisionをService Executionに反映するため、
Enforcementを独立した処理段階として扱う。

---

## 5.6 Service Execution

最終的なService ExecutionをAuthorization Decisionおよび
Enforcementによって制御する。

これにより、

Authentication
→ Authorization
→ Service

という単純な構成ではなく、

Authentication
→ Entitlement
→ Policy Evaluation
→ Authorization Decision
→ Enforcement
→ Service Execution

という多段階のサービス利用制御を構成する。

---

# 6. QR Codeおよび30秒Validityの位置付け

Version 2.0にはQR CodeをAuthentication Objectの一例として
使用する構成が含まれている。

また、QR Code等のAuthentication Objectについて、
所定のValidity Periodを設定する構成を含む。

30秒は、その具体的な実施例の一つである。

30秒そのものを本発明の核心とは位置付けていない。

本質的には、

「Authentication ObjectにValidityを設定できること」

および、

「Authentication ObjectのValidityとEntitlementのValidityを
独立したLifecycleとして管理できること」

を重視している。

したがって、弁理士には30秒という数値を独立した発明上の限定として
どのように位置付けるべきかを確認していただきたい。

---

# 7. 特許性上の懸念事項

これまでのレビューでは、Version 2.0について以下の点が
特許性上の主要な検討事項となることが確認されている。

## 7.1 Authentication自体

Authentication、Credential、QR Code、短時間Validity等は
既存技術が多数存在すると考えられる。

したがって、Authenticationそのものを発明の主要な差異とすることは
慎重な検討が必要である。

---

## 7.2 Authorization / Access Control

Authorization、Policy、Access Control、Policy-based Access Control等も
既存技術として広く存在する。

したがって、これらの一般的概念だけではVersion 2.0の
特許性を十分に説明できない可能性がある。

---

## 7.3 Entitlement

Entitlementという概念自体にも既存技術が存在する可能性がある。

したがって、

「Entitlementを使用する」

だけではなく、

Authentication ResultとEntitlementを分離し、
さらにAuthentication ObjectのValidityとEntitlementのValidityを
独立管理し、それをPolicy EvaluationおよびAuthorization Decisionに
利用するという構造全体が重要である。

---

## 7.4 Validity / Lifecycle

Validity、Expiration、Revocation、Revalidation等も
それぞれ単独では既知の技術要素である可能性が高い。

したがって、単なる「有効期限」の存在を発明の核心とせず、
Authentication ObjectとEntitlementについて異なるLifecycleを
管理できることを重視する。

---

## 7.5 QR Code

QR Codeによる認証自体は一般的な技術要素である可能性が高い。

そのため、QR Codeを独立した発明の中心としない。

---

# 8. 先行技術との境界

Version 2.0では、以下のような既存技術との境界が重要となる。

- Authentication systems
- FIDO / Passkey系技術
- QR-based authentication
- Access Control
- Policy-based authorization
- OAuth / Device Authorization
- Entitlement / Rights Management
- Session / Token expiration
- Revocation systems
- Distributed authorization

Version 2.0では、これらの単一技術を発明の中心とするのではなく、
複数の処理段階およびObject Lifecycleを組み合わせた
サービス利用制御構造を中心としている。

---

# 9. Claim 1についての重要な検討事項

Claim 1はVersion 2.0の最重要Claimである。

特に以下の関係が維持されていることが重要である。

Authentication
→ Authentication Result
→ Entitlement
→ Policy Evaluation
→ Authorization Decision
→ Enforcement
→ Service Execution

さらに、

Authentication Result
≠
Entitlement

および、

Authentication Object Validity
≠
Entitlement Validity

という関係が重要である。

弁理士には、これらの構成関係が先行技術に対して
どの程度の新規性・進歩性を有するかを重点的に検討していただきたい。

---

# 10. Claim 31およびClaim 36

Claim 31は方法として、
Claim 36はプログラムとして、
Claim 1と同一の発明概念を異なるカテゴリーで保護する構成としている。

この3つについて、日本出願上の請求項カテゴリー、
従属関係および記載形式について弁理士による確認を希望する。

---

# 11. 従属Claimの役割

現在の従属Claimには、以下のようなFallback構成が含まれている。

- Authentication Object Validity
- Expiration
- Authentication ObjectとEntitlementの異なるValidity Period
- QR Code
- Web表示QR Code
- 短時間Validity
- One-time Use
- Conditional Entitlement
- Cross-Service Entitlement
- Policy
- Revocation
- Revalidation
- その他のAuthorization / Enforcement関連構成

これらは発明の核心を補強するとともに、
先行技術との関係に応じたFallbackを提供することを目的としている。

現時点では、特許性検討の結果だけを理由として
これらを削除・変更していない。

---

# 12. 現時点で変更を避けている事項

Version 2.0では、技術仕様全体との整合性を維持することを
重要な設計原則としている。

そのため、現時点では以下を無断で変更しない。

- 発明核心
- Claim 1
- Claim 31
- Claim 36
- Authentication Result / Entitlementの分離
- Authentication Object / Entitlement Validityの分離
- Policy Evaluation
- Authorization Decision
- Enforcement
- Service Execution
- Object Model
- Canonical Vocabulary
- Design Freeze

変更が必要な場合は、技術的影響を確認した上で
最小限の変更とする。

---

# 13. 明細書・Protocol・DesignによるSupport

これまでのレビューにより、発明の主要な構成について
複数の技術文書によるSupportが存在することを確認している。

特に、

- patent/specification_v2.md
- patent/invention_core_v2.md
- protocol/object_model.md
- protocol/normative_requirements.md
- design/canonical_object_graph.md
- design/canonical_vocabulary.md

において、

Authentication ResultとEntitlementの分離、
Authentication ObjectのValidity、
EntitlementのLifecycle、
Policy Evaluation、
Authorization Decision、
Enforcement、
Service Execution等の技術関係が説明されている。

ただし、最終的な日本特許法上のサポート要件については
弁理士による確認を必要とする。

---

# 14. Version 1.0との関係

Version 1.0はVersion 2.0の前段階の技術仕様として存在する。

Version 2.0では、Authentication中心の構成から、
Authentication後のEntitlement、Policy Evaluation、
Authorization、Enforcement、Service Executionを含む
多段階のサービス利用制御モデルへ拡張している。

日本出願に際しては、Version 1.0の公開内容との関係について、

- 既に公開されている構成
- Version 2.0で新たに整理・追加された構成
- 両者を組み合わせた構成

を弁理士に確認していただきたい。

特に、Version 1.0の公開事実がVersion 2.0の新規性・進歩性に
どのような影響を与えるかは、重要な法的検討事項である。

---

# 15. 日本出願において弁理士に判断していただきたい事項

以下を重点的な相談事項とする。

## 15.1 Claim 1

Authentication ResultとEntitlementの分離、および
Authentication Object ValidityとEntitlement Validityの独立管理を
どの程度強く独立Claimに残すべきか。

## 15.2 Claim Scope

Claim 1が広すぎる場合のFallback構成と、
先行技術に対する十分な差異とのバランス。

## 15.3 Entitlement

Entitlementの「取得、生成、確認」等の表現を
日本特許法上どのように整理するのが適切か。

## 15.4 Validity

Authentication Object ValidityとEntitlement Validityの
独立性をどの程度Claimで明示するべきか。

## 15.5 QR / 30秒

QR Codeおよび30秒Validityを、
独立Claimではなく従属Claimまたは実施例として扱うことの妥当性。

## 15.6 Policy Evaluation

Policy Evaluationを単なる一般的Access Controlとして
評価されないよう、どの技術的関係を明細書・Claimで明確化すべきか。

## 15.7 Version 1.0

Version 1.0の公開内容がVersion 2.0の新規性・進歩性に与える影響。

## 15.8 Prior Art

FIDO、Passkey、QR Authentication、OAuth Device Flow、
Access Control、Entitlement/Rights Management等との
具体的な差異。

## 15.9 Claim 31 / 36

System / Method / Programの3カテゴリーを維持することの
日本出願上の妥当性。

## 15.10 Support

現行のSpecification / Protocol / Design / Figuresによる
Claim Supportが日本特許法上十分であるか。

---

# 16. 出願方針

現時点では、Version 2.0を大幅に再設計するのではなく、

1. 現行Version 2.0を出願候補原稿として固定する。
2. 特許性上の懸念を本書によって弁理士と共有する。
3. 弁理士による先行技術および日本特許法上の評価を受ける。
4. 必要な場合のみClaimおよび明細書を最小限修正する。
5. 技術仕様全体との整合性を維持したまま日本出願を行う。

という方針を推奨する。

---

# 17. 現時点での総合評価

Version 2.0には、単なるAuthentication技術、
単なるQR Code認証、単なるAuthorizationまたは
単なるPolicy-based Access Controlとは異なる発明概念として、

Authentication ResultとEntitlementを分離し、
それぞれのLifecycleを異なるものとして管理し、
その結果をPolicy EvaluationおよびAuthorization Decisionに利用し、
Enforcementを介してService Executionを制御するという
技術的構造が存在する。

一方、構成要素の一部には既存技術が多数存在すると考えられるため、
特許性は個々の要素ではなく、
これらの構成要素の組合せおよび技術的関係について
慎重に評価する必要がある。

特にClaim 1については、
「Authentication + Authorization」という一般的な構成を超えて、

- Authentication ResultとEntitlementの分離
- Authentication Object ValidityとEntitlement Validityの独立性
- EntitlementをPolicy Evaluationへ入力する構造
- Authorization Decision
- Enforcement
- Service Execution

という一連の関係をどのように評価するかが重要となる。

したがって、現時点では発明全体を放棄または大幅に再設計する段階ではなく、
現在の技術仕様を基礎として弁理士による日本出願向けの
特許性評価を受けることが適切と考える。

---

# 18. 技術側からの最終メッセージ

本資料は、発明者側がVersion 2.0について実施した
技術的検討の結果を整理したものである。

本発明の最重要点は、
Authenticationそのものではなく、

「Authenticationによって得られる結果」
と
「サービスを利用する権利」

を分離し、その権利を独立したLifecycleとして管理し、
Policy Evaluation、Authorization Decision、Enforcementを通じて
Service Executionを制御する点にある。

この技術的関係を維持した上で、
日本特許出願として最も適切なClaim Scopeおよび
明細書構成について弁理士の判断を求める。

