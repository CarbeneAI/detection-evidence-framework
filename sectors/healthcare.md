# Sector Overlay: Healthcare

**Applies to:** Organizations that create, receive, maintain, or transmit electronic protected health information (ePHI), and the technology vendors that support clinical operations.
**Use with:** [FRAMEWORK.md](../FRAMEWORK.md)
**Author:** Clint P. Garrison / CarbeneAI
**License:** CC BY 4.0

This overlay adapts the core cycle for healthcare. Patient safety and clinical availability outrank detection learning. If a test can harm care delivery, redesign the test.

---

## 1. What changes

### Threat priorities that shape technique selection

Healthcare environments are attractive for ransomware and for illicit access to patient data. Access brokers and initial-access markets make credential theft and remote access abuse plausible paths. Select techniques that exercise:

- Initial access paths that lead to clinical workstations or remote access gateways.
- Credential access and lateral movement toward systems that store or process ePHI.
- Impact behaviors that encrypt or interrupt systems supporting care, tested only in safe conditions (see Off limits).
- Exfiltration or bulk access patterns against data stores that hold ePHI, using synthetic data whenever possible.

Do not invent prevalence numbers. Argue from asset value and attacker incentives: care disruption creates urgency; ePHI has secondary market value; clinical workflows often depend on shared workstations and third-party remote support.

### HIPAA as a constraint on the exercise itself

The HIPAA Security Rule requires administrative, physical, and technical safeguards for ePHI. Purple teaming is not a free pass to handle real ePHI casually.

- **Minimum necessary:** Keep artifacts to what Blue needs. Do not copy real patient records into tickets, chat, or screenshots.
- **Access control:** Only workforce members (or BAA-covered partners) with a job need should see evidence that might contain ePHI.
- **Audit controls:** Prefer proving audit logs recorded the test behavior.
- **Integrity and availability:** Tests that risk altering clinical data or taking down systems used in active care fail the safety gate by default.

Map backlog items to safeguard themes when helpful. Do not claim a purple cycle "makes you HIPAA compliant." Compliance is a program. Purple teaming produces evidence about specific controls.

### Clinical availability limits

Clinical systems often have low tolerance for latency, reboot, or account lockout. Coordinate with clinical engineering for devices that touch patients. Prefer off-peak windows only when off-peak is clinically real for that unit. Use dedicated non-production systems for destructive or high-load behavior. Stop conditions must include any report of clinical workflow failure; pause first, investigate second.

---

## 2. What is off limits

Treat the following as default exclusions unless a written exception exists with clinical and security approval:

1. **Production life-support, monitoring, infusion, imaging in active use, and other devices where failure harms patients.** Do not scan aggressively, credential-test, or implant test tooling on these devices as part of purple teaming.
2. **Ransomware detonation on production clinical networks.** Emulate precursor behaviors (access, staging, backup targeting) in production only if containment is proven. Detonate encryption in isolated labs.
3. **Use of real ePHI as test bait.** Create synthetic patients and synthetic files. If a system cannot support synthetic data, constrain the test to authentication and authorization paths that do not require opening charts.
4. **Phishing real clinicians during peak care hours without explicit workforce and leadership approval.** Social engineering can be in scope, but fatigue and distraction are patient-safety issues.
5. **Breaking break-glass or emergency access procedures** in a way that disables them during the window.
6. **Exfiltrating real ePHI** off-network, even as a "proof." Prove detection of exfiltration patterns with synthetic payloads.

If Red needs a behavior that is off limits in production, substitute a procedure that hits the same telemetry source in a lab, or mark the technique **Not testable** with the safety reason. Not testable is an honest outcome.

---

## 3. Evidence oversight bodies actually want

Healthcare oversight includes OCR investigations after incidents, internal compliance programs, auditors assessing Security Rule implementation, and business associate due diligence. They rarely want a colorful ATT&CK poster. They want:

- **Policies and procedures that match practice.** Your purple scope, safety gate, and data handling rules should match stated incident response and change control procedures.
- **Access and audit evidence.** Logs showing who accessed what, and alerts (or reasoned absence of alerts) for unusual access.
- **Risk analysis linkage.** Findings tied to identified risks (for example, weak detection of privileged access to an EHR database), not orphaned technique IDs.
- **Remediation tracking.** Backlog items with owners and dates. Open misses without owners look like ignored risks.
- **Workforce sanctions and training artifacts** when human failure is in scope. Purple teaming that includes phishing should hand results to the training process without public shaming.

For a purple cycle evidence package in healthcare, add:

- Confirmation that synthetic data was used, or a justification and approval if not.
- Clinical stakeholder acknowledgment for any test near care delivery systems.
- Explicit statement of systems excluded for patient-safety reasons.

---

## 4. PHI handling during exercises

Prefer test accounts. Crop screenshots to alert metadata; never paste full EHR screens or chart content into tickets. Redact SIEM results that return names, MRNs, or addresses. Keep retained evidence access-controlled under policy, not on personal laptops. If Red must use a mirrored role, reset and document credentials after the window.

---

## 5. Worked scenario (end to end)

**Objective:** Determine whether a stolen VPN credential used from an unusual network, followed by access to a file share containing synthetic referral documents, produces an actionable alert within 30 minutes.

**Why this technique pair:** Ransomware and data theft crews often need remote access and then file discovery. VPN anomaly plus file-share enumeration exercises identity telemetry and data-access telemetry without touching bedside devices.

### Scope highlights

- In scope: VPN concentrator logs, identity provider sign-in logs, one Windows file server used for department shares, synthetic document set, one standard user test account.
- Out of scope: EHR production, medical devices, backup appliances, domain controllers (observe only; no intentional DC compromise).
- Stop conditions: Any clinical ticket about VPN outage; lockout of non-test accounts; discovery of real ePHI in the synthetic share (halt and escalate as an incident).

### Select

1. Valid Accounts (remote access) - ATT&CK style focus: use of legitimate credentials from anomalous conditions.
2. File and Directory Discovery against the department share.

Priority reason: uncertainty whether geo/velocity and file-enumeration detections are tuned for clinical remote staff who travel.

### Safety gate

Engagement Lead, IAM owner, and clinical informatics liaison sign. Synthetic data verified. Window: Sunday 09:00–12:00 local.

### Execute and observe

1. Red authenticates the test account to VPN from an approved anomalous egress (lab exit node).
2. Blue watches VPN and IdP consoles in real time.
3. Red maps and lists the synthetic share; opens two synthetic files.
4. Blue records alerts, log presence, and time-to-alert.

### Score (example of format, not a claim about your environment)

- VPN anomalous sign-in: Detected - weak (alert fired, severity low, no identity context).
- File share enumeration: Logged only (SMB audit present; no alert).

### Backlog

- `PT-H-001`: Raise severity and add device/geo context for anomalous VPN on accounts in clinical AD groups. Owner: IAM. Acceptance: retest same procedure; expect actionable alert in 15 minutes.
- `PT-H-002`: Alert on unusual volume of file enumerations by interactive users on shares tagged as containing ePHI (synthetic tag in lab; production classification in prod). Owner: Server platform + detection eng. Acceptance: retest enumeration threshold.

### Executive summary ask

Accept residual risk for 30 days while `PT-H-001` ships, or add temporary monitoring watchlist for clinical VPN accounts.

---

*Healthcare overlay v1.0 - use with the core framework. CC BY 4.0*
