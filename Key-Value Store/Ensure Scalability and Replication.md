
## Add scalability:

We store key-value data in storage nodes. With a change in demand, we might need to add or remove storage nodes. It means we need to partition data over the nodes in the system to distribute the load across all nodes.

We want to add and remove nodes with minimal change in our infrastructure. But in this method, when we add or remove a node, we need to move a lot of keys. This is inefficient. For example, node 2 is removed, and suppose for the same request ID, the new server to process a request will be node 1 because 10%3=1. Nodes hold information in their local caches, like keys and their values. So, we need to move that request’s data to the next node that has to process the request. But this replication can be costly and can cause high latency.

For minimal change in our infrastructure, we use:

## [[Consistent Hashing]]

## [[Data Replication]]:

- ### [[Single leader or primary-secondary replication]]

- ### [[Peer-to-peer or leaderless replication]]:

We’ll use a peer-to-peer relationship for replication. We’ll replicate the data on multiple hosts to achieve durability and high availability. Each data item will be replicated at **n** hosts, where **n** is a parameter configured per instance of the key-value store. For example, if we choose **n** to be 5, it means we want our data to be replicated to five nodes.

Each node will replicate its data to the other nodes. We’ll call a node coordinator that handles read or write operations. It’s directly responsible for the keys. A coordinator node is assigned the key “**K**.” It’s also responsible for replicating the keys to **n−1** successors on the ring (clockwise). These lists of successor virtual nodes are called preference lists. To avoid putting replicas on the same physical nodes, the preference list can skip those virtual nodes whose physical node is already in the list.

Let’s consider the illustration given below. We have a replication factor, **n**, set to 3. For the key “K,” the replication is done on the next three nodes: B, C, and D. Similarly, for key “L,” the replication is done on nodes C, D, and E.

![[Pasted image 20230806204146.png]]

### Note: For key-value stores, we prefer availability over consistency.