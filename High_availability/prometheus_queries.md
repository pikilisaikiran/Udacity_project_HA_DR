Availability

sum(increase(apiserver_requests_total{job="apiserver",code!~"5.."} [$__range])) / sum(increase(apiserver_requests_total{} [$__range])) * 100

Latency

histogram_quantile(0.95, sum by (le) (rate(apiserver_request_duration_seconds_bucket{job="apiserver"}[$__range])))

Throughput

sum(rate(apiserver_requests_total{job="apiserver"}[$__range]))

Error Budget

1 - ((1 - (sum(increase(apiserver_request_total{job="apiserver", code="200"}[7d])) by (verb)) / sum(increase(apiserver_request_total{job="apiserver"}[7d])) by (verb)) / (1 - .90))
