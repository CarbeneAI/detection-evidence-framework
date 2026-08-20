# Sector Overlay: Public Safety Tech

**Applies to:** Technology environments that support law enforcement, fire, EMS, emergency communications, courts, and vendors building software or infrastructure for those missions.
**Use with:** [FRAMEWORK.md](../FRAMEWORK.md)
**Author:** Clint P. Garrison / CarbeneAI
**License:** CC BY 4.0

This overlay adapts the core cycle for public safety technology. Life-safety continuity and evidence integrity outrank detection learning. If a test can delay response to a call for service, redesign the test.

---

## 1. What changes

### CJIS-adjacent data sensitivity

Many public safety systems handle Criminal Justice Information (CJI) or data that must be protected under CJIS Security Policy requirements when in scope for agencies and their contractors. Even when a specific system is not formally in a CJIS audit boundary, treat investigative data, CAD narratives, body-worn camera metadata, and NCIC-adjacent query results as high sensitivity.

Cycle adaptations:

- **Need to know:** Limit purple evidence access to cleared or authorized personnel per agency policy.
- **Audit logging:** Prefer techniques that validate whether access to sensitive records is logged and reviewable.
- **Media control:** Screenshots, exports, and packet captures from justice systems follow evidence-handling rules, not casual ticket attachments.
- **Contractor boundaries:** Vendors running purple tests need written authorization that names systems, windows, and data rules. Verbal "go ahead" is not enough.

Do not claim this overlay makes an environment "CJIS certified." It helps you test controls that CJIS-aligned programs care about: access control, auditing, authentication monitoring, and protection of sensitive data at rest and in transit.

### Field and disconnected-device realities

Public safety work includes vehicles, radios, tablets, offline caches, and intermittent connectivity. Detections built only for always-online corporate laptops miss the field.

Ask: Does telemetry leave the device when offline, and what happens on reconnect? Are MDM and endpoint signals delayed? Score time-to-detect with field-realistic delay, not lab LAN timing. Does VPN failure fail open or closed for the workflow under test? The safety gate must know before testing.

### Chain of custody

Security testing and digital forensics collide in this sector. If purple teaming produces artifacts that could later be questioned in court or internal affairs, handle them carefully:

- Use designated test cases and test records when possible.
- Do not alter production evidence stores.
- Document hash, time, handler, and storage location for any retained capture that could be mistaken for investigative material.
- Keep purple evidence in a repository separate from evidentiary evidence lockers unless policy explicitly unifies them.

### Life-safety systems

Computer-aided dispatch (CAD), 9-1-1 call handling, radio console systems, station alerting, and medical devices in EMS contexts are life-safety adjacent. Availability is the primary control objective.

---

## 2. What is off limits

Default exclusions:

1. **Any test that can drop, delay, or degrade 9-1-1 call taking, dispatch, or radio for live operations.** No port scans of call-handling hosts during operations, no credential lockouts for dispatcher accounts, no "chaos" experiments on production telephony.
2. **Ransomware or destructive testing on networks that path to dispatch or records systems used for active incidents.** Use isolated labs with mirrored control planes.
3. **Implanting test malware on devices that leave for the field the same day** unless the device is a dedicated test unit and cannot be assigned to live duty until cleaned and verified.
4. **Using real investigative case content as test data.** Build synthetic cases. If a system requires real record structures, scrub and tokenize under records management approval.
5. **Techniques that falsify location, unit status, or AVL in production** in ways that could misroute responders.
6. **Breaking encryption or attempting to bypass evidence-grade signing** on body-worn or in-car video systems.

If leadership requests assurance on a life-safety system, prefer:

- Tabletop plus configuration review, or
- Parallel test environment failover drills under operations control, or
- Read-only detection validation (confirm logs for admin access) without offensive execution.

Mark unsafe techniques **Not testable** and document the residual risk in the agency risk register.

---

## 3. Evidence oversight bodies actually want

Oversight may include CJIS audits, state criminal justice information auditors, city/county IT risk committees, accreditation bodies, and grantors. They tend to ask whether access is controlled, whether audit trails exist, and whether vendors are held to contract security terms.

Useful purple evidence: authorization for the window; systems touched and life-safety exclusions; proof of synthetic data; alert and log evidence for admin and remote access into justice applications; backlog items in the agency tracker with owners; retest results after patches or rule changes.

Less useful: ATT&CK heatmaps with no agency outcomes; vendor marketing without timestamps in your tenant; screenshots with real subject names, victim data, or case narratives.

---

## 4. Worked scenario (end to end)

**Objective:** Determine whether privileged admin access to a records-management system (RMS) from a vendor support account, outside the agreed support window, produces an actionable alert within 20 minutes, without touching CAD or telephony.

**Why:** Third-party remote support is a common necessity and a common abuse path. Validating monitoring of vendor accounts exercises identity, VPN/jump host, and application audit logs. CAD remains out of scope to protect life-safety.

### Scope highlights

- In scope: Vendor jump host, VPN, RMS application admin audit logs, SIEM, one synthetic case file, vendor test admin account.
- Out of scope: CAD, 9-1-1, radio controllers, AVL, production body-worn video, live case content.
- Stop conditions: Any operations report of RMS slowness affecting field queries during major incidents; any access to non-synthetic cases; any spill into CAD credentials.

### Select

1. Valid Accounts - vendor support authentication outside approved window.
2. Account Manipulation or policy change attempt in RMS admin (low-impact setting toggled in lab tenant, or production toggle that is immediately reversed if pre-approved). Prefer lab tenant.

### Safety gate

Engagement Lead, RMS application owner, vendor liaison, and on-duty communications supervisor informed of window (even though CAD is out of scope, supervisors hate surprises). Confirm synthetic case IDs. Window: Tuesday 10:00–11:30 local, outside planned vendor maintenance.

### Execute and observe

1. Red authenticates as vendor test admin via jump host outside the allow-listed support schedule.
2. Blue watches VPN, jump host, and RMS audit feeds.
3. Red opens the synthetic case and exports an allowed test PDF.
4. Red attempts a reversible admin setting change in lab; skips if lab unavailable and marks that step Not testable rather than forcing production change.

### Score (format example)

- After-hours vendor auth: Detected - weak (log + low-severity alert; no on-call page).
- Synthetic case export: Logged only (application audit present; no SIEM alert).
- Admin setting change: Not testable (no lab tenant that week).

### Backlog

- `PT-PS-006`: Page on-call for vendor account auth outside support window. Owner: Agency SOC + identity. Acceptance: retest; page within 5 minutes.
- `PT-PS-007`: SIEM rule for bulk or admin exports from RMS by vendor roles. Owner: Detection eng with vendor log field dictionary. Acceptance: synthetic export alerts within 20 minutes.
- `PT-PS-008`: Provision RMS lab tenant for admin-change tests. Owner: Vendor + application owner. Acceptance: next cycle executes admin change procedure.

### Executive summary ask

Approve on-call paging for vendor account anomalies; accept temporary manual log review for exports until `PT-PS-007` ships; fund lab tenant to avoid production admin testing.

---

*Public Safety Tech overlay v1.0 - use with the core framework. CC BY 4.0*
