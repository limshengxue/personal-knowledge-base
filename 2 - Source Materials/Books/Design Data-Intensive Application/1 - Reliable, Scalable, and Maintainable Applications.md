2026-03-23 10:02

# 1 - Reliable, Scalable, and Maintainable Applications
## Data Intensive Application
- Most application is data-intensive, they are bounded by data instead of CPU cycle (compute intensive)
- They provide functions like
	- Store data (database)
	- Remember result of expensive operation (cache)
	- Allow users to search data (indexes)
	- Send message to another process (stream)
	- Periodically crunch a large amount of accumulated data (batch processing)

## Selecting Data Systems Tools
- Data system is an umbrella term for databases, queues, caches, etc.
- These tools required carefully selection and combination when building data-intensive application
- The common criteria that become the factor of selecting these tools are Reliability, Scalability, and Maintainability
![[Attachments/Pasted image 20260323100738.png]]

## Reliability
- Continue to work, even when something goes wrong
- Definition
	- It does what user expected
	- Tolerate user making mistake
	- Performance good enough
	- Prevent unauthorized access and abuse
- Fault tolerance/resilient
	- Fault is one of the component deviating from its spec
	- Failure is when the system failed to provide required service to the user
	- We design fault-tolerance system to prevent failure

### Hardware Fault
- Hard disks, RAM, power grid, etc
- Add redundancy can reduce chances of this failure
- But redundancy become hardware when consumption is high and when flavor flexibility

### Software Errors
- Systematic error within the systems, tends to correlate
- Can caused by wrong assumption
- May stay hidden until specific circumstances

### Human Errors
- How to prevent
	- Good design (encourage right thing and discourage wrong thing)
	- Decouple
	- Test thoroughly
	- Allow quick and easy recovery
	- Set up detailed and clear monitoring
	- Good management practices

## Scalability
- System's ability to cope with increased load

### Load
- Can be describe with load parameters, which depends on the system
- For example: request per seconds, ratio of reads to writes in database
- Twitter example, 2 designs
	- 1: Write tweets to database, when user view timeline, read from database, joining user and tweets table to get all followee tweets
	- Read costs higher than write cost
	- 2: Maintain a cache for timeline read, when followee publish tweet, add to the cache 
	- Write cost higher than read cost
- The second design is more scalable initially because timeline read is higher than tweet
- But will face problem for user with large amount of followers
- Hybrid approach was then used

### Performance
- We can observe what happened when load increases
	- When resources unchanged, how much impact to the performance
	- How much resource increment required to keep the performance unchanged
- Performance numbers that we care about change in different systems
- Batch processing - throughput (no of records processed per second/ total time taken to run a job with certain size)
- Online - response time
	- Latency vs response time
	- Response time is what client see, service time + network delay + queue delay
	- Latency is the duration of the request waiting to be handled
- Metrics we used usually include mean or percentiles (better)
- *Why high percentile (p95) can be important 
	- They are usually user with many data in the system
	- When it is a backend service, the frontend might make multiple call in parallel serving a single user, and the longest waiting time decide the response time (*tail latency amplification*)
	- It takes a small amount of slow request to block the whole server (*head-of-line blocking*)
- Percentile often used in service level objective (SLO) and service level agreements (SLA)

### How to Cope with Load
- Scaling up vs scaling out
- Elastic - automatically add computing resources when load increases
- System that handle high load usually highly specific to the particular application

## Maintainability
- 3 principles
- Operability - Easy to operation to run smoothly
- Simplicity - Easy for new engineers to understand the systems
- Evolvability - Easy for engineer to make changes

### Operability
- Operations usually include
	- Monitor health
	- Tracking down problem
	- Keeps software and platforms up-to-date
	- Keeping tabs of how systems interact with each other
- Good operability on system-side
	- Providing visibility into runtime behaviour/internals
	- Provide good support for automation and integration
	- Avoiding dependency on machines
	- Provide good documentation
	- Provide good default behaviour
	- Self-healing
	- Predictable behaviour

### Simplicity
- Does not mean reduce functionality
- We reduce accidental complexity (not inherent from problem but the implementation)
- 1 good tool : abstraction

### Evolvability
- Agile working patterns
- TDD and refactoring



# References
