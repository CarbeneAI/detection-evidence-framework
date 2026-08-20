# Purple Team Framework

An original, open teaching framework for running purple team engagements that produce honest detection evidence and a backlog that closes.

**Author:** Clint P. Garrison / CarbeneAI
**License:** [CC BY 4.0](LICENSE)

## What it is

Purple teaming is offense and defense working the same scenario at the same time so you can prove which attacker behaviors you detect, which you miss, and what you will fix next.

This repository gives you a **sector-neutral core** (`FRAMEWORK.md`) for scope, technique selection, safety gates, scoring, backlog, roles, cadence, and executive reporting, plus **sector overlays**:

- [`sectors/healthcare.md`](sectors/healthcare.md) - HIPAA constraints, clinical availability, PHI handling, ransomware-path realism without unsafe detonation
- [`sectors/fintech.md`](sectors/fintech.md) - examiner-ready evidence, fraud/security handoff, PCI DSS scope boundaries
- [`sectors/public-safety-tech.md`](sectors/public-safety-tech.md) - CJIS-adjacent sensitivity, field devices, chain of custody, life-safety exclusions

It teaches derivation: why each step exists, what coverage metrics do not prove, and how to turn misses into owned work. It is not a product pitch, a vendor heatmap, or a compliance certificate.

## Who it is for

Detection engineers and SOC leads who need proof; offensive operators who want their work to change defenses; security managers who report to executives or examiners; practitioners in healthcare, financial services, and public safety technology who must test without harming missions or patients.

## How to use it

1. Read [`FRAMEWORK.md`](FRAMEWORK.md) end to end.
2. If you operate in a covered sector, read the matching overlay before you write scope.
3. Run one cycle with a short technique list (often 3 to 7 items).
4. Score with the core outcome labels (Prevented, Detected - actionable, Detected - weak, Logged only, Missed, Not testable).
5. File backlog items with acceptance tests. Retest next cycle.
6. Keep the executive summary to one page and one ask.

Start small. One honest cycle beats a matrix you never retest.

## What this is not

Not a replacement for penetration tests, red team operations, or incident response. Not legal advice or compliance certification. Not exploit code or instructions to attack systems without authorization.

Only test systems you are authorized to test. Sector overlays list default off-limits behaviors; your laws, contracts, and safety rules still govern.

## Repository layout

```
FRAMEWORK.md                 Core cycle (read first)
sectors/healthcare.md        Healthcare overlay
sectors/fintech.md           FinTech overlay
sectors/public-safety-tech.md
AI-DISCLOSURE.md             AI assistance disclosure
CONTRIBUTING.md              How to propose changes
LICENSE                      CC BY 4.0
```

## License

Copyright © Clint P. Garrison / CarbeneAI

This work is licensed under Creative Commons Attribution 4.0 International. See [LICENSE](LICENSE). You may share and adapt with attribution.

## Attribution

If you adapt this framework, keep author credit to Clint P. Garrison / CarbeneAI and link to the source repository when published.

---

Training and writing: [carbene.ai](https://carbene.ai)
