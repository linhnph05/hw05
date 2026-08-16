# Bug report

Issue: [#1 — Coupon SAVE10 returns incorrect negative discount](https://github.com/linhnph05/hw05/issues/1)

## Reproduction

Send this request to the running backend:

```json
POST /api/apply-coupon
{"code":"SAVE10","total_amount":500000}
```

Expected for a 10% coupon: `discount_amount` 50000 and `final_amount` 450000.

Actual response:

```json
{"success":true,"coupon_id":1,"discount_amount":-4500000,"final_amount":5000000}
```

The issue was created on 2026-08-16. It is a functional defect in the coupon
calculation. The performance JTL shows HTTP status only, so this response body
was reproduced separately before filing the issue.

![Published GitHub Issue #1](../images/github_issue_1.jpg)
