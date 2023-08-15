It involves assigning a set of counters to each node, with each counter representing the number of events that node has encountered. By comparing these counters, the system can determine the causal relationships and ordering of events, even in a distributed environment where different nodes might experience events at different times.

YT Link -> youtube.com/watch?v=Q69Thj9vFw4

![[Pasted image 20230808142205.png]]
### Example:

Let’s consider an example. Say we have a write operation request. Node A handles the first version of the write request, E1. The corresponding vector clock has node information and its counter—that is, {A,1}. Node A handles another write for the same object on which the previous write was performed. So, for E2, we have {A,2}. E1 is no longer required because E2 was updated on the same node. E2 reads the changes made by E1, and then new changes are made. Suppose a network partition happens. Now, the request is handled by two different nodes, B and C. The context with updated versions, which are E3, E4, and their related clocks, which are ({A,2},{B,1}) and ({A,2},{C,1}), are now in the system.

Suppose the network partition is repaired, and the client requests a write again, but now we have conflicts. The context ({A,3},{B,1},{C,1}) of the conflicts are returned to the client. After the client does reconciliation and A coordinates the write, we have E5 with the clock ({A,4}).





## Limitation:

The size of vector clocks may increase if multiple servers write to the same object simultaneously. It’s unlikely to happen in practice because writes are typically handled by one of the top n nodes in a preference list.

For example, if there are network partitions or multiple server failures, write requests may be processed by nodes not in the top n nodes in the preference list. As a result we can have a long version like this: ({A,10},{B,4},{C,1},{D,2},{E,1},{F,3},{G,5},{H,7},{I,2},{J,2},{K,1},{L,1}). It’s a hassle to store and maintain such a long version history.

We can limit the size of the vector clock in these situations. We employ a clock truncation strategy to store a timestamp with each (node, counter) pair to show when the data item was last updated by the node. Vector clock pairs are purged when the number of (node, counter) pairs exceeds a predetermined threshold (let’s say 10). Because the descendant linkages can’t be precisely calculated, this truncation approach can lead to a lack of efficiency in reconciliation.

