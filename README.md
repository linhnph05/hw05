# HW05 AI Performance Testing

Student ID: 23127081

Full name: Nguyễn Phan Hùng Linh

Repository: <https://github.com/linhnph05/hw05>

## Submission summary

I tested the EShop backend with Apache JMeter. The work followed this order:
scope selection, AI-assisted design, human review, execution, raw-result
analysis, endurance testing, and a continuous-testing proposal.

| Item | Result |
| --- | --- |
| Scenarios | Load, Stress, Spike, and 10-minute endurance |
| Endpoint groups | Read-heavy, auth-heavy, and transactional |
| Error result | 0.00% errors in every measured run |
| Endurance result | At least 92.70 RPS for 10 minutes at 100 users; p95 83 ms; peak backend RSS 76,000 KB |
| Performance issues | None observed in the measured runs |
| Video | Not recorded by instruction; narration and steps are provided |

## Reading order

1. `reports/01_scope_and_test_design.md` — selected workflows and final design
2. `reports/HW05_Performance_Report.md` — full process, commands, results, and conclusion
3. `reports/AI_Audit_Report.md` — AI prompts, outputs, and human corrections
4. `reports/AI_Critique.md` — required 200–300 word critique
5. `reports/Video_Narration_and_Steps.md` — Vietnamese narration and recording guide

Supporting evidence is stored in:

- `test-plans/` — three JMeter plans;
- `test-data/` — three separate CSV input files;
- `results/` — raw JTL files, resource CSV files, JMeter logs, and HTML reports;
- `images/` — rendered JMeter dashboard screenshots; and
- `skills/eshop-performance-testing/` — reusable performance-testing skill.

## Self-assessment

| No. | Criteria | Maximum | Self-assessed |
| --- | --- | ---: | ---: |
| 1 | Task 1 — Load testing | 20 | 18 |
| 2 | Task 1 — Stress testing | 20 | 18 |
| 3 | Task 1 — Spike testing | 20 | 18 |
| 4 | Task 2 — AI analysis and misinterpretation hunt | 10 | 10 |
| 5 | Task 3 — Continuous Performance Testing proposal | 10 | 10 |
| 6 | Agent Skill | 10 | 10 |
|  | **Total** | **100** | **084** |

The score is reduced because the requested demo video was not recorded.
