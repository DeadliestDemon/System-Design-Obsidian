
### We need the following characteristics from our data store:

- #Availability under faults (failure of some disk, nodes, and network and power outages).

- #Scalability (with increasing reads, writes, and other operations).

- Performance (low latency and high throughput for the clients).

## **Replication** refers to keeping multiple copies of the data at various nodes (preferably geographically distributed) to achieve availability, scalability, and performance.

### Additional complexities that could arise due to replication are as follows:

- How do we keep multiple copies of data consistent with each other?
- How do we deal with failed replica nodes?
- Should we replicate synchronously or asynchronously?
    - How do we deal with replication lag in case of asynchronous replication?
- How do we handle concurrent writes?
- What consistency model needs to be exposed to the end programmers?


![[Pasted image 20230805152357.png]]

## Synchronous v/s Asynchronous replication

There are two ways to disseminate changes to the replica nodes:

- ### [[Synchronous replication]]
- ### [[Asynchronous replication]]

![[Pasted image 20230805153047.png]]

## Data replication models:

- [[Single leader or primary-secondary replication]]
- [[Multi-leader replication]]
- [[Peer-to-peer or leaderless replication]]