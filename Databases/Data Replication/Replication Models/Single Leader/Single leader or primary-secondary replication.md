In **primary-secondary replication,** data is replicated across multiple nodes. One node is designated as the primary. It’s responsible for processing any writes to data stored on the cluster. It also sends all the writes to the secondary nodes and keeps them in sync.

## Use-Case: 

Read-Heavy application but it makes the replicating data to secondary nodes a major #bottleneck

## Advantage:

Another advantage of primary-secondary replication is that it’s read resilient. Secondary nodes can still handle read requests in case of primary node failure.

## Disadvantage:

Replication via this approach comes with inconsistency if we use asynchronous replication. Clients reading from different replicas may see inconsistent data in the case of failure of the primary node.

![[Pasted image 20230805153611.png]]

## Methods of implementation:

- [[Statement-based replication]]
- [[Write-ahead log (WAL) shipping]]
- [[Logical (row-based) log replication]]