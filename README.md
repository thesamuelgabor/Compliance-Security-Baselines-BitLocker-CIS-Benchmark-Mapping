# Compliance, Security Baselines, BitLocker & CIS Benchmark Mapping

## Objective

This project builds the full Windows 11 security posture on top of the devices from the earlier Windows 11 track projects: a Microsoft Security Baseline, a dedicated BitLocker encryption policy, a Settings Catalog hardening profile, and the CIS Microsoft Windows 11 Benchmark (Level 2) policy set, all scoped to the pilot device group.

### Skills Learned

- Deploying and customizing a Microsoft Security Baseline profile
- Difference between Security Baselines and the Settings Catalog
- BitLocker policy design: encryption method, TPM startup, silent enablement, key escrow
- Sourcing and importing a CIS Benchmark Level 2 policy set - either as a ready-made CIS build or hand-built from CIS's free documentation
- Bulk-importing and assigning a large policy set with InToolz, vs. importing and assigning each profile manually through the GUI

### Tools Used

- Microsoft Intune / Endpoint Manager admin center
- Microsoft Security Baselines (Windows 11)
- CIS WorkBench / CIS Build Kit
- CIS Microsoft Windows 11 Benchmark (free PDF documentation)
- InToolz (bulk import and assignment of the converted policy set)
## Steps

#### 1. Deploy the Microsoft Security Baseline

Applied the built-in Security Baseline for Windows 11, then reviewed and adjusted the handful of settings that conflicted with the org's own requirements before assigning it.

<img width="1139" height="348" alt="image" src="https://github.com/user-attachments/assets/e140d152-a3b5-44f7-98bb-28f5f2379ca4" />

*Ref 1: Security baseline*

#### 2. Configure the BitLocker Policy

| Setting | Value |
|---|---|
| Encryption method (OS drive) | XTS-AES 256-bit |
| Require startup authentication | TPM only (no PIN, for cloud-only pilot) |
| Silent enablement | Enabled |
| Recovery key escrow | Microsoft Entra ID |
| Deny write access to unprotected removable drives | Enabled |

<img width="1388" height="424" alt="image" src="https://github.com/user-attachments/assets/9e6ba533-4124-413c-8130-e2bd7f55d46c" />

*Ref 2: BitLocker policy*

#### 3. Settings Catalog Hardening Profile

Built a supplementary Settings Catalog profile for controls not covered by the baseline (local admin removal, Defender configuration, unknown-source install blocking).

<img width="963" height="408" alt="image" src="https://github.com/user-attachments/assets/1acc44a9-6347-4784-91a4-06eeef44143f" />

*Ref 3: Settings Catalog hardening*

#### 4. Import the CIS Level 2 Benchmark Policies

Rather than hand-picking a handful of settings, this project applies the **CIS Microsoft Windows 11 Benchmark — Level 2** control set in full — the stricter profile intended for higher-security environments, building on top of (and in some cases overriding) the Level 1 baseline. Level 2 policies can be obtained two ways, both sourced from CIS directly:

- **CIS-provided build** - downloaded as a GPO backup / CIS Build Kit from [CIS WorkBench](https://workbench.cisecurity.org/) (free account) or, with a CIS SecureSuite membership, as a ready-made Intune configuration profile.
- **Custom-built from the free PDF** - the CIS Microsoft Windows 11 Benchmark PDF is free to download from CIS and lists every Level 2 recommendation with its exact registry path/value, which can be hand-built into Intune Settings Catalog profiles for full control over what's imported.

For this project, the Level 2 policy set was downloaded from CIS WorkBench, then bulk-imported and assigned to the pilot device group using **InToolz** rather than uploading each profile by hand. The same result is achievable through the Intune admin center GUI - importing and assigning each converted profile one at a time - InToolz was used here purely to save time on a policy set this large.

<img width="871" height="595" alt="image" src="https://github.com/user-attachments/assets/7259916a-0461-4c99-b923-bd9452939ac1" />

*Ref 4: CIS-mapped compliance policy*

#### 5. Validate Compliance

Confirmed a device is evaluated against every policy above and reported as Compliant

<img width="800" height="450" alt="image" src="docs/img/05-compliance-validation.png" />

*Ref 5: Compliance validation*
