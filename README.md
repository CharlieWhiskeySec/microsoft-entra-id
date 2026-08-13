# Microsoft Entra ID

Enterprise identity and access management engineering within the **Charlie Whiskey Security Labs (CWSL)** environment.

This repository documents the design, implementation, testing, and validation of identity security controls using Microsoft Entra ID and Microsoft 365.

Rather than functioning as a collection of isolated lab exercises, the environment is being developed as a simulated enterprise identity architecture integrating cloud identity, on-premises Active Directory, Conditional Access, authentication controls, security monitoring, and identity lifecycle management.

---

## Objectives

- Design and implement enterprise identity architectures
- Deploy and validate Conditional Access controls
- Develop hybrid Active Directory and Microsoft Entra ID integration
- Implement identity lifecycle and access management processes
- Build practical experience with Zero Trust identity controls
- Develop hands-on experience aligned with Microsoft SC-300
- Integrate identity telemetry with enterprise security monitoring

---

# Enterprise Environment

| Component | Technology |
|---|---|
| Cloud Identity | Microsoft Entra ID |
| Productivity & Security | Microsoft 365 Business Premium |
| On-Premises Identity | Active Directory Domain Services |
| Authentication | Microsoft Authenticator / MFA |
| Access Control | Conditional Access / RBAC |
| Endpoints | Windows 11 Enterprise |
| SIEM | Splunk Enterprise |
| Hybrid Identity | AD → Entra Integration *(In Development)* |

---

# Identity Architecture

Current identity architecture:

```text
                    Charlie Whiskey Security Labs
                               │
                ┌──────────────┴──────────────┐
                │                             │
                ▼                             ▼
        Active Directory               Microsoft Entra ID
          (On-Premises)                    (Cloud)
                │                             │
                │                       ┌─────┴─────┐
                │                       │           │
                ▼                       ▼           ▼
        Users & Groups              Microsoft   Conditional
                                    365          Access
                                                    │
                                                    ▼
                                             Authentication
                                               Controls

                 Hybrid Identity Integration
                       (In Development)

        Active Directory ───────────────► Microsoft Entra ID
```

The current development phase is focused on connecting the on-premises Active Directory environment with Microsoft Entra ID to create a functional hybrid identity architecture.

---

# Implemented Security Controls

## 🔐 CA-001 — Require MFA for All Users

**Status:** Implemented / Validated

CA-001 establishes a baseline Multi-Factor Authentication requirement for users accessing enterprise cloud resources.

### Configuration

- Users: All users
- Administrative account excluded for recovery/testing
- Target resources: All cloud resources
- Grant control: Require Multi-Factor Authentication
- Validation: Conditional Access **What If**
- Deployment approach: Validated prior to broader enforcement

### Security Objective

Passwords alone are insufficient protection against credential compromise.

Requiring MFA adds an additional authentication factor and reduces the likelihood that stolen credentials alone can be used to access enterprise resources.

### Validation

The Microsoft Entra Conditional Access **What If** tool was used to verify policy applicability and expected grant controls before enforcement.

---

## 🖥️ CA-002 — Require Compliant Device

**Status:** Implemented / Report-only

CA-002 introduces device trust into the authentication decision by requiring users to access enterprise resources from devices that meet organizational compliance requirements.

### Configuration

- Users: All users
- Administrative account excluded for recovery/testing
- Target resources: All cloud resources
- Grant control: Require device to be marked as compliant
- Policy state: Report-only
- Validation: Conditional Access **What If**

### Security Objective

Successful authentication establishes the identity of a user but does not establish whether the endpoint being used should be trusted.

Device compliance adds another Zero Trust decision point by evaluating the security state of the endpoint before allowing access to enterprise resources.

### Validation

Conditional Access **What If** testing confirmed that CA-002 applies to the targeted Windows authentication scenario and requires a compliant device.

---

# Conditional Access Framework

**Status:** 🚧 In Development

CWSL is transitioning from Microsoft's baseline Security Defaults configuration to a custom Conditional Access framework.

## Security Defaults Transition

Microsoft Security Defaults were disabled to allow granular Conditional Access policies to manage authentication and access requirements.

This transition allows access decisions to be based on factors including:

- User identity
- Authentication method
- Device compliance
- Administrative privilege
- Application
- Sign-in context
- Risk

Rather than relying exclusively on tenant-wide baseline protections, individual security controls can now be designed, tested, validated, and deployed independently.

### Current Policies

| Policy | Purpose | Status |
|---|---|---|
| CA-001 | Require MFA for All Users | ✅ Implemented |
| CA-002 | Require Compliant Device | 🧪 Report-only |
| CA-003 | Block Legacy Authentication | 📅 Planned |
| CA-004 | Require MFA for Administrators | 📅 Planned |
| CA-005 | Risk-Based Identity Protection | 📅 Planned |

---

# Zero Trust Strategy

The Conditional Access implementation follows the principle:

> **Verify explicitly.**

Authentication decisions should consider more than possession of a valid username and password.

CWSL is progressively introducing controls that evaluate:

```text
Identity
   │
   ├──► Authentication Strength
   │
   ├──► Device Trust
   │
   ├──► Privilege
   │
   ├──► Application
   │
   └──► Risk
            │
            ▼
       Access Decision
```

CA-001 begins by strengthening user authentication.

CA-002 expands the decision by introducing device compliance.

Future policies will incorporate administrative privilege, legacy authentication restrictions, and identity risk.

---

# Administrative Access Baseline

**Status:** 🚧 In Progress

Foundational controls for privileged identities have been established.

### Implemented

- Administrative accounts created
- Security groups implemented
- Global Administrator role assigned through group membership
- Microsoft Authenticator configured
- Multi-Factor Authentication implemented
- Conditional Access framework established

### Planned

- Dedicated administrative Conditional Access policy
- Privileged authentication requirements
- Administrative role monitoring
- Privileged Identity Management where licensing permits

---

# Hybrid Identity

**Status:** 🚧 In Development

The next major phase of the environment is integration between the existing CWSL Active Directory domain and Microsoft Entra ID.

Planned architecture:

```text
On-Premises Active Directory
            │
            │ Identity Synchronization
            ▼
     Microsoft Entra ID
            │
      ┌─────┴─────┐
      │           │
      ▼           ▼
Microsoft 365   Conditional
                Access
```

This environment will be used to explore:

- Identity synchronization
- Hybrid user identities
- Group synchronization
- Authentication architecture
- Identity lifecycle management
- Joiner / Mover / Leaver workflows
- Hybrid identity monitoring

---

# Identity Lifecycle Foundation

**Status:** 📅 Planned

Future development will introduce enterprise identity lifecycle processes including:

- User provisioning
- Department and organizational structure
- Group-based access
- Role assignment
- Access modification
- Account disablement
- Deprovisioning
- Joiner / Mover / Leaver workflows

---

# Authentication Modernization

**Status:** 📅 Planned

Future authentication engineering will explore:

- Authentication Methods
- Passwordless Authentication
- Temporary Access Pass
- Authentication Strengths
- Modern Authentication
- Legacy Authentication restrictions

---

# Enterprise Application Security

**Status:** 📅 Planned

Future application identity projects will include:

- Enterprise Applications
- App Registrations
- Service Principals
- OAuth permissions
- Application authentication
- Single Sign-On
- SCIM provisioning

---

# Identity Monitoring & Detection

Microsoft Entra identity activity will eventually be incorporated into the broader CWSL security monitoring architecture.

Planned workflow:

```text
Active Directory ──┐
                    │
Windows Endpoints ──┼──► Security Telemetry ──► Splunk Enterprise
                    │                              │
Microsoft Entra ID ─┘                              ▼
                                            Detection Engineering
                                                    │
                                                    ▼
                                             SOC Investigation
```

This will allow identity activity to be correlated with endpoint and Active Directory telemetry during security investigations.

---

# Current Development Priorities

1. Expand Conditional Access framework
2. Integrate Active Directory with Microsoft Entra ID
3. Validate hybrid identity synchronization
4. Develop identity lifecycle workflows
5. Expand identity monitoring and detection
6. Continue SC-300-aligned implementation

---

# Future Roadmap

- Hybrid Identity
- Identity Governance
- Access Reviews
- Privileged Identity Management
- Passwordless Authentication
- Authentication Strengths
- Enterprise Application SSO
- SCIM Provisioning
- Identity Risk Detection
- Microsoft Sentinel integration
- Splunk identity monitoring
- Microsoft SC-300 certification

---

# Related CWSL Projects

| Project | Purpose |
|---|---|
| 🏢 **[Charlie Whiskey Security Labs](https://github.com/CharlieWhiskeySec/charlie-whiskey-security-labs)** | Enterprise environment and project hub |
| 🖥️ **[Active Directory Security Lab](https://github.com/CharlieWhiskeySec/active-directory-security-lab)** | On-premises identity infrastructure and Windows security |
| 📊 **[Splunk Detection Engineering](https://github.com/CharlieWhiskeySec/splunk-detections)** | SIEM engineering and security detection |
| 🔎 **[SOC Investigations](https://github.com/CharlieWhiskeySec/soc-investigations)** | Security investigations and incident analysis |
| 🐧 **[Linux Administration](https://github.com/CharlieWhiskeySec/linux-administration)** | Linux administration and infrastructure |

---

<p align="center">
<strong>Part of the Charlie Whiskey Security Labs enterprise security environment.</strong>
</p>
