In the **key-range based sharding**, each partition is assigned a continuous range of keys.

Sometimes, a database consists of multiple tables bound by foreign key relationships. In such a case, the horizontal partition is performed using the same partition key on all tables in a relation. Tables (or subtables) that belong to the same partition key are distributed to one database shard.

![[Pasted image 20230805170214.png]]

##### Advantages

- Using this method, the range-query-based scheme is easy to implement.

- Range queries can be performed using the partitioning keys, and those can be kept in partitions in sorted order.

##### Disadvantages

- Range queries can’t be performed using keys other than the partitioning key.

- If keys aren’t selected properly, some nodes (servers) may have to store more data due to an uneven distribution of the traffic.