
Imagine you have a team of people working on the same document at different locations. Sometimes, the internet connection breaks, or someone's computer stops working while they are updating the document. When this happens, the different versions of the document might not match anymore. So, we need a way to make sure that we don't lose any of the updates and that the final document is consistent.

To solve this, we want to keep track of the order in which updates happened. One way to do this is by using timestamps like "date and time" to see which update came first. However, time can be tricky in a system with many computers because they might not all agree on the exact time.

Instead, we can use something called "vector clocks."
So, if we see that two versions of the document have different vector clocks, we know that they are not related to each other, and there might be conflicting changes. We need to figure out how to combine them correctly, so we have a consistent and complete document in the end. This process of combining and resolving conflicting changes is essential to keep everything in order and make sure the document is reliable.

## [[Vector Clocks]]

## [[Modify the API Design]]

## Get and Put Operations:

Every node can handle the `get` (read) and `put` (write) operations in our system. A node handling a read or write operation is known as a **coordinator**. The coordinator is the first among the top n nodes in the preference list.

There can be two ways for a client to select a node:

- We route the request to a generic load balancer.
- We use a partition-aware client library that routes requests directly to the appropriate coordinator nodes.

Both approaches have their benefits. The client isn’t linked to the code in the first approach, whereas lower latency is achievable in the second. The latency is lower due to the reduced number of hops because the client can directly go to a specific server.

Let’s make our service configurable by having an ability where we can control the trade-offs between availability, consistency, cost-effectiveness, and performance. We can use a consistency protocol similar to those used in #Quorums systems.

Let’s take an example. Say n in the top n of the preference list is equal to 33. It means three copies of the data need to be maintained. We assume that nodes are placed in a ring. Say A, B, C, D, and E is the clockwise order of the nodes in that ring. If the write function is performed on node A, then the copies of that data will be placed on B and C. This is because B and C are the next nodes we find while moving in a clockwise direction of the ring.

## Usage of r and w:

Now, consider two variables, r and w. The r means the minimum number of nodes that need to be part of a successful read operation, while w is the minimum number of nodes involved in a successful write operation. So if r=2, it means our system will read from two nodes when we have data stored in three nodes. We need to pick values of r and w such that at least one node is common between them. This ensures that readers could get the latest-written value. For that, we’ll use a quorum-like system by setting **r + w > n**.

The following table gives an overview of how the values of n, r, and w affect the speed of reads and writes:

Value Effects on Reads and Writes

|   |   |   |   |
|---|---|---|---|
|**n**|**r**|**w**|**Description**|
|3|2|1|It won't be allowed as it violates our constraint _r + w > n_ .|
|3|2|2|It will be allowed as it fulfills constraints.|
|3|3|1|It will provide speedy writes and slower reads since readers need to go to all _n_ replicas for a value.|
|3|1|3|It will provide speedy reads from any node but slow writes since we now need to write to all _n_ nodes synchronously.|

Let’s say n=3, which means we have three nodes where the data is copied to. Now, for w=2, the operation makes sure to write in two nodes to make this request successful. For the third node, the data is updated asynchronously.

In this model, the latency of a get operation is decided by the slowest of the r replicas. The reason is that for the larger value of r, we focus more on availability and compromise consistency.

The coordinator produces the vector clock for the new version and writes the new version locally upon receiving a `put()` request for a key. The coordinator sends n highest-ranking nodes with the updated version and a new vector clock. We consider a write successful if at least w−1 nodes respond. Remember that the coordinate writes to itself first, so we get w writes in total.

Requests for a `get()` operation are made to the n highest-ranked reachable nodes in a preference list for a key. They wait for r answers before returning the results to the client. Coordinators return all dataset versions that they regard as unrelated if they get several datasets from the same source (divergent histories that need reconciliation). The conflicting versions are then merged, and the resulting key’s value is rewritten to override the previous versions.
