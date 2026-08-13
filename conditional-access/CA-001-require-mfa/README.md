# CA-001 — Require Multi-Factor Authentication for All Users

## Overview

CA-001 establishes a baseline Multi-Factor Authentication (MFA) requirement for users accessing cloud resources within the Charlie Whiskey Security Labs (CWSL) Microsoft Entra ID environment.

The policy was implemented as part of the transition from Microsoft Security Defaults to a custom Conditional Access framework, allowing authentication requirements to be explicitly designed, tested, and managed.

---

## Security Objective

Passwords alone do not provide sufficient protection against credential compromise.

The objective of CA-001 is to require an additional authentication factor before users are granted access to enterprise cloud resources.

This control helps reduce the risk associated with:

- Compromised passwords
- Credential stuffing
- Password spraying
- Phishing-related credential theft
- Unauthorized cloud access using stolen credentials

---

## Policy Configuration

| Setting | Configuration |
|---|---|
| Policy Name | CA-001 — Require MFA for All Users |
| Users | All users |
| Exclusions | Administrative recovery account (`cwadmin`) |
| Target Resources | All cloud resources |
| Conditions | Not configured |
| Grant Control | Require Multi-Factor Authentication |
| Session Controls | Not configured |

---

## Administrative Account Exclusion

The `cwadmin` administrative account was excluded during initial Conditional Access implementation to reduce the risk of administrative lockout while policies were being developed and tested.

This exclusion is part of the lab's initial deployment strategy rather than the intended long-term privileged-access design.

A dedicated Conditional Access policy for administrative identities will be implemented separately.

---

## Implementation

CA-001 was created through the Microsoft Entra Conditional Access policy interface.

The policy was scoped broadly to establish MFA as the baseline authentication requirement across cloud resources while maintaining a temporary administrative recovery path during development.

### Access Decision

```text
User Authentication
        │
        ▼
   CA-001 Evaluated
        │
        ▼
Is MFA Requirement Satisfied?
        │
    ┌───┴───┐
    │       │
   Yes      No
    │       │
    ▼       ▼
 Access   MFA Required
Granted   Before Access
```

---

## Validation

Before relying on the policy, the configuration was evaluated using the Microsoft Entra **Conditional Access What If** tool.

The test evaluated a standard CWSL user accessing a Microsoft cloud resource.

### Expected Result

```text
CA-001
Policy applies
        │
        ▼
Grant Control
        │
        ▼
Require Multi-Factor Authentication
```

The What If analysis confirmed that CA-001 applied to the test scenario and that MFA was the required grant control.

---

## Validation Evidence

Screenshots captured during implementation and testing are stored in the [`images/`](images/) directory.

Suggested evidence:

```text
images/
├── ca-001-policy-overview.png
├── ca-001-user-scope.png
├── ca-001-target-resources.png
├── ca-001-grant-control.png
└── ca-001-what-if-validation.png
```

> Screenshots are sanitized where necessary to avoid exposing unnecessary tenant or account information.

---

## Security Engineering Considerations

### Policy Scope

Applying MFA to all users establishes a consistent authentication baseline rather than relying on users or applications to independently enforce stronger authentication.

### Administrative Access

Privileged identities require additional protections beyond the standard user baseline. Administrative access will therefore be addressed through a dedicated Conditional Access policy.

### Policy Testing

Conditional Access policies can have significant operational impact, including the possibility of tenant lockout.

The What If tool provides a method of evaluating policy scope and expected behavior before broader enforcement.

### Layered Controls

MFA establishes stronger user authentication but does not independently establish device trust.

Additional Conditional Access policies are therefore being developed to evaluate other access signals such as device compliance, privilege, authentication method, and sign-in context.

---

## Zero Trust Alignment

CA-001 implements the Zero Trust principle of **verify explicitly**.

```text
Username + Password
        │
        ▼
Identity Authentication
        │
        ▼
Additional Verification
      (MFA)
        │
        ▼
Access Decision
```

Possession of valid credentials alone is not considered sufficient evidence that access should be granted.

---

## Lessons Learned

Implementation of CA-001 reinforced several identity engineering concepts:

- Conditional Access separates authentication policy from individual applications.
- Policy scope must be carefully evaluated before enforcement.
- Administrative recovery must be considered when designing access controls.
- What If analysis provides a safe method for validating policy behavior.
- MFA represents one layer of an overall Zero Trust access strategy.
- Moving from Security Defaults to Conditional Access provides significantly greater control over authentication requirements.

---

## Related Controls

| Policy | Purpose | Status |
|---|---|---|
| **CA-001** | Require MFA for All Users | ✅ Implemented |
| [**CA-002**](../CA-002-require-compliant-device/) | Require Compliant Device | 🧪 Report-only |
| **CA-003** | Block Legacy Authentication | 📅 Planned |
| **CA-004** | Require MFA for Administrators | 📅 Planned |

---

## Next Steps

- Continue monitoring CA-001 behavior
- Develop dedicated protections for privileged identities
- Expand device-based access controls
- Block legacy authentication
- Integrate hybrid identities into the Conditional Access framework

---

[← Back to Microsoft Entra ID](../../README.md)
