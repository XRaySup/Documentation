Manual for Using Least Ping, Least Load, Burst Observatory, and Observatory
===========================================================================

Introduction
------------

The routing configuration allows for implementing various load balancing strategies for outbound connections. This manual focuses on the utilization of three strategies: "leastPing", "leastLoad", "burstObservatory", and "observatory". Each strategy has its specific configuration parameters and usage instructions.

### 1\. Least Ping Strategy

The "leastPing" strategy prioritizes outbound proxies based on the lowest ping response times.

#### Configuration Parameters:

-   Destination: The URL to which ping requests are sent to measure latency.
-   Interval: Frequency of ping checks.
-   Connectivity: URL for checking local network connectivity.
-   Timeout: Maximum time to wait for each ping request.
-   Sampling: Number of times each outbound proxy is tested per ping check round.

#### Usage:

-   Set the appropriate configuration parameters for "leastPing" in your routing configuration.
-   The strategy will periodically check the ping response times for each outbound proxy.
-   Traffic will be routed through the outbound proxy with the lowest ping response time.

Sample Configuration:

```json
{
  "strategy": "leastPing",
  "destination": "http://www.example.com/ping",
  "interval": "1h",
  "connectivity": "http://example.com/connectivity",
  "timeout": "5s",
  "sampling": 3
}
```

### 2\. Least Load Strategy

The "leastLoad" strategy distributes traffic across outbound proxies based on real-time performance determined by health checks.

#### Configuration Parameters:

-   Interval: Frequency of health checks.
-   Sampling: Number of recent response times considered for selecting outbound proxies.
-   Timeout: Maximum time to wait for each health check request.
-   Destination: URL for health check requests.
-   Connectivity: URL for checking local network connectivity.
-   Costs: Optional parameter to degrade response times based on specific criteria.
-   Baselines: Additional criteria for selecting outbound proxies.
-   Expected: Number of outbound proxies to be selected.
-   MaxRTT: Maximum acceptable round-trip time.
-   Tolerance: Maximum acceptable failure rate.

#### Usage:

-   Configure the "leastLoad" strategy with appropriate parameters in the routing configuration.
-   Health checks will be performed periodically to evaluate the performance of outbound proxies.
-   Traffic will be routed through outbound proxies based on their performance metrics, considering configured parameters such as expected number, baseline criteria, and maximum RTT.

Sample Configuration:

jsonCopy code

```json
{
  "strategy": "leastLoad",
  "interval": "5m",
  "sampling": 5,
  "timeout": "4s",
  "destination": "http://www.example.com/health",
  "connectivity": "http://example.com/connectivity",
  "costs": [
    {
      "match": "specific_proxy",
      "value": 10
    }
  ],
  "baselines": [
    "200ms",
    "400ms"
  ],
  "expected": 3,
  "maxRTT": "1000ms",
  "tolerance": 0.2
}
```

### 3\. Burst Observatory Strategy

The "burstObservatory" strategy conducts health checks by periodically probing specified URLs and subject selectors.

#### Configuration Parameters:

-   Subject Selector: Specifies the outbound proxy tags to monitor.
-   Ping Config: Configuration for ping health checks, including destination URL, interval, connectivity URL, timeout, and sampling.

#### Usage:

-   Define the "burstObservatory" strategy in the routing configuration with appropriate parameters.
-   Probes will be periodically sent to the specified URLs to assess the health and performance of outbound proxies.

Sample Configuration:

```json
{
  "subjectSelector": ["proxy"],
  "pingConfig": {
    "destination": "http://www.apple.com/library/test/success.html",
    "interval": "5m",
    "connectivity": "http://connectivitycheck.platform.hicloud.com/generate_204",
    "timeout": "4s",
    "sampling": 2
  }
}
```

### 4\. Observatory Strategy

The "observatory" strategy provides advanced tools for monitoring and managing outbound traffic.

#### Configuration Parameters:

-   Probe Interval: Frequency of probes to be sent.
-   Probe URL: URL to probe for monitoring purposes.
-   Subject Selector: Specifies the outbound proxy tags to monitor.
-   Enable Concurrency: Option to enable concurrency for faster probing.

#### Usage:

-   Configure the "observatory" strategy with desired parameters in the routing configuration.
-   Probes will be sent at regular intervals to the specified URL, allowing for monitoring and management of outbound traffic.

Sample Configuration:

jsonCopy code

```json
{
  "probeInterval": "5m",
  "probeURL": "https://api.github.com/_private/browser/stats",
  "subjectSelector": ["proxy"],
  "EnableConcurrency": true
}
```

Differences Between Burst Observatory and Observatory
-----------------------------------------------------

-   Burst Observatory: Focuses on conducting health checks by probing specified URLs at regular intervals, primarily used for assessing the health and performance of outbound proxies.
-   Observatory: Provides more advanced tools for monitoring and managing outbound traffic, including options for specifying probe intervals, URLs, and enabling concurrency for faster probing.

Conclusion
----------

By understanding and configuring the "leastPing", "leastLoad", "burstObservatory", and "observatory" strategies, users can effectively manage outbound traffic routing based on latency, performance, and advanced monitoring capabilities provided by V2Ray. Regular monitoring and adjustment of configuration settings ensure optimal performance and reliability of outbound connections.

* * * * *

For more detailed information and troubleshooting tips, you can refer to the official documentation:

-   [V2Ray Official Documentation](https://www.v2fly.org/)
-   [V2Ray GitHub Repository](https://github.com/v2ray/v2ray-core)

Additionally, community forums and discussion boards can be helpful resources for getting assistance from experienced users:

-   V2Ray Community Forum
-   [V2Ray Reddit Community](https://www.reddit.com/r/v2ray/)
