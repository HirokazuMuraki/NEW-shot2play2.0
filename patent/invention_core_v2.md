# NEW-shot2play Technical Specification Version 2.0
# Invention Core

## 1. 本文書の目的

本書は、NEW-shot2play Technical Specification Version 2.0に基づき、
本発明の技術的核心、構成要素、処理関係、技術的特徴および発明の
成立概念を整理するための基準文書である。

本書は、特許請求の範囲、発明の詳細な説明、実施形態、要約書その他の
特許出願関連文書を作成する際の上位概念として使用する。

本書は、技術仕様書の各Normative Requirementをそのまま特許請求項へ
変換することを目的とするものではない。

特許出願においては、本書により確定された発明の核心を基礎として、
特許法上の発明として成立する技術的構成、構成要件間の関係、作用、
効果および実施形態を再構成する。

---

## 2. 発明の名称

本発明は、認証結果とは独立して管理される権利情報をPolicyに基づいて
評価し、その評価結果に基づいてAuthorization Decisionを生成し、
当該DecisionをEnforcementを介してService Executionに結び付ける
情報処理システム、情報処理方法、情報処理装置およびプログラムに
関する。

発明の名称としては、出願時の請求項構成および対象範囲に応じて、
例えば以下の名称を検討することができる。

「権利情報に基づく認可制御を行う情報処理システム、情報処理方法、
情報処理装置およびプログラム」

または、

「認証、権利情報、Policy評価および認可決定に基づくサービス実行制御
システム、情報処理方法、情報処理装置およびプログラム」

最終的な発明の名称は、Phase 2以降の請求項構造を確認した上で確定する。

---

## 3. 発明の基本概念

本発明の基本概念は、Serviceへのアクセスを単純なAuthentication
結果だけによって決定するのではなく、Authenticationとは別個に
管理されるEntitlementを含む複数の情報をPolicyに基づいて評価し、
その評価結果からAuthorizationを決定し、決定されたAuthorizationを
EnforcementによってService Executionに適用することである。

本発明の基本的な処理関係は、次のように表現される。

Authentication
↓
Entitlement
↓
Policy Evaluation
↓
Authorization Evaluation
↓
Authorization Decision
↓
Enforcement
↓
Service Execution

ただし、この表現は必ずしも各処理が単純な直列処理としてのみ実行
されることを意味しない。

各処理は、Transaction、Security Context、Object State、Dependency、
Validity、Policyその他の条件に基づいて相互に関連付けられ得る。

重要なのは、各処理および各情報の意味を混同せず、それぞれ異なる
Protocol ObjectまたはProcessing Stageとして扱うことである。

---

## 4. 従来型アクセス制御との相違

従来のWebサービス等におけるアクセス制御では、Authenticationにより
主体を確認した後、その認証結果に基づいて直ちにServiceへのアクセス
可否を決定する構成が一般的である。

このような構成では、

「誰であるか」

と、

「現在の条件において何を実行する権利を有するか」

と、

「現在のPolicyにおいて当該権利を使用して当該Serviceを実行してよいか」

とを十分に分離できない場合がある。

本発明では、Authenticationによって確認される主体の識別または
認証状態と、当該主体が有するEntitlementを別個の概念として扱う。

さらに、EntitlementをPolicyその他の条件とともに評価し、その評価結果
からAuthorization Decisionを生成する。

これにより、Authenticationが成立していることと、特定のServiceを
実行することが許可されることとを同一視しない。

---

## 5. 発明の中心的特徴

本発明の中心的特徴は、少なくとも以下の構造を有することである。

### 5.1 AuthenticationとEntitlementの分離

Authenticationは、主体、Device、Credential、Authentication Object、
Challengeその他の情報に基づいてAuthenticationの成立を判断する
処理である。

これに対してEntitlementは、主体その他の対象に関連付けられた権利、
資格、利用資格、条件付き権利その他のService利用に関する情報であり、
Authenticationそのものとは異なるProtocol Objectとして管理される。

したがって、

Authenticationが成立している

ことと、

所定のEntitlementを有している

こととは、同一の条件ではない。

---

### 5.2 EntitlementとPolicyの分離

Entitlementは、それ自体がService Executionを直接許可するものとは
限らない。

Entitlementは、適用されるPolicy、Service、Resource、Action、
Context、時間その他の条件とともに評価され得る。

したがって、同一のEntitlementを有する主体であっても、適用される
PolicyまたはContextが異なることにより、異なるAuthorization Decision
が生成され得る。

---

### 5.3 Policy EvaluationとAuthorization Decisionの分離

Policy Evaluationは、適用されるPolicyおよび関連する情報を評価する
処理である。

Authorization Decisionは、Policy Evaluationその他の必要な評価結果に
基づき、ServiceまたはResourceに対するActionを許可するか否か等を
示すDecisionである。

したがって、Policyそのものと、Policyを評価した結果として生成される
Authorization Decisionとは別の概念として扱う。

---

### 5.4 AuthorizationとEnforcementの分離

Authorization Decisionは、Service Executionを許可または禁止する
判断情報である。

Enforcementは、そのAuthorization Decisionを実際のService Execution
に適用する処理である。

したがって、Authorization DecisionがPermitであることと、実際に
Service Executionが実行されたこととは同一ではない。

Enforcementにおいて必要な条件が満たされない場合、Permit Decisionが
存在していてもService Executionを実行しない構成が可能である。

---

### 5.5 EnforcementとService Executionの分離

Service Executionは、対象となるServiceまたはResourceに対して実際の
処理を実行する段階である。

Enforcementは、そのService Executionを実行可能な状態にするために、
Authorization Decisionその他の条件を適用する。

したがって、

Authorization Decision
≠
Enforcement
≠
Service Execution

として扱う。

この意味的分離により、Authorization Decisionの生成、Decisionの適用、
および実際のService処理を別々に検証、監査、制御することが可能になる。

---

## 6. Authentication

Authenticationは、Service利用等を要求する主体について、所定の
Authentication Mechanismに基づいて認証状態を成立させる処理である。

Authenticationは、例えば以下の情報を利用して実行することができる。

- Credential
- Public Key
- Private Keyに対応する認証情報
- Authentication Challenge
- Authentication Response
- QR Code
- Device情報
- Transaction情報
- Security Context
- Timestamp
- Nonce
- その他のAuthentication Object

Authenticationの具体的方式は、本発明の核心を特定の認証方式に限定
するものではない。

例えば、Public Key Cryptography、WebAuthn、Passkey、Device-bound
Credentialその他の方式を使用することができる。

---

## 7. Authentication Object

Authenticationに使用される情報は、Authentication Objectとして
扱うことができる。

Authentication Objectには、例えば以下が含まれ得る。

- Authentication Challenge
- QR Code
- Nonce
- One-time Token
- Authentication Transaction Identifier
- Device Identifier
- Credential Identifier
- Timestamp
- Expiration情報
- Security Context
- その他Authentication処理に必要な情報

Authentication Objectは、Authenticationの成立を目的として生成、
伝達、検証または消費され得る。

Authentication Objectは、必要に応じて一回限り使用可能なObjectとして
構成することができる。

---

## 8. QR Codeを利用したAuthenticationの一実施形態

本発明の一実施形態では、Serviceを提供するWeb側にAuthentication用の
QR Codeを表示し、Userが使用するMobile Device等によって当該QR Codeを
読み取る構成を採用することができる。

QR Codeには、Authentication Transactionに関連する情報を含めること
ができる。

例えば、QR Codeに以下の情報またはそれらを特定する情報を含めること
ができる。

- Transaction Identifier
- Challenge
- Nonce
- One-time Token
- Public Keyその他のCredential関連情報
- Expiration情報
- その他のAuthentication Context

Mobile DeviceはQR Codeを読み取り、所定のCredentialまたはPrivate Key
に基づく処理を行い、Authentication Responseを生成することができる。

ServerはAuthentication Responseを検証し、対応するAuthentication
TransactionについてAuthenticationを成立させることができる。

---

## 9. Authenticationの一時性

Authenticationに使用される一部の情報は、一時的なValidityを有する
Objectとして構成することができる。

例えば、Web画面に表示されるQR Codeは、生成時点から所定のValidity
Periodを有するAuthentication Objectとすることができる。

Validity Periodを経過したQR Codeは、Authenticationに使用することが
できない。

この構成により、過去に表示されたAuthentication Objectを後から使用
することによるReplayを防止または抑制することができる。

---

## 10. Authentication Validity Period

Authentication Objectには、必要に応じてValidity Periodを設定する
ことができる。

Validity Periodは、Authentication ObjectがAuthenticationの成立に
使用可能な期間を示す。

Authentication ObjectについてValidity Periodが設定されている場合、
その期間を経過したObjectはExpiredとして扱うことができる。

ExpiredとなったAuthentication Objectについては、Authenticationを
成立させてはならない構成とすることができる。

Validity Periodは固定値に限定されない。

例えば、

- 数秒
- 数十秒
- 数分
- その他の設定された時間

をValidity Periodとして使用することができる。

例えばQR CodeのValidity Periodを30秒に設定することができるが、
30秒という値自体は本発明の構成を限定するものではない。

---

## 11. Authentication Expiration

Authentication ObjectについてExpirationが設定されている場合、
Expiration後のAuthentication Objectは無効なObjectとして扱われる。

Expiration後に受信されたAuthentication RequestまたはAuthentication
Responseについては、対応するAuthentication Transactionを成立させ
ないことができる。

また、Authentication ObjectのExpirationは、Authentication Transaction
の状態変更、Security Contextの評価、Replay検出その他のSecurity
Controlと組み合わせることができる。

Authentication ObjectのExpirationとEntitlementのExpirationとは、
異なる対象に対する時間的制御である。

---

## 12. Authentication Freshness

Authenticationでは、Authentication ObjectまたはAuthentication
Responseが所定の時間的Freshness条件を満たしているかを評価する
ことができる。

Freshnessの評価には、例えば以下を使用することができる。

- Timestamp
- Nonce
- Challenge
- Transaction Identifier
- Generation Time
- Expiration Time
- Server Time
- その他の時間的またはTransaction上の情報

Freshness条件を満たさないAuthentication RequestまたはAuthentication
Responseについては、Authenticationを成立させないことができる。

FreshnessはValidity Periodと関連付けることができるが、両者は必ずしも
同一の概念ではない。

---

## 13. AuthenticationとReplay防止

Authentication Objectを一時的かつ一回限り使用可能なObjectとして
構成することにより、Replay Attackを防止または抑制することができる。

例えば、QR Codeに含まれるTokenまたはChallengeを一回のみ使用可能とし、
Authenticationの成立後に当該Objectを使用済みとして記録することが
できる。

この場合、以下の条件を組み合わせることができる。

- One-time Use
- Validity Period
- Expiration
- Nonce
- Challenge
- Transaction State
- Replay Detection

これらの条件は、Authenticationの安全性を確保するためのSecurity
Controlとして扱うことができる。

---

## 14. AuthenticationとEntitlementの時間的分離

AuthenticationにおけるValidity PeriodとEntitlementにおけるValidity
Periodは、異なる意味を持つ。

Authentication Validityは、

「特定のAuthentication ObjectまたはAuthentication Transactionを
現在のAuthentication処理に使用できるか」

を制御する。

一方、Entitlement Validityは、

「特定の主体または対象について、所定の権利が現在有効であるか」

を制御する。

したがって、Authentication ObjectのValidityが30秒である一方、
Authenticationによって確認された主体に関連するEntitlementが数時間、
数日またはその他の期間にわたって有効である構成が可能である。

Authentication ObjectがExpirationしたことは、必ずしもEntitlement
そのものがExpirationしたことを意味しない。

同様に、Entitlementが有効であることは、期限切れとなった
Authentication Objectを再利用できることを意味しない。

この分離は、本発明のProtocol Architectureにおける重要な意味的
分離の一つである。

---

## 15. Entitlement

Entitlementは、主体その他の対象に関連付けられた権利、資格、利用権、
条件付き利用権その他のServiceまたはResourceの利用に関する情報で
ある。

EntitlementはAuthentication Resultそのものではない。

Entitlementは、例えば以下の情報を含むことができる。

- Entitlement Identifier
- Subject Identifier
- Resource
- Service
- Action
- Scope
- Conditions
- Start Time
- Expiration Time
- Status
- Issuer
- Policy Reference
- Version
- その他のEntitlement Attribute

Entitlementは、Authenticationとは独立したObjectとして生成、保存、
更新、失効、停止または削除することができる。

---

## 16. Entitlementの発生

Entitlementは、所定の条件が成立した場合に生成することができる。

Entitlementの発生条件には、例えば以下を使用することができる。

- Authentication
- Registration
- Purchase
- Payment
- Membership
- Visit
- Check-in
- Event Participation
- Contract
- Administrative Grant
- External Systemからの通知
- その他の条件

AuthenticationはEntitlementの発生条件の一つとなり得るが、Authentication
が成立したことのみをもって、必ずしも所定のEntitlementが発生する
ものではない。

また、EntitlementはAuthenticationとは異なるLifecycleを有することが
できる。

---

## 17. 条件付きEntitlement

Entitlementは、単純な無条件の権利に限定されない。

Entitlementには、所定の条件が満たされた場合にのみ利用可能となる
条件付き権利を設定することができる。

条件には、例えば以下を使用することができる。

- Time
- Location
- Device
- Service
- Resource
- Action
- Membership Status
- Purchase Status
- Visit Status
- Security Context
- Transaction Context
- その他Policyに定義された条件

条件付きEntitlementは、Policy Evaluationにおいて評価することが
できる。

---

## 18. Entitlementの時間的有効性

Entitlementは、時間的なValidityを有することができる。

例えば、Entitlementについて、

- Effective Time
- Start Time
- Expiration Time
- Validity Period

等を設定することができる。

Entitlementが有効期間外である場合、当該EntitlementをAuthorization
Decisionの根拠として使用してはならない構成とすることができる。

Entitlementの時間的有効性はAuthentication ObjectのValidityとは
独立して管理される。

例えば、Authenticationに使用するQR CodeのValidity Periodが30秒で
あっても、Authenticationによって確認された主体に付与された
Entitlementは、別途設定された期間まで有効とすることができる。

---

## 19. Entitlementの状態

EntitlementはLifecycleを有するObjectとして扱うことができる。

Entitlementの状態には、例えば以下を含めることができる。

- Pending
- Active
- Suspended
- Expired
- Revoked
- Terminated

Entitlementの状態はAuthorization Evaluationにおいて評価することが
できる。

例えば、ExpiredまたはRevokedのEntitlementは、有効なEntitlement
としてAuthorization Decisionの根拠に使用してはならない。

---

## 20. Authentication Result

Authenticationの処理結果はAuthentication Resultとして扱うことが
できる。

Authentication Resultには、例えば以下を含めることができる。

- Authentication Status
- Subject Identifier
- Authentication Method
- Credential Reference
- Transaction Identifier
- Timestamp
- Security Context
- Authentication Assurance情報
- その他Authentication処理の結果を示す情報

Authentication Resultは、Authenticationが成立したことを示す情報で
あり、それ自体がEntitlementまたはAuthorization Decisionを意味する
ものではない。

Authentication ResultとEntitlementとAuthorization Decisionとは、
それぞれ異なるProtocol Objectとして扱うことができる。

---

## 21. AuthenticationとEntitlementの独立性

本発明では、Authentication ResultとEntitlementとを独立した情報として
管理することができる。

例えば、

Authentication Result:
「Subject AのAuthenticationが成立した」

Entitlement:
「Subject AはService Bについて所定の利用権を有する」

Authorization Decision:
「Subject Aは現在の条件においてService BのAction Cを実行してよい」

というように、それぞれ異なる意味を有する。

これらを独立して管理することにより、同一のAuthentication Resultから
異なるEntitlementまたは異なるPolicy条件に応じて異なるAuthorization
Decisionを生成することが可能となる。

---

## 22. Policy

Policyは、Entitlementその他の情報を評価し、ServiceまたはResourceに
対するActionの許可条件その他の制御条件を定める情報である。

Policyには、例えば以下の情報を含めることができる。

- Policy Identifier
- Policy Version
- Subject条件
- Entitlement条件
- Resource条件
- Action条件
- Time条件
- Location条件
- Device条件
- Security Context条件
- Transaction条件
- その他の条件

Policyは、単一の固定されたRuleに限定されず、複数の条件、Rule、
例外、優先順位その他の評価構造を有することができる。

---

## 23. Policy Evaluation

Policy Evaluationは、適用されるPolicyと、Authentication Result、
Entitlement、Service、Resource、Action、Security Context、Transaction
その他の関連情報を評価する処理である。

Policy Evaluationでは、例えば以下を判断することができる。

- Policyが適用可能であるか
- Entitlementが有効であるか
- 所定の条件が成立しているか
- Security Contextが要求条件を満たすか
- Transactionが有効であるか
- ActionがPolicyの対象となっているか
- その他のPolicy条件が成立しているか

Policy Evaluationの結果は、Authorization Evaluationまたは
Authorization Decisionの生成に使用することができる。

---

## 24. Authentication ResultとPolicy Evaluation

Policy Evaluationは、Authentication Resultのみを入力として実行する
ものに限定されない。

Policy Evaluationは、Authentication Resultに加えてEntitlementその他の
情報を評価することができる。

したがって、Authenticationが成立しているという事実だけでは、
必ずしも所定のService Executionが許可されるとは限らない。

例えば、

Authentication = 成立

であっても、

Entitlement = 存在しない

または、

Entitlement = Expired

または、

Policy Condition = 不成立

である場合には、Authorization DecisionとしてDenyまたはIndeterminate
を生成することができる。

---

## 25. Authorization Evaluation

Authorization Evaluationは、Policy Evaluationその他の評価結果に
基づいて、要求されたActionについてAuthorization Decisionを生成する
ための評価処理である。

Authorization Evaluationでは、例えば以下を評価することができる。

- Authentication Result
- Entitlement
- Policy Evaluation Result
- Service
- Resource
- Action
- Security Context
- Transaction Context
- Object State
- Time
- その他の必要な情報

Authorization Evaluationは、Service Executionそのものではない。

---

## 26. Authorization Decision

Authorization Decisionは、Authorization Evaluationの結果として
生成される判断情報である。

Authorization Decisionには、例えば以下の結果を使用することができる。

- Permit
- Deny
- Indeterminate
- Not Applicable
- その他のPolicyで定義されたDecision

Authorization Decisionには、Decision Identifier、Transaction
Identifier、Policy Reference、Timestamp、Validityその他の情報を
関連付けることができる。

Authorization DecisionはService Executionを直接意味するものではない。

---

## 27. Permit

Permitは、所定の条件の下で対象Actionの実行を許可するDecisionである。

ただし、Permitが生成された場合であっても、Enforcementにおいて必要な
条件が満たされない場合には、Service Executionを実行しないことが
できる。

例えば、Permit生成後に、

- Security Contextが変化した
- EntitlementがRevokedされた
- TransactionがExpiredした
- Required conditionが失われた
- Replayが検出された
- Integrity failureが発生した

等の場合には、EnforcementによってService Executionを拒否または
停止することができる。

---

## 28. Deny

Denyは、対象Actionの実行を許可しないDecisionである。

Denyが生成された場合、適用されるEnforcementはService Executionを
実行してはならない。

ただし、Error、Failure、Recoveryその他の処理は、Authorization Decision
そのものとは別のProcessingとして扱う。

---

## 29. Indeterminate

Indeterminateは、必要な情報または条件を確定できないこと等により、
PermitまたはDenyを確定できない状態を示すDecisionとして扱うことが
できる。

Indeterminateの場合、適用されるPolicyに応じて、

- Deny
- Revalidation
- Recovery
- Termination
- Fail-Closed

その他の処理を実行することができる。

IndeterminateをPermitとして扱う場合には、当該処理が適用されるPolicy
その他のNormative Requirementに適合している必要がある。

---

## 30. Enforcement

Enforcementは、Authorization Decisionその他の必要な条件を
Service Executionに適用する処理である。

Enforcementでは、少なくとも適用されるAuthorization Decisionの内容と、
そのDecisionを現在適用可能であるかを確認することができる。

Enforcementは、Authorization Decisionを単に記録するだけの処理ではない。

Enforcementは、許可されたActionのみがService Executionに到達する
ように制御することができる。

---

## 31. Service Execution

Service Executionは、Authorization DecisionおよびEnforcementを経て、
実際にService、ResourceまたはActionを実行する処理である。

Service Executionには、例えば以下が含まれ得る。

- Web Serviceへのアクセス
- API呼出し
- Resource取得
- Resource変更
- データ処理
- コンテンツ提供
- 決済処理
- 特典提供
- ECサイト上の割引適用
- その他の保護対象Service

Service Executionは、Authentication Resultだけによって直接許可される
ものではなく、適用されるPolicyおよびAuthorization Decisionに基づいて
Enforcementされた結果として実行され得る。

---

## 32. 本発明の基本的処理モデル

本発明の基本的処理モデルは、次のように整理することができる。

1. AuthenticationによりSubject等のAuthentication状態を確認する。
2. Authenticationに使用された一時的Object等のValidityを確認する。
3. Subject等に関連付けられたEntitlementを取得または確認する。
4. Entitlementの状態およびValidityを確認する。
5. 適用されるPolicyを特定する。
6. Authentication Result、Entitlement、Policyその他のContextを
   Policy Evaluationにより評価する。
7. Authorization Evaluationを実行する。
8. Authorization Decisionを生成する。
9. Authorization Decisionその他の条件をEnforcementに適用する。
10. 条件が満たされている場合にService Executionを実行する。
11. 必要に応じて処理結果、EvidenceおよびAudit情報を生成する。

これらの処理は、必ずしも単一の装置上で実行される必要はない。

複数のServer、Device、Service、Policy Engine、Authorization
Componentその他のProcessing Componentに分散して実行することが
できる。

---

## 33. 時間的有効性の多層構造

本発明では、時間という共通の属性を複数のProtocol Objectまたは
Processing Stageに対して独立して適用することができる。

例えば、

Authentication Object
→ 数十秒のValidity

Entitlement
→ 数時間のValidity

Policy
→ 特定期間に適用

Authorization Decision
→ 所定期間または所定条件まで有効

Transaction
→ 所定時間内に完了

Session
→ 所定時間内に維持

という構成が可能である。

これらのValidityは、単一の共通Expirationとして扱う必要はない。

各ObjectまたはProcessing Stageについて、その意味に応じたValidity
およびExpirationを設定することができる。

---

## 34. 来店認証とECサービスの一実施形態

本発明の一実施形態として、店舗における来店認証とECサイトにおける
サービス利用を組み合わせることができる。

例えば、Userが店舗に来店し、店舗に設置されたQR Code等を利用して
Authenticationを行う。

Authenticationが成立すると、所定の条件に基づいて、

「当該Userが店舗に来店した」

という事実に対応するEntitlementを生成または有効化することができる。

このEntitlementに対して、例えば、

「来店後24時間以内にECサイトで商品を購入する場合、所定の割引を
適用する」

というPolicyを設定することができる。

Userが後にECサイトへアクセスした場合、ECサイト側でAuthenticationを
実行した上で、当該Userに関連するEntitlementおよびPolicyを評価する
ことができる。

その結果、Authorization DecisionとしてPermitが生成され、
Enforcementによって割引処理をService Executionへ適用することが
できる。

この場合、

店舗におけるAuthentication
≠
来店Entitlement
≠
ECサイトにおけるAuthorization Decision
≠
割引Service Execution

である。

この分離によって、あるServiceにおいて成立した事実または権利を、
別のServiceにおけるPolicy条件として利用することが可能となる。

---

## 35. Authenticationと異なるServiceにおけるEntitlement利用

本発明では、Entitlementを生成したServiceと、Entitlementを利用して
Authorizationを決定するServiceとが異なる構成を採用することができる。

例えば、

Service A:
来店Authentication

↓
Entitlement:
来店権利情報

↓

Service B:
ECサイト

↓

Policy Evaluation

↓

Authorization Decision

↓

Discount Service Execution

という構成が可能である。

この構成により、Authentication、Entitlement、Policy Evaluation、
AuthorizationおよびService Executionを異なるServiceまたはProcessing
Componentに分離することができる。

---

## 36. Entitlementを利用したCross-Service Authorization

本発明では、あるServiceで取得または生成された事実、資格または権利を
Entitlementとして管理し、別のServiceにおけるAuthorizationの入力
として利用することができる。

この場合、Authenticationを単純に別Serviceへ転送するのではなく、
Authenticationに関連して生成または確認されたEntitlementを、別の
ServiceにおけるPolicy Evaluationの対象として利用することができる。

これにより、異なるService間で権利情報を利用した条件付きアクセス
制御を実現することができる。

---

## 37. Security Context

本発明では、Authorizationの判断においてSecurity Contextを利用する
ことができる。

Security Contextには、例えば以下の情報を含めることができる。

- Authentication Status
- Credential情報
- Device情報
- Transaction情報
- Session情報
- Network情報
- Location情報
- Time情報
- Integrity Status
- Risk情報
- その他Security Controlに関連する情報

Security Contextは、AuthenticationだけでなくPolicy Evaluation、
Authorization EvaluationおよびEnforcementにおいて利用することが
できる。

---

## 38. Transaction

本発明におけるTransactionは、複数のProcessing StageまたはProtocol
Objectを関連付ける処理単位として扱うことができる。

Transactionには、例えば以下を関連付けることができる。

- Transaction Identifier
- Authentication
- Entitlement
- Policy Evaluation
- Authorization Evaluation
- Authorization Decision
- Enforcement
- Service Execution
- Result
- Evidence
- Audit

Transactionの状態は、Processingの進行に応じて変化することができる。

---

## 39. Object State

Protocol Objectは、そのLifecycleに応じたStateを有することができる。

例えば、

Authentication Object:
Generated → Active → Consumed / Expired

Entitlement:
Pending → Active → Suspended / Expired / Revoked

Authorization Decision:
Created → Valid → Applied / Expired / Revoked

Transaction:
Created → Processing → Completed / Failed / Terminated

等のStateを使用することができる。

Object Stateは、Authorization EvaluationおよびEnforcementにおける
判断条件として使用することができる。

---

## 40. StateとAuthorizationの分離

Object Stateは、それ自体がAuthorization Decisionではない。

例えば、EntitlementがActiveであることは、Authorization Evaluationに
おいてPermitの条件となり得るが、ActiveというStateそのものがPermit
Decisionを意味するものではない。

同様に、Authentication ObjectがValidであることはAuthenticationに
使用可能であることを示すが、Service Executionの許可そのものを意味
しない。

この意味的分離を維持することにより、各処理段階の責任およびSecurity
Controlを明確にすることができる。

---

## 41. Evidence

本発明では、Processingに関するEvidenceを生成または保存することが
できる。

Evidenceには、例えば以下を含めることができる。

- Authentication Result
- Entitlement Reference
- Policy Reference
- Authorization Decision
- Transaction Identifier
- Timestamp
- Object State
- Processing Result
- Security Context
- その他処理を説明する情報

Evidenceは、実際のProcessingまたはDecisionそのものとは異なる。

例えば、

Authorization Decisionを示すEvidence

と、

Authorization Decisionそのもの

とは別のObjectとして扱うことができる。

---

## 42. Audit

本発明では、Authentication、Entitlement、Policy Evaluation、
Authorization、EnforcementおよびService Executionに関するAudit情報を
記録することができる。

Auditには、例えば以下を記録することができる。

- Processing開始
- Processing終了
- Authentication結果
- Entitlement状態
- Policy Evaluation結果
- Authorization Decision
- Enforcement結果
- Service Execution結果
- Error
- Failure
- Recovery
- Timestamp
- Processing Component

Audit情報は、処理そのもの、DecisionそのものまたはProtocol Object
そのものとは区別して扱う。

---

## 43. 発明の技術的効果

本発明の構成により、AuthenticationとAuthorizationを同一視する
ことなく、Authentication、Entitlement、Policy Evaluation、
Authorization Decision、EnforcementおよびService Executionを
それぞれ意味的に分離して管理することができる。

これにより、例えば以下の効果を得ることができる。

### 43.1 権利情報の独立管理

Authenticationとは独立してEntitlementを生成、保存、更新、停止、
失効および期限管理することができる。

### 43.2 条件付きサービス利用

EntitlementとPolicyを組み合わせることにより、時間、場所、購入、
来店、会員状態その他の条件に応じたService利用制御を実現できる。

### 43.3 Cross-Service利用

あるServiceにおいて成立した事実または権利をEntitlementとして管理し、
別のServiceにおけるAuthorizationの条件として利用できる。

### 43.4 Security Controlの分離

Authentication ObjectのExpiration、EntitlementのExpiration、
Authorization DecisionのValidity、TransactionのValidity等を独立して
管理できる。

### 43.5 ReplayおよびExpired Objectへの対策

Authentication ObjectにValidity PeriodおよびOne-time Use等の条件を
設定することで、期限切れObjectまたは再利用Objectによる不正な
Authenticationを防止または抑制できる。

### 43.6 Authorization Decisionと実行の分離

Authorization Decisionを生成した後、そのDecisionをEnforcementにより
実際のService Executionへ適用するため、DecisionとExecutionを独立
して制御できる。

### 43.7 監査可能性

AuthenticationからService Executionまでの各Processing Stageについて、
EvidenceおよびAuditを独立して記録できる。

---

## 44. 発明の核心

本発明の核心は、単にAuthenticationを実行することではない。

また、単にEntitlementを管理することでもない。

さらに、単にPolicyに基づいてAuthorizationを決定することでもない。

本発明の核心は、

Authenticationによって確認されるAuthentication状態と、
主体等に関連付けられたEntitlementとを分離し、

当該Entitlementおよびその他のContextをPolicyに基づいて評価し、

その評価結果に基づいてAuthorization Decisionを生成し、

当該Authorization DecisionをEnforcementによってService Executionに
適用する、

という一連の意味的に分離された処理構造を、単一のアクセス制御判断
としてではなく、複数の独立したProtocol ObjectおよびProcessing Stage
として構成する点にある。

特に、Authentication、Entitlement、Policy Evaluation、Authorization
DecisionおよびService Executionを異なる意味を有する情報および処理
として扱うことにより、Authenticationが成立しているという事実と、
特定のServiceを現在実行する権利が存在するという事実とを分離する。

---

## 45. 発明の一般化

本発明は、QR CodeによるAuthentication、Webサービス、ECサービス、
店舗サービスその他の特定用途に限定されない。

本発明の基本構造は、

Authentication
→ Entitlement
→ Policy Evaluation
→ Authorization
→ Enforcement
→ Service Execution

という関係を利用する任意の情報処理システムに適用することができる。

Authentication方式についても、QR Code、WebAuthn、Passkey、
Public Key Authentication、Device Authenticationその他の方式を
使用することができる。

Entitlementについても、来店、購入、会員資格、契約、決済、イベント
参加、ライセンス、利用権その他の権利情報を使用することができる。

Policyについても、時間、場所、Device、Service、Resource、Action、
Membership、Purchaseその他の条件を使用することができる。

---

## 46. 特許上の発明概念

特許出願においては、本発明を単一の具体的なAuthentication方式に
限定するのではなく、

「Authenticationとは独立して管理されるEntitlementをPolicyに基づいて
評価し、その結果に基づいてAuthorization Decisionを生成し、当該
DecisionをEnforcementを介してService Executionに適用する情報処理
構造」

として捉えることができる。

これにより、Authentication方式、Entitlementの具体的内容、
Policy条件、Serviceの種類その他の具体的実装に依存しない上位概念を
形成することができる。

一方、QR CodeのValidity Period、One-time Token、30秒のExpiration、
来店Entitlement、ECサイトでの割引その他の具体的構成は、実施形態、
従属的構成または具体的な実装例として展開することができる。

---

## 47. Phase 1における確定事項

Phase 1において、少なくとも以下を本発明の基本的構成として扱う。

1. AuthenticationとEntitlementは異なる概念である。
2. Authentication ResultはEntitlementそのものではない。
3. EntitlementはAuthenticationとは独立して管理され得る。
4. Entitlementは時間的Validityを有し得る。
5. Authentication Objectも時間的Validityを有し得る。
6. Authentication ObjectのValidityとEntitlementのValidityは異なる。
7. Authentication ObjectはExpiration後にAuthenticationに使用できない
   構成とすることができる。
8. Authentication ObjectはOne-time Useとして構成できる。
9. AuthenticationのFreshnessを評価できる。
10. EntitlementはPolicy Evaluationの入力となり得る。
11. Policy EvaluationとAuthorization Decisionは異なる処理である。
12. Authorization DecisionとEnforcementは異なる処理である。
13. EnforcementとService Executionは異なる処理である。
14. Authenticationの成立だけではService Executionが許可されるとは
    限らない。
15. Entitlementの存在だけではService Executionが許可されるとは
    限らない。
16. Authorization DecisionはPolicyおよびContextに依存し得る。
17. Authorization DecisionはService Executionそのものではない。
18. Authentication、Entitlement、Policy Evaluation、Authorization、
    EnforcementおよびService Executionは、異なるProtocol Objectまたは
    Processing Stageとして管理できる。
19. 異なるService間でEntitlementを利用したAuthorizationを実現できる。
20. 各Protocol ObjectおよびProcessing Stageは、それぞれ固有の
    Lifecycle、ValidityおよびSecurity Controlを有し得る。

---

## 48. Phase 2以降への引継ぎ

Phase 2では、本書により整理された発明概念を基礎として、特許請求の
範囲の骨格を設計する。

特許請求項の設計では、具体的なQR Codeや30秒という数値に過度に限定
されることを避けつつ、

Authentication
+
独立したEntitlement
+
Policy Evaluation
+
Authorization Decision
+
Enforcement
+
Service Execution

という構造的関係を中心に、独立請求項の成立可能性を検討する。

さらに、以下の具体的構成を従属請求項または実施形態として展開する
可能性を検討する。

- Authentication ObjectのValidity Period
- Authentication ObjectのExpiration
- One-time Authentication Object
- Replay防止
- Entitlementの時間的Validity
- 条件付きEntitlement
- Cross-Service Entitlement
- Security Context
- Transaction
- Authorization DecisionのValidity
- Fail-Closed
- Revalidation
- Evidence
- Audit
- StateおよびLifecycle
- QR Codeを利用したAuthentication
- 来店AuthenticationからEC ServiceへのEntitlement連携
- Entitlementに基づく条件付き割引その他のService Execution

Phase 2では、これらの要素のうち、どの構成を独立請求項に含めることが
発明の本質を最も適切に保護できるかを検討する。

---

## 49. 本書の位置付け

本書は、NEW-shot2play Technical Specification Version 2.0の
Normative Requirementsを参照しつつ、特許出願のために発明概念を
整理したものである。

Normative Requirementsに規定された個々の技術要件は、本書の発明概念を
具体化する実施形態または構成要件の候補として利用することができる。

ただし、Normative Requirementsの全てを特許請求項に取り込む必要はない。

特許出願では、本書により確定された発明の核心を中心として、先行技術
との差異、技術的課題、作用効果、実施可能性および権利範囲を考慮して
請求項および明細書を構成する。

---

## 50. Phase 1 Final Statement

本発明は、

「誰であるか」を確認するAuthenticationと、

「何をする権利を有するか」を示すEntitlementと、

「現在の条件において何を実行してよいか」を評価するPolicy Evaluation
およびAuthorizationと、

「その判断を実際のService Executionに適用する」Enforcementとを、

同一の概念として扱うのではなく、それぞれ独立した意味を有する
Protocol ObjectまたはProcessing Stageとして構成し、これらを関連付け
てService Executionを制御する情報処理技術である。

また、本発明ではAuthentication Object、Entitlement、Policy、
Authorization Decision、Transaction、Sessionその他の対象について、
それぞれの意味に応じたValidity、Expiration、StateおよびLifecycleを
独立して管理することができる。

特にAuthentication Objectの短時間のValidityとEntitlementの長時間の
Validityとを分離することで、AuthenticationのFreshnessおよびReplay
防止と、Service利用権の継続性とを独立して制御することができる。

したがって、本発明の中心的な技術思想は、

Authentication
↓
Entitlement
↓
Policy Evaluation
↓
Authorization Evaluation
↓
Authorization Decision
↓
Enforcement
↓
Service Execution

という処理関係を、各段階の意味的独立性、時間的有効性、状態、
依存関係およびSecurity Controlを維持しながら連携させることにある。

本書をPhase 1における発明核心の基準文書とし、Phase 2以降の特許請求
項および明細書の設計に使用する。

