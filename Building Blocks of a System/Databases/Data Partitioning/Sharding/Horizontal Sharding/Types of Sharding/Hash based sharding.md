**Hash-based sharding** uses a hash-like function on an attribute, and it produces different values based on which attribute the partitioning is performed. 

## Working:

The main concept is to use a hash function on the key to get a hash value and then mod by the number of partitions. 

Once we’ve found an appropriate hash function for keys, we may give each partition a range of hashes (rather than a range of keys). 

Any key whose hash occurs inside that range will be kept in that partition.

## Example:

![[Pasted image 20230805170424.png]]

##### Advantages

- Keys are uniformly distributed across the nodes.

##### Disadvantages

- We can’t perform range queries with this technique. Keys will be spread over all partitions.
