Tags: [[3 - Tags/software architecture|software architecture]]

# Response Time and Tail Latency

## Core Idea
Response time is the time a client observes between making a request and receiving its response. It includes active service time, time waiting in queues, and network delay. Throughput measures how much work the system completes per unit of time. Both must be considered under a defined load.

## Why Percentiles Matter
An average does not show how many users have a slow experience. The median describes the middle request; p95 and p99 describe increasingly slow tails. Tail latency matters when a user action fans out into several backend calls: the slowest required call can determine the whole response time.

Queueing can amplify variation. A slow request may block work behind it, producing head-of-line blocking. As throughput approaches capacity, queues grow and response time can deteriorate quickly even if the service time of each request changes little.

Percentiles are useful for service objectives, but they should be measured across the complete request population. Averaging percentiles from separate machines does not produce the percentile for all requests.

## Practical Use
Measure client-observed response time at representative load, then separate queueing, processing, and network delay to find the bottleneck. Choose percentile targets that reflect the user experience and the cost of improving the tail.

## Related Concepts
- [[Scalability and Load]]
- [[Overload and Retry Storms]]

# References
- [[2 - Source Materials/Books/Design Data-Intensive Application/1 - Reliable, Scalable, and Maintainable Applications]]
- [[2 - Source Materials/Books/Design Data-Intensive Application/2.1 Non Functional Requirements]]
