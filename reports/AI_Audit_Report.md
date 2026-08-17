# AI Audit Report

Student ID: 23127081

Full name: Nguyễn Phan Hùng Linh

## 1. Declaration

I use AI tools for the following tasks. I used Codex/GPT-5.6 for the first
test design and guided a separate Codex agent through the rerun analysis. I
acted as the human reviewer and made the final scope, parameter, execution,
and reporting decisions.

The AI did not create the final JTL evidence. The measured files came from the
JMeter runs on my computer.

## 2. Prompt 1 — Test design

| Field | Record |
| --- | --- |
| AI tool | Codex / GPT-5.6 |
| Date and time | 2026-08-16 ICT |
| Purpose | Get a first design for Load, Stress, and Spike tests |

### 2.1 My prompt

> Read HW05.md and inspect the eshop-sut project read-only. Recommend a simple,
> compliant plan: deliverables, non-conflicting endpoints for
> Load/Stress/Spike, test scenarios/metrics, and likely tools. Give concise
> advice and leave an audit-ready account of your consultation. Do not edit
> files.

### 2.2 AI output

The AI proposed this first mapping:

| Test | Suggested endpoint | Suggested CSV |
| --- | --- | --- |
| Load | `GET /api/orders/{order_id}` | `data/order_ids.csv` |
| Stress | `POST /api/register` | `data/register_users.csv` |
| Spike | `POST /api/apply-coupon` | `data/coupons.csv` |

It suggested 20 users, a 60-second ramp-up, and a five-minute steady period for
Load. It suggested increasing Stress from 10 to 50 users. For Spike, it
suggested 2 → 40 → 2 users.

It also suggested Summary Report, Aggregate Report, and View Results Tree as
three different listener types. It recommended HTTP and JSON assertions and
warned that View Results Tree may use too much memory in a long test.

### 2.3 My review and final decision

The proposed endpoints covered the three required endpoint groups. I chose a
broader workflow with three related endpoints in each scenario. This was my
scope choice, not an HW05 requirement.

I made these changes:

- used a separate CSV file for every scenario;
- added status assertions and required JSON extraction;
- reduced the first measured Stress test from 50 users to 30 users;
- used smoke tests before the measured runs; and
- kept View Results Tree only in the short Spike plan.

## 3. Prompt 2 — Result analysis

| Field | Record |
| --- | --- |
| AI tool | Codex / GPT-5.6 |
| Date and time | 2026-08-18 01:05 ICT |
| Purpose | Analyse the rerun JTL and resource files and suggest thresholds |

### 3.1 My prompt

> Analyze the new rerun evidence in `results/load.jtl`, `results/stress.jtl`,
> the three Spike stage JTLs, merged `results/spike.jtl`,
> `results/endurance.jtl`, and the four resource CSV files. Report samples,
> observed RPS, errors, mean, p95, p99, maximum, CPU, and RSS. Identify the
> stable endurance result, propose CI thresholds and optimizations, and use
> only performance-related findings. Do not edit files. State whether a
> genuine performance issue was found.

### 3.2 AI output

The AI produced this result table:

| Run | Samples | RPS | Errors | Mean | p95 | p99 | Maximum |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| Load | 5,345 | 17.91 | 0.00% | 7.94 ms | 28 ms | 67 ms | 262 ms |
| Stress | 8,540 | 47.64 | 0.00% | 24.06 ms | 91 ms | 265 ms | 651 ms |
| Spike baseline | 231 | 3.91 | 0.00% | 8.19 ms | 25 ms | 39 ms | 52 ms |
| Spike high | 3,327 | 56.26 | 0.00% | 11.14 ms | 39 ms | 143 ms | 206 ms |
| Spike recovery | 230 | 3.91 | 0.00% | 8.35 ms | 22 ms | 60 ms | 127 ms |
| Spike merged | 3,788 | 19.74 | 0.00% | 10.79 ms | 38 ms | 127 ms | 206 ms |
| Endurance | 56,058 | 93.58 | 0.00% | 9.36 ms | 35 ms | 91 ms | 273 ms |

It reported these backend resource values:

| Run | Average CPU | Peak CPU | Average RSS | Peak RSS |
| --- | ---: | ---: | ---: | ---: |
| Load | 1.87% | 12.6% | 33,925 KB | 45,872 KB |
| Stress | 10.12% | 29.3% | 50,280 KB | 72,912 KB |
| Spike | 2.68% | 16.0% | 38,908 KB | 64,368 KB |
| Endurance | 7.65% | 24.1% | 57,287 KB | 72,720 KB |

The AI found no genuine performance issue. It suggested an Endurance gate of
error rate at most 1%, throughput at least 85 RPS, p95 at most 75 ms, and p99
at most 200 ms. It suggested a Stress p95 limit of 150 ms and a Spike p95
limit of 100 ms. It treated database indexes and SQLite WAL as experiments,
not urgent fixes. It said a connection pool or cache was not supported by the
evidence.

### 3.3 My verification and correction

I recalculated the important values from the JTL elapsed column. The samples,
errors, mean, Spike percentiles, Endurance percentiles, and resource values
agreed with the AI output after rounding. The Stress dashboard uses an
interpolated p95 of 91.95 ms, so I show 92 ms in the main report instead of
the AI's nearest-rank 91 ms.

The important correction is the Endurance denominator. The AI called **93.58
RPS** the stable rate because it divided by the 599.03-second observed JTL
window. For the planned ten-minute result, I divide 56,058 samples by 600
seconds and report **93.43 RPS**. The AI calculation is useful, but it did not
clearly separate observed-window throughput from planned-duration throughput.
Neither value proves the absolute hardware limit.

## 4. Human responsibility

I verified that resource monitoring followed the backend process
`/opt/homebrew/bin/node server.js`. I kept the raw evidence, checked important
metrics, and reported only measured performance results. I take responsibility
for the final plans, results, and conclusions.
