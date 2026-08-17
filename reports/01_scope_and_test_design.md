# HW05 Scope and Test Design

Student ID: 23127081  
Full Name: Nguyễn Phan Hùng Linh  
Date: 2026-08-16  
Tool: Apache JMeter 5.6.3

## Scope

The three scenarios use different endpoint groups. I did not use the endpoint
workflows already selected by my teammate.

| Scenario | Endpoint group | Three tested endpoints | Input file | Report view |
| --- | --- | --- | --- | --- |
| Load | Read-heavy account and order reads | `GET /api/cart`; `GET /api/orders/my-orders`; `GET /api/orders/{id}` | `test-data/read_input.csv` | Summary Report |
| Stress | Auth-heavy password life cycle | `POST /api/register`; `POST /api/forgot-password`; `POST /api/reset-password` | `test-data/auth_input.csv` | Aggregate Report |
| Spike | Transactional coupon administration | `POST /api/apply-coupon`; `POST /api/admin/coupons`; `DELETE /api/admin/coupons/{id}` | `test-data/transaction_input.csv` | View Results Tree |

The first scenario is read-heavy because every sampler reads existing account or
order data. The last order-detail route does not require a token in the current
SUT; this is noted as a security observation, not used to change the test.

The stress scenario avoids `POST /api/login`, so its 3-failure lockout rule is
not triggered. Password reset uses the token returned by the previous sampler.

The spike scenario does not use cart or checkout. It applies a stable coupon,
creates a uniquely named temporary admin coupon, then deletes that same coupon.
This keeps the database close to its original state.

## AI-assisted design and my review

I asked a second AI for a first plan. It suggested one endpoint per scenario:
`GET /api/orders/{id}`, `POST /api/register`, and `POST /api/apply-coupon`.
That was a useful start, but it missed the assignment-specific requirement from
my chosen scope of three endpoints for each scenario. The HW05 requirement is
three endpoint groups; I expanded each workflow to three endpoints above.

The AI also suggested a five-minute stress run at 50 users. I reduced the first
real run to 30 users and a shorter ramp because this is a 16 GB student laptop
and registration writes to one SQLite file. I will report measured values only;
the AI's suggested thresholds are not results.

I added status-code assertions, JSON assertions, per-group CSV files, a
password-reset token extractor, and distinct report listeners. The AI warned
that View Results Tree uses memory during spikes. I keep it only in the short
spike plan and use the raw JTL/HTML report as the main evidence.
