**Consistent hashing** assigns each server or item in a distributed hash table a place on an abstract circle, called a ring, irrespective of the number of servers in the table. 

This permits servers and objects to scale without compromising the system’s overall performance.

### Working:

**Consistent hashing** is an effective way to manage the load over the set of nodes. In consistent hashing, we consider that we have a conceptual ring of hashes from `0 to N−1`, where `N` is the number of available hash values. We use each node’s ID, calculate its hash, and map it to the ring. We apply the same process to requests. Each request is completed by the next node that it finds by moving in the clockwise direction in the ring.

Whenever a new node is added to the ring, the immediate next node is affected. It has to share its data with the newly added node while other nodes are unaffected. It’s easy to scale since we’re able to keep changes to our nodes minimal. This is because only a small portion of overall keys need to move. The hashes are randomly distributed, so we expect the load of requests to be random and distributed evenly on average on the ring.

### Hotspots:

However, the request load isn’t equally divided in practice. Any server that handles a large chunk of data can become a bottleneck in a distributed system. That node will receive a disproportionately large share of data storage and retrieval requests, reducing the overall system performance. As a result, these are referred to as **hotspots**.

![[Pasted image 20230806203308.png]]
### Virtual Nodes:

We’ll use virtual nodes to ensure a more evenly distributed load across the nodes. Instead of applying a single hash function, we’ll apply multiple hash functions onto the same key.

#### Working of Virtual Nodes:

Let’s take an example. Suppose we have three hash functions. For each node, we calculate three hashes and place them into the ring. For the request, we use only one hash function. Wherever the request lands onto the ring, it’s processed by the next node found while moving in the clockwise direction. Each server has three positions, so the load of requests is more uniform. Moreover, if a node has more hardware capacity than others, we can add more virtual nodes by using additional hash functions. This way, it’ll have more positions in the ring and serve more requests.


#### Advantages of virtual nodes

Following are some advantages of using virtual nodes:

- If a node fails or does routine maintenance, the workload is uniformly distributed over other nodes. For each newly accessible node, the other nodes receive nearly equal load when it comes back online or is added to the system.

- It’s up to each node to decide how many virtual nodes it’s responsible for, considering the heterogeneity of the physical infrastructure. For example, if a node has roughly double the computational capacity as compared to the others, it can take more load.

## Advantages:
- It’s easy to scale horizontally.
- It increases the throughput and improves the latency of the application.

## Disadvantages:

- Randomly assigning nodes in the ring may cause non-uniform distribution.


