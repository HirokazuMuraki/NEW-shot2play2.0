
NEW-shot2play Technical Specification
Version 1.0 / Version 2.0 技術的関係整理
Attorney Review Draft

対象: 日本特許出願
目的: Version 1.0とVersion 2.0との技術的関係を弁理士に説明するための資料
関連資料: NEW-shot2play Technical Specification Version 1.0 / Version 2.0

1. 本書の目的

本書は、NEW-shot2play Technical Specification Version 1.0とVersion 2.0との関係を整理し、日本特許出願に際して弁理士が両者の技術的関係を評価するための資料として提供することを目的とする。

本書は特許出願書類そのものではない。

また、Version 1.0が日本特許法上の先行技術に該当するか、Version 2.0の新規性または進歩性にどのような影響を与えるかについて、発明者側で法的な結論を示すものではない。

公開日、公開内容、技術的同一性または類似性、および特許法上の評価については、弁理士による確認を前提とする。

2. Version 1.0の位置付け

Version 1.0は、Version 2.0を作成する以前に構成されていたNEW-shot2playの技術仕様である。

Version 1.0は、Authenticationを中心とした技術構成および関連する技術的概念を整理した仕様として作成された。

Version 2.0の設計に際しては、Version 1.0における技術的知見および構成を基礎として検討を行った。

したがって、Version 1.0はVersion 2.0に至るまでの技術的・設計的な前段階として位置付けられる。

3. Version 2.0の位置付け

Version 2.0は、Version 1.0の単なる文章修正版または仕様書の改訂版として作成したものではない。

Version 2.0では、Authenticationそのものに加えて、Authentication後にサービスを利用するための権利または資格を独立したEntitlementとして扱う構成を導入している。

さらに、Authentication Result、Entitlement、Policy、Security Contextその他の情報をPolicy Evaluationに入力し、その評価結果に基づいてAuthorization Decisionを生成し、当該Authorization DecisionをEnforcementを介してService Executionに適用する多段階の処理構造を構成している。

Version 2.0の中心的な技術概念は、Authentication方式そのものではなく、Authentication後のサービス利用制御を含む一連の情報処理構造にある。

4. Version 1.0からVersion 2.0への主要な技術的拡張

Version 2.0では、少なくとも以下の技術的関係を明確に分離している。

Authentication
Authentication Result
Entitlement
Policy
Policy Evaluation
Authorization Evaluation
Authorization Decision
Enforcement
Service Execution

これらを一連の処理として構成しながら、それぞれを異なる情報または処理段階として扱う。

特に重要なのは、Authentication ResultとEntitlementを同一の情報として扱わず、Entitlementを独立した情報として管理可能とする点である。

5. Authentication ResultとEntitlement

Version 2.0では、Authenticationによって得られるAuthentication Resultと、その後のサービス利用に関するEntitlementとを分離する。

Authentication Resultは、Authentication処理によって得られる結果を表す。

一方、Entitlementは、Subjectが所定のService、ResourceまたはActionを利用するための権利、資格または条件を表す情報として扱う。

したがって、Authenticationが成立したことだけをもって、すべてのService Executionが許可されるものではない。

Service Executionについて許可を与えるためには、必要に応じてEntitlement、Policy、Security Contextその他の情報を評価することができる。

この構造はVersion 2.0の重要な技術的特徴の一つである。

6. Authentication Object ValidityとEntitlement Validity

Version 2.0では、Authentication ObjectのValidityとEntitlementのValidityを同一の時間的条件として扱わない。

Authentication Objectには、短時間のValidity Periodを設定することができる。

一方、Authenticationによって得られた結果を利用して生成または確認されたEntitlementは、Authentication Objectとは異なるLifecycleおよびValidityを有することができる。

例えば、Authentication ObjectのValidity Periodが短時間で終了した場合であっても、それだけを理由として既に生成されたEntitlementのValidityが当然に終了するとは限らない。

逆に、EntitlementのValidityが継続していることによって、過去のAuthentication ObjectのValidityが延長されることもない。

このように、Authentication ObjectとEntitlementについて異なるValidityおよびLifecycleを管理可能とする点は、Version 2.0において特に重要な構成関係である。

なお、30秒等の具体的な時間は一つの実施例にすぎず、本発明の本質を特定の秒数に限定するものではない。

7. Policy Evaluation

Version 2.0では、Policy Evaluationを独立した処理段階として扱う。

Policy Evaluationには、Authentication Result、Entitlement、Subject、Resource、Action、Security Context、Transactionその他の情報を入力することができる。

Policy Evaluationでは、Policyに基づいてこれらの情報を評価し、その結果をAuthorization EvaluationまたはAuthorization Decisionの生成に利用する。

したがって、Authentication Resultが存在することと、特定のService Executionが許可されることとの間に、Policy Evaluationという技術的処理段階を設けることができる。

8. Authorization Decision

Version 2.0では、Policy Evaluation等の結果に基づいてAuthorization Decisionを生成する。

Authorization Decisionは、例えばPermit、DenyまたはIndeterminateとして表現することができる。

Authorization DecisionはPolicyそのものでもなく、Policy Evaluationそのものでもない。

また、Authorization DecisionはService Executionそのものでもない。

これらを異なる情報および処理段階として扱うことで、認証結果、権利情報、ポリシー評価、認可判断およびサービス実行を分離して管理できる。

9. Enforcement

Version 2.0では、Authorization DecisionとService Executionとの間にEnforcementを設ける。

Enforcementは、Authorization Decisionを実際のService Executionに適用する処理として構成することができる。

Enforcementでは、Permit、Denyまたはその他のAuthorization Decisionに基づいて、Service Executionの許可、拒否、停止、条件付き許可その他の制御を行うことができる。

このため、Authorization Decisionの生成とService Executionの実行とは同一の処理ではない。

10. Service Execution

Version 2.0におけるService Executionは、保護対象となる実際のサービス処理を意味する。

例えば、情報取得、情報変更、Resourceへのアクセス、決済、割引、機能利用その他のサービス処理を含むことができる。

Service Executionは、Authorization DecisionおよびEnforcementを経て制御される。

したがって、Authenticationの成立からService Executionまでを単一の処理として扱うのではなく、複数の情報処理段階として構成することができる。

11. Version 1.0とVersion 2.0の関係

Version 1.0とVersion 2.0には技術的な連続性が存在する。

特にAuthentication、Authentication Object、QR Code、公開鍵暗号その他のAuthentication関連技術については、Version 2.0においても利用可能な構成として整理されている。

しかし、Version 2.0では、これらのAuthentication関連技術だけを発明の中心とするのではなく、その後段に位置するEntitlement、Policy Evaluation、Authorization Decision、EnforcementおよびService Executionを含む構成を明確化している。

したがって、Version 1.0に存在するAuthentication関連の技術的構成と、Version 2.0において新たに整理・拡張されたAuthentication後のサービス利用制御構造とは、技術的に区別して評価する必要がある。

12. Version 1.0を基礎としていることの意味

Version 2.0がVersion 1.0を技術的基礎としていることは、Version 2.0のすべての構成がVersion 1.0に開示されていることを意味しない。

Version 1.0から得られたAuthentication関連の技術的知見を利用しながら、その後段に新たな情報処理構造を構成することは可能である。

したがって、Version 1.0との関係を評価する際には、

共通する構成
Version 1.0に既に存在する構成
Version 2.0で新たに明確化された構成
Version 1.0には存在しない構成
これらを組み合わせた全体構造

を区別して検討することが望ましい。

13. 特許性評価における確認事項

弁理士には、Version 1.0との関係について、少なくとも以下を確認していただきたい。

Version 1.0の公開日を確認すること。
Version 1.0の公開時点における実際の公開内容を確認すること。
Version 2.0のClaim 1に記載された各構成がVersion 1.0に開示されているか確認すること。
Authentication ResultとEntitlementの分離がVersion 1.0に開示されているか確認すること。
Authentication Object ValidityとEntitlement Validityを独立して扱う構成がVersion 1.0に開示されているか確認すること。
EntitlementをPolicy Evaluationの入力として利用する構成がVersion 1.0に開示されているか確認すること。
Policy Evaluation、Authorization Decision、EnforcementおよびService Executionを一連の関係として構成する点がVersion 1.0に開示されているか確認すること。
上記構成の組合せがVersion 1.0から当業者に容易に導かれるものか確認すること。
14. Version 1.0の公開に関する確認

Version 1.0は公開Web上で閲覧可能な状態にある。

参照先:

https://shot2play.com/s2p-patent/docs/

この公開状態について、弁理士には以下を確認していただきたい。

実際の公開開始日
公開された文書の内容
公開形態
Version 2.0の出願との時間的関係
日本特許法上の新規性・進歩性への影響
必要に応じた出願戦略上の対応

本資料では、Version 1.0の公開がVersion 2.0の特許性を失わせる、または失わせないという結論を示さない。

15. Version 1.0をVersion 2.0の単なる旧版として扱わない理由

Version 1.0とVersion 2.0は、技術的連続性を有する一方、発明の焦点が異なる。

Version 1.0ではAuthenticationに関連する技術構成が中心となっている。

Version 2.0では、そのAuthenticationを入口として、

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

というサービス利用制御の構造を明確化している。

したがって、Version 2.0を単なるVersion 1.0の改訂版としてのみ評価するのではなく、Version 1.0を基礎として新たに構成された技術的発明として評価することが適切と考える。

最終的な法的評価については弁理士の判断を求める。

16. Version 2.0の中心的な技術的関係

Version 2.0において特に重要な関係は、以下のとおりである。

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

この構造において、各段階は意味的に区別される。

さらに、

Authentication Object Validity

と

Entitlement Validity

も独立して管理可能である。

これにより、Authenticationに使用された一時的な情報のValidityと、Authentication後に成立したサービス利用権のValidityとを同一のLifecycleとして扱う必要がなくなる。

この関係がVersion 2.0のClaim Architectureおよび明細書構成の重要な基礎となっている。

17. Version 1.0との関係に関する発明者側の基本認識

発明者側の基本認識は以下のとおりである。

Version 1.0を否定するものではない。

Version 1.0に含まれるAuthentication関連技術は、Version 2.0においても技術的基礎として利用可能である。

一方、Version 2.0では、Authentication後のサービス利用制御について、Entitlementを独立した情報として扱い、Policy Evaluation、Authorization Decision、EnforcementおよびService Executionへ接続する構造を新たに中心的な技術構成として整理している。

したがって、Version 1.0とVersion 2.0との共通部分だけでなく、Version 2.0における新たな構成関係およびその組合せについて評価していただきたい。

18. 出願時の取扱いについて弁理士に求める事項

日本特許出願に際しては、Version 1.0との関係について、以下の観点から最終判断をお願いしたい。

Version 1.0の公開内容を先行技術として評価する必要があるか。
Version 1.0とVersion 2.0との共通部分をどのように扱うべきか。
Version 2.0の独自性をどの構成関係に置くべきか。
Claim 1において、Authentication関連構成をどの範囲まで含めるべきか。
Entitlement、Policy Evaluation、Authorization Decision、EnforcementおよびService Executionの関係をClaim Scopeとしてどのように表現することが最も適切か。
Authentication Object ValidityとEntitlement Validityの独立性を独立Claimまたは従属Claimのどちらで保護することが適切か。
Version 1.0との関係を明細書中でどの程度説明することが適切か。
19. 技術側からの結論

Version 1.0は、Version 2.0に至る技術的・設計的な前段階として重要な資料である。

しかし、Version 2.0はVersion 1.0の単なる文章修正ではなく、Authentication後のサービス利用制御を含む新たな技術構造を中心として構成している。

特に、

Authentication ResultとEntitlementの分離
Authentication Object ValidityとEntitlement Validityの独立性
EntitlementをPolicy Evaluationの入力として利用する構造
Policy EvaluationとAuthorization Decisionの分離
Authorization DecisionとEnforcementの分離
EnforcementとService Executionの分離
これらを一連の処理構造として構成すること

について、Version 1.0との技術的差異を明確に評価していただきたい。

発明者側としては、このVersion 2.0の中心的な技術構造を維持した上で、日本特許出願として適切なClaim Scopeおよび明細書構成について弁理士の最終判断を求める。

20. 本書の位置付け

本書は、弁理士との技術的協議を円滑にするための発明者側資料である。

本書によってVersion 1.0またはVersion 2.0について法的な結論を確定するものではない。

特許性、新規性、進歩性、先行技術該当性、補正可能性、出願戦略その他の法的事項については、弁理士による最終判断を前提とする。

End of Document
