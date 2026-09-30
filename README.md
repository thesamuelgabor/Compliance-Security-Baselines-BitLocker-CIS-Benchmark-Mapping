# Compliance, Security Baselines, BitLocker & CIS Benchmark Mapping

## Objective

This project builds the full Windows 11 security posture on top of the devices from the earlier Windows 11 track projects: a Microsoft Security Baseline, a dedicated BitLocker encryption policy, a Settings Catalog hardening profile, and a compliance policy, all scoped to `SG-Win11-Compliance-Pilot` — each setting mapped back to a specific CIS Microsoft Windows Benchmark control.

### Skills Learned

- Deploying and customizing a Microsoft Security Baseline profile
- Difference between Security Baselines and the Settings Catalog
- BitLocker policy design: encryption method, TPM startup, silent enablement, key escrow
- Mapping configuration settings to named CIS benchmark controls

### Tools Used

- Microsoft Intune / Endpoint Manager admin center
- Microsoft Security Baselines (Windows 11)
- CIS Microsoft Windows 11 Benchmark documentation

## Steps

#### 1. Deploy the Microsoft Security Baseline

Applied the built-in Security Baseline for Windows 11, then reviewed and adjusted the handful of settings that conflicted with the org's own requirements before assigning it.

<img width="800" height="450" alt="image" src="docs/img/01-security-baseline.png" />

*Ref 1: Security baseline*

#### 2. Configure the BitLocker Policy

| Setting | Value |
|---|---|
| Encryption method (OS drive) | XTS-AES 256-bit |
| Require startup authentication | TPM only (no PIN, for cloud-only pilot) |
| Silent enablement | Enabled |
| Recovery key escrow | Microsoft Entra ID |
| Deny write access to unprotected removable drives | Enabled |

<img width="800" height="450" alt="image" src="docs/img/02-bitlocker-policy.png" />

*Ref 2: BitLocker policy*

#### 3. Settings Catalog Hardening Profile

Built a supplementary Settings Catalog profile for controls not covered by the baseline (local admin removal, Defender configuration, unknown-source install blocking).

<img width="800" height="450" alt="image" src="docs/img/03-settings-catalog.png" />

*Ref 3: Settings Catalog hardening*

#### 4. CIS Benchmark Mapping Table

| CIS Control | Description | Intune Setting | Value |
|---|---|---|---|
| 1.1.1 | Minimum password/PIN length | Windows Hello for Business PIN length | 6 |
| 2.3.1.1 | Disable built-in Administrator account | Administrator account status | Disabled |
| 5.1 | Enforce disk encryption | BitLocker OS drive encryption | Required |
| 9.3.1 | Enable Windows Firewall (all profiles) | Firewall | Require |
| 18.9.47 | Configure Microsoft Defender real-time protection | Defender real-time protection | Enabled |
| 18.10.9.2 | Block installation from unknown sources | App Runtime restriction | Enabled |

<img width="800" height="450" alt="image" src="docs/img/04-cis-compliance-policy.png" />

*Ref 4: CIS-mapped compliance policy*

#### 5. Validate Compliance

Confirmed a device in `SG-Win11-Compliance-Pilot` evaluated against every policy above and reported Compliant, then reviewed one intentionally misconfigured device to confirm it was correctly flagged and blocked by Conditional Access.

<img width="800" height="450" alt="image" src="docs/img/05-compliance-validation.png" />

*Ref 5: Compliance validation*

## About

Windows 11 security posture — baseline, BitLocker, Settings Catalog, and compliance — built and mapped directly to named CIS Microsoft Windows Benchmark controls.
