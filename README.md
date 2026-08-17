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
| Endurance result | At least 93.43 RPS for 10 minutes at 100 users; p95 35 ms; peak backend RSS 72,720 KB |
| Performance issues | None observed in the measured runs |
| Demo video | <https://youtu.be/IB1nEht4ZVg> |

## Reading order

1. `reports/HW05_Performance_Report.md` — full process, commands, results, and conclusion
2. `reports/AI_Audit_Report.md` — AI prompts, outputs, and human corrections
3. `reports/AI_Critique.md` — required 200–300 word critique
4. `reports/Bug_Report.md` — performance issue result

Supporting evidence is stored in:

- `test-plans/` — three JMeter plans;
- `test-data/` — three separate CSV input files;
- `results/` — raw JTL files, resource CSV files, JMeter logs, and HTML reports;
- `images/` — JMeter dashboards, `htop`, and hardware screenshots; and
- `skills/eshop-performance-testing/` — reusable performance-testing skill.

## Self-assessment

| No. | Criteria | Maximum | Self-assessed |
| --- | --- | ---: | ---: |
| 1 | Task 1 — Load testing | 20 | 20 |
| 2 | Task 1 — Stress testing | 20 | 20 |
| 3 | Task 1 — Spike testing | 20 | 20 |
| 4 | Task 2 — AI analysis and misinterpretation hunt | 10 | 10 |
| 5 | Task 3 — Continuous Performance Testing proposal | 10 | 10 |
| 6 | Agent Skill | 10 | 10 |
|  | **Total** | **100** | **100** |
