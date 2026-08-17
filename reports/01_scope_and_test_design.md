# HW05 Scope and Test Design

Student ID: 23127081

Full name: Nguyễn Phan Hùng Linh

Date: 2026-08-16

Tool: Apache JMeter 5.6.3

## 1. Goal

This document records the scope and the final design before execution. HW05
requires the read-heavy, auth-heavy, and transactional endpoint groups. I
mapped one group to each performance-test type.

## 2. Selected workflows

| Test | Group | Endpoint workflow | Why it fits |
| --- | --- | --- | --- |
| Load | Read-heavy | `GET /api/cart` → `GET /api/orders/my-orders` → `GET /api/orders/{id}` | All three requests read account or order data. |
| Stress | Auth-heavy | `POST /api/register` → `POST /api/forgot-password` → `POST /api/reset-password` | The workflow tests account creation and password recovery under increasing traffic. |
| Spike | Transactional | `POST /api/apply-coupon` → `POST /api/admin/coupons` → `DELETE /api/admin/coupons/{id}` | The workflow reads and writes coupon data during a sudden traffic increase. |

I chose three endpoints inside each workflow as a broader scope. This is my
scope choice; HW05 itself requires three endpoint groups.

The Stress workflow does not call `POST /api/login`, so it does not trigger the
three-failed-login lockout. The Spike workflow deletes each temporary coupon
after creating it, which limits permanent test data.

## 3. Data and report design

| Test plan | CSV input | JMeter listener | Main parameters |
| --- | --- | --- | --- |
| `23127081_Load_20260816.jmx` | `test-data/read_input.csv` | Summary Report | 20 users, 60-second ramp-up, 300 seconds, 1-second think time |
| `23127081_Stress_20260816.jmx` | `test-data/auth_input.csv` | Aggregate Report | 30 users, 60-second ramp-up, 180 seconds, 500 ms think time |
| `23127081_Spike_20260816.jmx` | `test-data/transaction_input.csv` | View Results Tree | 2 → 30 → 2 users, three 60-second stages |

Each scenario has its own CSV file and a different listener type. Each plan
also checks the expected HTTP status. The Stress plan extracts the reset token
from the forgot-password response. The Spike plan extracts the temporary
coupon ID so it can delete the same coupon.

## 4. AI suggestion and human review

The AI first suggested one endpoint for each group. That met the basic HW05
group rule. I expanded the workflows to cover three related endpoints in each
scenario.

The AI also suggested up to 50 Stress users. I selected 30 users for the first
measured run because the backend uses a local SQLite database on a 16 GB
laptop. I ran smoke tests before collecting the final evidence.

The final design decisions were mine. AI suggestions were treated as a first
draft, not as measured facts.
