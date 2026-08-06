# Administrative Access Baseline

## Objective

Establish a secure administrative identity baseline for privileged Microsoft Entra accounts before implementing broader identity security controls.

---

## Business Need

Administrative identities represent the highest-value targets within Microsoft Entra. Establishing strong authentication and Conditional Access policies significantly reduces the risk of tenant compromise resulting from credential theft or phishing attacks.

---

## Technologies

Microsoft 365 Business Premium

Microsoft Entra ID

Role-Based Access Control

Conditional Access

Microsoft Authenticator

---

## Implementation

### Administrative Accounts

Configured dedicated administrative accounts.

### Role-Based Access Control

Implemented administrative security groups.

Assigned Global Administrator through group membership.

### Conditional Access

Created:

CA-001 | Administrative Access Protection

Configuration

- Microsoft recommended template
- Report-only deployment
- Applies to all built-in privileged administrator roles
- Protects all cloud resources
- Requires multifactor authentication
---

## Validation

### What If Evaluation

Policy was successfully evaluated using the Microsoft Entra What If tool.

Findings

- Policy correctly targeted privileged directory roles.
- Current administrative account was excluded by the Microsoft template.
- Policy did not apply because the excluded user matched the sign-in simulation.
- Validation confirmed the template behaved exactly as designed.

---

## Lessons Learned

Microsoft's recommended Conditional Access templates automatically exclude the current administrator during initial deployment to reduce the risk of accidental administrative lockout.

Future iterations will replace this exclusion with a dedicated emergency access account.

---

## Future Enhancements

- Create dedicated break-glass account
- Remove administrative account exclusion
- Validate enforcement
- Enable production deployment
