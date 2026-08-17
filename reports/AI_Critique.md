# AI Critique

Student ID: 23127081

Full name: Nguyễn Phan Hùng Linh

The AI was useful for creating the first test design, but its answer was not
ready for submission. It suggested one endpoint for each test and up to 50
Stress users before it knew the limits of my laptop and the local SQLite
database. I chose three related endpoints for each workflow, reduced Stress to
30 users, and ran smoke tests before the measured runs. These changes needed
human knowledge about the assignment, shared endpoint choices, and the local
test environment.

The separate AI reviewer analysed the rerun files accurately, but one result
was incomplete. It described 93.58 RPS as the stable Endurance rate. That value
uses the 599.03-second observed JTL window. For the planned ten-minute test, I
divided 56,058 samples by 600 seconds and reported 93.43 RPS. The difference is
small, but the denominator must be clear when results are compared in a report
or CI gate. The AI also showed Stress p95 as 91 ms using a nearest-rank
calculation, while the JMeter dashboard interpolated it as 91.95 ms. I rounded
the dashboard value to 92 ms.

The AI's optimization ideas also required review. Database indexes and SQLite
WAL are reasonable experiments, but the reruns had 0.00% errors and moderate
CPU and memory use. A connection pool or cache was not supported by the
evidence. The proposed CI thresholds are useful starting points, but one run is
not enough to prove that they will avoid false alarms.

The main lesson is that AI should provide a checked draft, not the final
decision. I must keep raw evidence, state calculation methods, compare advice
with the real system design, and remain responsible for every conclusion.
