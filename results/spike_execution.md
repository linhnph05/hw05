# Spike execution stages

The single `23127081_Spike_20260816.jmx` plan was run three times with the
same workflow and CSV input. The three raw stage outputs were merged, in time
order, into `spike.jtl`. The main `spike_html` dashboard shows the 30-user
high-load stage. The baseline and recovery dashboards are also kept.

| Stage | Users | Ramp-up | Duration | Samples | RPS | Average | p95 | Maximum | Errors |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| Baseline | 2 | 1 s | 60 s | 231 | 3.91 | 8.19 ms | 25 ms | 52 ms | 0 |
| Spike | 30 | 5 s | 60 s | 3,327 | 56.24 | 11.14 ms | 39 ms | 206 ms | 0 |
| Recovery | 2 | 1 s | 60 s | 230 | 3.91 | 8.35 ms | 22 ms | 127 ms | 0 |

The merged file contains 3,788 samples with 0.00% errors, mean response time
10.79 ms, p95 38 ms, and maximum response time 206 ms.

The backend monitor sampled for 190 seconds. Its average CPU was 2.68%, peak
CPU was 16.0%, average RSS was 38,908 KB, and peak RSS was 64,368 KB.
