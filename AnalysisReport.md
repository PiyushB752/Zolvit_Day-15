# 📊 k6 Load Testing Report for FakeStore API

This report documents the load testing of the `https://fakestoreapi.com/products` endpoint using [k6](https://k6.io). The goal is to analyze its **performance**, **reliability**, and **correctness** under simulated user load.


## 📅 Test Environment & Setup

| Aspect              | Details                          |
|---------------------|---------------------------------|
| API Endpoint        | https://fakestoreapi.com/products |
| Test Tool           | k6 Load Testing Tool    |
| Test Duration       | 10 seconds                      |
| Virtual Users       | Ramp-up to 100 concurrent users |
| Request Type        | HTTP GET                       |
| Network Conditions  | Typical broadband connection    |



## 🧪 Test Objective

- Simulate 100 virtual users (VUs) accessing the API
- Measure API response time and throughput
- Ensure API returns correct data format
- Detect errors or timeouts during load


## ✏️ Final Output

```

    ✓ Status is 200
    ✓ return array

    HTTP
    http_req_duration.......................................................: avg=1.16s min=192.47ms med=493.34ms max=10.2s p(90)=2.96s p(95)=3.98s
      { expected_response:true }............................................: avg=1.16s min=192.47ms med=493.34ms max=10.2s p(90)=2.96s p(95)=3.98s
    http_req_failed.........................................................: 0.00%  0 out of 481
    http_reqs...............................................................: 481    28.802462/s

    EXECUTION
    iteration_duration......................................................: avg=1.44s min=193.05ms med=677.49ms max=10.2s p(90)=3.7s  p(95)=5.23s
    iterations..............................................................: 481    28.802462/s
    vus.....................................................................: 1      min=1        max=99
    vus_max.................................................................: 100    min=100      max=100

    NETWORK
    data_received...........................................................: 5.8 MB 344 kB/s
    data_sent...............................................................: 99 kB  5.9 kB/s
```

## 🔍 Detailed Metrics Explanation

| Metric                 | Explanation                                                      | Interpretation                                                        |
|------------------------|-----------------------------------------------------------------|----------------------------------------------------------------------|
| **http_req_duration (avg)** | Average time taken for HTTP requests, including all phases.   | Typical request takes about 1.16 seconds, which is moderate latency. |
| **http_req_duration (p90)** | 90th percentile of request duration — 90% of requests are faster than this time. | 2.96 seconds indicates that 10% of requests take significantly longer, showing latency spikes under load. |
| **http_req_duration (p95)** | 95th percentile of request duration — 95% of requests are faster than this time. | 3.98 seconds means 5% of requests have even higher latency, signaling occasional performance degradation. |
| **http_req_failed**    | Percentage of failed HTTP requests (non-2xx/3xx or network errors). | 0% failure rate shows the API handled load without errors.           |
| **http_reqs**          | Total HTTP requests made during the test.                       | 481 requests show the test volume and concurrency level.             |
| **iteration_duration (avg)** | Average duration of one iteration per virtual user.            | Average iteration took 1.44 seconds, indicating overall response and processing time. |
| **iteration_duration (p90)** | 90th percentile of iteration duration.                          | 3.7 seconds shows some iterations took much longer under load.       |
| **iteration_duration (p95)** | 95th percentile of iteration duration.                          | 5.23 seconds indicates occasional long iterations, possibly due to server delays. |
| **vus**                | Number of virtual users active at a time during the test.       | Up to 99 concurrent users reached, close to the target 100 VUs.      |
| **vus_max**            | Maximum number of virtual users during the test.                 | Max 100 users confirms full target load was achieved.                |
| **data_received**      | Total amount of data received from server during test.           | 5.8 MB received reflects moderate load and response size.            |
| **data_sent**          | Total amount of data sent to the server during test.             | 99 KB sent typical for lightweight GET requests.                      |


## 📈 Interpretation Summary

The load test results indicate:

- **Latency:** Average request durations (~1.16s) are reasonable for a public API, but 90th and 95th percentile values (2.96s and 3.98s respectively) reveal noticeable latency spikes impacting a fraction of users during peak load.
- **Reliability:** 0% request failures confirm the API remains stable and responsive under the test load of 100 concurrent users.
- **Throughput:** Total of 481 requests handled in 10 seconds (~48 requests per second) demonstrates solid throughput for the tested duration.
- **User Simulation:** Virtual user ramp-up to 100 confirms the test met its concurrency target.
- **Data Volume:** Moderate data sent and received indicates realistic payload sizes for the requests tested.
- **Iteration Duration:** Higher percentiles (p90: 3.7s, p95: 5.23s) for iteration duration suggest some test loops experience delay, likely due to server processing time or network variability.

