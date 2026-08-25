# NEW-shot2play Technical Specification Version 2.0
# 図面Support最終確認表
# Attorney Review Draft

**対象:** 日本特許出願  
**目的:** 請求項・明細書・図面の技術的対応関係の最終確認  
**基礎仕様:** NEW-shot2play Technical Specification Version 2.0

---

# 1. 本書の目的

本書は、NEW-shot2play Technical Specification Version 2.0について、
請求項、明細書および図面の対応関係を整理し、
日本特許出願前に図面による技術的Supportが確保されているかを
確認することを目的とする。

本書は、既存のVersion 2.0の技術内容または図面構成を変更するものではない。

正式な出願図面の形式、符号、図面表現その他の法的・手続的事項については、
弁理士による最終確認および修正を前提とする。

---

# 2. 図面Supportの基本方針

Version 2.0では、図面によって個々の処理だけでなく、
AuthenticationからService Executionまでの処理関係および
各Objectの論理的な関係を説明する。

特に、本発明の重要な技術的関係については、
単一の処理要素ではなく、複数の処理段階およびObject間の関係として
図面によるSupportを確認する。

重要な構成は以下である。

- Authentication
- Authentication Object
- Authentication Result
- Entitlement
- Authentication Object Validity
- Entitlement Validity
- Policy Evaluation
- Authorization Evaluation
- Authorization Decision
- Enforcement
- Service Execution
- Revocation
- Revalidation
- Distributed Authorization Consistency

---

# 3. Canonical Figures

Version 2.0では、以下の図面をCanonical Figureとして扱う。

| Figure | 内容 | 主なSupport対象 |
|---|---|---|
| Figure 01 | Overall Protocol Architecture | 全体処理構造 |
| Figure 13 | Authentication and Entitlement Relationship | Authentication / Entitlement |
| Figure 15 | Entitlement Object / Lifecycle | Entitlement / Lifecycle |
| Figure 24 | Context and Policy Evaluation | Context / Policy Evaluation |
| Figure 25 | Authorization Decision and Capability | Authorization / Decision / Enforcement |
| Figure 35 | Authorization State / Revocation | State / Revocation / Revalidation |
| Figure 30 | Distributed Authorization Consistency | 分散環境 / Consistency |

---

# 4. Claim 1 Core Element Support Matrix

## 4.1 Authentication Processing

| 項目 | 明細書Support | 図面Support | 評価 |
|---|---|---|---|
| Authentication処理 | §7, §33 | Figure 01, Figure 13 | Strong |
| Authentication Object | §8, §9, §10 | Figure 01, Figure 13 | Strong |
| Authentication Object Validity | §9–12 | Figure 13, Figure 15 | Critical |
| Authentication Object Expiration | §10 | Figure 15 | Strong |
| Authentication Freshness | §12 | Figure 13, Figure 15 | Strong |
| Replay防止 | §13 | Figure 13, Figure 15 | Strong |

---

# 5. Authentication Result / Entitlement Separation

## 5.1 Core Separation

| 項目 | 明細書Support | 図面Support | 評価 |
|---|---|---|---|
| Authentication Result | §5, §7, §15 | Figure 13 | Strong |
| Entitlement | §14–17 | Figure 13, Figure 15 | Strong |
| Authentication ResultとEntitlementの分離 | §15 | Figure 13, Figure 15 | Critical |
| Entitlementの独立Lifecycle | §15, §29, §30 | Figure 15, Figure 35 | Critical |
| 複数Entitlement | §16, §17, §41, §42 | Figure 15 | Strong |
| Conditional Entitlement | §16 | Figure 15 | Strong |
| Cross-Service Entitlement | §17 | Figure 13, Figure 15 | Strong |

---

# 6. Validity Separation

## 6.1 Authentication Object Validity / Entitlement Validity

本発明における重要な構成の一つは、
Authentication Objectの時間的Validityと、
Entitlementの時間的Validityとを同一視せず、
それぞれ独立して管理可能とすることである。

| 項目 | 明細書Support | 図面Support | 評価 |
|---|---|---|---|
| Authentication Object Validity | §9 | Figure 13, Figure 15 | Critical |
| Authentication Object Expiration | §10 | Figure 15 | Strong |
| Entitlement Validity | §11, §14 | Figure 15, Figure 35 | Critical |
| 両者の独立性 | §11 | Figure 13, Figure 15 | Critical |
| 短時間Authentication Object | §9, §11, §35 | Figure 13, Figure 15 | Strong |
| 長時間Entitlement | §11, §34, §38 | Figure 15, Figure 35 | Strong |
| Authentication ObjectのValidity延長とは独立したEntitlement管理 | §11 | Figure 15 | Strong |

### 重要事項

Authentication ObjectのValidityについて、
「30秒」は発明の必須条件として扱わない。

30秒は実施形態の一例であり、
本発明上重要なのは、

「Authentication Objectが所定のValidityを有し得ること」

および

「Authentication ObjectのValidityとEntitlementのValidityとを
独立した条件として管理可能であること」

である。

したがって、図面Supportについても30秒という特定値ではなく、
Validityの独立したLifecycleをSupportすることを確認する。

---

# 7. Policy Evaluation Support

| 項目 | 明細書Support | 図面Support | 評価 |
|---|---|---|---|
| Policy | §18 | Figure 24 | Strong |
| Policy Evaluation | §19 | Figure 24 | Critical |
| Authentication Resultを入力として利用 | §19 | Figure 24 | Strong |
| Entitlementを入力として利用 | §19 | Figure 24 | Critical |
| Security Contextを利用 | §27 | Figure 24 | Strong |
| Transactionを利用 | §28 | Figure 24 | Strong |
| Policy Evaluation結果 | §19 | Figure 24 | Strong |

---

# 8. Authorization Support

| 項目 | 明細書Support | 図面Support | 評価 |
|---|---|---|---|
| Authorization Evaluation | §20 | Figure 24, Figure 25 | Strong |
| Authorization Decision | §21 | Figure 25 | Critical |
| Permit | §22 | Figure 25 | Strong |
| Deny | §23 | Figure 25 | Strong |
| Indeterminate | §24 | Figure 25 | Strong |
| DecisionとPolicyの分離 | §21 | Figure 24, Figure 25 | Strong |
| DecisionとService Executionの分離 | §21, §25, §26 | Figure 25 | Critical |

---

# 9. Enforcement / Service Execution Support

| 項目 | 明細書Support | 図面Support | 評価 |
|---|---|---|---|
| Enforcement | §25 | Figure 25 | Critical |
| Authorization DecisionからEnforcementへの適用 | §25 | Figure 25 | Critical |
| Service Execution | §26 | Figure 25 | Critical |
| EnforcementによるService Execution制御 | §25, §26 | Figure 25 | Critical |
| Permit後の追加条件確認 | §22, §25 | Figure 25 | Strong |
| Service Executionの拒否・停止 | §23, §25 | Figure 25 | Strong |
| DecisionとExecutionの分離 | §21, §25, §26 | Figure 25 | Critical |

---

# 10. Security Context / Transaction Support

| 項目 | 明細書Support | 図面Support | 評価 |
|---|---|---|---|
| Security Context | §27 | Figure 24 | Strong |
| Transaction | §28 | Figure 24, Figure 25 | Strong |
| Object State | §29 | Figure 15, Figure 35 | Strong |
| State Transition | §29, §30, §31 | Figure 35 | Strong |

---

# 11. Revocation / Revalidation Support

| 項目 | 明細書Support | 図面Support | 評価 |
|---|---|---|---|
| Revocation | §30 | Figure 35 | Critical |
| Revalidation | §31 | Figure 35 | Strong |
| Fail-Closed | §32 | Figure 35 | Strong |
| Entitlement State | §29, §30 | Figure 15, Figure 35 | Strong |
| Authorization State | §29, §30 | Figure 35 | Strong |
| Revocation後のDecision制御 | §30–32 | Figure 35 | Strong |

---

# 12. Distributed System Support

| 項目 | 明細書Support | 図面Support | 評価 |
|---|---|---|---|
| Distributed System | §37 | Figure 30 | Critical |
| 複数Service / Server | §37 | Figure 30 | Strong |
| Authorization情報のConsistency | §37 | Figure 30 | Critical |
| Entitlementの分散利用 | §17, §37 | Figure 15, Figure 30 | Strong |
| Revocationの分散反映 | §30, §37 | Figure 35, Figure 30 | Strong |

---

# 13. Claim 1 Overall Support

Claim 1の主要な処理関係について、
以下の一連の構造が明細書および図面によって説明されている。

AuthenticationからAuthentication Resultが得られ、
Authentication Resultとは独立して管理され得るEntitlementが存在し、
Authentication Result、Entitlementその他のContextをPolicy Evaluationに
入力し、その評価結果に基づいてAuthorization Decisionを生成する。

さらに、Authorization DecisionをEnforcementに適用し、
Enforcementを介してService Executionを制御する。

この一連の関係が本発明の中心的な技術構造である。

---

# 14. Claim 1における特に重要な図面Support

## A. Authentication Result / Entitlement Separation

**Support:** Figure 13, Figure 15

Authenticationの結果として得られるAuthentication Resultと、
Service利用に関するEntitlementを同一Objectとして扱うのではなく、
独立した論理Objectとして管理する構成を示す。

**評価:** Critical

---

## B. Independent Validity

**Support:** Figure 13, Figure 15

Authentication ObjectのValidityとEntitlementのValidityを
異なるLifecycleとして管理可能とする。

**評価:** Critical

---

## C. Policy Evaluation

**Support:** Figure 24

Authentication Result、Entitlementおよびその他のContextを
Policy Evaluationへ入力する関係を示す。

**評価:** Critical

---

## D. Authorization Decision / Enforcement / Service Execution

**Support:** Figure 25

Policy Evaluation等に基づいてAuthorization Decisionを生成し、
そのDecisionをEnforcementに適用し、
Enforcementを介してService Executionを制御する関係を示す。

**評価:** Critical

---

## E. Revocation / Revalidation

**Support:** Figure 35

Authorizationおよび関連ObjectのState、
RevocationおよびRevalidationを含むLifecycleを示す。

**評価:** Strong

---

## F. Distributed Authorization Consistency

**Support:** Figure 30

複数のServiceまたはServerにおいて、
Authorization関連情報および状態を扱う構成を示す。

**評価:** Strong

---

# 15. Claim 2以降のSupport

Claim 2以降の従属Claimについても、
各Claimの追加限定が明細書および図面の少なくとも一方によって
説明されることを確認する。

特に以下の事項については、
明細書中の具体的実施形態およびCanonical Figuresとの対応を維持する。

- Authentication Object Validity
- Expiration
- Freshness
- Replay Prevention
- Entitlement Validity
- Conditional Entitlement
- Cross-Service Entitlement
- Security Context
- Transaction
- Revocation
- Revalidation
- Fail-Closed
- Distributed System
- QR Code
- 公開鍵暗号
- Service Execution

従属Claimについて、図面に直接表現されていない事項が存在する場合でも、
明細書によって十分に説明されている事項については、
必ずしも新たな図面を追加することを意味しない。

---

# 16. 図面追加の必要性に関する暫定判断

現時点では、Version 2.0のCanonical Figuresによって
Claim 1の主要構成および本発明の中心的な技術的関係は
説明可能と考えられる。

ただし、本書は図面そのものの法的評価を行うものではない。

正式な日本特許出願用図面として、
符号、配置、接続関係、図面間の整合性その他については、
弁理士による最終確認を行う。

特に、以下の構成については重点的に確認する。

- Authentication ResultとEntitlementの分離
- Authentication Object ValidityとEntitlement Validityの独立性
- Policy Evaluationへの入力関係
- Authorization Decision
- Enforcement
- Service Execution
- Revocation / Revalidation
- Distributed Authorization

---

# 17. Version 2.0 Design Freezeとの関係

本書は、Version 2.0のDesign Freezeを変更しない。

図面Support確認の結果として修正が必要と判断された場合も、
以下の優先順位で検討する。

1. 既存図面でSupport可能か確認する。
2. 明細書の記載でSupport可能か確認する。
3. 出願上重要な不足であるか確認する。
4. それでも必要な場合のみ図面修正を検討する。

単なる表現改善または見栄えのための図面変更は行わない。

---

# 18. 総合評価

現時点のVersion 2.0について、
Claim 1の主要な構成は、明細書およびCanonical Figuresにより
技術的にSupportされていると考えられる。

特に重要な構成である、

- Authentication ResultとEntitlementの分離
- Authentication Object ValidityとEntitlement Validityの独立性
- Entitlementを含むContextのPolicy Evaluationへの入力
- Authorization Decision
- Enforcement
- Service Execution

について、対応する明細書記載および図面が存在する。

したがって、現時点ではVersion 2.0の技術構成を変更する必要性は
認められない。

正式な日本特許出願に際しては、弁理士が本Support状況を確認し、
必要に応じて図面の符号、表現、配置その他を調整する。

---

# 19. 弁理士への確認事項

弁理士には特に以下の点について確認を求める。

1. Claim 1の各構成について図面Supportが十分であるか。
2. Authentication ResultとEntitlementの分離が図面上明確であるか。
3. Authentication Object ValidityとEntitlement Validityの独立性が
   図面上十分に表現されているか。
4. Policy Evaluation、Authorization Decision、Enforcementおよび
   Service Executionの処理境界が明確であるか。
5. Revocation / Revalidationについて追加図面が必要か。
6. Distributed Authorizationについて追加図面が必要か。
7. 日本特許出願用図面として、符号その他の形式的修正が必要か。

---

# 20. 結論

現行Version 2.0について、図面Supportの観点から
現時点で重大な欠落は確認されていない。

本発明の中心的な技術的関係は、既存のCanonical Figuresによって
説明可能と考えられる。

したがって、本段階では発明内容またはClaim Architectureを変更せず、
弁理士による最終的な図面形式および法的表現の確認へ進むことが
適切である。

---

**End of Document**
