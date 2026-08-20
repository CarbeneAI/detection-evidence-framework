# Purple Team Framework

**Author:** Clint P. Garrison / CarbeneAI
**License:** [CC BY 4.0](LICENSE)
**Version:** 1.1

A teaching framework for purple team engagements that produce detection evidence you can defend, and a backlog that closes.

The core cycle (Sections 1 through 10) is sector-neutral. Read it first. It stands alone. Sections 11 through 14 apply the same cycle when healthcare, financial services, or public safety technology change scope, safety, or evidence handling. They do not add a second methodology.

---

## Contents

- [1. What purple teaming is](#1-what-purple-teaming-is)
- [2. Roles and RACI](#2-roles-and-raci)
- [3. The engagement cycle](#3-the-engagement-cycle)
- [4. Choosing and prioritizing ATT&CK procedures](#4-choosing-and-prioritizing-attck-procedures)
- [5. Measuring detection coverage honestly](#5-measuring-detection-coverage-honestly)
- [6. Turning findings into a detection backlog](#6-turning-findings-into-a-detection-backlog)
- [7. Cadence](#7-cadence)
- [8. Executive reporting](#8-executive-reporting)
- [9. Evidence package](#9-evidence-package)
- [10. Non-goals](#10-non-goals)
- [11. Applying the cycle in a regulated sector](#11-applying-the-cycle-in-a-regulated-sector)
- [12. Healthcare](#12-healthcare)
- [13. Financial services](#13-financial-services)
- [14. Public safety technology](#14-public-safety-technology)

---

## 1. What purple teaming is

Purple teaming is a structured practice where offensive operators and defensive operators work the same scenario at the same time, with a shared goal: prove which attacker behaviors your controls detect, which they miss, and what you will change next.

It is not a rebranded penetration test. A penetration test answers "can someone get in?" Purple teaming answers "when a specific behavior happens, what do we see, how fast, and what do we do?"

It is not a tabletop alone. Tabletop exercises test decision-making. Purple teaming tests instrumentation, telemetry, and response under controlled technical conditions.

### Why the practice exists

Security programs accumulate tools faster than they accumulate proof. Dashboards report "coverage." Vendors map product features to MITRE ATT&CK technique IDs. Neither proves that a real behavior on your network produces an alert your people trust and act on.

Each cycle should produce: a short procedure list chosen for stated reasons; observed detection outcomes; backlog items for misses and weak hits worth fixing; and an executive summary of what improved and what remains open.

If a cycle does not change a detection, a playbook, or a prioritized backlog, it was a demo, not an engagement.

### Derivation: why offense and defense must share the scenario

Offense without defense produces findings that sit in a PDF. Defense without offense tunes rules against imagined threats. Shared scenarios force both sides to use the same facts: the same host, the same timestamp, the same procedure, the same expected artifact. That shared fact set is the unit of learning. The rest of this framework exists to protect it from scope creep, unsafe testing, and dishonest metrics.

---

## 2. Roles and RACI

Keep roles few. More titles do not create more learning.

| Role | Accountable for | Typical background |
|------|-----------------|-------------------|
| Engagement Lead | Scope, safety gates, timeline, final report | Security manager or senior engineer |
| Red Operator | Executing agreed procedures within scope | Offensive security, adversary emulation |
| Blue Operator | Observing telemetry, validating alerts, documenting detection state | Detection engineering, SOC, IR |
| Control Owner | Approving changes to systems under test; owning backlog items that touch their stack | Platform, identity, endpoint, or network owner |
| Executive Sponsor | Authority to pause production risk; receives the summary | CISO or delegate |

### RACI for a single cycle

| Activity | Engagement Lead | Red | Blue | Control Owner | Sponsor |
|----------|-----------------|-----|------|---------------|---------|
| Approve scope and safety limits | A | C | C | C | I |
| Select ATT&CK sub-techniques or named procedures | A | R | C | C | I |
| Execute procedures | A | R | C | I | I |
| Record detection outcomes | A | C | R | I | I |
| Write detection backlog items | A | C | R | C | I |
| Accept backlog into work queue | C | I | C | A | I |
| Executive summary | R | C | C | C | A |

R = Responsible, A = Accountable, C = Consulted, I = Informed.

### Derivation: why Control Owner is not optional

A detection that nobody owns does not ship. Purple teaming that ends in "Blue should write a rule" without naming the team that can change the SIEM, EDR, or identity platform produces theater. The Control Owner accepts or rejects each backlog item. Rejection is allowed. Silence is not.

---

## 3. The engagement cycle

Run the cycle as six phases. Do not skip the safety gate or the backlog phase.

```
Scope → Select → Safety Gate → Execute & Observe → Score → Backlog & Report
```

### 3.1 Scope

Write a one-page scope before any procedure runs.

Required fields:

- **Objective:** One sentence. Example: "Determine whether lateral movement via stolen credentials from a standard user workstation to a file server produces a usable alert within 15 minutes."
- **In-scope assets:** Named hosts, accounts, network segments, or cloud projects.
- **Out-of-scope assets:** Explicit list. If it is not listed as in-scope, it is out.
- **Allowed time window:** Start and end. Include timezone.
- **Data handling rules:** What logs, screenshots, and artifacts may leave the environment; how long they are retained; who may view them.
- **Stop conditions:** Conditions that halt the exercise immediately (patient-impact risk, payment outage risk, life-safety system involvement, unexpected privilege gain, data exfiltration beyond the agreed test file).
- **Success definition for the cycle:** What "done" means. Prefer detection and response outcomes over "Red got domain admin."

### 3.2 Select procedures

See Section 4. Exit this phase with a short list. A teaching default is 3 to 7 sub-techniques or named procedures. Shorter is fine. A longer list that crowds out scoring and backlog is a failed Select, not a more complete cycle.

### 3.3 Safety gate

The Engagement Lead and Control Owner sign (electronically is fine) that:

1. Scope is accurate for the current environment.
2. Stop conditions are understood by Red and Blue.
3. Production impact risk is accepted for the stated window, or the test will run in a dedicated environment that mirrors production enough for the procedure under test.
4. Rollback or containment steps are known before execution starts.

If any item fails, do not execute.

### 3.4 Execute and observe

Red executes one procedure at a time when practical. Blue watches agreed consoles and queries in parallel. Both sides timestamp actions and observations against a shared clock (NTP-synced or agreed reference).

For each procedure, capture:

- **Action:** What Red did (command class or procedure, not a novel exploit dump unless that is the point).
- **Expected artifact:** What should appear in logs or telemetry if instrumentation works.
- **Observed artifact:** What actually appeared, where, and when.
- **Alert:** Fired / did not fire / fired with wrong severity or incomplete context.
- **Response:** Did an analyst or automated playbook act? Within what time?

Do not grade on Red creativity. Grade on whether the environment told the truth about the behavior.

### 3.5 Score

Use the scoring model in Section 5. Record outcomes per sub-technique or named procedure. Do not score the parent technique. Do not average outcomes into a single "purple score." Averages hide misses.

### 3.6 Backlog and report

Convert every miss and every weak hit into a backlog candidate (Section 6). Publish a short executive summary (Section 8). Archive the evidence package with retention rules from scope.

---

## 4. Choosing and prioritizing ATT&CK procedures

MITRE ATT&CK is a knowledge base of adversary behaviors, organized by tactic (goal), technique, and sub-technique. It is useful because it gives Red and Blue a shared vocabulary. It is dangerous when teams treat the matrix like a checklist to color in.

### Derivation: what ATT&CK is for in purple teaming

ATT&CK helps you name the behavior you are testing. It does not tell you which behaviors matter for your organization. That answer comes from your threat model, your crown-jewel processes, and your recent incidents or near-misses.

Score at the **sub-technique** when ATT&CK defines one (T1003.001 LSASS Memory, not T1003 OS Credential Dumping). Score at a **named procedure** when you are testing a specific implementation of a technique that has no finer ID, or when the implementation is the point (for example, "SMB collection of two synthetic files from `\\filesrv01\dept-share` using a domain user," recorded as T1039). Detecting one procedure does not detect its parent technique.

### Selection method

Use four filters in order. Stop when you have enough procedures for one cycle.

**Filter 1 - Relevance to your threat model.**
Ask: Would a capable adversary who wants our specific outcomes (fraud, ransomware payment, theft of regulated records, disruption of field operations) plausibly use this procedure against us? If you cannot argue yes from first principles or from observed activity you can point to, drop it for this cycle.

**Filter 2 - Detection uncertainty.**
Prefer procedures where you are unsure of the outcome. Testing something you already know is loudly detected wastes the cycle. Testing something you have never instrumented teaches more.

**Filter 3 - Blast radius you can contain.**
If the procedure cannot be executed safely under your stop conditions, substitute a narrower procedure that still exercises the same detection surface, or move the test to a lab that mirrors the control plane.

**Filter 4 - Teaching order, not a dependency.**
ATT&CK tactics group behaviors. They are not a linear kill chain and they are not prerequisites for one another. Adversaries skip, repeat, and start in the middle.

For teaching a detection program, a useful default is to validate earlier-stage procedures (credential access, remote services) before late-stage impact (for example, T1486 Data Encrypted for Impact) so you learn whether you would have seen the path. That is a teaching order. It is not a claim that impact depends on those steps. It is not a reason to refuse a scoped impact test when that is the question leadership asked, and the safety gate passes.

### Prioritization inside the short list

Rank remaining procedures by:

1. **Impact if missed** (would a miss leave a path to your highest-value asset or outcome?).
2. **Cost to fix if missed** (is the likely fix a rule, a sensor gap, or an architecture change?).
3. **Time to execute and observe** (fit the window).

Write the priority reason next to each ATT&CK ID. "Because it is popular" is not a reason. "Because our identity logs do not currently record this authentication pattern and ransomware crews need it for lateral movement" is a reason.

### Map the procedure you ran, not the cell you wish you had

If the procedure does not match the technique definition, do not use that ID.

- Remote access with valid credentials is **T1078 Valid Accounts** and **T1133 External Remote Services**. It is not a single catch-all ID.
- Listing files is **T1083 File and Directory Discovery**. Collecting those files from a host is **T1005 Data from Local System**. Collecting them from a share is **T1039 Data from Network Shared Drive**. Discovery is not collection.
- An application admin toggling a records-system setting is not **T1098 Account Manipulation** unless the change actually manipulates an account. If the change disables audit or a security tool, **T1562 Impair Defenses** (sub-technique when one fits) is the honest ID. If no ID fits, write a named procedure and leave the ID blank rather than forcing one.

### What not to do with ATT&CK

- Do not chase 100% technique "coverage." ATT&CK is not a finite exam.
- Do not equate a vendor's ATT&CK heatmap with your detection capability. A heatmap describes product features unless you have proven the alert in your tenant.
- Do not test only noisy procedures that always alert. That inflates scores.
- Do not collapse several procedures into one parent-technique score.

---

## 5. Measuring detection coverage honestly

### Outcome labels

For each sub-technique or named procedure you execute, assign one primary outcome:

| Outcome | Meaning |
|---------|---------|
| **Prevented** | Control blocked the behavior before meaningful progress. Prevention is success if intentional. Note whether an alert also fired. |
| **Detected-actionable** | A signal fired in time for response within your stated SLO, and a human or playbook that can stop the activity could act on it. A correct ATT&CK tag is useful for reporting. It is not required for this label. A page that names the host, user, and action in time is actionable even if the technique ID is wrong or missing. |
| **Detected-weak** | Something logged or alerted, but severity, mapping, context, or timing made it unlikely an analyst would act correctly. A wrong ATT&CK tag can contribute to weakness. It is not the definition of weakness. |
| **Logged only** | Telemetry exists; no alert; discovery would require hunting that did not happen during the window. |
| **Missed** | No useful telemetry and no alert for the behavior as executed. |
| **Not testable** | Safety gate or environment limits blocked a faithful test. Record why. Do not score as Detected. |

Do not collapse these labels into a single ratio, percentage, or "confidence" score. Counts by label, plus named misses, are the honest summary.

### What coverage metrics do not prove

State these limits in every report so executives are not misled.

1. **A colored ATT&CK cell does not prove detection.** It may prove a tool claims a feature, or that a single procedure was seen once in a lab.
2. **Alert volume does not prove quality.** High volume can be noise. Do not treat volume as quality.
3. **Prevention without telemetry can hide regressions.** If you only block and never log, you cannot tell when the block fails.
4. **One successful detection does not generalize across procedures.** Detecting T1003.001 LSASS Memory does not prove you detect every procedure under T1003 OS Credential Dumping.
5. **Purple team success does not prove resilience to a patient adversary.** Real attackers adapt. Your cycle tested specific behaviors under consent.
6. **Mean-time-to-detect on a synthetic test is not your breach MTTD.** On a synthetic test, with consent, a known window, and people watching the glass, the measured time is optimistic. It understates how long the same behavior could sit unnoticed in a real incident.

### Honest summary format

Report counts by outcome label (Prevented, Detected-actionable, Detected-weak, Logged only, Missed, Not testable). List misses and weak hits with backlog IDs. Do not bury them under a percentage. Numbers in examples in this document illustrate format only. They are not measurements of any named environment.

---

## 6. Turning findings into a detection backlog

Every weak hit and miss that you choose to fix becomes a backlog item with enough detail for an engineer who was not in the room.

### Required fields

- **ID:** Stable identifier (for example, `PT-2026-041`).
- **Procedure:** ATT&CK ID at sub-technique when one exists, plus the named procedure as tested.
- **Outcome:** Detected-weak / Missed / Logged only.
- **Evidence pointer:** Link or path to timestamps, host names (or aliases), query used, screenshot hash, or ticket attachment. Do not paste secrets.
- **Hypothesized gap:** Sensor missing, log not collected, rule absent, rule too narrow, enrichment missing, routing failure, or playbook gap.
- **Proposed change:** Concrete. "Add alert on X when Y" beats "improve detection."
- **Owner:** Named Control Owner or team.
- **Priority:** Critical / High / Medium / Low, with a one-line reason tied to impact if missed.
- **Acceptance test:** The exact purple retest that will prove the fix. Schedule it.
- **Status:** New / Accepted / Rejected / In progress / Retested-pass / Retested-fail.

### Derivation: why acceptance tests matter

Without a retest, "fixed" means "someone believes they deployed a rule." Purple teaming's value compounds when backlog items return to Execute & Observe. Close the loop or admit the item is still open.

### Rejection is a valid outcome

Control Owners may reject a backlog item when cost exceeds risk, when compensating controls already cover the path, or when the test procedure was unrealistic. Record the rejection reason. Do not silently drop it.

---

## 7. Cadence

Pick a cadence your organization can staff. Consistency beats intensity.

Teaching defaults, not a survey of the industry:

- Monthly micro-cycles (1 to 3 procedures) when a detection team can staff them.
- Quarterly cycles (5 to 7 procedures plus retests) when that is what you can staff.
- Extra cycles after major control changes (new EDR, SIEM migration, identity redesign) and after incidents or near-misses. Keep those technical, not blame-focused.

Minimum healthy pattern: plan from threat model and open retests; execute in a fixed window; ship Critical and High backlog items within an agreed SLA; retest them next cycle. If you cannot retest, reduce new procedure volume until the backlog moves.

---

## 8. Executive reporting

Executives need decisions, not ATT&CK tourism.

### One-page summary structure

1. **Objective and window** (2 to 3 lines).
2. **Outcome counts** using the labels in Section 5.
3. **Top risks still open** (misses and weak hits that matter to crown jewels), each with owner and target date.
4. **What changed since last cycle** (retests passed or failed).
5. **Ask** (budget, access, freeze exception, or acceptance of residual risk). One ask maximum.

### Language rules for reports

Say what you tested, not what you wish were true. Prefer "We did not detect T1039 on host H within 15 minutes" over "coverage gaps exist." Do not invent breach statistics. If you cite a figure, name the primary source; otherwise omit it. Do not present vendor heatmaps as organizational capability.

---

## 9. Evidence package

Retain enough to defend the work under audit or after staff turnover: signed scope and safety gate; procedure list with ATT&CK IDs and priority reasons; per-procedure run log (timestamps, operators, outcome label); queries or rule IDs checked; backlog export; executive summary. Apply data minimization. Redact secrets, personal data, and irrelevant host details. If a sector section in this document adds handling rules, apply those too.

---

## 10. Non-goals

This framework does not replace penetration testing, red team operations, or bug bounty programs; does not rank vendors; does not guarantee regulatory compliance by itself; and does not provide exploit code or step-by-step abuse instructions for production systems.

It teaches how to derive an honest detection program from shared scenarios, measured outcomes, and a backlog that closes.

The core cycle ends here. Sector sections below are optional. They do not change the labels, the scoring unit, or the backlog fields.

---

## 11. Applying the cycle in a regulated sector

Run Sections 1 through 10 as written. If you operate in healthcare, financial services, or public safety technology, read the matching section before you write Scope. Add that section's constraints to scope, stop conditions, and evidence handling.

Do not let a compliance narrative replace outcome scoring. A colored ATT&CK matrix is not evidence that a control operated. Prefer timestamps, ticket IDs, owners, and retest results.

Worked scenarios in Sections 12 through 14 are format examples. They are not measurements of any named organization.

---

## 12. Healthcare

This section applies the six-phase cycle when a failed test can delay care or expose patient data. If those two harms are not in play, skip this section. The cycle does not change. The contents of Scope, Safety, Execute, and the evidence package do.

Do not claim a purple cycle makes an organization compliant with the HIPAA Security Rule. Compliance is a program. Purple teaming produces evidence about specific procedures.

### 12.1 Scope

Fill every field in Section 3.1. In this sector those fields carry extra meaning.

**Objective.** Name the detection question, not a clinical outcome. "Prove whether a stolen domain account on the remote-access gateway, followed by collection from a department share of synthetic records, produces an actionable alert within 30 minutes" is an objective. "Prove we are safe from ransomware" is not.

**In-scope / out-of-scope.** Name hosts, accounts, and shares. Put production EHR, medical devices that touch patients, backup appliances used for recovery of care systems, and break-glass accounts on the out-of-scope list unless a written clinical and security exception exists. If it is not listed as in-scope, it is out.

**Data handling.** The HIPAA Security Rule (45 CFR Part 164 Subpart C) requires administrative, physical, and technical safeguards for electronic protected health information (ePHI). The Privacy Rule's minimum necessary standard (45 CFR 164.502(b)) is a teaching default for purple artifacts: keep only what Blue needs to score the procedure. Technical safeguards include audit controls (45 CFR 164.312(b)); prefer proving that audit logs recorded the test behavior.

Consider keeping evidence that might contain ePHI under the same access rules you already use for ePHI: workforce members or business associates with a job need, not personal laptops, not open tickets. Crop screenshots to alert metadata. Do not paste chart content, names, medical record numbers, or addresses.

Use synthetic patients and synthetic files. If a system cannot support synthetic data, constrain the test to authentication and authorization paths that do not require opening charts.

**Stop conditions.** Add any report of clinical workflow failure; lockout of a non-test shared clinical account; discovery of real ePHI in a synthetic share (halt and treat as an incident); unexpected involvement of a device used in active care.

**Success.** Detection and response outcomes. Taking down a clinic to prove Red can do it is not success.

### 12.2 Select

Argue from your asset list and incentives, not from a sector statistic. Care disruption creates urgency for an operator who wants payment. Bulk records have secondary value. Remote access and valid accounts sit on both paths. If those outcomes matter, test those procedures. If you have your own incident data, prefer procedures from that data.

Name each item at sub-technique or named procedure. Two honest pairs for this sector:

| ATT&CK ID | Name | Why it belongs on a short list |
|-----------|------|--------------------------------|
| T1133 | External Remote Services | Remote-access gateways are how off-network accounts become on-network. |
| T1078.002 | Valid Accounts: Domain Accounts | Stolen or reused domain credentials are the procedure, not "initial access" as a blob. |
| T1083 | File and Directory Discovery | Listing a share is discovery. Score it only if you ran a listing procedure. |
| T1039 | Data from Network Shared Drive | Opening or copying files from a share is collection. Do not score collection as T1083. |
| T1005 | Data from Local System | Collection from a workstation disk is a different procedure than collection from a share. |

Priority reason next to each ID. Uncertainty is a valid reason: "We do not know whether geo/velocity detections fire for clinical staff who travel, and we do not know whether share-collection alerts exist for this department share."

### 12.3 Safety gate

Engagement Lead and Control Owner still sign. Add the people who can speak for care delivery when the window touches clinical networks: clinical engineering for devices, clinical informatics or the application owner for systems near charts, IAM for account lockout risk.

Teaching defaults for exclusions (not an industry survey). Redesign the test, move it to a lab, or mark **Not testable** rather than run these in production:

1. Life-support, monitoring, infusion, imaging in active use, and other devices where failure harms patients. Do not scan aggressively, credential-test, or implant test tooling on these devices as part of purple teaming.
2. Ransomware detonation on production clinical networks. Emulate precursor behaviors (access, staging, backup targeting) in production only if containment is proven. Detonate encryption in isolated labs.
3. Real ePHI as test bait.
4. Phishing clinicians during peak care hours without explicit workforce and leadership approval. Distraction is a patient-safety issue.
5. Breaking break-glass or emergency access procedures in a way that disables them during the window.
6. Exfiltrating real ePHI off-network, even as a "proof." Prove detection of exfiltration patterns with synthetic payloads.

If Red needs a behavior that is excluded in production, substitute a procedure that hits the same telemetry source in a lab, or record **Not testable** with the safety reason.

Account lockout on a shared clinical workstation is a stop condition. A lockout that blocked a nurse is not a **Prevented** win.

### 12.4 Execute and observe

Run one procedure at a time. Timestamp against the shared clock.

Expected artifacts in this sector include VPN or gateway logs, identity provider sign-ins, SMB or file-audit events, and (when in scope) DLP or exfiltration signals. They do not include chart contents.

If SIEM queries return names, medical record numbers, or addresses, redact before the evidence store. Reset test credentials after the window and document that reset.

### 12.5 Score

Use the six labels. Score T1133 separately from T1078.002, and T1083 separately from T1039. An alert on anomalous gateway authentication that names the user and time can be **Detected-actionable** for T1133 even if the alert's ATT&CK field is blank or wrong. Collection that only appears in SMB audit with no alert is **Logged only** for T1039, not a pass for "file activity."

Do not score a parent technique because one child alerted.

### 12.6 Backlog and report

Backlog fields are those in Section 6. You may label a backlog item with a Security Rule safeguard theme (access control, audit, integrity, availability) when that helps an internal compliance reader find it. That label is navigation. It is not a certification.

Consider keeping in the evidence package, in addition to Section 9:

- Confirmation that synthetic data was used, or a written justification and approval if not.
- Clinical stakeholder acknowledgment for any test near care delivery systems.
- The list of systems excluded for patient-safety reasons.

Those items help you show how the control was tested and what was deliberately not tested. They are a teaching default for the package, not a prediction of any investigation.

### 12.7 Worked scenario (format example)

This is a completed cycle packet. It is not a claim about a named hospital.

**Scope (one page)**

- Objective: Determine whether use of a stolen domain account on the clinical remote-access gateway (T1133, T1078.002), followed by collection of two synthetic referral PDFs from a department file share (T1039), produces an actionable alert within 30 minutes.
- In scope: Remote-access gateway logs, identity provider sign-in logs, one Windows file server used for department shares, synthetic document set tagged as synthetic, one standard user test account in a clinical AD group.
- Out of scope: Production EHR, medical devices, backup appliances, domain controllers (observe only; no intentional DC compromise), break-glass accounts.
- Window: Sunday 09:00 to 12:00 local.
- Data handling: Synthetic files only. Screenshots cropped to alert metadata. Evidence access limited to named Blue operators and the Engagement Lead. Retention 90 days then delete.
- Stop conditions: Any clinical ticket about gateway outage; lockout of non-test accounts; real ePHI found in the synthetic share; any medical device in the traffic path.
- Success: Outcome labels and timestamps for T1133, T1078.002, and T1039. Not "Red reached the share."

**Select**

| ID | Procedure as tested | Priority reason |
|----|---------------------|-----------------|
| T1133 | Authenticate the test account to the remote-access gateway from an approved lab egress that is anomalous for this user. | Unknown whether gateway and IdP detections fire for traveling clinical staff. |
| T1078.002 | Same authentication, scored as use of a valid domain account under anomalous conditions. | Separate scoring unit from T1133. A gateway alert is not an account-misuse alert. |
| T1039 | Copy two synthetic referral PDFs from the department share to the test host. | Unknown whether file-collection on this share alerts, or only lists. |

T1083 is not on this list because the question is collection, not listing. If Red lists the share as a distinct step you want scored, add T1083 as a fourth procedure.

**Safety gate**

Signed by Engagement Lead, IAM owner, file-server Control Owner, and clinical informatics liaison. Synthetic data verified. Rollback: disable the test account; revoke the gateway session.

**Execute and observe (run log)**

1. Red authenticates the test account to the gateway from the lab egress. Blue watches gateway and IdP consoles.
2. Red copies two synthetic PDFs from the department share. Blue watches file-audit and SIEM.

**Score (format only)**

| Procedure | Outcome | Note |
|-----------|---------|------|
| T1133 External Remote Services | Detected-weak | Alert fired; severity low; no identity context; no page. |
| T1078.002 Domain Accounts | Logged only | IdP sign-in recorded; no account-misuse alert. |
| T1039 Data from Network Shared Drive | Logged only | SMB audit present; no alert. |

**Backlog**

- `PT-H-001`: Raise severity and add device/geo context for anomalous gateway use on accounts in clinical AD groups. Owner: IAM. Acceptance: retest T1133; expect Detected-actionable within 15 minutes.
- `PT-H-002`: Alert on collection volume by interactive users on shares classified as containing ePHI (synthetic tag in lab; production classification in prod). Owner: Server platform and detection engineering. Acceptance: retest T1039 against the stated threshold.

**Executive ask**

Accept residual risk for 30 days while `PT-H-001` ships, or add a temporary watchlist for clinical gateway accounts. One ask.

---

## 13. Financial services

This section applies the cycle when customer financial data, payment flows, or examiner-facing evidence are in play. Banks, credit unions, payments companies, fintechs, and service providers in that scope can use it. The cycle does not change.

Do not claim a purple cycle replaces a PCI Report on Compliance, a self-assessment, or a regulatory examination.

### 13.1 Scope

Label every in-scope asset as **PCI in-scope**, **connected to the cardholder data environment (CDE)**, or **out of PCI scope**. Do not move cardholder data into out-of-scope tools for the exercise. Prefer documented test PANs and test card data over any live PAN.

If a procedure requires action inside the CDE, follow the change control you already use for that environment (approvals, logging, restricted admin paths).

Name fraud operations as Consulted during Select and Score when procedures touch customer authentication, sessions, or payment initiation. Record which console is supposed to produce the signal. Success is a detection question, not a payout.

Stop conditions include live authorization-path degradation, unexpected production credentials, and any encounter with live cardholder data (halt, treat as incident, rotate as required).

### 13.2 Select

Account takeover and privileged misuse sit on the path to fraud. Testing password login alone misses session and token reuse.

Honest IDs for a cloud-console session replay plus a lab payout-rule change:

| ATT&CK ID | Name | What you are actually testing |
|-----------|------|-------------------------------|
| T1550.004 | Use Alternate Authentication Material: Web Session Cookie | Replay of a session cookie or refreshed token from an unexpected network path. Theft of the cookie, if you run it, is T1539 and a separate score. |
| T1078.004 | Valid Accounts: Cloud Accounts | Use of a payments-ops cloud role. Score separately from the cookie replay if both run. |
| T1565.001 | Stored Data Manipulation | Modify a payout configuration object at rest in the lab account, then roll it back. |

Do not record a payout-rule change as T1098 unless you manipulated an account.

### 13.3 Safety gate

Default exclusions (teaching defaults):

1. Live payment processing disruption. No intentional latency injection, host reboots, or network blocks on authorization paths during business processing without executive and operations approval and a tested rollback.
2. Real cardholder data as proof.
3. Production HSM abuse or key material extraction attempts. Test access controls and monitoring around key ceremonies and admin paths in approved ways. Do not extract keys for purple learning.
4. Customer-notification-triggering events (mass password resets, false fraud freezes on real customers) without a customer-impact plan.
5. Crossing into another legal entity's environment (partner bank, processor) without written authorization naming the window and procedures.
6. Destroying audit logs or disabling logging to "see if anyone notices." Test monitoring of log stops with staged, reversible controls in non-production first.

Mark procedures that cannot be made safe as **Not testable** and raise compensating monitoring through risk management.

### 13.4 Execute and observe

Invite fraud operations to watch their console in parallel with Blue. Record which queue saw the signal. "Detected-actionable" means a human or playbook that can stop loss saw it in time. A log line in an unused index is **Logged only**. A SOC alert that fraud never received, when fraud is the queue that can freeze the payout, is **Detected-weak** or a routing miss; name the owning queue on the backlog item.

### 13.5 Score

Same six labels. Same rule: do not collapse them into a ratio. PCI DSS v4.0.1 includes requirements for logging, monitoring, and intrusion detection inside the CDE. Purple outcomes can support evidence that those controls operated for the procedure you ran. They do not satisfy v4.0.1 by themselves.

### 13.6 Backlog and report

Name the owning queue (fraud case management vs SOC) on every item.

U.S. federal banking regulators examine information security as part of safety and soundness. The FFIEC IT Examination Handbook is a public reference used in U.S. banking examinations. This framework does not interpret it for your charter.

When an examination samples information security, the sample is policy plus proof the policy operates. Consider keeping, in addition to Section 9:

- Procedure-to-system mapping with PCI scope status.
- Ticket numbers in the system of record you already use for findings.
- Control Owner sign-off, and fraud operations sign-off when they were Consulted.
- Retention aligned with your existing record-retention policy.

Weak artifacts: "EDR covers ATT&CK," a green status slide, a tabletop PDF alone, verbal PCI assurance. Stronger artifacts: alert ID, time, user, ticket, and outcome for a privilege-use test; a backlog item with owner, date, and retest result.

Do not invent industry-average MTTD charts.

### 13.7 Worked scenario (format example)

**Objective:** Determine whether replay of a stolen cloud console session cookie for a payments-ops role (T1550.004) produces an actionable alert in Security or Fraud within 15 minutes, before a simulated payout rule change (T1565.001).

**Why:** Password-login tests miss token and session detections. Privileged stored-config change is the loss-facing action.

**Scope highlights**

- In scope: Non-production cloud organization mirroring production IAM; test payments-ops role; SIEM ingestion of cloud audit logs; fraud notification channel if wired in lab.
- Out of scope: Production CDE; production payout rails; customer accounts; HSMs.
- PCI note: Lab is out of PCI scope; still forbid live cardholder data.
- Stop conditions: Any indication the session touches production credentials; logging pipeline outage in a shared production SIEM.

**Select**

| ID | Procedure as tested | Priority reason |
|----|---------------------|-----------------|
| T1550.004 | Replay a lab session cookie from an unexpected network path. | Unknown whether session reuse alerts for payout-capable roles. |
| T1565.001 | Submit a payout configuration change in the lab account; roll back immediately. | Unknown whether Fraud is routed when SOC already alerts. |

**Safety gate**

Engagement Lead, Cloud IAM owner, Fraud operations lead. Compliance informed for evidence retention. Window: Wednesday 14:00 to 16:00 local in lab.

**Execute and observe**

1. Red obtains a lab session token via the agreed method (phishing simulation against a volunteer, or admin-issued token for the test).
2. Red replays the session from an unexpected network path.
3. Blue and Fraud watch cloud audit alerts and fraud hooks.
4. Red proposes a payout rule change; stops at submit if an alert is already actionable; otherwise submits in lab and rolls back.

**Score (format only)**

| Procedure | Outcome | Note |
|-----------|---------|------|
| T1550.004 Web Session Cookie | Missed | Audit log present; no alert routed. |
| T1565.001 Stored Data Manipulation | Detected-actionable | Cloud audit alert to SOC in time. Fraud console silent. Actionable for SOC; still a routing gap for Fraud. |

The SOC alert is Detected-actionable for T1565.001 even if it lacks an ATT&CK tag. Silence in Fraud is a backlog item, not a reason to downgrade a page that already stopped the change.

**Backlog**

- `PT-F-014`: Alert on session reuse / impossible travel for roles with payout permissions; page SOC and notify the Fraud queue. Owner: Detection engineering and Fraud. Acceptance: retest T1550.004; both queues receive a signal within 15 minutes.
- `PT-F-015`: Add Fraud routing for payout configuration changes even when SOC already alerts. Owner: Fraud systems. Acceptance: a single T1565.001 change creates linked cases in both systems.

**Executive ask**

Fund engineering time for dual-route alerting; accept residual risk on session reuse until `PT-F-014` ships, with a temporary watch on payout-capable roles.

---

## 14. Public safety technology

This section applies the cycle to technology environments that support law enforcement, fire, EMS, emergency communications, courts, and vendors building software or infrastructure for those missions. Life-safety continuity and evidence integrity outrank detection learning. If a test can delay response to a call for service, redesign the test.

Do not claim this section makes an environment "CJIS certified."

### 14.1 Scope

Public safety systems may handle Criminal Justice Information (CJI) or data that must be protected under CJIS Security Policy requirements when the system is in a CJIS audit boundary. Even when a system is not in that boundary, treat investigative data, CAD narratives, body-worn camera metadata, and NCIC query results as high sensitivity.

Need to know: limit purple evidence access to personnel authorized under agency policy.

Media control: screenshots, exports, and packet captures from justice systems follow evidence-handling rules, not casual ticket attachments. Keep purple evidence in a repository separate from evidentiary evidence lockers unless policy explicitly unifies them. Document hash, time, handler, and storage location for any retained capture that could be mistaken for investigative material.

Use designated test cases and test records. Do not alter production evidence stores.

Vendors running purple tests need written authorization that names systems, windows, and data rules.

**Field devices.** Vehicles, radios, tablets, offline caches, and intermittent connectivity are in-scope realities. Detections built only for always-online corporate laptops miss the field. Ask: Does telemetry leave the device when offline, and what happens on reconnect? Are MDM and endpoint signals delayed? Score time-to-detect with field-realistic delay, not lab LAN timing. Does VPN failure fail open or closed for the workflow under test? The safety gate must know before testing.

Stop conditions include any operations report that RMS slowness is affecting field queries during major incidents; access to non-synthetic cases; any spill into CAD credentials; any degradation of 9-1-1, dispatch, or radio.

### 14.2 Select

Vendor remote support exists because RMS and CAD vendors require a path in. That design fact makes vendor accounts a high-value procedure to test. It is a teaching reason from access design, not a statistic.

Honest IDs for vendor after-hours access plus records collection plus a lab audit-disable attempt:

| ATT&CK ID | Name | What you are actually testing |
|-----------|------|-------------------------------|
| T1133 | External Remote Services | Vendor VPN or jump host. |
| T1078 | Valid Accounts | Vendor support account. Use T1078.002 if it is a domain account. |
| T1213 | Data from Information Repositories | Open a synthetic RMS case and export an allowed test PDF. Score the export as repository collection, not as T1083 (discovery) and not as T1098 (account manipulation). If your mapping standard uses a different ID for application data export, write the named procedure and that ID. |
| T1562.001 | Impair Defenses: Disable or Modify Tools | Attempt to disable RMS audit logging in a lab tenant. An RMS admin display toggle is not T1098 Account Manipulation. |

### 14.3 Safety gate

Computer-aided dispatch (CAD), 9-1-1 call handling, radio console systems, station alerting, and medical devices in EMS contexts are life-safety adjacent. Availability is the primary control objective.

Default exclusions (teaching defaults):

1. Any test that can drop, delay, or degrade 9-1-1 call taking, dispatch, or radio for live operations. No port scans of call-handling hosts during operations, no credential lockouts for dispatcher accounts, no chaos experiments on production telephony.
2. Ransomware or destructive testing on networks that path to dispatch or records systems used for active incidents. Use isolated labs with mirrored control planes.
3. Implanting test malware on devices that leave for the field the same day unless the device is a dedicated test unit and cannot be assigned to live duty until cleaned and verified.
4. Using real investigative case content as test data. Build synthetic cases. If a system requires real record structures, scrub and tokenize under records management approval.
5. Techniques that falsify location, unit status, or AVL in production in ways that could misroute responders.
6. Breaking encryption or attempting to bypass evidence-grade signing on body-worn or in-car video systems.

If leadership asks for assurance on a life-safety system, prefer tabletop plus configuration review; parallel test-environment failover drills under operations control; or read-only detection validation (confirm logs for admin access) without offensive execution.

Inform the on-duty communications supervisor of the window even when CAD is out of scope. Supervisors should not learn about the test from an alarm.

Mark unsafe procedures **Not testable** and document residual risk in the agency risk register.

### 14.4 Execute and observe

Prefer procedures that validate whether access to sensitive records is logged and reviewable. Chain of custody: if purple teaming produces artifacts that could later be questioned in court or internal affairs, handle them as Section 14.1 states.

If the lab tenant for T1562.001 is unavailable, skip that step and mark it **Not testable**. Do not force a production admin change to fill the cell.

### 14.5 Score

Same six labels. Score T1133, T1078, and T1213 separately. Field-realistic delay applies to time-to-detect; a lab LAN timestamp that ignores store-and-forward from a vehicle is optimistic in the same sense as Section 5, item 6.

### 14.6 Backlog and report

Oversight may include CJIS audits, state criminal justice information auditors, city or county IT risk committees, accreditation bodies, and grantors. This framework does not speak for them.

Consider keeping, in addition to Section 9: authorization for the window; systems touched and life-safety exclusions; proof of synthetic data; alert and log evidence for admin and remote access into justice applications; backlog items in the agency tracker with owners; retest results after patches or rule changes.

Less useful: ATT&CK heatmaps with no agency outcomes; vendor marketing without timestamps in your tenant; screenshots with real subject names, victim data, or case narratives.

### 14.7 Worked scenario (format example)

**Objective:** Determine whether privileged admin access to a records-management system (RMS) from a vendor support account, outside the agreed support window, produces an actionable alert within 20 minutes, without touching CAD or telephony.

**Why:** Vendor remote access is required by the support model. Validating monitoring of vendor accounts exercises identity, jump host, and application audit logs. CAD stays out of scope to protect life-safety.

**Scope highlights**

- In scope: Vendor jump host, VPN, RMS application admin audit logs, SIEM, one synthetic case file, vendor test admin account.
- Out of scope: CAD, 9-1-1, radio controllers, AVL, production body-worn video, live case content.
- Stop conditions: Any operations report of RMS slowness affecting field queries during major incidents; any access to non-synthetic cases; any spill into CAD credentials.

**Select**

| ID | Procedure as tested | Priority reason |
|----|---------------------|-----------------|
| T1133 | Authenticate via vendor jump host / VPN outside the allow-listed support schedule. | Unknown whether after-hours vendor remote access pages anyone. |
| T1078 | Use of the vendor support account for that authentication. | Separate scoring unit from T1133. |
| T1213 | Open the synthetic case and export an allowed test PDF. | Unknown whether repository export by a vendor role alerts in SIEM. |
| T1562.001 | Attempt to disable RMS audit logging in the lab tenant. | Unknown whether impairing RMS audit is visible. Prefer lab. If no lab, Not testable. |

**Safety gate**

Engagement Lead, RMS application owner, vendor liaison. On-duty communications supervisor informed of the window. Synthetic case IDs confirmed. Window: Tuesday 10:00 to 11:30 local, outside planned vendor maintenance.

**Execute and observe**

1. Red authenticates as vendor test admin via jump host outside the allow-listed support schedule.
2. Blue watches VPN, jump host, and RMS audit feeds.
3. Red opens the synthetic case and exports an allowed test PDF.
4. Red attempts to disable RMS audit logging in lab; skips if lab unavailable and marks T1562.001 Not testable rather than forcing a production change.

**Score (format only)**

| Procedure | Outcome | Note |
|-----------|---------|------|
| T1133 External Remote Services | Detected-weak | Log plus low-severity alert; no on-call page. |
| T1078 Valid Accounts | Detected-weak | Same alert; no account-specific page. |
| T1213 Data from Information Repositories | Logged only | Application audit present; no SIEM alert. |
| T1562.001 Disable or Modify Tools | Not testable | No lab tenant that week. |

**Backlog**

- `PT-PS-006`: Page on-call for vendor account auth outside the support window. Owner: Agency SOC and identity. Acceptance: retest T1133 and T1078; page within 5 minutes.
- `PT-PS-007`: SIEM rule for admin exports from RMS by vendor roles. Owner: Detection engineering with vendor log field dictionary. Acceptance: retest T1213; synthetic export alerts within 20 minutes.
- `PT-PS-008`: Provision RMS lab tenant for T1562.001 tests. Owner: Vendor and application owner. Acceptance: next cycle executes the audit-disable procedure.

**Executive ask**

Approve on-call paging for vendor account anomalies; accept temporary manual log review for exports until `PT-PS-007` ships; fund the lab tenant to avoid production admin testing.

---

*Purple Team Framework v1.1 - Clint P. Garrison / CarbeneAI - CC BY 4.0*
