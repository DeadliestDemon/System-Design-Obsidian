#Concept 
## Non-functional System Characteristics

1. [[Availability]]
2. [[Reliability]]
3. [[Scalability]]
4. [[Maintainability]]
5. [[Fault Tolerance]]

### Performance vs scalability
A service is scalable if it results in increased performance in a manner proportional to resources added. Generally, increasing performance means serving more units of work, but it can also be to handle larger units of work, such as when datasets grow.
- If you have a **performance** problem, your system is slow for a single user.
- If you have a **scalability** problem, your system is fast for a single user but slow under heavy load.

### Latency vs throughput
**Latency** is the time to perform some action or to produce some result.
**Throughput** is the number of such actions or results per unit of time.
Generally, you should aim for <mark style="background: #D2B3FFA6;">maximal throughput with acceptable latency.</mark>
