# API Service

| Category     | SLI | SLO                                                                                                         |
|--------------|-----|-------------------------------------------------------------------------------------------------------------|
| Availability |  total number of successful requests with respect to total number of requests to the server   | 99%                                                                                                         |
| Latency      |  90th percentile latency over a 10 min period   | 90% of requests below 100ms                                                                                 |
| Error Budget |  the number of error requests/total number of requests in budget   | Error budget is defined at 20%. This means that 20% of the requests can fail and still be within the budget |
| Throughput   |  total number of (successful) requests over a period of time   | 5 RPS indicates the application is functioning


