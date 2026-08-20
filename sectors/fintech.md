# Sector Overlay: FinTech

**Applies to:** Banks, credit unions, payments companies, fintechs that touch customer financial data or payment flows, and service providers in examiner scope.
**Use with:** [FRAMEWORK.md](../FRAMEWORK.md)
**Author:** Clint P. Garrison / CarbeneAI
**License:** CC BY 4.0

This overlay adapts the core purple team cycle for financial services. Examiner-ready evidence and clear PCI DSS scope boundaries matter as much as detection outcomes. Fraud operations and security operations often split ownership; purple teaming must respect that split without dropping the ball between them.

---

## 1. What changes

### Examiner expectations (OCC, FDIC, FFIEC context)

U.S. federal banking regulators examine information security as part of safety and soundness. FFIEC guidance sets a common language examiners know. You do not need to recite handbook titles in every report. You do need artifacts that show governance (who approved the test and owns residual risk), access-control monitoring (privileged, remote, and authentication anomalies), detection and response with timestamps and ticket IDs, corrective action in the system of record examiners already sample, and third-party log boundaries when a technique crosses a SaaS or processor.

Purple teaming helps when it produces repeatable evidence of control operation, not a one-off demo for a steering committee.

### Fraud and security convergence

Account takeover, mule activity, and social engineering sit between fraud and security. A detection may fire in a fraud console while Security never sees it, or the reverse.

Invite fraud operations as Consulted during Select and Score when techniques touch customer authentication, sessions, or payment initiation. Record which console produced the signal. "Detected" means a human or playbook that can stop loss saw it, not that a log line existed in an unused index. Name the owning queue on backlog items (fraud case management vs SOC). Mis-owned alerts are weak detections.

### PCI DSS scope boundaries

PCI DSS applies to the cardholder data environment (CDE) and connected systems in scope. Purple teaming often spans in-scope and out-of-scope assets. Be explicit:

- Label each in-scope asset as **PCI in-scope**, **connected to CDE**, or **out of scope**.
- Do not move cardholder data into out-of-scope tools "for the exercise."
- Prefer test PANs and documented test card data over any live PAN.
- If a technique requires action inside the CDE, follow change control that PCI assessments already expect (approvals, logging, restricted admin paths).

Purple outcomes can support PCI DSS requirements that ask for logging, monitoring, and intrusion detection in scope. They do not replace a ROC or self-assessment.

---

## 2. What is off limits

Default exclusions:

1. **Live payment processing disruption.** No intentional latency injection, host reboots, or network blocks on authorization paths during business processing without executive and operations approval and a tested rollback.
2. **Real cardholder data as proof.** Use test data. If Red encounters live CHD unexpectedly, stop, treat as incident, rotate as required.
3. **Production HSM abuse or key material extraction attempts.** Test access controls and monitoring around key ceremonies and admin paths in approved ways; do not attempt to extract keys for purple learning.
4. **Customer-notification-triggering events** (mass password resets, false fraud freezes on real customers) without a customer-impact plan.
5. **Crossing into another legal entity's environment** (partner bank, processor) without written authorization naming the window and techniques.
6. **Destroying audit logs** or disabling logging to "see if anyone notices." Test monitoring of log stops with staged, reversible controls in non-production first.

Mark techniques that cannot be made safe as **Not testable** and raise compensating monitoring requirements through risk management.

---

## 3. Evidence examiners actually want

Examiners sample. They ask for a policy, then ask for proof it operates.

Weak artifacts: "EDR covers ATT&CK," a green status slide, tabletop PDF alone, verbal PCI assurance.

Stronger artifacts: alert ID, time, user, ticket, and outcome for a privilege-use test; backlog item in GRC with owner, date, and retest result; purple run log showing detection-to-ticket handoff; asset labels in the scope doc and no CHD in the evidence store.

Add to the core evidence package: technique-to-system mapping with PCI scope status; ticket numbers in the system of record examiners already request; Control Owner sign-off (and fraud ops when relevant); retention aligned with policy and examination needs.

If you show a metric, show your formula from FRAMEWORK.md Section 5. Do not invent industry-average MTTD charts.

---

## 4. Worked scenario (end to end)

**Objective:** Determine whether use of a stolen cloud console session cookie (or refreshed token) for a payments-ops role produces an actionable alert in Security or Fraud within 15 minutes, before a simulated payout rule change.

**Why:** Session theft and privileged misuse sit on the path to fraud. Testing password login alone misses token/session detections.

### Scope highlights

- In scope: Non-production cloud organization mirroring prod IAM; test payments-ops role; SIEM ingestion of cloud audit logs; fraud notification channel (email/chat) if wired in lab.
- Out of scope: Production CDE; production payout rails; customer accounts; HSMs.
- PCI note: Lab is out of PCI scope; still forbid live CHD.
- Stop conditions: Any indication the session touches production credentials; logging pipeline outage in shared prod SIEM.

### Select

1. Use of valid cloud credentials / session reuse under anomalous conditions.
2. Attempt to modify a payout configuration object in the lab account (change is rolled back immediately).

### Safety gate

Engagement Lead, Cloud IAM owner, Fraud ops lead, and Compliance (informed) for evidence retention. Window: Wednesday 14:00–16:00 local in lab.

### Execute and observe

1. Red obtains a lab session token via the agreed method (phishing simulation against a volunteer, or admin-issued token for the test).
2. Red replays session from an unexpected network path.
3. Blue and Fraud watch cloud audit alerts and fraud hooks.
4. Red proposes a payout rule change; stops at submit if alert already actionable; otherwise submits in lab and rolls back.

### Score (format example)

- Anomalous session reuse: Missed (audit log present; no alert routed).
- Payout rule modification: Detected - actionable in cloud audit alert to SOC; Fraud console silent.

### Backlog

- `PT-F-014`: Alert on session reuse / impossible travel for roles with payout permissions; page SOC and notify Fraud queue. Owner: Detection eng + Fraud. Acceptance: retest token replay; both queues receive signal within 15 minutes.
- `PT-F-015`: Add Fraud routing for payout configuration changes even when SOC already alerts. Owner: Fraud systems. Acceptance: single change creates linked cases in both systems.

### Executive summary ask

Fund engineering time for dual-route alerting; accept residual risk on session reuse until `PT-F-014` ships, with temporary watch on payout-capable roles.

---

*FinTech overlay v1.0 - use with the core framework. CC BY 4.0*
