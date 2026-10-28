
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
- - Step 0.8, bug hunt: The original request used lowercase 'pepperoni' and did not consistently exclude the ingredient. The tool exclusion also needed to use 'Knife' in excludedTools. After changing both values to match the API's tool and ingredient names, all checks passed (225/225, 100%), with 0% HTTP request failures across 75 requests. Under load, the checks exposed incorrect pizza recommendations that a successful HTTP status alone would not detect.

## Part A results
| Step | Test type    | Users | Req/s | p(95) | Max | Failed | Checks |
| ---- | ------------ | ----- | ----- | ----- | --- | ------ | ------ |
| A1   | Smoke        | 1     |  0.488 | 358.66 ms | 367.03 ms | 0% | 100% (20/20) 
| A2   | Average load | 3     |       |       |     |        |        |
| A3   | Stress       | 5     |       |       |     |        |        |
| A4   | Spike        | 5     |       |       |     |        |        |
| A5   | Soak         | 2     |       |       |     |        |        |

## Part B
1.
2.
3.
4.
5.
6.
7.
8.
9.
