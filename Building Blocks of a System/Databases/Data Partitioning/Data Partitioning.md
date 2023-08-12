Data partitioning (or sharding) enables us to use multiple nodes where each node manages some part of the whole data. To handle increasing query rates and data amounts, we strive for balanced partitions and balanced read/write load.

![[Pasted image 20230805162925.png]]

## Sharding:

To divide load among multiple nodes, we need to partition the data by a phenomenon known as **partitioning** or **sharding**. In this approach, we split a large dataset into smaller chunks of data stored at different nodes on our network.

### Generally, we use the following two ways to shard the data:

- ### [[Vertical sharding]]
- ### [[Horizontal sharding]]

![[Pasted image 20230805170740.png]]

## Rebalance the partitions:

Query load can be imbalanced across the nodes due to many reasons, including the following:

- The distribution of the data isn’t equal.
- There’s too much load on a single partition.
- There’s an increase in the query traffic, and we need to add more nodes to keep up.

We can apply the following strategies to rebalance partitions.
#### Avoid hash mod n

Usually, we avoid the hash of a key for partitioning (we used such a scheme to explain the concept of hashing in simple terms earlier). The problem with the addition or removal of nodes in the case of "hash % n" is that every node’s partition number changes and a lot of data moves. For example, assume we have "hash(key)=1235". If we have five nodes at the start, the key will start on node 1 (12351235 % 5=0). Now, if a new node is added, the key would have to be moved to node 6 (12351235 % 6=5), and so on. This moving of keys from one node to another makes rebalancing costly.

#### Fixed number of partitions

In this approach, the number of partitions to be created is fixed at the time when we set our database up. We create a higher number of partitions than the nodes and assign these partitions to nodes. So, when a new node is added to the system, it can take a few partitions from the existing nodes until the partitions are equally divided.

There’s a downside to this approach. The size of each partition grows with the total amount of data in the cluster since all the partitions contain a small part of the total data. If a partition is very small, it will result in too much overhead because we may have to make a large number of small-sized partitions, each costing us some overhead. If the partition is very large, rebalancing the nodes and recovering from node failures will be expensive. It’s very important to choose the right number of partitions. A fixed number of partitions is used in Elasticsearch, Riak, and many more.

#### Dynamic partitioning

In this approach, when the size of a partition reaches the threshold, it’s split equally into two partitions. One of the two split partitions is assigned to one node and the other one to another node. In this way, the load is divided equally. The number of partitions adapts to the overall data amount, which is an advantage of dynamic partitioning.

However, there’s a downside to this approach. It’s difficult to apply dynamic rebalancing while serving the reads and writes. This approach is used in HBase and MongoDB.

#### Partition proportionally to nodes

In this approach, the number of partitions is proportionate to the number of nodes, which means every node has fixed partitions. In earlier approaches, the number of partitions was dependent on the size of the dataset. That isn’t the case here. While the number of nodes remains constant, the size of each partition rises according to the dataset size. However, as the number of nodes increases, the partitions shrink. When a new node enters the network, it splits a certain number of current partitions at random, then takes one half of the split and leaves the other half alone. This can result in an unfair split. This approach is used by Cassandra and Ketama.




## Partition secondary indexes by document:

#### Partition secondary indexes by document:

Each partition is fully independent in this indexing approach. Each partition has its secondary indexes covering just the documents in that partition. It’s unconcerned with the data held in other partitions. If we want to write anything to our database, we need to handle that partition only containing the document ID we’re writing. It’s also known as the local index. In the illustration below, there are three partitions, each having its own identity and data. If we want to get all the customer IDs with the name `John`, we have to request from all partitions.

However, this type of querying on secondary indexes can be expensive. As a result of being restricted by the latency of a poor-performing partition, read query latencies may increase.

![[Pasted image 20230805173123.png]]






#### Partition secondary indexes by the term:

Instead of creating a secondary index for each partition (a local index), we can make a global index for secondary terms that encompasses data from all partitions.

In the illustration below, we create indexes on names (the term on which we’re partitioning) and store all the indexes for names on separated nodes. To get the `cust_id` of all the customers named `John`, we must determine where our term index is located. The `index 0` contains all the customers with names starting with “A” to “M.” The `index 1` includes all the customers with names beginning with “N” to “Z.” Because `John` lies in `index 0`, we fetch a list of `cust_id` with the name `John` from `index 0`.

Partitioning secondary indexes by the term is more read-efficient than partitioning secondary indexes by the document. This is because it only accesses the partition that contains the term. However, a single write in this approach affects multiple partitions, making the method write-intensive and complex.

![[Pasted image 20230805173309.png]]







## [[Request Routing]]


## [[Zookeeper]]