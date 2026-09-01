# NEW-shot2play Technical Specification Version 2.0
# Phase 4 — 従属請求項・Fallback Strategy

## 50. Fallback Structure — 第一段階

第一Fallbackは、Claim 1の中心構造を維持したまま、Entitlementの独立性を明確化する構成とする。

```text
Authentication Result
+
Independent Entitlement
↓
Policy Evaluation
↓
Authorization Decision
↓
Enforcement
↓
Service Execution
```

これは本発明の最も重要なFallbackの一つとする。

---

## 51. Fallback Structure — 第二段階

第二Fallbackでは、Claim 1において既に採用されているAuthentication ObjectのValidityとEntitlement Validityの分離を前提として、これらの時間的有効性をさらに具体化する構成を追加する。

```text
Authentication Object
    ↓
Short Validity
    ↓
Authentication Result

Independent Entitlement
    ↓
Longer Validity
    ↓
Policy Evaluation
```

Authentication ObjectのValidityとEntitlement Validityとは独立して管理され得るものとして扱い、例えばAuthentication Objectを短時間有効とし、Authenticationによって確認された結果に基づくEntitlementをより長期間有効とする構成を従属請求項上の具体化として保護する。

ここで、Validity分離そのものをClaim 1に対して新たに追加するFallbackとして扱うのではなく、Claim 1における独立性を前提として、その時間的構成をさらに限定するFallbackとして位置付ける。

---

## 52. Fallback Structure — 第三段階

第三Fallbackでは、Conditional Entitlementを追加する。

Authentication Resultその他の条件に基づいて、Entitlementを生成、付与または有効化する構成を含む。
