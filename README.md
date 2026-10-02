# Compliance, Security Baselines, BitLocker & CIS Benchmark Mapping

## Objective

This project builds the full Windows 11 security posture on top of the devices from the earlier Windows 11 track projects: a compliance policy, a Microsoft Security Baseline, a dedicated BitLocker encryption policy, a Settings Catalog hardening profile, and the CIS Microsoft Windows 11 Benchmark (Level 2) policy set, all scoped to the pilot device group.

### Skills Learned

- Designing a Windows compliance policy around device health (BitLocker, Secure Boot)
- Deploying and customizing a Microsoft Security Baseline profile
- Difference between Security Baselines and the Settings Catalog
- BitLocker policy design: encryption method, TPM startup, silent enablement, key escrow
- Sourcing and importing a CIS Benchmark Level 2 policy set, either as a ready-made CIS build or hand-built from CIS's free documentation
- Bulk-importing and assigning a large policy set with InToolz, vs. importing and assigning each profile manually through the GUI

### Tools Used

- Microsoft Intune / Endpoint Manager admin center
- Microsoft Security Baselines (Windows 11)
- CIS WorkBench / CIS Build Kit
- CIS Microsoft Windows 11 Benchmark (free PDF documentation)
- InToolz (bulk import and assignment of the converted policy set)

## Steps

#### 1. Create the Compliance Policy

Created a compliance policy under **Devices → Compliance → Create policy**, with platform **Windows 10 and later** and profile type **Windows 10/11 compliance policy**. It defines the minimum state a device must be in to count as compliant, and the policies in the following steps are what bring devices into that state.

| Setting | Value |
|---|---|
| Name | Win-Compliance |
| Platform | Windows 10 and later |
| Profile type | Windows 10/11 compliance policy |
| Device Health → BitLocker | Required |
| Device Health → Secure Boot | Required |

<img width="800" height="450" alt="Win-Compliance policy properties showing BitLocker and Secure Boot required" src="docs/img/01-compliance-policy.png" />

*Ref 1: Compliance policy*

#### 2. Deploy the Microsoft Security Baseline

Applied the built-in Security Baseline for Windows 11, then reviewed and adjusted the handful of settings that conflicted with the organization's own requirements before assigning it.

<img width="1139" height="348" alt="Windows 11 security baseline profile" src="https://github.com/user-attachments/assets/e140d152-a3b5-44f7-98bb-28f5f2379ca4" />

*Ref 2: Security baseline*

#### 3. Configure the BitLocker Policy

This policy encrypts the drives, which is what satisfies the BitLocker requirement in the compliance policy from step 1.

| Setting | Value |
|---|---|
| Encryption method (OS drive) | XTS-AES 256-bit |
| Require startup authentication | TPM only (no PIN, for cloud-only pilot) |
| Silent enablement | Enabled |
| Recovery key escrow | Microsoft Entra ID |
| Deny write access to unprotected removable drives | Enabled |

<img width="1388" height="424" alt="BitLocker policy settings" src="https://github.com/user-attachments/assets/9e6ba533-4124-413c-8130-e2bd7f55d46c" />

*Ref 3: BitLocker policy*

#### 4. Settings Catalog Hardening Profile

Built a supplementary Settings Catalog profile for controls not covered by the baseline (local admin removal, Defender configuration, unknown-source install blocking).

<img width="963" height="408" alt="Settings Catalog hardening profile" src="https://github.com/user-attachments/assets/1acc44a9-6347-4784-91a4-06eeef44143f" />

*Ref 4: Settings Catalog hardening*

#### 5. Import the CIS Level 2 Benchmark Policies

Rather than hand-picking a handful of settings, this project applies the **CIS Microsoft Windows 11 Benchmark - Level 2** control set in full. Level 2 is the stricter profile intended for higher-security environments; it builds on top of (and in some cases overrides) the Level 1 baseline. Level 2 policies can be obtained two ways, both sourced from CIS directly:

- **CIS-provided build:** downloaded as a CIS Build Kit from [CIS WorkBench](https://workbench.cisecurity.org/) as a ready-made Intune configuration profile.
- **Custom-built from the free PDF:** the CIS Microsoft Windows 11 Benchmark PDF is free to download from CIS and lists every Level 2 recommendation with its exact registry path and value, which can be hand-built into Intune Settings Catalog profiles for full control over what is imported.

For this project, the Level 2 policy set was downloaded from CIS WorkBench, then bulk-imported and assigned to the pilot device group using **InToolz** rather than uploading each profile by hand. The same result is achievable through the Intune admin center GUI by importing and assigning each converted profile one at a time; InToolz was used here purely to save time on a policy set this large.

<img width="871" height="595" alt="CIS Level 2 policy set imported into Intune" src="https://github.com/user-attachments/assets/7259916a-0461-4c99-b923-bd9452939ac1" />

*Ref 5: CIS Level 2 policies imported*

#### 6. Validate Compliance

Confirmed the device is evaluated against every policy above, including BitLocker and Secure Boot from the compliance policy, and is reported as **Compliant**.

<img width="622" height="203" alt="Device reported as compliant" src="https://github.com/user-attachments/assets/79818ead-1047-4aec-9500-fa88ebf38e75" />

*Ref 6: Compliance validation*
