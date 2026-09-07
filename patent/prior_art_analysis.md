# NEW-shot2play Technical Specification Version 2.0
# Prior Art Analysis — Preliminary / Attorney Review Draft

## 1. Purpose and Status

本書は、NEW-shot2play Version 2.0について、これまでに行った先行技術調査・
暫定評価を弁理士レビュー用に整理するための内部資料である。

**重要：本書は最終的な新規性・進歩性の法的判断を示すものではない。**
最終判断は、弁理士・弁理士による調査結果、特許庁審査および引用文献の
具体的な開示内容に基づいて行う。

本Versionでは、既存の調査で確認した先行技術領域と、本発明との差異の
中心を整理することを目的とする。

---

## 2. Invention Core Used for Comparison

比較対象となる本発明の中心構造は、

Authentication
→ Authentication Result
→ Entitlement
→ Policy Evaluation
→ Authorization Decision
→ Enforcement
→ Service Execution

である。

特に重要な構成は以下である。

1. Authentication Result と Entitlement を独立した情報として扱う。
2. Authentication Object Validity と Entitlement Validity を独立して管理する。
3. Authentication Result / Entitlement / Policy / Context等をPolicy Evaluationに入力する。
4. Authorization DecisionをEnforcementと分離する。
5. Enforcementを介してService Executionを制御する。
6. Entitlementを異なるServiceへCross-Serviceで利用できる。
7. Conditional Entitlement、Security Context、Transaction、Object State等を
   Authorization判断に利用できる。

---

## 3. Previously Identified Prior-Art Domains

これまでの暫定調査では、少なくとも以下の技術領域が本発明との比較対象となる。

### 3.1 FIDO / WebAuthn / Passkey

公開鍵暗号方式によるAuthentication、Challenge / Response、端末側秘密鍵、
Authenticator等は既知の技術領域である。

したがって、これらのAuthentication mechanismそのものは本発明の中心的な
新規性主張としない。

本発明ではWebAuthn / Passkey等をAuthentication方式の一実施形態または
従属請求項のFallbackとして位置付ける。

### 3.2 QR Authentication / Temporary Authentication Objects

QR Code、One-time Token、短時間Validity、Expiration、Replay防止等は
既知のAuthentication技術領域である。

したがって「QR Codeを使うこと」や「短時間で失効すること」単独を本発明の
中心的差異とはしない。

本発明では、短時間有効なAuthentication Objectと、それによって成立・確認
されたEntitlementのLifecycleを独立して管理する点を重視する。

### 3.3 Access Control / Policy-based Authorization

PolicyによってSubject、Resource、Action等を評価し、Permit / Deny等の
Authorization結果を生成する技術は既知の領域である。

したがってPolicy-based Access Controlそのものを単独の発明核心とはしない。

本発明ではAuthentication Resultと独立管理されるEntitlementを含む複数の
ContextをPolicy Evaluationに入力し、Authorization Decision、Enforcement、
Service Executionまでを明確な処理境界として関連付ける。

### 3.4 OAuth / Device Authorization / Token-based Access

OAuth等におけるToken、Authorization、Access Token、Expiration等の
仕組みは既知の技術領域である。

本発明の比較上重要なのは、Authenticationに使用される一時的Objectの
Validityと、別途管理されるEntitlementのValidityを区別する構造である。

### 3.5 Entitlement / Rights Management

権利、資格、利用可能性等をEntitlementとして管理する技術は既知の領域である。

本発明では、EntitlementをAuthentication Resultの単なる別表現とせず、
Authenticationとは異なるLifecycleを持つ独立Objectとして扱い、Policy
Evaluation / Authorizationに利用する点を重視する。

### 3.6 Session / Token Expiration and Revocation

Session、Token、Credential等にValidity、Expiration、Revocationを設定する
技術は既知である。

本発明では、Authentication ObjectのValidity終了、Authentication Resultの
Freshness、Entitlement Validity、Entitlement Revocation、Object State等を
別々の条件として扱い、Policyに基づくAuthorization判断へ反映できる。

### 3.7 Distributed Authorization

複数Service・Server・APIにまたがるAuthorization Contextの伝達、
再構成、Consistency等は既知の技術領域である。

本発明ではCross-Service Entitlementを含め、あるServiceで成立・確認された
Entitlementを別ServiceのAuthorization条件として利用できる構成をFallback
として保持する。

### 3.8 Concrete Prior-Art References

以下は、上記の既知技術領域を具体的な文献・仕様書に結び付けるための代表例である。
これらはClaim 1の全構成を単一文献が開示することを意味するものではなく、
各技術領域との比較対象を明確にするために記載する。

| 文献番号等 | 文献名 | 日付情報 | 主な開示内容 | Claim 1との比較対象 | 本発明との差異・留意点 |
|---|---|---|---|---|---|
| US20080184336A1 | Policy resolution in an entitlement management system | Priority: 2007-01-29 / Publication: 2008-07-31 | Entitlement management、policy administration、policy decision point、policy enforcement point、分散環境でのpolicy-based enforcement | Entitlement、Policy Evaluation、Authorization、Enforcement | Entitlement / policy / enforcementの既知例。ただし本発明のAuthentication Resultとの独立管理およびAuthentication Object ValidityとEntitlement Validityの独立管理との関係は別途確認が必要 |
| US20140282831A1 | Dynamic policy-based entitlements from external data repositories | Priority: 2013-03-15 / Publication: 2014-09-18 | Context-based policy、entitlement request、policy decision point、外部データリポジトリを利用したentitlement評価 | Entitlement、Policy Evaluation、Context | Contextに基づくentitlement評価の既知例。本発明のAuthentication Resultとの分離およびValidityの独立管理との組合せは別途確認が必要 |
| US20230049227A1 | System and method for authenticating client devices communicating with an enterprise system | Filing: 2021-08-13 | PEP、PIP、PDP、entitlements database、client deviceの認証・権限評価、API invocationの許可 | Authentication、Entitlement、Policy / Authorization、Enforcement | Authentication、entitlement、policy enforcementの組合せを含む既知例。ただしClaim 1の全構成が単一文献に開示されるとの判断ではない |
| US20240305635A1 | System and method for authenticating client devices communicating with an enterprise system | Filing: 2024-05-16 / Publication: 2024-09-12 | PEP、PIP、PDP、entitlements database、client deviceの認証・権限評価、API invocationの許可 | Authentication、Entitlement、Policy / Authorization、Enforcement | 上記技術の具体的公開例。本発明の独立Validity構造との関係は別途比較が必要 |
| RFC 8628 | OAuth 2.0 Device Authorization Grant | August 2019 | 別端末のuser agentを用いたdevice authorization、device code / user code、verification URI、短時間のauthorization flow | OAuth / Device Authorization、Temporary Object | Device Authorizationおよび一時的codeの既知例。本発明のEntitlement lifecycleとの分離とは異なる |
| W3C WebAuthn Level 2 / Level 3 | Web Authentication: An API for accessing Public Key Credentials | Level 2 Recommendation: 2021-04-08 / Level 3 Candidate Recommendation Snapshot: 2026-05-26 | Public-key credentials、challenge-response、authenticatorを利用したWeb Authentication | Authentication、Public-key Authentication | Authentication mechanismの既知仕様。本発明のEntitlement、Policy Evaluation、Authorization、Enforcementの構成とは別の技術領域 |
| FIDO Alliance | Passkeys / Cross-Device Authentication | 公開仕様・技術情報 | 別端末からのQR等を利用したCross-Device Authentication、passkeyによる認証 | QR / Cross-Device Authentication、Authentication | Cross-device / QR authenticationの既知例。本発明のCross-Service Entitlementとは異なる概念 |

**文献情報についての注記：** 上表の日付・文献番号等は、今回確認できた公開情報に
基づく代表例である。日本国内での公開公報、同族公報、引用関係、優先権の連鎖、
具体的な請求項・段落・図面の対応については、弁理士がJ-PlatPat等を用いて一次資料を
確認した上で確定する。

---

## 4. Preliminary Difference Analysis

### 4.1 Authentication Result と Entitlement の分離

単独のAuthentication技術では、Authentication成功がそのままService Accessへ
結び付けられる構成が一般的に存在する。

本発明では、

Authentication Result
≠
Entitlement

として扱い、Authentication後もEntitlementを独立して保存、更新、停止、
失効、再有効化等できる。

この分離はClaim 1およびClaims 12–17の中心的構成である。

### 4.2 Independent Validity

Authentication ObjectのValidityとEntitlementのValidityを異なるLifecycle
として管理できる。

例えば、Authentication Objectを30秒有効とし、そこから成立した
Entitlementを数時間有効とすることができる。

30秒自体ではなく、**短時間Authentication Objectと長時間Entitlementの
独立性**が重要である。

### 4.3 Policy Evaluationへの複数Context入力

本発明ではAuthentication ResultとEntitlementに加えて、Subject、Resource、
Action、Security Context、Transaction、Object State、時間条件等を
Policy Evaluationの入力として利用できる。

これにより、単純な「認証成功＝許可」または「Entitlement保有＝許可」ではなく、
現在のContextに応じたAuthorization Decisionを生成できる。

### 4.4 Authorization Decision と Enforcement の分離

Authorization Decisionを生成する処理と、実際にService Executionを許可・拒否・
停止等するEnforcementを別処理として扱う。

この分離により、Decisionと実際のService Executionとの間に制御境界を設ける。

### 4.5 Cross-Service Entitlement

あるServiceでAuthenticationまたはその他の条件によって生成・確認された
Entitlementを、別のServiceのAuthorization条件として利用できる。

これは、AuthenticationとAuthorizationを同一Service・同一処理として扱う
必要がないことを明確にする。

---

## 5. Claim 1 — Preliminary Patentability Focus

Claim 1の比較上の中心は、個々の既知技術ではなく、少なくとも以下の
構成要素の組合せと相互関係にある。

- Authentication
- Authentication Result
- Authentication Resultとは独立して管理され得るEntitlement
- Policy Evaluation
- Authorization Decision
- Enforcement
- Service Execution
- Authentication ResultとEntitlementの別個の取扱い
- Authentication Object ValidityとEntitlement Validityの独立管理

したがって、先行技術調査では「Authentication」「Entitlement」「Policy」
「Authorization」等の各要素が個別に存在するかだけでなく、

**これらの構成がClaim 1の関係で一つの技術的構成として開示されているか**

を確認する必要がある。

---

## 6. Dependent-Claim Fallback Strategy

先行技術によりClaim 1の一部構成が問題となった場合にも、以下のFallbackを
保持する。

### Validity / Lifecycle
- Authentication Object Validity
- Expiration
- Entitlement Validity
- Independent Revocation
- Authentication Freshness
- One-time Use

### Authentication Mechanism
- QR Code
- Public-key Authentication
- WebAuthn
- Passkey
- Secure Enclave / Trusted Execution Environment

### Entitlement
- Conditional Entitlement
- Cross-Service Entitlement

### Authorization Context
- Subject
- Resource
- Action
- Security Context
- Transaction
- Object State

### Execution Boundary
- Authorization Decision
- Enforcement
- Service Execution

---

## 7. Prior-Art Risk Assessment

現時点での暫定評価は以下の通り。

| 技術領域 | 個別要素の既知性 | 本発明との比較上の重要点 |
|---|---|---|
| FIDO / WebAuthn / Passkey | 高い | Authentication mechanism自体は中心差異としない |
| QR / Temporary Token | 高い | Object ValidityとEntitlement Validityの独立性 |
| Policy-based Access Control | 高い | Entitlement / Contextを含む処理連鎖 |
| OAuth / Token Authorization | 高い | Authentication ObjectとEntitlementのLifecycle分離 |
| Entitlement / Rights Management | 高い | Authentication Resultとの独立性 |
| Expiration / Revocation | 高い | 複数Object / Resultの独立したValidity |
| Distributed Authorization | 高い | Cross-Service Entitlement / Context continuity |

### Preliminary conclusion

既知技術が個々の構成要素を開示している可能性は高い。

一方、現時点での本発明の評価上重要なのは、個々の要素の新規性だけではなく、

**Authentication ResultとEntitlementを独立管理し、両者のValidityを分離し、
Policy Evaluationを介してAuthorization Decisionを生成し、
Enforcementを介してService Executionへ適用する一連の構成**

が、単一の先行技術にどこまで開示されているか、また複数文献を組み合わせる
ことによって当業者に容易に想到されるか、という点である。

この点は最終的に弁理士による具体的引用文献ベースの評価を必要とする。

---

## 8. Attorney Review / Next Actions

弁理士には、既存の先行技術調査資料と本書を併用して、少なくとも以下を
確認してもらう。

1. Claim 1の全構成要件を単一文献が開示しているか。
2. 特にAuthentication ResultとEntitlementの独立性が開示されているか。
3. Authentication Object ValidityとEntitlement Validityの独立性が開示されているか。
4. Policy Evaluationへの複数Context入力が同一構成に存在するか。
5. Authorization DecisionとEnforcementの分離が開示されているか。
6. Cross-Service Entitlementが開示されているか。
7. 上記構成を複数文献から組み合わせることが容易か。
8. Claim 1が広すぎる場合、Claims 4–5、11–17、18–30等のFallbackをどの順序で
   使用するのが適切か。

---

## 9. Important Qualification

本書は、既存の技術領域およびこれまでの暫定評価を整理したものであり、
新規性・進歩性について最終的な法律判断を行うものではない。

具体的な特許番号、公開番号、引用箇所、priority date等については、
弁理士が確認する先行技術調査資料を一次資料として確定する。

本書では、確認できていない具体的文献情報を推測して追加しない。
