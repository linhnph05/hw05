# AI Critique

Student ID: 23127081

Full name: Nguyễn Phan Hùng Linh

The AI was useful for creating a first test design, but its answer was not
ready for submission. It suggested one endpoint for each test. That covered the
three endpoint groups required by HW05, but I chose a broader workflow with
three related endpoints in each scenario. The difference was a scope choice,
not an AI error. The AI also suggested up to 50 Stress users before it knew the
limits of my laptop and the local SQLite database. I reduced the first measured
run to 30 users and ran smoke tests first.

The clearest AI error appeared during result analysis. It reported 97 ms for
the merged Spike p95 and 105 ms for the high-load stage p95. I checked the raw
elapsed-time values and found 93 ms for the merged file and 103 ms for the
high-load stage. The difference was small, but it could still change a pass or
fail decision near a threshold. The AI may have used a different percentile
position or mixed the merged and stage files.

Some recommendations were reasonable but still needed evidence. Database
indexes and SQLite WAL are possible experiments. A normal database connection
pool was not suitable because this SUT uses one local SQLite connection.
Caching was also not justified because CPU usage was low and every measured run
had 0.00% errors.

The main lesson is that AI should provide a draft, not the final decision. I
must give clear constraints, keep raw evidence, check important calculations,
and compare every recommendation with the real system design. I remain
responsible for the scope, measurements, and conclusions.
