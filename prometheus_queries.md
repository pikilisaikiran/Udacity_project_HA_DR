## Availability SLI
### The percentage of successful requests over the last 5m

sum(increase(apiserver_requests_total{job="apiserver",code!~"5.."} [5m])) / sum(increase(apiserver_requests_total{} [5m])) * 100

## Latency SLI
### 90% of requests finish in these times

histogram_quantile(0.90, sum by (le) (rate(apiserver_request_duration_seconds_bucket{job="apiserver"}[$__range])))

## Throughput
### Successful requests per second

sum(rate(apiserver_requests_total{job="apiserver"}[$__range]))

## Error Budget - Remaining Error Budget
### The error budget is 20%

1 - ((1 - (sum(increase(apiserver_request_total{job="apiserver", code="200"}[15d])) by (verb)) / sum(increase(apiserver_request_total{job="apiserver"}[15d])) by (verb)) / (1 - .90))
