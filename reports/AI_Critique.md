# AI Critique

The AI was helpful for making a first testing plan, but it was not correct by
default. At first it suggested only one endpoint for each scenario. My
assignment required three endpoints for each scenario, so I had to read the
requirement again and expand the workflows. This happened because the prompt
asked for endpoint ideas but did not strongly repeat the special group rule.
The AI also suggested a high stress setting before it knew my laptop or the
SQLite design. I changed this to a smaller, measured first run and used a smoke
test before the real test.

The AI analysis had another small error. It said the merged spike p95 was 97
ms and the main spike stage p95 was 105 ms. I sorted the elapsed-time column of
the raw JTL files myself. The correct values were 93 ms for the merged spike
JTL and 103 ms for the spike stage. The difference is small, but it shows why a
nice-looking AI table is not enough evidence. An AI can choose a different
percentile position, round at a different time, or accidentally mix stage and
merged data. I used the direct raw values in my report.

The AI made good recommendations too. Database indexes and SQLite WAL are
possible improvements, while a normal database connection pool is not a good
fit for one local SQLite connection. The main lesson is that AI is a planning
assistant, not the owner of the result. I should give it clear constraints,
keep raw artifacts, reproduce functional bugs separately, and check every
number that will be used in a conclusion. I remain responsible for the test
scope, measurements, and final report.
