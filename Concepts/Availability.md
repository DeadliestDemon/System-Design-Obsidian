#non-functional_requirement
## What is Availability
percentage of time that some service or infrastructure is accessible to clients and is operated upon under normal conditions. 
100% availability = service functions and responds as intended (operates normally) all the time.

## Measuring Availability
![[Pasted image 20230807164705.png]]
![[Pasted image 20230807164821.png]]


## Availability Patterns
### Fail-Over
#### Active-Passive
With active-passive fail-over, heartbeats are sent between the active and the passive server on standby. If the heartbeat is interrupted, the passive server takes over the active's IP address and resumes service.

The length of downtime is determined by whether the passive server is already running in 'hot' standby or whether it needs to start up from 'cold' standby. Only the active server handles traffic.

Active-passive failover can also be referred to as master-slave failover.

#### Active-Active
In active-active, both servers are managing traffic, spreading the load between them.

If the servers are public-facing, the DNS would need to know about the public IPs of both servers. If the servers are internal-facing, application logic would need to know about both servers.

Active-active failover can also be referred to as master-master failover.

### Replication
- [[Data Replication]]
## Availability vs consistency

**CAP Theorem**
[[CAP Theorem]]

