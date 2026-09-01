# NEW-shot2play Version 2.0 特許出願書類 総合レビューまとめ

**作成日:** 2026年8月29日(2026年8月31日 一部更新)
**対象:** patent/ 配下の出願関連文書一式、docs/ 配下の技術仕様書(Chapter 1-10)
**レビュー実施者:** Claude(AI) — 本レビューは法的助言ではなく、出願前チェックの参考情報です。最終判断は弁理士にご確認ください。

---

## 目次

1. [レビュー範囲と前提](#1-レビュー範囲と前提)
2. [総合評価サマリー](#2-総合評価サマリー)
3. [重大度:高 — 出願前に必ず解消すべき点](#3-重大度高--出願前に必ず解消すべき点)
4. [重大度:中 — 弁理士確認を推奨する点](#4-重大度中--弁理士確認を推奨する点)
5. [重大度:低 — 記載の質を高める改善点](#5-重大度低--記載の質を高める改善点)
6. [技術仕様書(docs/)由来の追加検討候補](#6-技術仕様書docs由来の追加検討候補)
7. [attorney_package/ のレビュー結果](#7-attorney_package-のレビュー結果)
8. [ファイルごとの状態一覧](#8-ファイルごとの状態一覧)
9. [推奨アクションリスト(優先順位付き)](#9-推奨アクションリストの優先順位付き)
10. [留意事項](#10-留意事項)

---

## 1. レビュー範囲と前提

以下の文書を通読した上でのレビューです。

**特許出願文書 (patent/)**
- `specification_v2.md`(明細書、27K)
- `claims_v2.md`(クレーム、33K)
- `invention_core_v2.md`(発明の核心、43K)
- `claim_structure_v2.md`(クレーム構成、50K)
- `dependent_claim_strategy_v2.md`(従属請求項戦略、42K)
- `invention_summary.md`(発明サマリー、**新規追加**)
- `prior_art_analysis.md`(先行技術分析、**新規追加**)
- `claim_mapping.md`(クレーム対応表、**新規追加**)

**技術仕様書 (docs/)**
- Chapter 01〜10(Framework Overview 〜 Use Cases)

**今回追加で確認した文書 (attorney_package/、7サブディレクトリすべて)**
- `01_patentability_review/NEW-shot2play_V2.0_patentability_review_memo.md`(16K)
- `02_application_documents/application_document_mapping.md`(4.7K)、`claims_v2_attorney_review.md`(25K、日本語クレーム原稿)、`specification_v2_attorney_review.md`(23K、日本語明細書原稿)
- `03_drawings/drawing_support_matrix_v2.md`(14K)
- `04_technical_documents/`(空)
- `05_claim_support/claim1_support_matrix_v2.md`(14K)
- `06_prior_art/claim1_prior_art_matrix_v2.md`(17K、具体的製品名を含む先行技術比較表)
- `07_version1_reference/version1_version2_relationship.md`(15K)

**未確認/範囲外**
- `design/`、`protocol/` ディレクトリの内容(`design/canonical_object_graph.md`、`design/canonical_vocabulary.md`、`protocol/object_model.md`、`protocol/normative_requirements.md` は `attorney_package/` 内の複数文書から参照されていますが、今回は未読み込みです。次回の重要な確認対象としてお勧めします)
- `.html`版ファイル(`.md`版と内容が異なる可能性があります)
- `figures/` 配下の図面ファイルそのもの(参照のみ)

---

## 2. 総合評価サマリー

文書群全体の設計プロセス(Phase 1発明の核心 → Phase 2クレーム構成 → Phase 3クレーム初稿 → Phase 4従属請求項戦略 → 先行技術分析・クレームマッピングによる裏付け)は一貫しており、**弁理士に引き継ぐための準備としては非常に高い完成度**です。特に、

- Authentication Result と Entitlement の非同一性、
- Authentication Object Validity と Entitlement Validity の独立管理、
- Authorization Decision と Enforcement の分離、

という発明の中心思想が、明細書・クレーム・サマリー・先行技術分析・マッピング表のすべてで一貫して繰り返されている点は、サポート要件・明確性要件の観点から強みです。

一方で、**新規追加された3文書(先行技術分析・サマリー・マッピング表)を既存のクレーム戦略文書(Phase 2〜4)と突き合わせた結果、いくつかの整合性の齟齬**が見つかりました。以下、重大度別に記載します。

---

## 3. 重大度:高 — 出願前に必ず解消すべき点

### 4.1 Validity分離のFallback戦略と実クレームの不整合(継続課題)

**該当箇所:** `dependent_claim_strategy_v2.md` Phase 4 vs `claims_v2.md` Claim 1 vs `claim_mapping.md` §3

- `claim_mapping.md`(新規)では、「Authentication Object Validity と Entitlement Validity の独立管理」を**Claim 1の中核構成要素**として明記し、Specification §9〜§11の裏付けがあるとしています。
- 一方 `dependent_claim_strategy_v2.md`(Phase 4)は、同じ要素をPriority 3の**Fallback(従属請求項への後退用の予備手段)**として位置付けたままです。
- つまり、**「Claim 1にすでに組み込んだ要素」を「Claim 1が拒絶された場合の逃げ場」として二重に扱っている**状態です。先行技術によりClaim 1が拒絶された場合、このFallbackは実質的に使えません(すでに使い切っているため)。

**修正方法:**
`dependent_claim_strategy_v2.md` のPriority 3の記載を見直し、①Validity分離をClaim 1の必須要素として確定させた上でFallbackリストから外す、②またはClaim 1からValidity分離を外して真のFallbackとして温存する、のいずれかに統一してください。現状のクレーム(`claims_v2.md`)がすでに①の状態にあるため、実務上は**Phase 4文書側を①に合わせて修正するのが自然**です。

> **追加確認(attorney_package/):** この不整合は、より新しい `attorney_package/02_application_documents/claims_v2_attorney_review.md` §9(Fallback候補一覧)でも**そのまま引き継がれています**。同文書のFallback Bとして「Authentication Object ValidityとEntitlement Validityとの時間的独立性を追加する」とありますが、同じ文書内の請求項1本文(§4)には、この独立性の記載が既に組み込まれています。つまり、日本語クレーム原稿として整えられた最新版でも、この矛盾は未解消のまま持ち越されています。**出願原稿を確定させる前に必ず解消してください。**

### 4.2 prior_art_analysis.md の想定先行技術と、実際の調査結果が未統合

**該当箇所:** `prior_art_analysis.md` 全体

- 本書冒頭に「既存の調査で確認した先行技術領域と、本発明との差異の中心を整理する」とあり、FIDO/WebAuthn、QR認証、Policy-based Access Control、OAuth、Entitlement/Rights Management、Session/Token管理、Distributed Authorizationの7領域について「個別要素の既知性:高い」との暫定評価が記載されています。
- しかし、**具体的な特許番号・公開番号・引用箇所・出願人名等が一切記載されていません**(本書内でも「本書では、確認できていない具体的文献情報を推測して追加しない」と明記されています)。
- 前回のやり取りで「先行技術調査自体は完了している」とのことでしたので、この温度差は「調査は完了しているが、本書へのアウトプットがまだ抽象的なレベルに留まっている」ことを意味していると理解しています。

**修正方法:**
弁理士に引き継ぐ前に、§3の各領域について、少なくとも代表的な**文献名・公開番号・出願人・公開日**を1〜2件ずつ紐付けてください。現状の記載(技術領域名の列挙のみ)では、弁理士が実質的な新規性・進歩性の判断を行うための一次資料として機能しません。「8. Attorney Review / Next Actions」に記載の8項目は、具体的な引用文献なしには弁理士側でも答えようがない設問です。

> **追加確認(attorney_package/):** `06_prior_art/claim1_prior_art_matrix_v2.md` では、`prior_art_analysis.md` より一歩踏み込み、**FIDO Cross-Device Authentication、Apple Passkey、Google Password Manager、Microsoft Authenticator、WhatsApp Web、GitHub Device Flow、OAuth 2.0 Device Authorization Grant**という具体的な製品・仕様名を挙げた比較表(§18)を作成しています。これは前進ですが、依然として**具体的な特許文献番号・公開番号は一件も記載されていません**。弁理士への引き継ぎ資料としては、この製品名リストを起点に、対応する特許出願(あれば)を特定する作業が追加で必要です。

### 4.3 Claim 19の三値(Permit/Deny/Indeterminate)と技術仕様書側の状態モデルの不一致

**該当箇所:** `claim_mapping.md` §7(Claims 18-23)、`invention_summary.md` §4.4、技術仕様書 Chapter 07(7.17 Decision State)

- 特許文書側では、Authorization Decisionの結果を一貫して **Permit / Deny / Indeterminate** の三値として説明しています(`invention_summary.md` §4.4、`claim_mapping.md` Claim 19)。
- 一方、技術仕様書 Chapter 07 §7.17では、Decision Stateとして **Pending / Allow / Deny / Invalid / Revoked / Expired** という6状態のライフサイクルモデルが規定されており、「Indeterminate」という語は主にChapter 06(Policy Evaluation)側の評価結果(True/False/Indeterminate)として登場します。
- 特許文書の「Authorization Decisionの三値(Permit/Deny/Indeterminate)」は、技術仕様書でいう「Policy Evaluationの結果」と「Authorization Decisionの状態」を混在させた表現になっている可能性があります。

**修正方法:**
明細書・クレームにおいて、①Policy Evaluationの評価結果(True/False/Indeterminate)、②Authorization Decisionの状態(Allow/Deny/Invalid/Revoked/Expired等)を明確に区別して記載しているか、`specification_v2.md` の該当節(§21-§24)を再確認してください。両者を同一の三値として扱っている場合、日本の特許実務では用語の技術的正確性(明確性要件)の観点から、審査官から釈明を求められる可能性があります。

> **追加確認(attorney_package/):** `specification_v2_attorney_review.md`(日本語明細書原稿)でも、Permit/Deny/Indeterminateの三値表現がそのまま採用されています(§16-17)。つまり、この用語選択は既に意図的な方針として固まりつつあると見受けられます。その前提であれば、技術仕様書(docs/ Chapter 07)側のAllow/Deny/Invalid/Revoked/Expiredという状態モデルとの関係を、明細書中で一言(「Authorization Decisionの状態遷移の詳細は技術仕様書のライフサイクルモデルに準じ、Permit/Deny/Indeterminateはその結果表現の一形態である」等)補足しておくと、整合性の疑義を予防できます。

---

## 4. 重大度:中 — 弁理士確認を推奨する点

### 5.1 Claim 1の抽象度(ミーンズプラスファンクション的表現)

Claim 1は先行技術回避のために意図的に広く抽象化されていますが、「〜処理部」「〜評価部」等の機能表現が多く、日本の特許法36条6項2号(明確性要件)や均等論の射程との関係で、出願実務家による表現の精査が必要です。特に、`claim_mapping.md` が対応付けているSpecification §5・§6の記載が、Claim 1の各構成要件を「どのように実現するか」まで十分に裏付けているか、原文を対照読みすることをお勧めします。

### 5.2 実施形態が単一シナリオに偏っている

明細書の具体的実施形態は、依然として「来店確認QR認証 → EC割引適用」という単一の例にほぼ限定されています(`invention_summary.md` §8でも同じ例が再掲されているのみ)。Claim 22で言及されている決済・特典付与・API処理等、他の適用例についても簡単な実施形態の記載を追加すると、クレームの広さに対する明細書のサポートが強化されます。

> **追加確認(attorney_package/):** `specification_v2_attorney_review.md` §27でも、同じ「来店認証+ECサービス」の実施形態のみが記載されています(Scenario Dとしてクレーム原稿側`claims_v2_attorney_review.md` §12にも再掲)。日本語原稿として整えられた段階でもこの点は未解消のままです。

### 5.3 claim_mapping.md の図面対応(§12)の精度、および図面の共有問題

`claim_mapping.md` §12では「図1〜図7」をcanonical figureとして扱うとしていますが、`docs/index.html` に掲載されている技術仕様書側の図面番号(図01, 13, 15, 24, 25, 30, 35等)とは**採番体系が異なります**。

> **追加確認(attorney_package/):** `03_drawings/drawing_support_matrix_v2.md` を確認したところ、実際の出願用図面としては `docs/index.html` 側の採番(Figure 01, 13, 15, 24, 25, 30, 35)がそのまま採用されていることが分かりました。つまり採番の「混同」ではなく、**特許出願用の図面と技術仕様書公開ページの図面が、意図的に同一ファイルとして共有されている**状態です。図面としての対応関係自体は問題ありませんが、両者が同一ファイルである点は、今後図面を個別に差し替える際に一方だけ更新漏れが起きないよう留意してください。

### 5.4 claim_mapping.md と claims_v2.md の請求項数の整合性、および独立請求項の数え方の矛盾

`claim_mapping.md` は「37請求項」を前提に記載されています(§13)。`claims_v2.md`(33K)の実際の請求項数がこれと一致しているか、最終稿を作成する前に機械的に数え直すことをお勧めします(手作業でのマッピング表更新は、クレーム番号のズレが生じやすい典型的なミス箇所です)。

> **追加確認(attorney_package/):** `claims_v2_attorney_review.md` を精査した結果、**文書内で独立請求項の数え方に矛盾**が見つかりました。
> - §16「Claim間の構造」では、独立Claimとして **Claim 1・Claim 31・Claim 36・Claim 37(記録媒体)の4つ** を挙げています。
> - しかし同じ文書の§8に記載された請求項37の実際の文言は「**請求項36に記載のプログラムを記録した**、コンピュータ読み取り可能な記録媒体。」であり、これは明確に**請求項36に従属する記載**です。
> - また `01_patentability_review/...memo.md` §4では「独立請求項は3つ(Claim 1・31・36)」と説明されており、こちらとも数が一致しません。
>
> 弁理士に引き継ぐ前に、独立請求項が実際に何本(1・31・36の3本か、記録媒体を含む4本か)なのか、文書間で統一してください。日本の特許実務では、記録媒体クレームを独立形式(「情報処理方法を実行させるプログラムを記録した記録媒体」のように完結した表現)にするか、プログラムクレームに従属させるかで扱いが変わるため、この点は弁理士確認の必須項目です。

---

## 5. 重大度:低 — 記載の質を高める改善点

- **invention_summary.md §11「Source Position」**: 本書の基礎資料として `specification_v2.md`、`claims_v2.md`、「既存の発明核心資料」とありますが、`invention_core_v2.md` を明示的に指していないため、文書間の参照関係をやや分かりにくくしています。ファイル名を明記した方が、後から読む第三者(弁理士含む)にとって親切です。
- **prior_art_analysis.md §7の表**: 「個別要素の既知性:高い」という評価が7領域すべてで同一になっており、リスクの相対的な強弱が視覚的に区別しにくくなっています。特に3.1(FIDO/WebAuthn)と3.4(OAuth)は、Claim 1の中心構成(Validity分離)との近接度が異なるはずなので、リスクレベルを段階的に表現する(例:高/中/要確認)と、弁理士が優先的に確認すべき領域が伝わりやすくなります。
- **用語の英日混在**: 全文書を通じて英語の技術用語(Entitlement、Authorization Decision等)がそのままカタカナ化されず使われています。日本語出願書類として最終化する際、これらの用語をそのまま残すか、対応する日本語訳を当てるか(例:Entitlement→権利情報等)、統一方針を早めに決めておくと、翻訳・補正段階での手戻りを防げます。

---

## 6. 技術仕様書(docs/)由来の追加検討候補

前回のレビューで指摘した、技術仕様書には存在するが特許文書側に未反映の技術的特徴です。先行技術分析(`prior_art_analysis.md`)が今回追加されたことで、これらを「追加のFallbackとして採用する価値があるか」を判断する材料が揃いました。

| # | 技術的特徴 | 出典(docs/) | 特許文書での状況 | 推奨アクション |
|---|---|---|---|---|
| 1 | Entitlementの数量管理・消費のAtomicity(二重消費防止) | Chapter 05 §4.11, 5.13, 5.14 / Chapter 10 §10.10 | 未反映 | `prior_art_analysis.md` §3.5(Entitlement/Rights Management)の評価と合わせて、従属請求項候補として追加検討 |
| 2 | Emergency Authorization(緊急時認可) | Chapter 10 §10.30 | 未反映 | 権利範囲を狭める可能性があるため、明細書の実施形態として言及するに留めるか要検討 |
| 3 | Authorization Decisionのキャッシュ・障害復旧後の状態再構築 | Chapter 10 §10.27, 10.28 | 未反映(Distributed Architectureへの言及はあるが運用面は未記載) | Cross-Service関連クレーム(Claim 14, 34)の技術的裏付けとして明細書に追記する価値あり |
| 4 | 複数Policy間の優先順位・競合解決(Policy Precedence) | Chapter 06 §6.17-6.20 | 未反映(単一Policy評価が前提) | `prior_art_analysis.md` §3.3(Policy-based Access Control)との差別化要素になり得るため、先行技術との比較後に追加検討 |
| 5 | Trust Domain(信頼領域)の区分 | Chapter 08 §7.6-8.8 | 未反映 | Cross-Service Entitlement(Claim 14)の技術的裏付け強化に利用可能 |

---

## 7. attorney_package/ のレビュー結果

`attorney_package/` は、`patent/` 配下の技術文書一式を**日本特許出願の実際の書式(明細書・請求項の法的表現)に落とし込んだ、弁理士レビュー用の原稿一式**です。全体として、`patent/` 側の技術的検討内容を非常に忠実に、かつ丁寧に法的書式へ変換しようとしている印象で、完成度は高いです。以下、ディレクトリ単位での所見です。

### 8.1 01_patentability_review/(特許性検討メモ)

`patent/prior_art_analysis.md` や `invention_summary.md` の内容を踏まえた、弁理士向けの総括メモです。Version 1.0との関係(§14)についても触れられていますが、この点は確認済みとのことですので本レビューでは扱いません。

### 8.2 02_application_documents/(出願書類原稿)

実際の日本語クレーム原稿(`claims_v2_attorney_review.md`)と明細書原稿(`specification_v2_attorney_review.md`)が既に作成されています。**請求項1の実際の法的文言(「〜処理部」形式)まで書き起こされており**、出願準備は想定以上に進んでいます。既存の「4.2」「5.4」で述べた不整合(Fallback戦略の矛盾、独立請求項の数え方)は、この最新原稿にも引き継がれているため、優先的な修正対象です。

### 8.3 03_drawings/(図面対応表)

前述の通り、特許図面と技術仕様書公開ページの図面が同一である点を確認しました。図面Support自体(明細書・クレームとの対応関係)は体系的に整理されており、内容面での不備は見当たりません。

### 8.4 04_technical_documents/

空のディレクトリでした。将来的に技術文書を格納する予定と思われますが、現時点では中身がありません。

### 8.5 05_claim_support/(Claim1構成要件ごとのSupport表)

Claim 1を14の構成要素に分解し、それぞれの明細書サポート状況を"STRONG"、"SUPPORTED"等で評価した詳細な表です。全体として評価は高く出ていますが、これは発明者側の自己評価である点に留意してください(弁理士による独立した評価とは異なります)。`design/canonical_object_graph.md` という今回未読み込みのファイルが複数箇所で参照根拠として引用されているため、次回はこのファイルも確認することをお勧めします。

### 8.6 06_prior_art/(Claim1構成要件ごとの先行技術比較表)

`patent/prior_art_analysis.md` より踏み込んだ内容で、FIDO・Passkey・Google Password Manager・Microsoft Authenticator・WhatsApp Web・GitHub Device Flow・OAuth 2.0 Device Authorization Grant等、具体的な製品・仕様との比較表(§18)を含みます。「4.2」で述べた通り、依然として特許文献番号までは踏み込めていませんが、比較の解像度は`patent/`側より高くなっています。

### 8.7 07_version1_reference/(Version1/2関係整理)

Version 1.0とVersion 2.0の関係を技術的な観点から丁寧に整理した文書です。両バージョンの技術仕様書がかつて公開Web上にあった経緯についても触れられていますが、この点は既に弁理士確認済みとのことです。

---

## 8. ファイルごとの状態一覧

| ファイル | サイズ | 状態 | 主なレビュー結果 |
|---|---|---|---|
| specification_v2.md | 27K | 完成 | §3.3参照 |
| claims_v2.md | 33K | 完成 | §3.1、§3.3、§4.4参照 |
| invention_core_v2.md | 43K | 完成 | 前回レビュー済み(一貫性高い) |
| claim_structure_v2.md | 50K | 完成 | 前回レビュー済み(Option A/B併記の名残に注意) |
| dependent_claim_strategy_v2.md | 42K | 完成 | §3.1参照(要修正) |
| invention_summary.md | (新規) | 完成 | §5参照 |
| prior_art_analysis.md | (新規) | **暫定版** | §3.2参照(具体的引用文献が未記載) |
| claim_mapping.md | (新規) | 完成 | §4.3、§4.4参照 |
| attorney_package/01_patentability_review/...memo.md | 16K | 完成 | §7.1参照 |
| attorney_package/02_application_documents/application_document_mapping.md | 4.7K | 完成 | §7.2参照 |
| attorney_package/02_application_documents/claims_v2_attorney_review.md | 25K | 完成(要修正) | §3.1、§4.4、§7.2参照 |
| attorney_package/02_application_documents/specification_v2_attorney_review.md | 23K | 完成 | §3.3、§4.2、§7.2参照 |
| attorney_package/03_drawings/drawing_support_matrix_v2.md | 14K | 完成 | §4.3、§7.3参照 |
| attorney_package/04_technical_documents/ | - | **空** | §7.4参照 |
| attorney_package/05_claim_support/claim1_support_matrix_v2.md | 14K | 完成 | §7.5参照 |
| attorney_package/06_prior_art/claim1_prior_art_matrix_v2.md | 17K | 完成 | §3.2、§7.6参照 |
| attorney_package/07_version1_reference/version1_version2_relationship.md | 15K | 完成 | §7.7参照 |

---

## 9. 推奨アクションリスト(優先順位付き)

**出願前に必須:**
1. `dependent_claim_strategy_v2.md` および `claims_v2_attorney_review.md` §9のPriority 3 / Fallback B(Validity分離)の位置付けを、実際のClaim 1の記載と整合させる(§3.1)
2. `prior_art_analysis.md` および `claim1_prior_art_matrix_v2.md` で挙げられた具体的製品(FIDO、Passkey、OAuth Device Flow等)について、対応する特許文献番号・出願人・公開日を特定する(§3.2)
3. Authorization Decisionの結果表現(Permit/Deny/Indeterminate vs Allow/Deny/Invalid/Revoked/Expired)を明細書・クレーム内で統一する、または両者の関係を明記する(§3.3)
4. 独立請求項の本数(1・31・36の3本か、37を含む4本か)を文書間で統一する(§4.4)

**出願前に推奨:**
5. Claim 1の機能表現(処理部・評価部)について、明確性要件の観点から弁理士に確認してもらう(§4.1)
6. Claim 22(決済・特典付与・API処理等)に対応する実施形態を明細書に追加する(§4.2)
7. `claim_mapping.md` の請求項総数(37)と `claims_v2.md` の実際の請求項数を突き合わせる(§4.4)

**時間があれば対応:**
8. `design/canonical_object_graph.md`、`design/canonical_vocabulary.md`、`protocol/object_model.md`、`protocol/normative_requirements.md` を確認し、`claim1_support_matrix_v2.md` 等が引用する裏付けの実在性を検証する(§7.5)
9. 技術仕様書由来の追加候補(数量管理、Policy優先順位、Trust Domain等)を、先行技術との関係を踏まえて従属請求項に追加するか判断する(§6)
10. 用語の英日混在方針を統一する(§5)

---

## 10. 留意事項

- 本レビューはAIによる一次チェックであり、法的助言ではありません。新規性・進歩性・記載要件の最終判断については、必ず弁理士にご確認ください。
- `design/`、`protocol/` ディレクトリは今回も未読み込みです。`attorney_package/` 内の複数文書がこれらを裏付け資料として引用しているため、次回優先的に確認することをお勧めします。
- `.html` 版のファイル(`claims_v2.html`、`specification_v2.html`、`dependent_claim_strategy_v2.html`)は `.md` 版と更新日時が異なっており、内容差分がある可能性があります。最終稿として `.md` と `.html` のどちらを正本とするか、事前に決めておくことをお勧めします。

