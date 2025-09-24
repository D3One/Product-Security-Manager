# Product Security Glossary

**What this is:** a comprehensive, engineering-and-business glossary for **Product Security**—the practice of embedding security into digital/connected products and their ecosystems from concept through end-of-life. It complements but is broader than **AppSec** (code-focused) and **DevSecOps** (pipeline-focused) by spanning **hardware, firmware, software, cloud backends, manufacturing, field operations, and customer support**.

**How it was compiled:** synthesis of widely used frameworks and standards, including **NIST SSDF (SP 800-218)**, **NIST SP 800-53/61/161/171**, **NISTIR 8259 (IoT)**, **ISO/IEC 27001/27002**, **ISO/IEC 29147/30111** (vuln disclosure/handling), **IEC 62443** (industrial/OT), **ISO/SAE 21434** & **UNECE R155/156** (automotive), **IEC 62304** (medical software), **FIPS 140-3** (crypto modules), **OWASP** (ASVS, SAMM, Top 10, API Top 10), **MITRE ATT\&CK/D3FEND/CWE/CVE/CVSS**, **SLSA + in-toto/Sigstore**, **CIS Benchmarks**, and cloud provider hardening (AWS/Azure/GCP).

**How to use it:** scan by section; items include concise definitions plus **implementation notes** or **examples** where helpful.

---

## 1) Governance, Organization & Program Basics

1. **Product Security** — holistic security across the product, its cloud/backend, mobile/desktop clients, manufacturing, supply chain, and support. *Comment:* Treat the **product** (not just an app) as the threat surface.

2. **Product Security Office (PSO)** — cross-functional program that defines strategy, policies, metrics, and roadmaps for product security across BU/R\&D/Operations.

3. **PSIRT (Product Security Incident Response Team)** — dedicated team handling product vulnerabilities and incidents across the fleet; coordinates fixes, advisories, and customer updates.

4. **CVD (Coordinated Vulnerability Disclosure)** — process and policy enabling researchers and customers to report issues responsibly; aligned to **ISO/IEC 29147** (disclosure) and **30111** (handling).

5. **Security Champion (Product)** — engineer embedded in a product team who localizes security practices and acts as first contact for PSO/PSIRT.

6. **Design Controls** — regulated development practices ensuring traceability from requirements to tests (e.g., **IEC 62304**, ISO 13485 contexts).

7. **Product Risk Rating** — a business-aligned method to rate product/fleet risk (safety, privacy, compliance, uptime, brand), driving prioritization and SLAs.

8. **Security Policy Stack** — hierarchy: **Policy → Standard → Procedure → Guideline**; mapped to controls and verification activities.

---

## 2) Threat Modeling & Requirements

9. **Threat Modeling** — structured analysis of what you’re building, its assets, trust boundaries, and attacker goals. *Example:* DFD + abuse cases for a connected device with a mobile app and cloud.

10. **STRIDE / PASTA / LINDDUN** — common methodologies (security + privacy perspectives).

11. **Attack Surface** — sum of ways an adversary can interact with or influence the product/ecosystem (device ports, local web UI, mobile BLE, cloud APIs, OTA channel).

12. **Trust Boundary** — point where data/privilege changes control (e.g., device ↔ cloud, app ↔ OS).

13. **Abuse/Misuse Case** — user story describing attacker behavior (e.g., “as a counterfeiter, I clone device certs to unlock premium features”).

14. **Security Requirements** — verifiable statements derived from threats/regulations (e.g., “support secure boot with rollback protection”).

15. **Privacy by Design** — minimize data, purpose-limit processing, and default to protective choices across product UX and telemetry.

---

## 3) Secure Design Principles (Product-centric)

16. **Least Privilege** — components and users get only necessary permissions (device services, microservices, support tools).

17. **Defense in Depth** — layered controls (boot chain, OS sandboxing, app authz, network policies, WAF).

18. **Secure by Default** — safe out-of-box config (admin password set at first boot with strength checks; remote admin disabled until claimed).

19. **Fail Secure (Fail Closed)** — default to denial if validator/update check fails.

20. **Attack Surface Reduction** — disable unused interfaces (JTAG/SWD/UART), remove sample endpoints, limit open ports.

21. **Separation of Duties / Isolation** — privilege separation between updater, telemetry, and user apps (namespaces/containers/TEE).

22. **Memory Safety** — prefer memory-safe languages (Rust/Go) or use hardening (CET/CFI, ASLR, W^X) for C/C++ components.

23. **Tamper Evidence/Resistance** — physical seals, secure enclosures, sensors that trigger safe modes/logging.

24. **Revocation & Recovery** — design for compromised key/device revocation and safe re-provision.

---

## 4) Hardware, Firmware & Device Identity

25. **Root of Trust (RoT)** — minimal, immutable code/hardware anchoring trust (boot ROM, fused keys).

26. **Secure Boot** — verify each boot stage’s signature before execution (bootloader → kernel → app).

27. **Measured Boot** — record measurements (hashes) in secure hardware (e.g., TPM PCRs) for **remote attestation**.

28. **Rollback Protection** — prevent installing firmware older than the last trusted version.

29. **TPM / TEE / Secure Element (SE)** — hardware for key storage and isolated execution (e.g., TPM 2.0, ARM TrustZone, discrete SE).

30. **DICE / RIoT** — device identity derivation from a Unique Device Secret to form per-firmware keys.

31. **PUF (Physically Unclonable Function)** — derive secrets from device-unique silicon characteristics.

32. **JTAG/SWD Lockdown** — disable or gate debug ports in production; enable secure unlock with signed tokens.

33. **IETF RATS Attestation** — standardized evidence/claims about device state to a verifier.

34. **IETF SUIT** — secure firmware update metadata and manifests.

35. **FIPS 140-3 Validation** — assurance that crypto modules meet NIST requirements (often mandated in gov/regulated sectors).

---

## 5) OTA Updates & Key Management

36. **OTA (Over-the-Air) Updates** — signed, integrity-checked, optionally encrypted updates; staged rollouts and rollback plan.

37. **Offline/Root Signing Keys** — keep release keys offline or in HSMs; use short-lived intermediate signing keys for CI.

38. **Key Ladder** — hierarchical derivation limiting key exposure (device root → update → session keys).

39. **Per-Device Certificates** — unique identity for mutual TLS and fleet control; rotate on schedule and on compromise.

40. **Anti-Rollback Counters (eFuses/Monotonic)** — hardware or secure counters to enforce version monotonicity.

---

## 6) Cloud & Backend (Product Ecosystem)

41. **Device Twin / Shadow** — cloud state mirror for devices; enforce authz and schema validation.

42. **Fleet Management** — enrollment/claim, configuration, OTA targeting, certificate lifecycle.

43. **Protocol Security** — MQTT/AMQP/HTTPs with **mTLS**; topic/queue ACLs to prevent cross-tenant access.

44. **Multi-Tenant Isolation** — tenant-scoped identities, data stores, and keys; separate audit trails.

45. **Rate Limiting / DoS Protection** — protect device endpoints and cloud APIs; per-device quotas.

46. **Data Residency / Sovereignty** — region pinning and cross-border transfer controls for product data.

---

## 7) Microservices & API Security (Product Context)

47. **API Gateway** — central authn/authz (OAuth2/OIDC), schema validation, throttling, and JWT verification.

48. **mTLS Service Mesh** — mutual TLS and fine-grained authorization between services (SPIFFE/SPIRE identities).

49. **Claims-Based Authorization** — device/app claims drive allow/deny (e.g., device model, ownership).

50. **Schema-First (OpenAPI/Protobuf)** — generate validators; reject unknown fields; prevent mass assignment.

51. **BOLA/IDOR** — broken object level auth; ensure resource-owner checks on every request.

52. **Client Hardening** — secure local admin UI: CSRF tokens, SameSite cookies, origin checks, local auth rate limits.

---

## 8) Secure SDLC for Products

53. **SSDF (NIST SP 800-218)** — secure tasks across Plan, Protect, Produce, Respond; map to product artifacts.

54. **Secure Coding Standards** — CERT C/C++, MISRA (safety), language-specific guides for crypto, memory, and concurrency.

55. **SAST/DAST/IAST/RASP** — layered testing; prioritize findings in code paths reachable on the device or through cloud APIs.

56. **Fuzzing (Coverage-Guided)** — for parsers/protocols/firmware update handlers; integrate into CI.

57. **Secrets Scanning** — block commits with embedded keys/tokens; validate in CI.

58. **Code Review with Security Checklist** — authz, input validation, error handling, logging/PII rules.

59. **HIL (Hardware-in-the-Loop) Testing** — run firmware/tests on real or emulated hardware before release.

---

## 9) Supply Chain & Build Integrity

60. **SBOM (SPDX/CycloneDX)** — machine-readable inventory of components and licenses shipped in the product.

61. **VEX** — states whether a CVE actually affects your shipped product.

62. **SLSA** — maturity levels for build provenance and tamper resistance (hermetic, reproducible, two-person control).

63. **in-toto Attestations** — cryptographically signed statements of build steps, materials, and provenance.

64. **Artifact Signing (Sigstore/Cosign)** — sign firmware, containers, and packages; verify at install/admission.

65. **Pinned Dependencies / Vendoring** — lock versions, verify checksums; avoid dependency confusion and typosquatting.

66. **Third-Party Risk** — vet suppliers (security posture, SBOM, patch cadence, CVD), include **right-to-audit** clauses.

---

## 10) Vulnerability Management (Fleet & Product)

67. **CVE / CWE / CVSS** — vulnerability identifiers, weakness taxonomy, and severity scoring.

68. **Exploitability & Exposure** — prioritize by whether the vulnerable code is present, reachable, and exposed in your product.

69. **Remediation SLA** — time-bound targets by severity/product tier; tie to customer communications.

70. **Backporting** — port fixes to supported older firmware when major upgrades are impractical.

71. **EoS/EoL (End of Support/Life)** — declared timelines and alternatives; publish clear customer guidance.

72. **KEV Catalog Awareness** — track **Known Exploited Vulnerabilities** to expedite patches/workarounds.

---

## 11) Deployment, Provisioning & Manufacturing

73. **Secure Provisioning** — inject unique secrets/certs securely on the line; segregate manufacturing networks.

74. **FDO (FIDO Device Onboard)** — standardized secure, zero-touch onboarding from factory to owner.

75. **Secure Defaults at First Boot** — force credential setup with strength checks; bind device to owner account.

76. **Golden Image/Config** — signed, validated baseline firmware and configuration per SKU/region.

77. **Field Service Mode** — limited, audited service access with time-boxed credentials and tamper logging.

---

## 12) Anti-Counterfeiting & Product Integrity

78. **Serialization & Traceability** — unique IDs tied to manufacturing and supply events.

79. **Challenge-Response Authentication** — cryptographic checks to verify genuine accessories/parts.

80. **Secure Labels/Tags** — signed QR/NFC with anti-tamper features and online verification.

81. **Grey-Market Controls** — region lock, activation policies, and server-side fraud analytics.

---

## 13) Monitoring, Telemetry & Safety

82. **Privacy-Aware Telemetry** — minimal, aggregated, and scrubbed logs; clear consent and retention policies.

83. **Remote Attestation Monitoring** — verify device state (boot measurements, policy) before enabling sensitive features.

84. **Anomaly Detection** — detect impossible travel/device behavior, OTA failure spikes, cert expiry.

85. **Safety-Security Co-Engineering** — coordinate with functional-safety (e.g., ISO 26262) to ensure secure controls do not jeopardize safety.

86. **Kill Switch / Feature Gating** — remote disablement or restriction when compromise is detected; use with strict governance.

---

## 14) Incident Response, Advisories & Legal

87. **Security Advisory** — customer-facing notice describing issue, impact, versions, fixes, and mitigations.

88. **CNA (CVE Numbering Authority)** — vendor or partner authorized to assign CVEs for your products.

89. **Forensics Readiness** — fleet logs, time sync, signed logs, secure crash dumps for investigation.

90. **Regulatory Notifications** — sector rules (e.g., medical/auto) for reporting and patch timing.

91. **Safe Harbor Language** — legal terms encouraging research under CVD without punitive action.

---

## 15) Metrics & Executive Communication

92. **Upgrade Adoption Rate** — % of fleet on latest security version over time.

93. **Mean Time to Remediate (MTTR)** — from report to fixed version shipped; track per severity.

94. **Exposure Window** — time from public disclosure to majority of fleet patched.

95. **Defect Density (Security)** — normalized vulns per KLOC/module; used carefully to avoid perverse incentives.

96. **Fleet Risk Heatmap** — by product line, region, and dependency exposure.

---

## 16) Training & Enablement

97. **Role-Based Training** — tailored content for firmware devs, mobile/web engineers, cloud SREs, support, and manufacturing.

98. **Playbooks/Runbooks** — OTA rollback, cert rotation, device quarantine, advisory templates.

99. **Golden Paths** — secure templates for device apps, OTA handlers, cloud endpoints, and deployment.

100. **Supplier Enablement** — provide security checklists and escrowed signing processes for ODMs/contract manufacturers.

---

## 17) Handy Mini-Examples

* **Rollback Protection (example):** “The bootloader rejects any firmware with a version counter lower than the stored monotonic value; counters are advanced only after signature verification.”
* **CVD SLA (example):** “Critical: acknowledge in 48h, mitigation in 7 days, fix in 30 days; public advisory coordinated with reporter.”
* **API Gate (example):** “Gateway enforces mTLS from devices; JWT must include `sub=deviceID` and `scope=telemetry:write`; requests over rate quota receive `429` with backoff headers.”
* **Supply-Chain (example):** “Require Cosign signatures and in-toto attestations for firmware; deny install if provenance is missing or mismatched.”

---

## 18) Abbreviation Quick Table (selected)

**ASVS, ATT\&CK, BLE, CNA, CPE/CVE/CVSS, CVD, DICE, DFD, FDO, FIPS, HIL, HSM/SE/TPM/TEE, IAST, IETF RATS/SUIT, IoT, ISO 21434/62304/27001/29147/30111, JTAG/SWD, JWT, KEV, mTLS, OTA, PASTA/STRIDE/LINDDUN, PCR (TPM), PUF, PSIRT/PSO, RoT, SBOM (SPDX/CycloneDX), SCA, SLSA, SPIFFE/SPIRE, SUIT, TEE, VEX, ZTA.**

---
