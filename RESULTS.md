
# K6 lab results

## Part 0
- Step 0.1, http_req_duration: 252.72 ms
- Step 0.2, requests, rate and p(95), and why the rate stays below 5: 80 requests, 3.77 requests/s, p(95) = 330.74 ms. The request rate stays below 5 requests/s because each virtual user spends time waiting for the HTTP response and then sleeps for 1 second before starting the next iteration. The average iteration duration was 1.31 seconds.
- Step 0.3, iterations: 20, checks passed: 100%, http_req_duration avg: 260.07 ms, p(95): 273.5 ms, http_req_failed: 0%.
- Step 0.4, API checks: 100% passed (3/3), http_req_duration avg: 370.15 ms, http_req_failed: 0%.
- Step 0.5, missing-token check: 100% passed (1/1), expected status: 401, http_req_duration avg: 255.94 ms, http_req_failed: 0%.
- Step 0.6, normal run: all thresholds passed; p(95) = 353.39 ms; http_req_failed = 0%; checks = 100%; exit code = 0.
- Step 0.7, requirement test: all thresholds passed; VUs = 3; duration = 30s; p(95) = 362.1 ms (< 1000 ms); p(99) = 368.51 ms (< 1500 ms); http_req_failed = 0%; checks = 100% (136/136); total requests = 68.
- Step 0.7, failing threshold test: changed p(95) threshold to 300 ms; actual p(95) = 365.95 ms, so the threshold failed; p(99) = 373.23 ms; http_req_failed = 0%; checks = 100% (138/138); exit code = 99.
- Step 0.8, bug hunt: The original request used lowercase 'pepperoni' and did not consistently exclude the ingredient. The tool exclusion also needed to use 'Knife' in excludedTools. After changing both values to match the API's tool and ingredient names, all checks passed (225/225, 100%), with 0% HTTP request failures across 75 requests. Under load, the checks exposed incorrect pizza recommendations that a successful HTTP status alone would not detect.

## Part A results
| Step | Test type    | Users | Req/s | p(95) | Max | Failed | Checks |
| ---- | ------------ | ----- | ----- | ----- | --- | ------ | ------ |
| A1   | Smoke        | 1     | 0.488 | 358.66 ms | 367.03 ms | 0% | 100% (20/20) | 
| A2   | Average load | 3     | 1.331 | 348.58 ms | 535.16 ms | 0% | 100% (320/320) |    
| A3   | Stress       | 5     | 2.178 | 394.18 ms | 1.5 s | 0% | 100% (524/524) |      
| A4   | Spike        | 5     | 1.028 | 355.93 ms | 390.05 ms | 0% | 100% (192/192) | 
| A5   | Soak         | 2     |   2   | 363.28 ms | 2.17 s | 0% | 100% (856/856) |   

## Part B
## Part B

1. The results table for A1–A5 is completed in Part A.

2. The request rate stays below 5 requests per second with 5 virtual users because each user must wait for the HTTP response and then sleep for 1 second before starting the next iteration. Therefore, each user cannot send requests continuously.

3. The response time increased slightly from average load to stress testing. The average response time increased from 281.64 ms with 3 VUs to 306.98 ms with 5 VUs, while p(95) increased from 348.58 ms to 394.18 ms. All checks passed and no HTTP requests failed in either test. Based on these results, the system handled up to 5 VUs within the defined performance thresholds. However, these tests do not establish the system's maximum capacity.

4. The spike test had a p(95) response time of 355.93 ms and a maximum response time of 390.05 ms. All checks passed, and no HTTP requests failed. The summary results do not show exactly when the response time increased or returned to its previous level. The A4 HTML report must be examined to determine the recovery time.

5. During the soak test, p(95) was 363.28 ms and the maximum response time was 2.17 seconds. All checks passed, and no HTTP requests failed. These results do not show whether response times or errors worsened over the 11-minute test. The A5 HTML report should be examined for changes over time. A longer soak test could help reveal defects that appear only after extended operation.

6. The p(95) response time is useful because it shows the time within which 95% of requests completed. Unlike the average, it is less affected by the influence of a small number of unusually slow requests and helps assess the experience of most users.

7. Two reasons public-internet performance tests can differ from lab results are network latency or instability between the test machine and the server, and changing server or network conditions such as shared resources and varying traffic. These factors can affect response times and request failures.

8. We should never run stress, spike, or breakpoint tests with hundreds of users against a website we do not own or have permission to test because the traffic could overload the service, disrupt access for real users, or cause an unintended denial of service. Tests should only be performed with authorization and within an agreed load limit.

9. A vague performance requirement might be: "The application should be fast for everyday users." A testable requirement could be: "Under a load of 20 virtual users for 5 minutes, 95% of HTTP requests must complete in less than 1 second, fewer than 1% of requests may fail, and at least 99% of checks must pass." In k6, these requirements can be represented by thresholds such as `http_req_duration: ['p(95)<1000']`, `http_req_failed: ['rate<0.01']`, and `checks: ['rate>0.99']`.