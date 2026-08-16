# Spike execution stages

The single `23127081_Spike_20260816.jmx` plan was run three times with the
same workflow and CSV input. The three raw stage outputs were merged, in time
order, into `spike.jtl`; its HTML report was generated from that merged JTL.

| Stage | Users | Ramp-up | Duration | Samples | RPS | Average | Maximum | Errors |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| Baseline | 2 | 1 s | 60 s | 230 | 3.83 | 10.43 ms | 139 ms | 0 |
| Spike | 30 | 5 s | 60 s | 3,255 | 54.25 | 23.37 ms | 622 ms | 0 |
| Recovery | 2 | 1 s | 60 s | 226 | 3.77 | 14.84 ms | 377 ms | 0 |

The backend monitor sampled for 190 seconds. Its maximum CPU was 18.0% and
maximum RSS was 63,664 KB.
