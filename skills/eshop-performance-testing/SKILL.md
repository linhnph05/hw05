---
name: eshop-performance-testing
description: Run, review, and report repeatable Apache JMeter performance tests for the EShop REST API. Use when an EShop endpoint group needs a data-driven load, stress, spike, or endurance test with JTL evidence and human-reviewed metrics.
---

# EShop Performance Testing

Use the API specification and backend source to choose endpoints. Avoid a
teammate's selected endpoint or workflow.

1. Select three endpoints in one endpoint group. Use a separate CSV input file
   for that group.
2. Build one JMeter plan named `{StudentID}_{ScenarioType}_{YYYYMMDD}`. Add
   realistic threads, ramp-up, think time, status assertions, and one distinct
   listener type.
3. Run a short smoke test first. Fix a failed assertion before the measured
   run.
4. Run JMeter in non-GUI mode with `-l`, `-e`, and `-o` to keep the raw JTL and
   HTML report. Sample the backend PID once per second with `ps`.
5. Calculate samples, error rate, RPS, mean, p95, maximum latency, and peak
   RSS from the raw files. Do not call a tested level the hardware maximum
   unless a failure boundary was measured.
6. Ask an AI to analyse the results, then independently check its numbers
   against the JTL. Record both the prompt and output in an AI audit report.
7. Separate functional defects from performance results. Reproduce a response
   body before filing a defect.
