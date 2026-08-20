# Purple Team Framework

An open teaching framework for running purple team engagements that produce honest detection evidence and a backlog that closes.

**Author:** Clint P. Garrison / CarbeneAI
**License:** [CC BY 4.0](LICENSE)

## What it is

Purple teaming is offense and defense working the same scenario at the same time so you can prove which attacker behaviors you detect, which you miss, and what you will fix next.

[`FRAMEWORK.md`](FRAMEWORK.md) is the document. It contains a sector-neutral core cycle (scope, procedure selection, safety gates, scoring, backlog, roles, cadence, executive reporting) and three sector sections that apply the same cycle in healthcare, financial services, and public safety technology.

It teaches derivation: why each step exists, what coverage metrics do not prove, and how to turn misses into owned work. It is not a product pitch, a vendor heatmap, or a compliance certificate.

## Who it is for

Detection engineers and SOC leads who need proof; offensive operators who want their work to change defenses; security managers who report to executives or examiners; practitioners in healthcare, financial services, and public safety technology who must test without harming missions or patients.

## How to use it

1. Read [`FRAMEWORK.md`](FRAMEWORK.md) end to end. The core cycle (Sections 1 through 10) stands alone.
2. If you operate in a covered sector, read that sector section before you write scope.
3. Run one cycle with a short procedure list. A teaching default is 3 to 7 sub-techniques or named procedures.
4. Score with the core outcome labels (Prevented, Detected-actionable, Detected-weak, Logged only, Missed, Not testable). Do not collapse those labels into a number.
5. File backlog items with acceptance tests. Retest next cycle.
6. Keep the executive summary to one page and one ask.

Start small. One honest cycle beats a matrix you never retest.

## What this is not

Not a replacement for penetration tests, red team operations, or incident response. Not legal advice or compliance certification. Not exploit code or instructions to attack systems without authorization.

Only test systems you are authorized to test. Sector sections list default off-limits behaviors; your laws, contracts, and safety rules still govern.

## Repository layout

```
FRAMEWORK.md                 Core cycle and sector sections
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
