# NEW-shot2play Technical Specification Version 2.0
# 日本特許出願用 明細書ドラフト
# Attorney Review Draft

**対象:** 日本特許出願  
**位置付け:** 弁理士レビュー用技術原稿  
**基礎文書:** `patent/specification_v2.md`

> 本書は、NEW-shot2play Technical Specification Version 2.0の技術内容を基礎として、日本特許出願に向けて明細書形式に再構成した弁理士レビュー用ドラフトである。正式な出願書式、手続事項、法的な最終表現については弁理士による確認・修正を前提とする。

# 【発明の名称】

認証結果と権利情報とを独立して管理し、ポリシー評価に基づいて認可を決定し、当該認可をサービス実行に適用する情報処理システム、情報処理方法、プログラムおよび記録媒体

# 【技術分野】

本発明は、情報処理システム、アクセス制御、認証、権利管理、ポリシー評価、認可制御およびサービス実行制御に関する。

より具体的には、本発明は、Authenticationによって確認された結果と、当該Authenticationとは独立して管理され得るEntitlementとを用いてPolicy Evaluationを行い、その評価結果に基づいてAuthorization Decisionを生成し、当該Authorization DecisionをEnforcementを介してService Executionに適用する情報処理技術に関する。

# 【背景技術】

従来の情報処理システムでは、利用者をAuthenticationによって確認し、その結果に基づいてサービスへのアクセスを許可する構成が広く用いられている。

Authenticationには、パスワード、ワンタイムパスワード、QR Code、公開鍵暗号、WebAuthn等を利用することができる。

しかし、Authenticationによって確認された事実と、その利用者が現在有する権利または資格と、特定のサービスについて実際に許可される操作とは、必ずしも同一ではない。

例えば、店舗におけるAuthenticationによって「来店した」という事実を確認し、その事実に基づいて数時間有効なEntitlementを付与する場合、Authenticationに使用した一時的情報が失効したことだけを理由としてEntitlementまで失効させる必要はない。

また、Entitlementだけによってサービス実行を許可すると、サービス、資源、操作、時間、取引状態その他のContextを考慮した細かな制御が困難になる。

したがって、Authentication、Entitlement、Policy Evaluation、AuthorizationおよびService Executionを異なる情報および処理として扱いながら、所定の関係によって連携させる技術が求められる。

# 【発明が解決しようとする課題】

本発明の一つの目的は、Authenticationの結果と、Authenticationとは独立して管理され得るEntitlementとを区別して扱うことである。

本発明の他の目的は、Authentication ObjectのValidityとEntitlementのValidityとを独立して管理可能とすることである。

本発明の他の目的は、Authentication Result、Entitlement、Policy、Security Contextその他の情報をPolicy Evaluationに利用し、その結果に基づいてAuthorization Decisionを生成することである。

本発明の他の目的は、Authorization DecisionとService Executionとの間にEnforcementを設け、Authorization Decisionを実際のサービス実行に適用することである。

# 【課題を解決するための手段】

本発明の一態様に係る情報処理システムは、Authenticationを実行するAuthentication処理部と、Authentication Resultを生成または取得する処理部と、Authentication Resultとは独立して管理され得るEntitlementを取得、生成または確認するEntitlement処理部と、Authentication Result、Entitlementおよびその他のContext情報の少なくとも一つに基づいてPolicy Evaluationを実行するPolicy Evaluation処理部と、Policy Evaluationの結果に基づいてAuthorization Decisionを生成するAuthorization処理部と、Authorization Decisionに基づいてEnforcementを実行するEnforcement処理部と、Enforcementに基づいて保護対象のService Executionを制御するService Execution処理部と、を備える。

Authentication ResultとEntitlementとは同一のObjectである必要はない。また、Authentication ObjectのValidityとEntitlementのValidityとは異なる期間として管理することができる。

# 【発明の効果】

AuthenticationとEntitlementとを異なる概念として管理できる。

Authentication ObjectのValidityとEntitlementのValidityとを独立して管理できる。

Authentication Result、Entitlement、Policyその他のContextをPolicy Evaluationに利用し、その結果としてAuthorization Decisionを生成できる。

Authorization DecisionとEnforcementとを分離することにより、Decisionと実際のService Executionとの間に制御境界を設けることができる。

あるServiceで成立したEntitlementを別のServiceにおけるAuthorizationに利用することもできる。

# 【図面の簡単な説明】

【図1】本発明の全体的なProtocol Architectureを示す図である。  
【図2】Authentication、Entitlement、Policy EvaluationおよびAuthorizationの関係を示す図である。  
【図3】Entitlement ObjectおよびそのLifecycleを示す図である。  
【図4】Security ContextのAssembly、Policy EvaluationおよびAuthorization Decisionの関係を示す図である。  
【図5】Authorization Decisionを保護対象OperationにBindingし、Service Executionへ適用する関係を示す図である。  
【図6】Authorization StateのInvalidation、RevocationおよびFail-Closedによる伝播制御の関係を示す図である。  
【図7】Distributed AuthorizationにおけるConsistencyの関係を示す図である。

# 【発明を実施するための形態】

## 1. 全体構成

本発明の基本的な処理連鎖は、Authentication → Authentication Result → Entitlement → Policy Evaluation → Authorization Evaluation → Authorization Decision → Enforcement → Service Executionとして表すことができる。

各処理は必ずしも一つの装置またはサーバに配置される必要はなく、複数のサーバ、サービス、API、データベース、端末その他に分散して実装できる。

EntitlementはAuthenticationの直後に生成される必要はなく、異なる時点、処理またはServiceにおいて生成、取得、更新、停止または確認されてもよい。

## 2. Authentication

Authenticationは、主体、利用者、装置、アプリケーションその他について所定の確認処理を行うことをいう。方式は特定されず、パスワード、ワンタイムパスワード、QR Code、公開鍵暗号、WebAuthn、Passkey、生体認証、端末認証、証明書その他を利用できる。

本発明で重要なのは、Authenticationの具体的方式そのものではなく、Authenticationによって得られるAuthentication ResultをEntitlementその他の情報と区別して扱えることである。

## 3. Authentication Object

Authenticationに使用される情報をAuthentication Objectとすることができる。識別情報、Token、Challenge、Response、QR Codeその他を含めることができ、一時的Objectとして生成できる。

Authentication ObjectにはValidity Period、Expiration、One-time Useその他の利用条件を設定できる。

## 4. Authentication ObjectのValidity

Authentication Objectは所定のValidity Periodを有することができる。Validity PeriodはAuthentication ObjectがAuthenticationに使用可能な期間を示す。

Web画面に表示されるQR CodeをAuthentication Objectとして使用する場合、生成時点から所定時間だけ有効とすることができる。この所定時間は30秒に限定されず、数秒、10秒、30秒、1分、5分その他の任意の期間を設定できる。

## 5. Authentication ObjectのExpiration

Authentication ObjectにはExpirationを設定できる。ExpirationはAuthentication Objectが使用できなくなる時点を示す。ExpirationしたObjectによるAuthenticationを拒否できる。Authentication ObjectのExpirationはEntitlementのExpirationとは独立して管理できる。

## 6. Authentication ValidityとEntitlement Validity

Authentication ObjectのValidityとEntitlementのValidityとは異なる概念である。

例えばAuthentication ObjectのValidity Periodを30秒とし、Authenticationによって確認された事実に基づくEntitlementを数時間有効とすることができる。

Authentication Objectが30秒後に使用不能となっても、既に生成されたEntitlementのValidityが当然に終了するわけではない。反対に、Entitlementが失効しても、そのことだけによって過去のAuthentication ObjectのValidityが延長されることはない。

## 7. Authentication Freshness

Authentication ResultにはFreshness条件を設定できる。Policyは所定のFreshnessを要求でき、条件を満たさない場合には再Authentication、再Validation、Deny、Indeterminateその他の処理を実行できる。

## 8. Replay防止

Authentication ObjectにはReplay防止条件を設定できる。例えば一回のみ使用可能とし、Authentication成功後に同一Objectを再使用できないようにできる。Replay防止はAuthentication ObjectのValidityとは別個の条件として管理できる。

## 9. Entitlement

Entitlementは、主体、対象、Service、資源、取引その他について、所定の権利、資格、状態または事実が成立していることを示す情報である。

EntitlementはAuthentication Resultとは異なるObjectとして管理できる。Authentication完了時に必ず生成される必要はなく、異なる時点、処理またはServiceで生成、取得、更新、停止または確認できる。

## 10. Authentication ResultとEntitlementの独立性

Authentication ResultとEntitlementとを独立した情報として扱うことができる。

Authenticationによって本人であることが確認されても、所定のEntitlementを有していない場合には特定のService Executionを許可しないことができる。

反対に、過去のAuthentication等によって成立した事実に基づくEntitlementを保持している場合、そのEntitlementを現在のPolicy Evaluationに利用できる。

## 11. Conditional Entitlement

Entitlementは所定の条件が成立した場合に生成、付与または有効化できる。条件としてAuthentication Result、取引結果、位置情報、時刻、購入履歴、会員状態、サービス利用履歴その他のContextを利用できる。

## 12. Cross-Service Entitlement

あるServiceで生成または確認されたEntitlementを、異なるServiceにおけるPolicy EvaluationまたはAuthorizationに利用できる。

例えば店舗Serviceで来店を確認し、「来店済み」というEntitlementをEC Serviceで取得し、そのEntitlementに基づいて割引のAuthorization Decisionを生成できる。この場合、店舗ServiceにおけるAuthenticationとEC ServiceにおけるAuthorizationとは同一の処理ではない。

## 13. Policy

Policyは、Authentication Result、Entitlement、Subject、Resource、Action、Security Context、Transaction、時刻その他の情報に基づいて所定の判断を行うための規則である。Serviceごとに異なるPolicyとして管理できる。

## 14. Policy Evaluation

Policy EvaluationはPolicyに基づいて入力情報を評価する処理である。入力にはAuthentication Result、Entitlement、Security Context、Transactionその他を含めることができる。結果はAuthorization EvaluationまたはAuthorization Decisionの生成に利用できる。

## 15. Authorization Evaluation

Authorization EvaluationはPolicy Evaluationその他の必要な情報に基づいて、所定のActionまたはService Executionが許可されるべきかを評価する処理である。EntitlementのState、Validity、Expiration、Revocation、Security Context、Transactionその他を評価できる。

## 16. Authorization Decision

Authorization DecisionはAuthorization Evaluationの結果として生成される判断情報であり、Permit、DenyまたはIndeterminateを含むことができる。

Authorization DecisionはPolicyそのものでもPolicy Evaluationそのものでもなく、Policy Evaluation等によって得られた結果をサービス実行に適用するための判断情報として管理できる。

## 17. Permit、DenyおよびIndeterminate

Permitは所定のActionまたはService Executionについて実行を許可するDecisionである。Permit生成後でもEnforcementにおいて追加条件を確認できる。

Denyは実行を許可しないDecisionであり、Enforcementは対応するService Executionを拒否または停止できる。

Indeterminateは必要な情報または条件を確定できずPermitまたはDenyを確定できない状態を示し、Policyに応じて再Authentication、Revalidation、再Evaluation、Deny、処理停止その他を実行できる。

## 18. Enforcement

EnforcementはAuthorization Decisionを実際のService Executionに適用する処理である。Service Executionの許可、拒否、停止、条件付き許可、処理内容変更その他の制御を行うことができる。

Authorization DecisionとEnforcementとは別個の処理として管理できる。

## 19. Service Execution

Service Executionは保護対象となるサービス処理の実行であり、情報取得、情報変更、Resourceへのアクセス、取引処理、購入処理、割引処理その他を含めることができる。

Service ExecutionはAuthorization DecisionおよびEnforcementを経て実行可能な状態となる。

## 20. Security Context

Policy EvaluationおよびAuthorization EvaluationではSecurity Contextを利用できる。時刻、位置、端末状態、ネットワーク状態、セッション状態、利用環境その他を含めることができる。

## 21. Transaction

TransactionはAuthentication、Entitlement、AuthorizationまたはService Executionに関連する一連の処理を識別または管理するために利用できる。識別子、時刻、状態、関連Objectその他を関連付けることができる。

## 22. Object State

Authentication Object、Entitlementその他のProtocol ObjectにはStateを設定できる。Active、Expired、Revoked、Consumed、Suspendedその他を含めることができ、Object StateはAuthorization Decisionそのものとは異なる情報として管理できる。

## 23. Revocation

Entitlementその他のObjectはValidity Periodの終了とは独立してRevocationされ得る。RevocationされたObjectはPolicyに従ってAuthorization Evaluationにおいて無効なObjectとして扱うことができる。

## 24. Revalidation

必要に応じてAuthentication Result、Entitlementその他の情報を再Validationできる。成功しない場合、Policyに従ってDeny、Indeterminate、再Authenticationその他を実行できる。

## 25. Fail-Closed

必要な条件を確認できない場合、Policyに従ってService Executionを許可しない構成とできる。例えば必要なEntitlementのValidityを確認できない場合、Authorization DecisionをPermitとせずDenyまたはIndeterminateとして扱うことができる。

## 26. 基本処理モデル

一実施形態として、Authenticationを実行し、Authentication Resultを取得し、Authentication ObjectのValidity等を確認し、Entitlementを取得、生成または確認し、EntitlementのStateおよびValidityを確認し、Authentication Result、Entitlementその他のContextを用いてPolicy Evaluationを実行し、Authorization Evaluationを実行し、Authorization Decisionを生成し、Authorization DecisionをEnforcementに適用し、Enforcementに基づいてService Executionを制御する。

上記処理順序は一実施形態であり、本発明はこれに限定されない。

## 27. 来店認証とECサービスの実施形態

店舗ServiceでAuthenticationを実行し、QR Codeを利用することができる。QR Codeを生成時点から30秒間だけ有効とすることができる。

Authentication成功後、店舗ServiceはAuthentication Resultを取得し、それに基づいて「来店済み」というEntitlementを生成または有効化できる。当該EntitlementはAuthentication Objectとは異なるObjectとして管理され、例えば数時間有効とできる。

その後、EC ServiceはEntitlementを取得してPolicy Evaluationを実行し、Entitlementの有効性、対象商品その他の条件を確認する。条件成立時にPermitを生成し、Enforcementが割引処理をService Executionとして実行可能とする。

## 28. QR Codeを利用する実施形態

Authentication ObjectとしてQR Codeを利用できる。QR CodeにはAuthenticationに必要な一時的情報を含めることができる。

QR CodeのValidity Periodは任意に設定でき、例えば30秒とできるが、本発明は30秒という数値に限定されない。Validity終了時にはAuthenticationを拒否できる。

QR CodeをOne-time Useとして管理することにより、同一QR CodeのReplayを防止できる。

## 29. 公開鍵暗号を利用する実施形態

Authenticationでは公開鍵暗号方式を利用できる。端末側に秘密鍵を保持し、サーバ側に対応する公開鍵を保持することができる。

ChallengeおよびResponseを利用して秘密鍵を保持する端末であることを確認できる。秘密鍵はSecure Enclave、Trusted Execution Environmentその他の安全性を備えた領域に保持できるが、特定の鍵保管方式に限定されない。

## 30. 分散システムへの適用

Authentication、Entitlement、Policy Evaluation、Authorization、EnforcementおよびService Executionは、異なるサーバ、Service、APIまたはネットワークノードに配置できる。

Authentication Service、Entitlement Service、Policy Service、Authorization ServiceおよびApplication Serviceを別々に配置し、API、メッセージ、Tokenその他で情報交換できる。

## 31. 時間的有効性の多層構造

時間に関するValidityを複数のObjectおよび処理について独立して設定できる。

例えば、Authentication Object Validity、Authentication Result Freshness、Entitlement Validity、Entitlement Expiration、Transaction Validity、Policy上の時間条件等を、それぞれ異なる条件として管理できる。

これにより、Authenticationの短時間ValidityとEntitlementの長時間Validityとを両立できる。

## 32. 一般化および変形例

本発明はQR Codeを利用するAuthentication、特定の認証方式、通信方式、データベース、サーバ構成、クラウドサービスまたはサービス業種に限定されない。

Authenticationによって確認された結果と、Authenticationとは独立して管理され得るEntitlementとを用いてPolicy Evaluationを行い、その結果に基づいてAuthorization Decisionを生成し、当該Authorization DecisionをEnforcementを介してService Executionに適用する構成であれば、様々な情報処理システムに適用できる。

Entitlementを一つのServiceから別のServiceに伝達でき、複数のEntitlementを同時にPolicy Evaluationに利用できる。

Authentication Result、Entitlement、Security ContextおよびTransactionの組合せによって異なるAuthorization Decisionを生成できる。

Authorization DecisionはPermit、DenyまたはIndeterminate以外の状態を含むことができる。

Enforcementは、Service Executionの単純な許可または拒否だけでなく、処理内容の変更、利用可能な機能の限定、価格または条件の変更その他の制御を行うことができる。

# 【産業上の利用可能性】

本発明は、Webサービス、ECサービス、会員サービス、店舗サービス、金融関連サービス、業務システム、APIサービス、クラウドサービス、モバイルサービスその他の情報処理サービスに利用できる。

また、複数のサービスを横断して権利情報または事実情報を利用する必要があるシステムに適用できる。

# 【Claim 1 Support確認】

本明細書は、Claim 1に含まれる以下の構成を支持する。

1. Authentication処理
2. Authentication Resultの生成または取得
3. Authentication Resultとは独立して管理され得るEntitlement
4. Authentication ResultとEntitlementとの別個の取扱い
5. Authentication ObjectのValidity
6. Authentication Object ValidityとEntitlement Validityとの独立性
7. Authentication Result、Entitlementその他の情報を用いるPolicy Evaluation
8. Policy Evaluationに基づくAuthorization Decision
9. Authorization Decisionに基づくEnforcement
10. Enforcementに基づくService Executionの制御

特に、Authentication ResultとEntitlementとの分離、およびAuthentication Object ValidityとEntitlement Validityとの独立性は、本発明の中心的な技術的関係として記載されている。

# 【従属Claim Support確認】

以下の従属Claim候補について、本明細書に対応する技術的説明を含む。

- Validity Period
- Expiration
- 異なるValidity Period
- QR Code
- Web表示QR Code
- QR CodeのValidity Period
- 30秒という具体例
- One-time Use
- Conditional Entitlement
- Cross-Service Entitlement
- Revocation
- Revalidation
- Fail-Closed
- 分散システム
- 公開鍵暗号
- 複数Entitlement
- Authorization Decisionの状態
- Enforcementによる各種制御

30秒はValidity Periodの一具体例であり、Validity Periodそのものを30秒に限定する趣旨ではない。

# 【弁理士による最終確認事項】

1. 発明の名称および法的表現
2. 背景技術および先行技術文献
3. Claim 1の新規性・進歩性
4. 従属Claimの新規性・進歩性
5. Claimと明細書とのSupport関係
6. 明確性
7. 実施可能要件
8. 新規事項の有無
9. 図面との対応
10. Version 1.0との関係および公開履歴
11. 先行技術との境界
12. 日本特許出願に適した法的表現および書式
13. 必要に応じたClaim Scopeの補正・再構成

# 【本ドラフトの位置付け】

本書は、NEW-shot2play Technical Specification Version 2.0を基礎として、日本特許出願用に再構成した弁理士レビュー用明細書ドラフトである。

正式な出願書類としての確定版ではなく、弁理士による法的・実務的レビューを経て必要な修正を行うことを前提とする。

Version 2.0 Technical Baselineである `patent/specification_v2.md` は本書作成のために変更しない。
