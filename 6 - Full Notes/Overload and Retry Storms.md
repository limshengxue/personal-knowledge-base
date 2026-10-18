Tags: [[3 - Tags/software architecture|software architecture]]

# Overload and Retry Storms

## Core Idea
When incoming work exceeds a service's capacity, requests queue and time out. If clients retry immediately, those retries add more work, causing further timeouts. This positive feedback can keep a service overloaded even after the original traffic spike ends; the raw notes call this a *metastable failure*.

## Breaking the Feedback Loop
- Use exponential backoff with randomness so clients do not retry together.
- Limit retries and use circuit breakers or rate limits to avoid repeatedly sending work to a failing dependency.
- Shed load when the service is near capacity, preserving resources for requests it can complete.
- Apply backpressure so producers slow down when consumers cannot keep up.

Each mechanism changes user-visible behavior. Rejection and delayed work should be explicit parts of the service contract. Retries are safest when the operation is idempotent or carries a key that prevents duplicate effects.

## Related Concepts
- [[Response Time and Tail Latency]]
- [[Reliability and Fault Tolerance]]

# References
- [[2 - Source Materials/Books/Design Data-Intensive Application/2.1 Non Functional Requirements]]
