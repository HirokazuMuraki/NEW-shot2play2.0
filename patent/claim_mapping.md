# NEW-shot2play Technical Specification Version 2.0
# Claim Mapping / Specification Support Matrix

## 1. Purpose

本書は、Version 2.0 の特許請求の範囲について、各請求項の構成要件と
`patent/specification_v2.md` の記載との対応関係を整理する内部レビュー資料である。

本書は出願書類本文そのものではなく、弁理士による請求項・明細書の整合性確認、
補正検討およびサポート要件確認を容易にすることを目的とする。

**Primary sources**
- `patent/claims_v2.md`
- `patent/specification_v2.md`
- `figures/` 配下の図面
- `patent/invention_core_v2.md`

---

## 2. Core Claim Architecture

本発明の中心となる処理構造は、概念的に以下である。

Authentication
→ Authentication Result
→ Entitlement
→ Policy Evaluation
→ Authorization Evaluation
→ Authorization Decision
→ Enforcement
→ Service Execution

重要な中心概念は以下である。

1. Authentication Result と Entitlement の分離
2. Authentication Object Validity と Entitlement Validity の独立管理
3. Policy Evaluation に複数のContextを入力できる構成
4. Authorization Decision と Enforcement の分離
5. Enforcement を介した Service Execution の制御
6. Conditional Entitlement
7. Cross-Service Entitlement
8. Security Context / Transaction / Object State の利用

---

## 3. Claim 1 — Independent System Claim

| Claim element | Specification support |
|---|---|
| Authentication | §5, §6, §7 |
| Authentication Result | §5, §6, §7 |
| Authentication Result と独立して管理され得る Entitlement | §5, §14, §15 |
| Policy Evaluation | §5, §18, §19 |
| Authorization Decision | §5, §19, §20, §21 |
| Enforcement | §5, §25 |
| Service Execution | §5, §26 |
| Authentication Result と Entitlement の別個の取扱い | §15 |
| Authentication Object Validity と Entitlement Validity の独立管理 | §9, §10, §11 |
| 全体処理連鎖 | §6, §33 |

### Claim 1 support assessment

Claim 1 の主要構成は Specification の §5「課題を解決するための手段」、
§6「発明の基本構成」および各機能別節に直接対応している。

特に発明の核心である
`Authentication Result ≠ Entitlement`
および
`Authentication Object Validity` と `Entitlement Validity` の独立性は、
§11、§14、§15で明示されている。

---

## 4. Claims 2–5 — Authentication Object Validity / Expiration

| Claim | Main limitation | Specification support |
|---|---|---|
| 2 | Authentication Object の Validity Period | §8, §9 |
| 3 | Authentication Object の Expiration | §10 |
| 4 | Authentication Object Validity と Entitlement Validity の期間差 | §11 |
| 5 | Authentication Object Validity Period が Entitlement より短い | §11 |

Claim 2～5は、Claim 1 の中心概念である「認証に使用する一時的Object」と
「Authenticationによって成立・確認されたEntitlement」の異なるLifecycleを
具体化するFallback群である。

---

## 5. Claims 6–10 — QR Code / Time-limited Authentication Object

| Claim | Main limitation | Specification support |
|---|---|---|
| 6 | Authentication Object が QR Code | §8, §9, §35 |
| 7 | QR Code が Web画面上に表示 | §9, §35 |
| 8 | QR Code の所定 Validity Period | §9, §35 |
| 9 | QR Code 生成時点から所定時間 | §9, §35 |
| 10 | Validity Period = 30秒 | §9, §34, §35 |

30秒は独立発明概念ではなく、短時間Authentication Objectの具体例として
従属請求項に配置されている。

---

## 6. Claims 11–17 — Replay / Entitlement Lifecycle

| Claim | Main limitation | Specification support |
|---|---|---|
| 11 | One-time Use | §8, §13 |
| 12 | 条件成立時の Entitlement 生成・有効化 | §16 |
| 13 | Authentication / location / transaction / service / time / Object State 等の条件 | §16, §29 |
| 14 | 異なるServiceでのEntitlement利用 | §17 |
| 15 | Authentication完了後も独立して保存・更新・停止・失効等 | §14, §15, §30 |
| 16 | Entitlement Validity Period | §14, §30, §31 |
| 17 | Validity Period終了とは独立したRevocation | §30 |

Claims 12～17は、EntitlementをAuthentication Resultの単なる別名ではなく、
独立したLifecycleを持つ情報Objectとして扱う構成を段階的に具体化する。

---

## 7. Claims 18–23 — Policy / Authorization / Enforcement

| Claim | Main limitation | Specification support |
|---|---|---|
| 18 | Policy Evaluation inputs | §18, §19, §27–§29 |
| 19 | Permit / Deny / Indeterminate | §21–§24 |
| 20 | Indeterminate時のRecovery | §24, §31, §32 |
| 21 | Authorization Decision → Enforcement → Service Execution | §21, §25, §26 |
| 22 | Service Execution の具体例 | §26 |
| 23 | Authentication Freshness | §12, §31 |

Claim 18はPolicy Evaluationを単一のAuthentication結果だけに依存させず、
Entitlement、Subject、Resource、Action、Security Context、Transaction、
Object State等を入力として利用できる構成を具体化する。

Claims 19～21は、Policy Evaluation / Authorization Decision / Enforcement /
Service Execution を混同せず、異なる処理境界として扱う構成を具体化する。

---

## 8. Claims 24–27 — Object State / Security Context / Transaction

| Claim | Main limitation | Specification support |
|---|---|---|
| 24 | Object State | §29 |
| 25 | Object State と Authorization Decision の独立性 | §29 |
| 26 | Security Context の構成要素 | §27 |
| 27 | Transaction による処理関連付け | §28 |

Object StateはAuthorization Decisionそのものではなく、Policy Evaluation /
Authorization Evaluationに利用され得る独立した状態情報として説明されている。

Security ContextおよびTransactionは、複数の条件・処理・Objectを横断して
Authorizationを評価するためのContextとして位置付けられる。

---

## 9. Claims 28–30 — Public-key / WebAuthn / Passkey

| Claim | Main limitation | Specification support |
|---|---|---|
| 28 | 公開鍵暗号方式 | §7, §36 |
| 29 | WebAuthn / Passkey | §7, §36 |
| 30 | Secure Enclave / TEE 等への秘密鍵保持 | §36 |

これらはAuthentication mechanismをClaim 1に限定せず、具体的実装方式を
Fallbackとして保持する従属請求項群である。

---

## 10. Claims 31–35 — Information Processing Method

| Claim | Main limitation | Specification support |
|---|---|---|
| 31 | Claim 1相当の方法構成 | §5, §6, §33 |
| 32 | Authentication Object Validity Period | §9, §33 |
| 33 | Authentication / Entitlement Validity の独立管理 | §11, §33 |
| 34 | Authentication結果に基づくEntitlement生成＋Cross-Service利用 | §16, §17, §34 |
| 35 | Entitlement Validity / Revocation とAuthorizationへの反映 | §30, §31, §33 |

---

## 11. Claims 36–37 — Program / Recording Medium

| Claim | Main limitation | Specification support |
|---|---|---|
| 36 | Claim 1相当の処理をコンピュータに実行させるProgram | §5, §6, §33, §37 |
| 37 | Claim 36のProgramを記録したComputer-readable medium | §1, §43, §37 |

---

## 12. Figure Support

Specification §40では、基本構造について以下の図面が正式に説明されている。

- 図1：全体Protocol Architecture
- 図2：Authentication / Entitlement / Policy Evaluation / Authorization
- 図3：Entitlement Object / Lifecycle
- 図4：Security Context / Policy Evaluation / Authorization Decision
- 図5：Authorization Decision / Service Execution
- 図6：Invalidation / Revocation / Fail-Closed
- 図7：Distributed Authorization / Consistency

これらは特にClaims 1, 12–27および31–35の概念的支持に対応する。

`figures/` には追加の詳細図面も存在するが、本書では図1～図7を
Specification上のcanonical figure referenceとして扱う。

---

## 13. Overall Assessment

現Version 2.0では、37請求項について、主要な構成要件をSpecificationの
各節に対応付けることができる。

特にClaim 1の中核である、

- Authentication Result と Entitlement の分離
- Authentication Object Validity と Entitlement Validity の独立性
- Policy Evaluation
- Authorization Decision
- Enforcement
- Service Execution

については、Specification §5、§6、§11、§14–§26に直接的な記載がある。

本書は弁理士レビュー時の「Claim → Specification support」の確認資料として使用する。
