# Version 2.0 → 日本特許出願書類 変換マッピング

## 1. 基本方針

本表は、NEW-shot2play Technical Specification Version 2.0の
既存技術文書を、日本特許出願用書類へ変換する際の対応関係を示す。

Version 2.0のTechnical Baselineそのものは変更しない。

---

## 2. 出願書類とSource Document

| 出願資料 | 主Source | 補助Source |
|---|---|---|
| 明細書 | patent/specification_v2.md | patent/invention_core_v2.md |
| 特許請求の範囲 | patent/claims_v2.md | patent/claim_structure_v2.md |
| 従属Claim検討 | patent/dependent_claim_strategy_v2.md | patent/claims_v2.md |
| 要約書 | specification_v2.md / invention_core_v2.md | claims_v2.md |
| 図面説明 | figures/ | design/canonical_object_graph.md |
| 実施形態 | specification_v2.md | invention_core_v2.md |
| Object構造 | design/canonical_object_graph.md | protocol/object_model.md |
| Normative処理 | protocol/normative_requirements.md | protocol/object_model.md |

---

## 3. 明細書への変換

### 技術分野

Version 2.0のAuthentication、
Entitlement、
Policy Evaluation、
Authorization、
Enforcement、
Service Executionを対象とする情報処理技術として整理する。

### 背景技術

既存のAuthentication、
Authorization、
Access Control、
Policy-based Control等との関係を整理する。

ここでは既知技術を本発明そのものとして記載しない。

### 課題

Authenticationの成立とService利用権の成立を
単一の状態として扱うことによる制約を整理する。

特に、

Authentication ObjectのValidity

と

Service利用権であるEntitlementのValidity

を異なるLifecycleとして管理する必要性を記載する。

### 課題を解決するための手段

以下の処理関係を中心に記載する。

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

### 発明の効果

Authentication、
Entitlement、
Authorization、
Service ExecutionのLifecycleを分離して管理できること、
およびPolicyに基づくService利用制御を柔軟に構成できることを中心に整理する。

---

## 4. 発明の核心

以下の2つの関係を明確に維持する。

### 4.1 Result Separation

Authentication Result
≠
Entitlement

### 4.2 Validity Separation

Authentication Object Validity
≠
Entitlement Validity

これらをVersion 2.0の中心的な技術関係として扱う。

---

## 5. QR Code

QR CodeはAuthentication Objectの具体的実施形態として扱う。

QR Codeそのものを発明の中心としない。

---

## 6. 30秒

30秒はValidity Periodの具体例として扱う。

30秒そのものを発明の核心としない。

一般化されたValidity Periodを上位概念として維持する。

---

## 7. Claimとの対応

Claim 1を中心としてSpecificationを構成する。

特にClaim 1の以下の要素を明細書でSupportする。

1. Authentication
2. Authentication Result
3. Entitlement
4. Authentication ResultとEntitlementの分離
5. Authentication Object Validity
6. Entitlement Validity
7. Policy Evaluation
8. Authorization Decision
9. Enforcement
10. Service Execution

---

## 8. 従属Claim

従属Claimに対応する実施形態および技術的説明を
Specification側に確保する。

対象には以下を含む。

- Validity Period
- Expiration
- 異なるValidity Period
- QR Code
- Web表示QR Code
- One-time Use
- Conditional Entitlement
- Cross-Service Entitlement
- Revocation
- Revalidation
- Fail-Closed
- その他のAuthorization / Enforcement構成

---

## 9. 図面

Canonical Figuresを基礎として、
日本出願用図面との対応を確認する。

特に以下を重視する。

1. Overall Protocol Architecture
2. Authentication / Entitlement Relationship
3. Entitlement Object / Lifecycle
4. Context / Policy Evaluation
5. Authorization Decision / Capability
6. Authorization State / Revocation
7. Distributed Authorization Consistency

---

## 10. Version 1.0

Version 1.0はVersion 2.0の前段階の仕様として扱う。

Version 1.0で公開済みの構成とVersion 2.0で追加・整理された構成を
区別し、弁理士による新規性・進歩性評価の対象とする。

---

## 11. 変更管理

本マッピングは出願原稿作成の設計資料である。

本表だけを理由としてVersion 2.0 Technical Baselineを変更しない。

変更が必要となった場合は、

1. 変更理由
2. Claimへの影響
3. Specificationへの影響
4. Figureへの影響
5. Object Modelへの影響
6. Vocabularyへの影響

を確認してから実施する。
