# Purple Team Framework

**Author:** Clint P. Garrison / CarbeneAI
**License:** [CC BY 4.0](LICENSE)
**Version:** 1.0

This document is the sector-neutral core. Sector overlays in `sectors/` adapt the same cycle for regulated environments. Read this first. Apply an overlay only after the core cycle is clear.

---

## 1. What purple teaming is

Purple teaming is a structured practice where offensive operators and defensive operators work the same scenario at the same time, with a shared goal: prove which attacker behaviors your controls detect, which they miss, and what you will change next.

It is not a rebranded penetration test. A penetration test answers "can someone get in?" Purple teaming answers "when a specific behavior happens, what do we see, how fast, and what do we do?"

It is not a tabletop alone. Tabletop exercises test decision-making. Purple teaming tests instrumentation, telemetry, and response under controlled technical conditions.

### Why the practice exists

Security programs accumulate tools faster than they accumulate proof. Dashboards report "coverage." Vendors map product features to MITRE ATT&CK technique IDs. Neither proves that a real behavior on your network produces an alert your people trust and act on.

Each cycle should produce: a short technique list chosen for stated reasons; observed detection outcomes; backlog items for misses and weak hits worth fixing; and an executive summary of what improved and what remains open.

If a cycle does not change a detection, a playbook, or a prioritized backlog, it was a demo, not an engagement.

### Derivation: why offense and defense must share the scenario

Offense without defense produces findings that sit in a PDF. Defense without offense tunes rules against imagined threats. Shared scenarios force both sides to use the same facts: the same host, the same timestamp, the same technique, the same expected artifact. That shared fact set is the unit of learning. The rest of this framework exists to protect it from scope creep, unsafe testing, and dishonest metrics.

---

## 2. Roles and RACI

Keep roles few. More titles do not create more learning.

| Role | Accountable for | Typical background |
|------|-----------------|-------------------|
| Engagement Lead | Scope, safety gates, timeline, final report | Security manager or senior engineer |
| Red Operator | Executing agreed techniques within scope | Offensive security, adversary emulation |
| Blue Operator | Observing telemetry, validating alerts, documenting detection state | Detection engineering, SOC, IR |
| Control Owner | Approving changes to systems under test; owning backlog items that touch their stack | Platform, identity, endpoint, or network owner |
| Executive Sponsor | Authority to pause production risk; receives the summary | CISO or delegate |

### RACI for a single cycle

| Activity | Engagement Lead | Red | Blue | Control Owner | Sponsor |
|----------|-----------------|-----|------|---------------|---------|
| Approve scope and safety limits | A | C | C | C | I |
| Select ATT&CK techniques | A | R | C | C | I |
| Execute techniques | A | R | C | I | I |
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

Write a one-page scope before any technique runs.

Required fields:

- **Objective:** One sentence. Example: "Determine whether lateral movement via stolen credentials from a standard user workstation to a file server produces a usable alert within 15 minutes."
- **In-scope assets:** Named hosts, accounts, network segments, or cloud projects.
- **Out-of-scope assets:** Explicit list. If it is not listed as in-scope, it is out.
- **Allowed time window:** Start and end. Include timezone.
- **Data handling rules:** What logs, screenshots, and artifacts may leave the environment; how long they are retained; who may view them.
- **Stop conditions:** Conditions that halt the exercise immediately (patient-impact risk, payment outage risk, life-safety system involvement, unexpected privilege gain, data exfiltration beyond the agreed test file).
- **Success definition for the cycle:** What "done" means. Prefer detection and response outcomes over "Red got domain admin."

### 3.2 Select techniques

See Section 4. Exit this phase with a short list (often 3 to 7 techniques), each with a stated priority reason.

### 3.3 Safety gate

The Engagement Lead and Control Owner sign (electronically is fine) that:

1. Scope is accurate for the current environment.
2. Stop conditions are understood by Red and Blue.
3. Production impact risk is accepted for the stated window, or the test will run in a dedicated environment that mirrors production enough for the technique under test.
4. Rollback or containment steps are known before execution starts.

If any item fails, do not execute.

### 3.4 Execute and observe

Red executes one technique at a time when practical. Blue watches agreed consoles and queries in parallel. Both sides timestamp actions and observations against a shared clock (NTP-synced or agreed reference).

For each technique, capture:

- **Action:** What Red did (command class or procedure, not a novel exploit dump unless that is the point).
- **Expected artifact:** What should appear in logs or telemetry if instrumentation works.
- **Observed artifact:** What actually appeared, where, and when.
- **Alert:** Fired / did not fire / fired with wrong severity or wrong mapping.
- **Response:** Did an analyst or automated playbook act? Within what time?

Do not grade on Red creativity. Grade on whether the environment told the truth about the behavior.

### 3.5 Score

Use the scoring model in Section 5. Record outcomes per technique. Do not average them into a single "purple score" for marketing. Averages hide misses.

### 3.6 Backlog and report

Convert every miss and every weak hit into a backlog candidate (Section 6). Publish a short executive summary (Section 8). Archive the evidence package with retention rules from scope.

---

## 4. Choosing and prioritizing ATT&CK techniques

MITRE ATT&CK is a knowledge base of adversary behaviors, organized by tactic (goal) and technique (how). It is useful because it gives Red and Blue a shared vocabulary. It is dangerous when teams treat the matrix like a checklist to color in.

### Derivation: what ATT&CK is for in purple teaming

ATT&CK helps you name the behavior you are testing. It does not tell you which behaviors matter for your organization. That answer comes from your threat model, your crown-jewel processes, and your recent incidents or near-misses.

### Selection method

Use four filters in order. Stop when you have enough techniques for one cycle.

**Filter 1 - Relevance to your threat model.**
Ask: Would a capable adversary who wants our specific outcomes (fraud, ransomware payment, PHI theft, disruption of field operations) plausibly use this technique against us? If you cannot argue yes from first principles or from observed activity in your sector, drop it for this cycle.

**Filter 2 - Detection uncertainty.**
Prefer techniques where you are unsure of the outcome. Testing something you already know is loudly detected wastes the cycle. Testing something you have never instrumented teaches more.

**Filter 3 - Blast radius you can contain.**
If the technique cannot be executed safely under your stop conditions, substitute a narrower procedure that still exercises the same detection surface, or move the test to a lab that mirrors the control plane.

**Filter 4 - Dependency order.**
Test earlier kill-chain behaviors before late-stage ones when the goal is detection maturity. A ransomware detonation test teaches little if you have never validated credential access or lateral movement detections.

### Prioritization inside the short list

Rank remaining techniques by:

1. **Impact if missed** (would a miss leave a path to your highest-value asset or outcome?).
2. **Cost to fix if missed** (is the likely fix a rule, a sensor gap, or an architecture change?).
3. **Time to execute and observe** (fit the window).

Write the priority reason next to each technique ID. "Because it is popular" is not a reason. "Because our identity logs do not currently record this authentication pattern and ransomware crews need it for lateral movement" is a reason.

### What not to do with ATT&CK

- Do not chase 100% technique "coverage." ATT&CK is not a finite exam.
- Do not equate a vendor's ATT&CK heatmap with your detection capability. Heatmaps usually describe product features, not proven alerts in your tenant.
- Do not test only noisy techniques that always alert. That inflates scores.

---

## 5. Measuring detection coverage honestly

### Outcome labels

For each technique procedure you execute, assign one primary outcome:

| Outcome | Meaning |
|---------|---------|
| **Prevented** | Control blocked the behavior before meaningful progress. Prevention is success if intentional. Note whether an alert also fired. |
| **Detected - actionable** | Alert or high-confidence signal fired, mapped to the right behavior, in time for response within your stated SLO. |
| **Detected - weak** | Something logged or alerted, but severity, mapping, context, or timing made it unlikely an analyst would act correctly. |
| **Logged only** | Telemetry exists; no alert; discovery would require hunting that did not happen during the window. |
| **Missed** | No useful telemetry and no alert for the behavior as executed. |
| **Not testable** | Safety gate or environment limits blocked a faithful test. Record why. Do not score as Detected. |

### What coverage metrics do not prove

State these limits in every report so executives are not misled.

1. **A colored ATT&CK cell does not prove detection.** It may prove a tool claims a feature, or that a single procedure was seen once in a lab.
2. **Alert volume does not prove quality.** High volume often proves noise.
3. **Prevention without telemetry can hide regressions.** If you only block and never log, you cannot tell when the block fails.
4. **One successful detection does not generalize across procedures.** Detecting one credential-dumping method does not prove you detect every method under that technique.
5. **Purple team success does not prove resilience to a patient adversary.** Real attackers adapt. Your cycle tested specific behaviors under consent.
6. **Mean-time-to-detect on synthetic tests is not your breach MTTD.** It is a lower bound under ideal attention.

### Honest summary format

Report counts by outcome label (Prevented, Detected - actionable, Detected - weak, Logged only, Missed, Not testable). List misses and weak hits with backlog IDs. Do not bury them under a percentage. Numbers in examples elsewhere in this repo illustrate format only.

### Optional: detection confidence note

For internal trend tracking only:

`actionable / (actionable + weak + logged-only + missed)`

Exclude Prevented from the denominator if prevention was the design goal. Exclude Not testable. Publish the formula next to the number every time.

---

## 6. Turning findings into a detection backlog

Every weak hit and miss that you choose to fix becomes a backlog item with enough detail for an engineer who was not in the room.

### Required fields

- **ID:** Stable identifier (for example, `PT-2026-041`).
- **Technique:** ATT&CK ID and procedure description as tested.
- **Outcome:** Weak / Missed / Logged only.
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

Common patterns: monthly micro-cycles (1 to 3 techniques) for mature detection teams; quarterly full cycles (5 to 7 techniques plus retests) for most mid-size programs; plus immediate cycles after major control changes (new EDR, SIEM migration, identity redesign) and after incidents or near-misses (keep those technical, not blame-focused).

Minimum healthy pattern: plan from threat model and open retests; execute in a fixed window; ship Critical and High backlog items within an agreed SLA; retest them next cycle. If you cannot retest, reduce new technique volume until the backlog moves.

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

Say what you tested, not what you wish were true. Prefer "We did not detect technique T on host H within 15 minutes" over "coverage gaps exist." Do not invent breach statistics. If you cite a figure, name the primary source; otherwise omit it. Do not present vendor heatmaps as organizational capability.

---

## 9. Evidence package

Retain enough to defend the work under audit or after staff turnover: signed scope and safety gate; technique list with priority reasons; per-technique run log (timestamps, operators, outcome label); queries or rule IDs checked; backlog export; executive summary. Apply data minimization. Redact secrets, personal data, and irrelevant host details per sector overlay rules.

---

## 10. How to use sector overlays

Run the core cycle as written. Read your sector overlay before Scope. Add overlay constraints to scope, stop conditions, and evidence handling. Prefer the overlay's worked scenario when teaching a new team. Do not let compliance theater replace outcome scoring. Oversight bodies want evidence of control effectiveness; colored matrices alone rarely satisfy that need.

---

## 11. Non-goals

This framework does not replace penetration testing, red team operations, or bug bounty programs; does not rank vendors; does not guarantee regulatory compliance by itself; and does not provide exploit code or step-by-step abuse instructions for production systems.

It teaches how to derive an honest detection program from shared scenarios, measured outcomes, and a backlog that closes.

*Purple Team Framework v1.0 - Clint P. Garrison / CarbeneAI - CC BY 4.0*
