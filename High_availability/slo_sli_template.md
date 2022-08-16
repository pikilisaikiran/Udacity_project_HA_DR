**Availability:**

slo: Application up 99% of the time over the last 7 days

sli: sum (rate(apiserver_request_total{job="apiserver",code!~"5.."}[7d]))/sum (rate(apiserver_request_total{job="apiserver"}[7d]))

**Error budget:**

slo: Error budget at 10% usage over the last 15 days.

sli: 1 - ((1 - (sum(increase(apiserver_request_total{job="apiserver", code="200"}[15d])) by (verb)) / sum(increase(apiserver_request_total{job="apiserver"}[15d])) by (verb)) / (1 - .90))

**Latency:** 

slo: 99% of web requests completed successfully in 50ms; 95% of API requests fulfilled in 50ms or less over the last 5 minutes.

sli: histogram_quantile(0.95,sum(rate(apiserver_request_duration_seconds_bucket{job="apiserver"}[5m])) by (le, verb))

(or) histogram_quantile(0.99,sum(rate(apiserver_request_duration_seconds_bucket{job="apiserver"}[5m])) by (le, verb))

**Throughput:**

slo: 15 orders per minute in a shopping cart; 500 transactions per second on a database over the last 10 minutes.

sli: sum(rate(apiserver_request_total{job="apiserver",code=~"2.."}[5m]))

**Correctness:**

99% of the orders are correct over the last 24 hours.

**Freshness:** 

90% of the data read for the website should be using the most recent data written to the database over the last 15 minutes.
