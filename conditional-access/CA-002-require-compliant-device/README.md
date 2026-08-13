# CA-002 — Require Compliant Device

## Overview

CA-002 introduces device trust into the Charlie Whiskey Security Labs (CWSL) Conditional Access framework by requiring users to access cloud resources from devices that meet organizational compliance requirements.

While CA-001 strengthens authentication by requiring Multi-Factor Authentication, CA-002 expands the access decision beyond user identity by evaluating the security posture of the device being used.

The policy was deployed in **Report-only mode** to evaluate its expected impact before enforcement.

---

## Security Objective

Successful authentication establishes confidence in the identity of a user, but it does not establish whether the device being used should be trusted.

A compromised, unmanaged, or otherwise non-compliant endpoint can introduce risk even when the user successfully completes MFA.

CA-002 is designed to incorporate device compliance into the access decision so that enterprise resources can eventually be restricted to trusted and managed endpoints.

This control supports protection against:

- Access from unmanaged devices
- Access from devices that do not meet organizational security requirements
- Use of valid credentials from untrusted endpoints
- Unauthorized access following credential compromise

---

## Policy Configuration

| Setting | Configuration |
|---|---|
| Policy Name | CA-002 — Require Compliant Device |
| Users | All users |
| Exclusions | Administrative recovery account (`cwadmin`) |
| Target Resources | All cloud resources |
| Conditions | Not configured |
| Grant Control | Require device to be marked as compliant |
| Session Controls | Not configured |
| Policy State | Report-only |

---

## Administrative Account Exclusion

The `cwadmin` administrative account was excluded during initial Conditional Access development to reduce the risk of administrative lockout while access policies were being configured and validated.

This exclusion is a temporary lab deployment consideration rather than the intended long-term security architecture.

Privileged identities will ultimately receive dedicated Conditional Access protections.

---

## Report-Only Deployment

CA-002 was initially deployed in **Report-only mode**.

Requiring device compliance before the endpoint management and compliance architecture is fully established could prevent legitimate users from accessing cloud resources.

Report-only deployment allows the policy to be evaluated without enforcing the grant control.

This provides an opportunity to:

- Validate policy scope
- Identify affected users and applications
- Evaluate expected access decisions
- Prepare device management infrastructure
- Reduce the risk of unintended access disruption

Once compliant device management is implemented and validated, CA-002 can be evaluated for enforcement.

---

## Implementation

CA-002 was created through the Microsoft Entra Conditional Access policy interface.

The policy targets users accessing cloud resources and evaluates whether the device has been marked as compliant.

### Access Decision

```text
User Authentication
        │
        ▼
   CA-002 Evaluated
        │
        ▼
Is Device Compliant?
        │
    ┌───┴───┐
    │       │
   Yes      No
    │       │
    ▼       ▼
 Access   Access Would
Allowed   Be Blocked
          When Enforced
```

Because the current policy state is **Report-only**, the compliance requirement is evaluated without actively blocking the sign-in.

---

## Security Defaults Transition

During the initial creation of CA-002, Microsoft Entra identified that **Security Defaults** were still enabled.

Security Defaults provide Microsoft-managed baseline identity protections but do not provide the granular policy control required for the developing CWSL Conditional Access architecture.

Security Defaults were therefore disabled as part of the transition to custom Conditional Access management.

After the transition, Microsoft Entra confirmed that the organization was using Conditional Access policies.

This represents an architectural transition from:

```text
Microsoft Security Defaults
          │
          ▼
Baseline Tenant Protection
```

to:

```text
Custom Conditional Access
          │
    ┌─────┴─────┐
    │           │
    ▼           ▼
User MFA    Device Trust
  CA-001       CA-002
    │           │
    └─────┬─────┘
          ▼
   Access Decision
```

This allows CWSL to progressively introduce additional identity signals and security requirements into cloud access decisions.

---

## Validation

CA-002 was validated using the Microsoft Entra **Conditional Access What If** tool.

The test scenario evaluated a standard CWSL user accessing a Microsoft cloud resource from a Windows device.

### Test Scenario

| Parameter | Test Value |
|---|---|
| User | Standard CWSL user |
| Cloud Application | Microsoft Office 365 Portal |
| Device Platform | Windows |
| Client Application | Browser |

### Result

The What If evaluation identified both existing Conditional Access controls as applicable:

```text
Authentication Request
          │
          ▼
Conditional Access Evaluation
          │
    ┌─────┴─────┐
    │           │
    ▼           ▼
  CA-001      CA-002
    │           │
    ▼           ▼
Require MFA   Require
             Compliant
              Device
```

CA-002 appeared under **Policies that will apply** with:

- Grant control: **Require compliant device**
- State: **Report-only**

This confirmed that the policy scope and grant control were configured as intended.

---

## Validation Evidence

Screenshots captured during implementation and testing can be stored in the [`images/`](images/) directory.

Suggested structure:

```text
images/
├── ca-002-policy-overview.png
├── ca-002-user-scope.png
├── ca-002-grant-control.png
├── ca-002-report-only.png
├── ca-002-what-if-validation.png
└── security-defaults-disabled.png
```

> Screenshots should be sanitized where necessary to avoid exposing unnecessary tenant or account information.

---

## Security Engineering Considerations

### Identity Alone Is Not Sufficient

MFA strengthens authentication but does not establish the security posture of the endpoint.

Combining CA-001 and CA-002 allows the access decision to consider both:

```text
WHO is requesting access?
        +
WHAT device are they using?
```

This represents a more mature Zero Trust access model than authentication based solely on credentials.

### Device Compliance Requires Supporting Infrastructure

Conditional Access can require device compliance, but compliance status must be established through an endpoint management platform such as Microsoft Intune.

CA-002 therefore represents one component of a larger device trust architecture.

### Staged Deployment

Controls capable of blocking enterprise access should be tested before broad enforcement.

Using Report-only mode allows policy behavior and potential impact to be evaluated while the supporting device management architecture is developed.

---

## Zero Trust Alignment

CA-002 expands the CWSL implementation of the Zero Trust principle:

> **Verify explicitly.**

CA-001 verifies the user's authentication requirements.

CA-002 introduces verification of the endpoint being used.

```text
        Access Request
              │
              ▼
       Verify Identity
           CA-001
              │
              ▼
        Verify Device
           CA-002
              │
              ▼
       Access Decision
```

Future Conditional Access policies will introduce additional signals including administrative privilege, authentication method, and identity risk.

---

## Lessons Learned

Implementation and validation of CA-002 reinforced several identity engineering concepts:

- Authentication and device trust represent separate access decisions.
- MFA alone does not establish whether an endpoint should be trusted.
- Device compliance requires integration between identity and endpoint management.
- Report-only mode provides a safer method for evaluating potentially disruptive policies.
- Conditional Access policies should be validated before enforcement.
- Administrative recovery paths must be considered during initial policy deployment.
- Security Defaults and custom Conditional Access represent different approaches to tenant identity protection.
- Moving to Conditional Access enables more granular Zero Trust policy design.

---

## Related Controls

| Policy | Purpose | Status |
|---|---|---|
| [**CA-001**](../CA-001-require-mfa/) | Require MFA for All Users | ✅ Implemented |
| **CA-002** | Require Compliant Device | 🧪 Report-only |
| **CA-003** | Block Legacy Authentication | 📅 Planned |
| **CA-004** | Require MFA for Administrators | 📅 Planned |

---

## Next Steps

- Establish device management and compliance capabilities
- Enroll a Windows 11 Enterprise endpoint
- Define device compliance requirements
- Validate compliance status in Microsoft Entra
- Test CA-002 against a managed compliant endpoint
- Evaluate CA-002 for eventual enforcement
- Continue development of the Conditional Access framework

---

[← Back to Microsoft Entra ID](../../README.md)
