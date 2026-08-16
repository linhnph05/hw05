# HW05 AI Performance Testing

Student ID: 23127081  
Repository: <https://github.com/linhnph05/hw05>

## Test summary

| Item | Result |
| --- | --- |
| Scenarios run | Load, Stress, Spike, and 10-minute endurance |
| Endpoint groups covered | Read-heavy account/order reads; auth password life cycle; transactional coupon administration |
| Endurance threshold | At least 92.70 RPS for 10 minutes at 100 users; p95 83 ms; 0.00% errors; peak backend RSS 76,000 KB |
| Bugs / performance issues | 1 functional issue: incorrect `SAVE10` percentage calculation ([Issue #1](https://github.com/linhnph05/hw05/issues/1)) |
| Demo video | Not recorded by instruction. See `reports/Video_Narration_and_Steps.md`. |

## Main files

- `reports/HW05_Performance_Report.md` — main report
- `reports/AI_Audit_Report.md` — AI prompts, output, and human review
- `reports/AI_Critique.md` — 200–300 word AI critique
- `test-plans/` — three JMeter plans
- `test-data/` — three separate data files
- `results/` — raw JTL logs, JMeter HTML reports, logs, and resource traces
- `skills/eshop-performance-testing/` — reusable Agent Skill

## Self-assessment

| No. | Criteria | Grade | Self-assessed grade |
| --- | --- | ---: | ---: |
| 1 | Task 1 — Load testing | 20 | 18 |
| 2 | Task 1 — Stress testing | 20 | 18 |
| 3 | Task 1 — Spike testing | 20 | 18 |
| 4 | Task 2 — AI analysis + misinterpretation hunt | 10 | 10 |
| 5 | Task 3 — Continuous Performance Testing proposal | 10 | 10 |
| 6 | Agent Skills | 10 | 10 |
|  | **Total** | **100** | **084** |

The reduced self-assessment reflects that the requested demo video was not
recorded and no live GUI screenshot was captured. The real non-GUI results,
raw logs, resource monitoring, reports, and GitHub issue are included.
