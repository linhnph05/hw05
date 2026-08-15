SOFTWARE TESTING · AI-FIRST · REDESIGNED (2026)
Performance & Stress
Testing
How fast, how much, how long — and what breaks first. Modern tools, observability, and an AI-first workflow.
10 test types Metrics & SLOs Tools 2026 AI-first
IT Faculty, HCMUS · examples in k6 / Gatling / Artillery

MOTIVATION
Slow is the new down
Users don't wait. A response that is slow enough is indistinguishable from broken.
› Speed is a feature — every extra second of latency costs conversions and trust.
› Availability isn't binary — “up but slow” fails the user just as badly as “down”.
› Failures hide until load — a query fast on 1k rows can collapse on 50M.
› Performance is a requirement — measurable targets (p95 < 2 s), not adjectives.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 2

TWO LENSES
Product vs system performance
The same slowness has two very different root causes —test for both.
PRODUCT —the code SYSTEM —the deployment
› Algorithms & data structures. › How many servers / replicas.
› Searches, loops, recomputation. › Database schema & indexes.
› Big-O of hot paths (e.g. sort). › Caching layers (CDN, Redis).
› Caching & memoization in code. › Network, I/O, connection pools.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 3

METRICS
What performance testing measures
Assert on distributions and resources —not a single happy-path number.
Throughput Response time
requests/sec, transactions/sec —how much work in a given time. latency as p50 / p95 / p99 percentiles, not the average.
Concurrency Error rate
how many simultaneous users before it degrades. % of failed/timed-out requests under load.
Saturation Availability
CPU, memory, I/O, DB connections, thread pool, GC. stays reachable and within its latency target.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 4

THE MAP
Six kinds of performance test
Each answers a different question. Pick the ones your risk demands.
Type Question it answers
Load Does it meet targets at expected load?
Stress Where does it break, and how?
Spike Can it survive a sudden surge?
Soak / Endurance Does it degrade or leak over hours?
Breakpoint / Capacity What is the maximum it can handle?
Scalability Does adding resources add throughput?
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 5

THE MAP · EXTENDED
Four more you'll be asked about
Beyond the core six —the data, config, fault-isolation and comparison angles.
Volume Configuration
Large data, not many users —how do queries/indexes hold as the DB Vary the setup (CPU, pool size, cache, JVM flags) —which knob
grows to 50M+ rows? actually moves the number?
Isolation Benchmark
Replay one known-bad transaction repeatedly to confirm and pin Compare against a reference —a prior release, a competitor, or an
down a specific fault. agreed standard.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 6

IT'S ALL IN THE CONFIG
One engine, many tests
Same load tool —you change VU count, ramp shape and hold time to get each type.
| Type | VU / arrival | Ramp-up | Hold / duration | What you watch |
| ---- | ------------ | ------- | --------------- | -------------- |
Load → expected peak (e.g. 500) gradual (2m) hold 5m p95, error rate
Stress past peak (3k→6k) keep climbing no safe hold breaking point
| Spike | 50 → 3000      | instant (10s) | short burst | recovery time |
| ----- | -------------- | ------------- | ----------- | ------------- |
| Soak  | moderate (300) | gradual       | hold 2–8 h  | leaks / drift |
Breakpoint arrival-rate ↑ continuous until it fails max stable load
| Scalability | fixed load | same | same, ×replicas | throughput gain     |
| ----------- | ---------- | ---- | --------------- | ------------------- |
| Volume      | low VU     | —    | huge dataset    | query vs data size  |
| Config      | fixed load | same | same, ×1 knob   | which knob moves it |
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 7

THE RECIPES
Flip the type by editing the numbers
The k6 stages block below is the whole difference between load, stress and spike.
SAME options.stages —DIFFERENT NUMBERS DURATION & EXECUTOR DO THE REST
// LOAD — ramp to expected peak, hold // SOAK — moderate load, held for hours
stages:[{ duration:"2m", target:500 }, stages:[{ duration:"5m", target:300 },
{ duration:"5m", target:500 }, // hold { duration:"4h", target:300 },
{ duration:"2m", target:0 }] { duration:"5m", target:0 }]
// STRESS — keep climbing past the peak // BREAKPOINT — arrival-rate to failure
stages:[{ duration:"2m", target:1000 }, executor:"ramping-arrival-rate",
{ duration:"3m", target:3000 }, startRate:100, timeUnit:"1s",
{ duration:"3m", target:6000 }] stages:[{ target:5000, duration:"20m" }]
// SPIKE — instant jump, then drop // VOLUME / CONFIG — hold VU fixed;
stages:[{ duration:"10s", target:3000 }, // vary the DATA size or ONE infra knob
{ duration:"1m", target:3000 }, // (index, pool, cache) per run.
{ duration:"10s", target:0 }]
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 8

SCENARIOS · CORE
eShop scenarios — the six core types
One eShop scenario per type, plus two everyday apps that need the same test.
| Type | eShop performance scenario | Everyday-app examples (2) |
| ---- | -------------------------- | ------------------------- |
• Grab at the 6pm dinner rush
5,000 shoppers browsing + 500 checkouts at weekday peak; hold, keep p95 <
Load
|     | 800 ms. | • Banking app on payday |
| --- | ------- | ----------------------- |
• Concert ticket on-sale minute
| Stress | Add shoppers past the Black-Friday forecast until checkout errors climb. |     |
| ------ | ------------------------------------------------------------------------ | --- |
• Course-registration at open
• Breaking-news site surge
| Spike | 12.12 flash sale —traffic jumps 20×the instant the sale opens. |     |
| ----- | -------------------------------------------------------------- | --- |
• World-Cup kickoff on a live stream
• Zalo running continuously for days
| Soak | Hold moderate load for 48 h over a sale weekend; watch memory & DB pool. |     |
| ---- | ------------------------------------------------------------------------ | --- |
• Airline site over a holiday week
• MoMo / VNPay max TPS
| Breakpoint | Ramp orders/sec until p95 turns up —the max the cluster sustains. |     |
| ---------- | ----------------------------------------------------------------- | --- |
• Ride-hailing requests/sec per city
• Video CDN adding edge nodes
| Scalability | Double the catalog-service replicas —orders/sec should ~double. |     |
| ----------- | --------------------------------------------------------------- | --- |
• SaaS auto-scaling pods under load
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 9

SCENARIOS · EXTENDED
eShop scenarios — the four extended types
The data, config, fault-isolation and comparison angles —same pattern.
| Type | eShop performance scenario | Everyday-app examples (2) |
| ---- | -------------------------- | ------------------------- |
Catalog grows to 50M products & 100M orders —is search & reporting still  • Google Photos: years of images
Volume
fast?
• Hospital records over decades
Same load; vary DB pool 50→200 and cache on/off —which setting wins on  • Game server tick-rate / instance size
Config
|     | p95? | • DB index settings A/B |
| --- | ---- | ----------------------- |
• Bank: slow interest-batch call
| Isolation | Replay only the slow 'apply coupon' call to confirm and pin the fault. |     |
| --------- | ---------------------------------------------------------------------- | --- |
• Maps: slow route-recalculation
• Speedometer browser benchmark
| Benchmark | Compare this release's checkout p95 against last release's baseline. |     |
| --------- | -------------------------------------------------------------------- | --- |
• Phone app cold-start vs previous model
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 10

TYPE · LOAD
Load testing — meet targets at expected load
Ramp concurrent users to the expected peak and hold; confirm targets hold.
EXAMPLE —k6
TEST TECHNIQUE // load: ramp to 500 VUs, hold, assert p95
export const options = {
stages:[
› Ramp then hold —increase VUs in stages to the expected peak. { duration:"2m", target:500 },
{ duration:"5m", target:500 }, // hold
› Correlate —tie response times to resource metrics.
{ duration:"2m", target:0 } ],
thresholds:{ http_req_duration:["p(95)<2000"],
› Find the benchmark —the point where response times go
http_req_failed:["rate<0.01"] }
erratic.
};
› Then tune —fix the bottleneck; scale vertically/horizontally.
GOAL Confirm p95 < 2 s and error rate < 1% at the expected peak load.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 11

TYPE · STRESS
Stress testing — find the breaking point
Push beyond normal until it fails; learn how it fails and whether it recovers.
EXAMPLE —k6 —beyond capacity
TEST TECHNIQUE // stress: keep climbing past expected peak
export const options = { stages:[
{ duration:"2m", target:1000 },
› Go past the knee —increase load until errors climb. { duration:"3m", target:3000 },
{ duration:"3m", target:6000 }, // over-drive
› Observe the failure mode —graceful degradation vs hard crash.
{ duration:"2m", target:0 } ] };
› Check recovery —does it come back when load drops?
// watch: error rate, timeouts, recovery time
› Blind spot —a passing load test says nothing about the edge. // expect: fail gracefully (429/503), not crash
GOAL Know the ceiling and the failure mode —degrade gracefully, then recover.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 12

TYPES · SPIKE & SOAK
Two failure modes averages miss
Sudden surges and slow leaks break systems that pass a steady load test.
SPIKE —the flash crowd SOAK —the endurance run
› Jump from low to very high instantly. › Moderate load held for hours/days.
› Simulates a sale, a viral event, a cron burst. › Catches memory leaks & resource drift.
› Tests autoscaling reaction time. › Catches slow DB/connection exhaustion.
› Assert: no dropped requests, quick recovery. › Assert: metrics flat over time, no creep.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 13

TYPES · CAPACITY & SCALABILITY
Breakpoint & scalability testing
Find the maximum capacity —then prove that adding resources adds throughput.
EXAMPLE —k6 —steady ramp to saturation
TEST TECHNIQUE // ramp continuously; find where p95 turns up
export const options = {
executor:"ramping-arrival-rate",
› Breakpoint —ramp until it saturates; record the max stable startRate:100, timeUnit:"1s",
load. stages:[
{ target:2000, duration:"10m" } ],
› Scalability —add replicas; throughput should rise near-linearly. };
// then: double the replicas, re-run
› Watch the knee —where latency turns up sharply = capacity.
// throughput should ~double if it scales
› Plan capacity —size infra from the numbers, not guesses.
GOAL Know the capacity number and confirm the system scales horizontally.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 14

READING RESULTS
Percentiles, not averages
The average hides your worst users. Numbers alone aren't enough —you need trends.
ALSO TRACK THE TREND
Why the mean lies
• 100 fast + 1 very slow → good average, › Latency over time, not one snapshot.
one furious user. › Response time vs concurrency curve.
• p95 = 95% are at least this fast. › Error rate as load climbs.
• p99 exposes the tail that pages you.
› Compare each run to a baseline.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 15

DIAGNOSIS
Bottlenecks — what to monitor
A load test that reports only latency can't tell you why. Correlate with resources.
CPU & threads Memory & GC
saturation, context-switching, thread-pool exhaustion. leaks, heap growth, long garbage-collection pauses.
Database Network & I/O
slow queries, missing indexes, lock contention, pool limits. bandwidth, latency, disk I/O, connection limits.
Queues & caches External deps
queue depth, backpressure, cache hit-rate. downstream APIs, rate limits, timeouts.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 16

PROCESS
The performance testing loop
A repeatable cycle —and every step belongs in CI, not a one-off event.
Model Script Baseline Run Analyze Tune
workload & SLOs scenarios + data first run = ref load in CI correlate metrics fix & repeat
BASELINE Save the first clean run as the reference; every later run is judged against it —regressions become obvious.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 17

MODELING
Model reality, or you test a fiction
Uniform robots hitting one endpoint tell you nothing about production.
› Workload modeling —mix of journeys in real proportions, not one endpoint.
› Think-time & pacing —real users pause; back-to-back requests distort results.
› Realistic data & skew —hot keys, big accounts, cold cache —not uniform-random.
› Production traffic replay —record real traffic and replay it (PFLB, GoReplay).
› Ramp profiles —open vs closed models; arrival-rate for realistic pressure.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 18

FRONTEND
Client-side performance — Core Web Vitals
Server p95 can be great while the page still feels slow. Measure the browser too.
CORE WEB VITALS (Google) TOOLS
› LCP —Largest Contentful Paint < 2.5 s (loading). › Lighthouse / PageSpeed Insights.
› INP —Interaction to Next Paint < 200 ms (responsiveness). › WebPageTest —filmstrip & waterfall.
› CLS —Cumulative Layout Shift < 0.1 (visual stability). › Chrome DevTools performance panel.
› Playwright + Lighthouse in CI.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 19

WHERE YOU MEASURE
Three measurement lenses
The same request is 'fast' or 'slow' depending on where you stand —use all three.
| Protocol-level load | Browser-level load | RUM / synthetic |
| ------------------- | ------------------ | --------------- |
Generate raw HTTP/gRPC/WS traffic —no  Drive real (headless) browsers —captures JS,  Watch real users in production (RUM) or
browser. Cheap, massive VU counts, measures  render, and true page timing at lower scale. scripted probes 24/7 (synthetic) —the ground
| the server/API. |     | truth. |
| --------------- | --- | ------ |
k6 · Gatling · JMeter · Locust Playwright + k6 · LoadNinja Datadog RUM · Grafana Faro ·
Sitespeed.io
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 20

THE SRE LENS
SLI · SLO · error budget
Turn 'be fast' into a contract the whole team shares and CI can enforce.
SLI SLO
a measured signal —e.g. p95 latency, success rate. the target for that signal —e.g. p95 < 300 ms, 99.9% success.
Error budget The gate
the allowed shortfall —0.1% may fail; spend it wisely. breach the SLO in a test → fail the build / block release.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 21

OBSERVABILITY
Load + observability = root cause
A number tells you it's slow; traces & metrics tell you where and why.
› Correlate the run — overlay the load test on system metrics in one dashboard.
› Metrics — Prometheus + Grafana; Grafana k6 ties results to infra metrics.
› Traces — OpenTelemetry distributed traces find the slow span/service.
› Profiles — continuous profiling (Pyroscope) pinpoints hot functions.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 22

AUTOMATION
Shift-left performance — perf in CI
Catch regressions per-commit with a fast smoke load test and threshold gates.
EXAMPLE —k6 CI thresholds
TEST TECHNIQUE # runs in CI; non-zero exit fails the job
k6 run --vus 20 --duration 1m smoke.js
› Small smoke test on PR —short, low VUs, quick signal. // smoke.js
export const options = { thresholds:{
› Thresholds as gates —fail the build when p95 regresses.
http_req_duration:["p(95)<300"],
http_req_failed:["rate<0.01"] } };
› Full stress on schedule —nightly/release; too slow for every PR.
// breach a threshold -> exit 1 -> PR blocked
› Trend over builds —catch slow creep, not just hard breaks.
RULE A performance regression should break the build the day it lands —not in production.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 23

CONTINUOUS PERF
From an event to a pipeline stage
Performance testing in 2026 is not a pre-release gate —it runs at every stage.
| PR              | Nightly     | Pre-release | Production           |
| --------------- | ----------- | ----------- | -------------------- |
| smoke load,     | full load + | stress &    | RUM + synthetic,     |
| frontend budget | soak        | breakpoint  | continuous profiling |
PERFORMANCE AS CODE   scripts, thresholds and SLOs live in the repo and version with the app; results feed the same Grafana dashboards as
production, so a test regression and a prod incident look identical.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 24

TOOLING · 2026
The load-testing toolbox
Script in the language your team knows; most integrate with CI and Grafana.
k6 (Grafana) Gatling
JavaScript · dev-centric · deep Grafana/Prometheus integration Scala/Java/JS · high VUs, efficient · great HTML reports
JMeter Locust
Java, GUI · mature, huge plugin ecosystem · protocol breadth Python · code-first, distributed · flexible scenarios
Artillery Cloud / SaaS
JS/YAML · modern protocols: gRPC, WebSocket, Kafka BlazeMeter · NeoLoad · LoadRunner · k6 Cloud · global load
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 25

TOOLING · LANDSCAPE 2026
The whole toolbox, by category
You need more than a load generator —five layers cover the full picture.
Protocol load (OSS) k6 · Gatling · JMeter · Locust · Artillery
Frontend / vitals Lighthouse CI · WebPageTest · Sitespeed.io · DevTools
Cloud / SaaS scale Grafana Cloud k6 · BlazeMeter · OctoPerf · NeoLoad · LoadRunner
Profiling & APM Datadog · Dynatrace · New Relic · Pyroscope · Parca
AI-driven PFLB · LoadForge · LoadNinja · Akamas (autotuning)
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 26

BEYOND THE LOAD GEN
Profiling & APM — the 'why'
Load tools say the p95 rose; profilers and APM say which line of code did it.
CONTINUOUS PROFILING APM / OBSERVABILITY
› CPU / heap flame graphs —see the hot function, not just the › Distributed traces —follow one request across services to the
slow endpoint. slow span.
› Always-on, low overhead —Pyroscope / Parca sample › Golden signals —latency, traffic, errors, saturation on one
production continuously. pane.
› Diff two runs —compare a release against its baseline profile. › Auto anomaly + RCA —Datadog / Dynatrace flag and rank the
cause.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 27

DECIDING
Choosing a load-testing tool
Match the tool to your language, protocols and reporting needs.
If you want… Reach for
Code-first JS + Grafana dashboards k6
Very high VUs, low resource use Gatling
GUI, legacy protocols, plugins JMeter
Python scenarios, full flexibility Locust
gRPC / WebSocket / Kafka load Artillery
Enterprise, cloud, distributed BlazeMeter / NeoLoad / LoadRunner
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 28

AI-FIRST
AI in performance testing
Let AI draft, watch and explain —you set the targets and judge the results.
| Scenario generation | Anomaly detection | Auto root-cause |
| ------------------- | ----------------- | --------------- |
Turn an OpenAPI spec or user story into a k6  ML baselines flag abnormal latency/error  Correlate a spike with the metric/trace that
script to review. patterns automatically. moved —ranked hypotheses.
| guide | sensor | sensor |
| ----- | ------ | ------ |
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 29

THE HARD PARTS
Why performance testing is hard
The technique is the easy part —these are what actually derail projects.
Environment parity Realistic test data
A test box is not prod —different CPU, data volume and topology skew Synthetic uniform data hides hot keys, skew and cache behaviour; masking
every number. prod data is costly.
Cost & time Noise & flakiness
Big load runs burn cloud spend and hours —hard to justify for every Shared infra, cold caches and network jitter make runs non-repeatable.
commit.
Reading results Third-party limits
Correlating latency with the right resource metric needs skill, not just a You can't stress a payment gateway or SMS provider you don't own —mock
dashboard. or coordinate.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 30

WHAT AFFECTS PERFORMANCE
Every layer of the stack can be the bottleneck
The factors that make an app slow — with concrete, real examples for each.
31

THE BIG PICTURE
Performance factors, layer by layer
Slowness hides anywhere from the browser to the disk —test end to end, not one tier.
CLIENT / FRONTEND render-blocking JS, big images, heavy DOM, no lazy-load
NETWORK latency & round-trips, no CDN, payload size, no compression
APPLICATION / CODE algorithms, N+1 queries, blocking I/O, GC pauses, serialization
DATA missing indexes, locks, connection pool, data volume, no cache
INFRASTRUCTURE / ARCH scaling model, sync coupling, service fan-out, region, limits
A fast API behind a 5 MB page still feels slow; a fast page on a full-table-scan query still times out.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 32

FACTOR · CODE & RUNTIME
In the application code
What the program does per request —concrete examples with the tech and the numbers.
Algorithmic complexity N+1 queries (ORM)
O(n²) nested-loop dedup on 50k items ≈ 2.5B ops; a HashSet makes it O(n). Hibernate/Django lazy-load: 1 list + 100 row queries = 101 trips → eager JOIN.
Blocking I/O on request threads Oversized serialization
A sync HTTP call to a slow API holds a Tomcat thread; 200 block → server Returning 5 MB JSON (10k rows) every call; paginate + project only needed
'hangs'. fields.
GC / memory pressure Repeated work in hot path
A 32 GB JVM heap → multi-second stop-the-world pauses; cut allocations, Recomputing the same value inside a loop; memoize / cache —find it on a
tune GC. flame graph.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 33

FACTOR · DATA
In the database
The data tier is the most common bottleneck —and the easiest to get wrong.
Missing index → full scan Lock contention
WHERE email=? on 10M rows with no index scans the table (seconds); B-tree Every order updates one counter row → a row-lock queue; use atomic /
index → ms. sharded counters.
Connection pool too small Data volume growth
Pool=10 but 200 concurrent requests → 190 wait; size the pool, add A query fine at 100k rows scans 100M after a year; partition, archive, index.
PgBouncer.
No caching layer Over-fetching (SELECT *)
The same catalog query hits Postgres every request; add Redis / a result cache. Pulling 50 columns to show 3, or the whole table to count; project & aggregate
in SQL.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 34

FACTOR · ARCHITECTURE
In the system design
How the pieces are wired decides how it scales —or where it jams.
Synchronous coupling Chatty microservices
Checkout blocks on the email service; move email to a queue → async, One page fans out to 30 serial service calls; batch / aggregate / add a BFF.
decoupled.
No horizontal scaling Single write bottleneck
A single stateful app instance; make it stateless → load balancer + autoscale. One Postgres primary for all writes; add read replicas, shard, or CQRS.
Region / topology Container limits
App in ap-southeast, DB in us-east → ~200 ms RTT ×N queries; co-locate + CPU limit 0.5 vCPU → Kubernetes CFS throttling; right-size requests/limits.
cache.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 35

FACTOR · EDGE & CONFIG
Network, frontend, third-party, tuning
The last mile to the user —and the settings nobody remembers to change.
Latency & round-trips No CDN / edge
200 ms RTT ×30 sequential calls = 6 s; batch, parallelize, use HTTP/2 Static assets from one origin across the ocean; serve them from a CDN near
multiplexing. users.
Payload size / compression Frontend rendering
An uncompressed 3 MB JS bundle; enable gzip/brotli, code-split, tree-shake. 5 MB hero image + render-blocking JS → LCP 8 s; lazy-load, compress, defer.
Third-party scripts Config & tuning
A slow analytics/ads SDK blocks the main thread; load async/defer and budget Default 200 Tomcat threads, no keep-alive, wrong GC; tune pools, timeouts,
it. flags.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 36

REAL INCIDENTS
When performance testing was skipped
Famous outages, the architecture that broke, and the test that would have caught it.
37

REAL INCIDENTS · 1
Three outages, three test types
Each maps to a test type from this deck —and to an architecture flaw.
Incident · test type Architecture root cause Fix / how to prevent
Healthcare.gov Mandatory account-creation was a single choke point;
Let users browse before signup (decouple); cache catalog;
LOAD · capacity tightly-coupled tiers; almost no end-to-end load test before
scale tiers independently; run realistic load tests pre-launch.
2013 · ~5×expected users launch.
Ticketmaster · Taylor Swift
A flash crowd plus bots hit checkout with no waiting room or Virtual waiting room / queue-based leveling; bot filtering;
SPIKE
admission control; the buy path saturated. load shedding (429) so the core stays up.
2022 · 3.5B requests, ~4×peak + bots
Slack Cold-cache thundering herd; an AWS Transit Gateway did Pre-scale the gateway before peaks; load-test the
SPIKE + AUTOSCALE not autoscale fast enough; the provision-service then hit provisioning path; fix health-checks & OS/quota limits; add
2021-01-04 · first Monday back open-files & quota limits so scale-up stalled. headroom.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 38

REAL INCIDENTS · 2
Three more — breakpoint, soak, scale
Capacity limits, slow leaks, and scaling that doesn't scale.
Incident · test type Architecture root cause Fix / how to prevent
AWS Kinesis (us-east-1) Adding servers grew an n² mesh of connections; each new
Measure the breakpoint before prod; raise limits; cut fan-out;
BREAKPOINT · capacity server pushed every server past the OS thread limit —a hard
cellular / sharded fleet so capacity grows linearly.
2020-11 · 17-hour outage ceiling nobody had measured.
Memory-leak degradation A heap or connection leak grows under sustained load; Run a soak test; watch memory/GC/connections for upward
SOAK · endurance latency drifts up, then the service OOM-restarts —invisible drift; fix the leak; cap heap; recycle workers; alert on the
common · OOM every few hours/days to a short load test. trend.
Pokémon GO launch Demand far exceeded the capacity model; a write-hot data Model capacity from real forecasts; shard / use horizontally-
SCALABILITY path became the bottleneck that adding app servers could scalable stores; managed autoscaling; verify throughput rises
2016 · ~50×forecast traffic not relieve. with resources.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 39

CAUSE → FIX
Architecture bottleneck → remediation
The recurring root causes behind the incidents —and the standard fix for each.
Bottleneck Architecture cause Standard fix
Thread-pool exhaustion unbounded fan-out, blocking calls bulkheads · async I/O · cap threads
Connection-pool exhaustion pool too small; slow queries hold conns size pool · timeouts · circuit breaker
DB slow under load N+1 queries, missing index, locks indexes · batch · cache · read replicas
Thundering herd cold caches; synchronized retries request coalescing · jitter · warm cache
Retry storm / cascade aggressive retries amplify load backoff+jitter · circuit breaker · budgets
No load shedding accept everything until collapse queue leveling · admission control · 429
Autoscale too slow reactive scaling lags the spike pre-warm · predictive scale · headroom
Single hot resource one DB/leader/region bottleneck shard · replicate · partition · CDN
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 40

CASE STUDY · SLACK 2021
How one cold Monday cascaded
A textbook chain: a spike meets an architecture that can't scale in time.
Cold caches Traffic spike Gateway saturates Packet loss Scale-up stalls
clients pull extra data 7am mini-peak TGW won't autoscale monitoring goes blind provision hits limits
THE FIXES
› Pre-scale ahead of peaks —request gateway capacity before predictable surges (holidays, launches).
› Load-test the control plane —the provisioning/autoscaling path itself must be tested, not just the app.
› Remove hidden dependencies —monitoring shouldn't die with the data plane; keep dashboards on a separate path.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 41

THE TOOLKIT
Resilience patterns = your fixes
Each pattern answers a specific failure the tests above expose.
Cache / CDN Autoscale + pre-warm
offload reads; kill repeated work → fixes DB & catalog overload. add capacity ahead of the spike → fixes slow reactive scaling.
Load shedding Circuit breaker
reject early (429) to protect the core → fixes total collapse. stop calling a failing dep → fixes cascades & retry storms.
Bulkhead / pool limits Queue-based leveling
isolate resources → fixes thread/connection exhaustion. buffer bursts, process steadily → fixes spikes & herds.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 42

BE HONEST
Pitfalls that make a perf test lie
A green performance report can be as misleading as a green flaky test.
› Averages, not percentiles —hides the slow tail that actually pages you.
› No warm-up / cold cache —first-run numbers aren't steady-state numbers.
› Unrealistic load model —one endpoint, no think-time, uniform data.
› Ignoring the frontend —fast API, slow page; users feel the page.
› One-off, not in CI —a number with no baseline and no trend proves little.
RULE Test the distribution under a realistic model, on a baseline, in CI —or you're measuring a fiction.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 43

GET STARTED
A free stack + an AI-first workflow
Everything below is free / open-source and runs on a laptop and in CI.
FREE STACK AI-FIRST WORKFLOW
› k6 —scripting & thresholds › AI drafts the k6 scenario from the spec.
› Grafana + Prometheus —dashboards › You review the model & data.
› OpenTelemetry —traces › Run in CI with SLO thresholds.
› Lighthouse —frontend vitals › AI flags anomalies & ranks causes.
› GitHub Actions —run in CI › You decide the fix —then re-baseline.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 44

TAKEAWAY
Performance is a distribution, in CI
• 10 test types: load, stress, spike, soak, breakpoint,
READING
scalability, volume, config, isolation, benchmark.
• Assert on p95/p99 and resources, not the mean.
k6 · Gatling docs
• Model reality; baseline; run in CI.
Google Core Web Vitals
• Turn 'be fast' into an SLO the build enforces.
Google SRE book (SLOs)
• AI drafts & explains; you set targets & judge. Grafana k6 + OpenTelemetry
Pyroscope · Datadog APM
Deliverable idea: a k6 suite for the EShop API —load + stress + a CI threshold gate on p95.

APPENDIX
Concepts & abbreviations
SLI·SLO·SLA, p95/p99, APM, RUM, HAR, SRE, OTel — every term in this deck, defined once.
46

THE KEY CHAIN
SLI → SLO → SLA + error budget
From a measured signal, to an internal target, to a contract with penalties.
SLI · indicator SLO · objective SLA · agreement
A measured signal. The internal target. The external contract + penalty.
e.g. % of requests < 300 ms, over 5 min e.g. 99.9% < 300 ms per month e.g. 99.9% uptime or 10% credit
ERROR BUDGET = 100% − SLO. 99.9% ⇒0.1% ⇒~43 min/month of allowed failure; when it's spent, freeze risky releases.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 47

GLOSSARY · 1
Latency & throughput metrics
How fast and how much —the numbers you assert on.
Term / abbrev. What it means
p50 / median Half of requests are faster than this —the 'typical' user.
p95 95% of requests are at least this fast; the slow-but-common tail.
p99 99%; the worst 1% —the tail that pages you. Assert on this, not the average.
TTFB Time To First Byte —server responsiveness before content arrives.
TTFT Time To First Token —for streaming/LLM: wait until the first token.
E2EL End-to-End Latency —total time for the complete response.
Throughput (RPS/TPS) Requests / transactions per second the system sustains.
Goodput Throughput that MEETS the SLO —the honest capacity number.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 48

GLOSSARY · 2
Reliability & SRE
The vocabulary of running services to a target.
Term / abbrev. What it means
SRE Site Reliability Engineering —running ops with software; owns SLOs.
SLI Service Level Indicator —a measured signal (latency, success rate).
SLO Service Level Objective —the internal target for an SLI.
SLA Service Level Agreement —an external contract, with penalties.
Error budget Allowed failure = 100% − SLO; spend it, then freeze risky releases.
Availability 'nines' 99.9% ≈ 8.8 h/yr down · 99.99% ≈ 52 min/yr · 99.999% ≈ 5 min/yr.
MTTR Mean Time To Recovery —how fast you recover from an incident.
MTBF Mean Time Between Failures —how often it breaks.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 49

GLOSSARY · 3
Observability & tooling
How you see what the system is doing.
Term / abbrev. What it means
Observability Infer internal state from outputs —the 3 pillars: metrics, logs, traces.
APM Application Performance Monitoring —Datadog · Dynatrace · New Relic.
RUM Real User Monitoring —performance measured in real users' browsers.
Synthetic monitoring Scripted probes run 24/7 from fixed locations —the canary.
Distributed tracing Follow one request across services to the slow span.
OTel OpenTelemetry —vendor-neutral standard for traces/metrics/logs.
HAR HTTP Archive —a JSON capture of all browser network requests (waterfall).
Profiling / flame graph Sample the running code to find the hot function.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 50

GLOSSARY · 4
Web, load-testing & architecture
Frontend vitals, load-test terms, and common bottleneck names.
Term / abbrev. What it means
LCP Largest Contentful Paint —main content loaded (< 2.5 s = good).
INP Interaction to Next Paint —responsiveness (< 200 ms = good).
CLS Cumulative Layout Shift —visual stability (< 0.1 = good).
CDN Content Delivery Network —caches assets at the edge, near users.
VU Virtual User —one simulated concurrent user in a load test.
Arrival rate Requests/sec started (open model) —pressure regardless of speed.
Think-time The pause a real user takes between actions.
N+1 1 query + N per-row queries —a classic ORM performance bug.
CS423 · CSC15003 —Software Testing (AI-First) · Performance & Stress Testing 51
