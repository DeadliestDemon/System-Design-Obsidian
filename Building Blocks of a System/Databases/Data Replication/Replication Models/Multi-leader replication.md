There are multiple primary nodes that process the writes and send them to all other primary and secondary nodes to replicate.

However, it's important to note that not all multi-leader replication setups support reading from followers. In some configurations, followers are strictly used for replicating data from leaders and are not accessible for direct read operations. It depends on the specific implementation and configuration of the multi-leader replication system being used.

## Examples:

This type of replication is used in databases along with external tools like the Tungsten Replicator for MySQL.

![[Pasted image 20230805155407.png]]

## Use-Case:

This kind of replication is quite useful in applications in which we can continue work even if we’re offline—for example, a calendar application in which we can set our meetings even if we don’t have access to the internet. Once we’re online, it replicates its changes from our local database (our mobile phone or laptop acts as a primary node) to other nodes.

## Conficts:

Since all the primary nodes concurrently deal with the write requests, they may modify the same data, which can create a conflict between them.

For example, suppose the same data is edited by two clients simultaneously.

![[Pasted image 20230805155721.png]]
## Handling Conficts:

### Conflict avoidance:

#### Har record ek specific server se hi update hoga

A simple strategy to deal with conflicts is to prevent them from happening in the first place. Conflicts can be avoided if the application can verify that all writes for a given record go via the same leader.

#### Disadvantage:

However, the conflict may still occur if a user moves to a different location and is now near a different data center. If that happens, we need to reroute the traffic. In such scenarios, the conflict avoidance approach fails and results in concurrent writes.

### Last-write-wins:

#### Latest update only

Using their local clock, all nodes assign a timestamp to each update. When a conflict occurs, the update with the latest timestamp is selected.

#### Disadvantage:

This approach can also create difficulty because the clock synchronization across nodes is challenging in distributed systems. There’s clock skew that can result in data loss.

### Custom logic:

In this approach, we can write our own logic to handle conflicts according to the needs of our application. This custom logic can be executed on both reads and writes. When the system detects a conflict, it calls our custom conflict handler.





## Topologies:

![[Pasted image 20230805161112.png]]