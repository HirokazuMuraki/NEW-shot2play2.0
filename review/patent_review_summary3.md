# NEW-shot2play Version 2.0 特許出願書類 総合レビュー(第3版)
# patent_review_summary3.md

**作成日:** 2026年9月9日
**対象:** `patent_review_summary2.md`の指摘事項に対する修正反映状況の確認
**参照元:** `https://shot2play.com/s2p-patent2.0/`(GitHubリポジトリと同一内容をVPS上に展開したもの)

---

## 目次

1. [レビュー範囲と前提](#1-レビュー範囲と前提)
2. [総合評価サマリー](#2-総合評価サマリー)
3. [解消済みの指摘事項](#3-解消済みの指摘事項)
4. [重大度:高 — 未解消・出願前に必ず解消すべき点](#4-重大度高--未解消出願前に必ず解消すべき点)
5. [重大度:中 — 未解消・弁理士確認を推奨する点](#5-重大度中--未解消弁理士確認を推奨する点)
6. [重大度:低 — 未解消・記載の質を高める改善点](#6-重大度低--未解消記載の質を高める改善点)
7. [今回未確認の項目](#7-今回未確認の項目)
8. [ファイルごとの状態一覧](#8-ファイルごとの状態一覧)
9. [推奨アクションリスト(優先順位付き)](#9-推奨アクションリストの優先順位付き)
10. [留意事項](#10-留意事項)

---

## 1. レビュー範囲と前提

`patent_review_summary2.md`の指摘事項を踏まえて修正されたとのことでしたので、更新日時をもとに実際に修正されたファイルを特定し、内容を確認しました。

**今回、更新が確認できたファイル**

| ファイル | 更新日時 |
|---|---|
| `patent/claims_v2.md` | 2026-09-07 |
| `patent/specification_v2.md` | 2026-09-08 |
| `patent/dependent_claim_strategy_v2.md` | 2026-09-08 |
| `patent/prior_art_analysis.md` | 2026-09-07 |
| `attorney_package/02_application_documents/claims_v2_attorney_review.md` | 内容確認済み(§16の記載が修正されていることを確認) |
| `attorney_package/02_application_documents/specification_v2_attorney_review.md` | 内容確認済み |

**今回、更新が確認できなかった(前回と同じ内容と判断した)ファイル**

- `patent/claim_mapping.md`
- `patent/invention_summary.md`
- `attorney_package/01_patentability_review/`
- `attorney_package/03_drawings/`
- `attorney_package/06_prior_art/`
- `attorney_package/07_version1_reference/`

このレビューでは、`patent_review_summary2.md`の指摘事項ごとに、①解消済み、②未解消、の判定を行い、未解消の項目については重大度別に再掲します。修正不要と判断された項目についても、その旨を明記した上でそのまま再掲しています。

---

## 2. 総合評価サマリー

今回の修正で、特に以下の3点について明確な改善が確認できました。

1. **Authorization Decisionの用語不一致が解消**:クレーム・明細書の両方に、Permit/Deny/Indeterminateと、技術仕様書側のDecision State(Allow/Deny/Invalid/Revoked/Expired等)との関係を明記する説明が追加されました。
2. **先行技術文献が具体化**:`prior_art_analysis.md`に、米国特許出願番号(US20080184336A1等)、RFC 8628、W3C WebAuthn仕様等、具体的な文献番号と公開日が追加されました。
3. **独立請求項の数え方の矛盾が解消**:`claims_v2_attorney_review.md`で、独立請求項がClaim 1・31・36の3本であり、Claim 37はClaim 36に従属する記録媒体であることが明確に整理されました。

一方で、**Fallback戦略とClaim 1本文との重複**(Validity分離)、および**実施形態が単一シナリオに留まっている点**については、今回の修正では対応されていないようです。以下、重大度別に詳細を記載します。

---

## 3. 解消済みの指摘事項

### 3.1 Authorization Decisionの結果表現の用語不一致(旧・重大度高)

**修正内容:** `claims_v2.md`の請求項19、および`specification_v2.md`の第21節に、以下の説明が追加されました。

> 「Permit、DenyおよびIndeterminateはAuthorization Decisionの結果を示す概念であり、Pending、Allow、Deny、Invalid、Revoked、Expired等のDecision Stateとは区別される。」

これにより、Authorization Decision Result(Permit/Deny/Indeterminate)とDecision State(Allow/Deny/Invalid/Revoked/Expired)が別の階層の概念であることが明記され、技術仕様書(docs/ Chapter 07)側の状態モデルとの関係が整理されました。**解消済みと判断します。**

### 3.2 先行技術文献番号の未特定(旧・重大度高)

**修正内容:** `prior_art_analysis.md`の第3.8節に、以下の具体的な文献情報を含む比較表が追加されました。

- US20080184336A1(Policy resolution in an entitlement management system)
- US20140282831A1(Dynamic policy-based entitlements from external data repositories)
- US20230049227A1 / US20240305635A1(client device authentication system)
- RFC 8628(OAuth 2.0 Device Authorization Grant)
- W3C WebAuthn Level 2 / Level 3
- FIDO Alliance Passkeys / Cross-Device Authentication

各文献について、優先日・公開日、開示内容、Claim 1との比較対象、本発明との差異・留意点が整理されています。文献の一次情報(具体的請求項・段落との対応等)は引き続き弁理士による確認が必要ですが、**代表的な文献番号の特定という点では大きく前進しており、解消済みと判断します。**

### 3.3 独立請求項の本数の食い違い(旧・重大度中)

**修正内容:** `claims_v2_attorney_review.md`の第16節が、以下のように整理されました。

> 独立Claim: Claim 1(情報処理システム)・Claim 31(情報処理方法)・Claim 36(プログラム)
> その他の従属Claim: Claim 37 — Claim 36に従属する記録媒体

以前は同文書内で「独立Claim 4本(Claim 37を含む)」とする記載と矛盾していましたが、今回、Claim 37が明確にClaim 36に従属する記録媒体として位置付けられ、独立請求項は3本(Claim 1・31・36)に統一されました。**解消済みと判断します。**

---

## 4. 重大度:高 — 未解消・出願前に必ず解消すべき点

### 4.1 Validity分離のFallback戦略と実クレームの不整合(継続課題・未解消)

**該当箇所:** `dependent_claim_strategy_v2.md`第77節(Priority 3)、`claims_v2_attorney_review.md`第9節(Fallback B)

今回の修正版でも、以下の状態が継続しています。

- `claims_v2.md`のClaim 1本文には、既に「前記EntitlementのValidityを前記Authenticationに使用されるAuthentication ObjectのValidityとは独立して管理可能とし」という記載が含まれています。
- にもかかわらず、`dependent_claim_strategy_v2.md`第14節・第77節(Priority 3)、および`claims_v2_attorney_review.md`第9節(Fallback B)は、依然として「Authentication ObjectのValidityとEntitlementのValidityとの時間的独立性を追加する」ことを**Fallback(Claim 1が拒絶された場合の縮小先)**として位置付けたままです。

Claim 1に既に組み込まれている構成を、Claim 1が拒絶された場合の「逃げ場」として温存することはできません。この矛盾は`patent_review_summary2.md`でも指摘しましたが、**今回の修正では対応されていません。**

**修正方法(再掲):** `dependent_claim_strategy_v2.md`および`claims_v2_attorney_review.md`のFallback一覧から、Validity分離の項目を削除するか、「Claim 1に既に含まれる中心構成であり、Fallbackではない」旨に記載を改めてください。

**具体的な文言案(最小限の修正)**

文言を大きく書き換えるのではなく、「これはFallbackではなく、Claim 1の中心構成である」という一文を追記するだけで矛盾を解消できます。

*修正箇所①:`dependent_claim_strategy_v2.md` 第14節*

現在の記載:
```
Authentication ObjectのValidityは、EntitlementのValidityとは独立して管理できる。

この限定は重要なFallbackとする。
```

最小修正後:
```
Authentication ObjectのValidityは、EntitlementのValidityとは独立して管理できる。

この限定は、Claim 1の本文に既に含まれる中心構成であり、Fallbackではない。
```

*修正箇所②:`dependent_claim_strategy_v2.md` 第77節(Priority 3)*

現在の記載:
```
### Priority 3

Authentication Object ValidityとEntitlement Validityの分離
```

最小修正後(見出しの直後に1行追加するだけ):
```
### Priority 3

Authentication Object ValidityとEntitlement Validityの分離

※この構成はClaim 1に既に含まれているため、独立したFallbackとしては機能しない。Claim 1が拒絶された場合のFallbackとしては、Priority 4以降(Conditional Entitlement等)を先に検討する。
```

*修正箇所③:`claims_v2_attorney_review.md` 第9節(Fallback B)*

現在の記載:
```
## Fallback B

Authentication ObjectのValidityとEntitlementのValidityとの時間的独立性を追加する。
```

最小修正後:
```
## Fallback B(※参考情報 — 現Claim 1に含まれる構成)

Authentication ObjectのValidityとEntitlementのValidityとの時間的独立性。

本構成は既にClaim 1本文に含まれているため、Claim 1が拒絶された場合の縮小先としては機能しない。仮にClaim 1からこの限定を外す補正を行う場合にのみ、再度Fallbackとして機能し得る。
```

この3箇所を直せば、「Claim 1に既に入っている構成を、Claim 1が拒絶された時の逃げ場として使う」という矛盾は解消されます。

---

## 5. 重大度:中 — 未解消・弁理士確認を推奨する点

### 5.1 Claim 1の抽象度(ミーンズプラスファンクション的表現)(継続課題・未解消)

Claim 1は引き続き「〜処理部」という機能表現が中心です。今回、明細書側(`specification_v2.md`第44節、`specification_v2_attorney_review.md`のClaim 1 Support確認節)で、Claim 1の各構成要素に対応する説明が明示的に整理された点は改善ですが、明確性要件(特許法36条6項2号)の観点からの最終判断は、引き続き弁理士確認が必要です。

**具体的な対応方針**

率直にお伝えすると、これは文言の追記だけでは完全には解消できない項目です。「〜処理部」という表現自体は、日本の実務ではミーンズプラスファンクション的claimとして一定のリスクを伴いますが、それが実際に問題になるかどうかは審査官の判断や個別の技術分野の慣行によるところが大きく、AIの一次チェックでは断定できません。

最小限できることとしては、`specification_v2.md`第44節(明細書と請求項との対応)に、各処理部が「何を入力とし、何を出力するか」を1行ずつ補足する程度です。ただし、これは**「リスクを多少下げる」処置であり「解消」ではありません**。この項目については、修正ではなく**弁理士への確認事項として残す**のが最も適切な対応です。

### 5.2 実施形態が単一シナリオに偏っている(継続課題・未解消)

`specification_v2.md`第34節、`specification_v2_attorney_review.md`第27節、`claims_v2_attorney_review.md`第12節(Scenario D)のいずれも、依然として「来店確認QR認証 → EC割引適用」という単一の実施形態のみです。Claim 22(`claims_v2.md`)で言及されている決済・特典付与・API処理等、他の適用例についての実施形態は、今回の修正でも追加されていません。

**修正方法(再掲):** Claim 22に対応する決済・特典付与・API処理の具体的な実施形態を、明細書に1つずつでも追加することをお勧めします。

**具体的な文言案(最小限の修正)**

既存の「来店確認+EC割引」の実施形態(`specification_v2.md`第34節/`specification_v2_attorney_review.md`第27節)の直後に、決済・特典付与の2パターンを2〜3文ずつ追加するだけで、Claim 22への対応が最小限確保できます。

*追加箇所:`specification_v2.md`(および`specification_v2_attorney_review.md`)の実施形態節の末尾*

```
## 34-2. 決済処理への適用実施形態

利用者が所定の店舗においてAuthenticationを実行し、その結果に基づいて
「本人確認済み」というEntitlementを生成することができる。

決済Serviceは、当該Entitlementを取得してPolicy Evaluationを実行し、
所定の決済条件が成立していることを確認する。

条件が成立した場合、Authorization DecisionとしてPermitを生成し、
Enforcementによって決済処理をService Executionとして実行可能とする。

## 34-3. 特典付与への適用実施形態

利用者が所定のAuthenticationを完了した結果に基づいて、
「特典付与対象」というEntitlementを生成または有効化することができる。

特典付与Serviceは、当該Entitlementおよび所定のPolicyに基づいて
Authorization Decisionを生成し、Permitの場合にポイントまたは
クーポンの付与をService Executionとして実行する。
```

これでClaim 22に列挙されている「決済」「特典の付与」への言及が明細書に最低限具体化されます(API処理については、第37節「分散システムへの適用」の記載を援用できるため、追加不要と考えます)。

---

## 6. 重大度:低 — 未解消・記載の質を高める改善点

### 6.1 用語の英日混在(継続課題・未解消)

全文書を通じて、英語の技術用語(Entitlement、Authorization Decision等)がそのままカタカナ化されず使われている状態は変わっていません。日本語出願書類として最終化する際の統一方針は、引き続き検討事項です。

**具体的な対応方針**

これは出願直前の翻訳・用語統一フェーズでまとめて対応するのが効率的なため、現時点での個別修正は不要と考えます(現状のまま進めて問題ありません)。

---

## 7. 今回未確認の項目

以下は、更新日時に変化が見られなかったため、内容を再確認していません。`patent_review_summary2.md`時点での指摘がそのまま該当すると考えられます。

- **claim_mapping.mdの図面番号・請求項総数の整合性**(旧§4.3、§4.4):`claims_v2.md`の請求項数は現在37で一致していますが、`claim_mapping.md`自体の記載内容(図面番号の対応等)は前回から更新されていないため、再確認をお勧めします。
- **attorney_package/03_drawings/、06_prior_art/、07_version1_reference/**:更新なしのため、前回の確認結果(図面の共有状況、先行技術比較の製品名ベースの記載等)がそのまま該当します。
- **design/、protocol/ディレクトリ**:引き続き未確認です。`claim1_support_matrix_v2.md`等が引用する裏付け資料(`canonical_object_graph.md`等)の実在性は、まだ検証できていません。

---

## 8. ファイルごとの状態一覧

| ファイル | 状態 | 今回の判定 |
|---|---|---|
| patent/specification_v2.md | 更新あり(9/8) | §21に用語整理を追加、解消済み(§3.1)。実施形態は単一のまま(§5.2) |
| patent/claims_v2.md | 更新あり(9/7) | Claim 19に用語整理を追加、解消済み(§3.1) |
| patent/invention_core_v2.md | 更新なし | 前回同様 |
| patent/claim_structure_v2.md | 更新なし | 前回同様 |
| patent/dependent_claim_strategy_v2.md | 更新あり(9/8) | Fallback B/Priority 3の矛盾は未解消(§4.1) |
| patent/invention_summary.md | 更新なし | 前回同様 |
| patent/prior_art_analysis.md | 更新あり(9/7) | 具体的文献番号を追加、解消済み(§3.2) |
| patent/claim_mapping.md | 更新なし | 前回同様(§7) |
| attorney_package/01_patentability_review/ | 更新なし | 前回同様 |
| attorney_package/02_application_documents/claims_v2_attorney_review.md | 内容更新確認 | 独立請求項の数え方を修正、解消済み(§3.3)。Fallback Bの矛盾は未解消(§4.1) |
| attorney_package/02_application_documents/specification_v2_attorney_review.md | 内容更新確認 | Claim 1 Support確認節を整理。実施形態は単一のまま(§5.2) |
| attorney_package/03_drawings/ | 更新なし | 前回同様(§7) |
| attorney_package/05_claim_support/ | 更新なし | 前回同様 |
| attorney_package/06_prior_art/ | 更新なし | 前回同様(§7) |
| attorney_package/07_version1_reference/ | 更新なし | 前回同様(§7) |

---

## 9. 推奨アクションリスト(優先順位付き)

**出願前に必須:**
1. `dependent_claim_strategy_v2.md`および`claims_v2_attorney_review.md`のFallback一覧から、Validity分離(Fallback B / Priority 3)をClaim 1と重複しない形に整理する(§4.1に具体的な文言案あり)

**出願前に推奨:**
2. Claim 1の機能表現(処理部・評価部)について、明確性要件の観点から弁理士に確認してもらう(§5.1、対応方針あり・要弁理士確認)
3. Claim 22(決済・特典付与・API処理等)に対応する実施形態を明細書に追加する(§5.2に具体的な文言案あり)

**時間があれば対応:**
4. `claim_mapping.md`の図面番号・請求項総数(37)の対応関係を、最新のClaim構成と突き合わせて再確認する(§7)
5. `design/canonical_object_graph.md`等、`claim1_support_matrix_v2.md`が引用する裏付け資料の実在性を検証する(§7)
6. 用語の英日混在方針を統一する(§6.1)

---

## 10. 留意事項

- 本レビューはAIによる一次チェックであり、法的助言ではありません。新規性・進歩性・記載要件の最終判断については、必ず弁理士にご確認ください。
- 今回のレビューは、更新日時をもとに変更箇所を特定する方式で行いました。タイムスタンプが更新されていないファイルについても、軽微な修正(誤字修正等、ファイルサイズやタイムスタンプに表れない変更)が行われている可能性はゼロではありません。心当たりがある場合はお知らせください。
- `.html`版のファイルは今回も確認していません。`.md`版との差分が生じていないか、最終稿を作成する前にご確認ください。

