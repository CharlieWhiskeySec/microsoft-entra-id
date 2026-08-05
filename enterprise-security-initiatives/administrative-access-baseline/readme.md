# Administrative Access Baseline

## Objective

Establish foundational security controls for privileged Microsoft Entra identities.

---

## Business Need

Privileged administrator accounts are among the highest-value targets in any cloud environment. This initiative establishes a secure administrative baseline before implementing more advanced identity protection controls.

---

## Technologies

- Microsoft Entra ID
- Microsoft 365 Business Premium
- Conditional Access
- Multi-Factor Authentication
- Role-Based Access Control (RBAC)

---

## Implementation

### Administrative Accounts

Configured dedicated administrative accounts for tenant administration.

### Security Groups

Implemented role-based security groups for administrative delegation.

### Role Assignment

Assigned the Global Administrator role through group membership.

### Conditional Access

Implemented:

**CA-001 | Administrative Access Protection**

Configuration:

- State: Report-only
- Scope: Built-in administrative roles
- Resources: All cloud resources
- Grant Control: Require multifactor authentication

---

## Validation

- ✅ Policy deployed successfully
- ✅ Microsoft recommended template utilized
- ⏳ "What If" validation pending
- ⏳ Production enforcement pending

---

## Future Enhancements

- Dedicated break-glass account
- Legacy authentication blocking
- User MFA baseline
- Authentication Strengths
