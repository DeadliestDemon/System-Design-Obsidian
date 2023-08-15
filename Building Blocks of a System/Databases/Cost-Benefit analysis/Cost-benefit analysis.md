### Advantages and disadvantages of a centralized database

#### Advantages
- Data maintenance, such as updating and taking backups of a centralized database, is easy.

- Centralized databases provide stronger consistency and ACID transactions than distributed databases.

- Centralized databases provide a much simpler programming model for the end programmers as compared to distributed databases.
  
- It’s more efficient for businesses to have a small amount of data to store that can reside on a single node.

#### Disadvantages

- A centralized database can slow down, causing high latency for end users, when the number of queries per second accessing the centralized database is approaching single-node limits.

- A centralized database has a single point of failure. Because of this, its probability of not being accessible is much higher.

### Advantages and disadvantages of a distributed database

#### Advantages

- It’s fast and easy to access data in a distributed database because data is retrieved from the nearest database shard or the one frequently used.

- Data with different [[levels of distribution transparency]] can be stored in separate places.

- Intensive transactions consisting of queries can be divided into multiple optimized subqueries, which can be processed in a parallel fashion.


#### Disadvantages

- Sometimes, data is required from multiple sites, which takes more time than expected.

- Relations are partitioned vertically or horizontally among different nodes. Therefore, operations such as joins need to reconstruct complete relations by carefully fetching data. These operations can become much more expensive and complex.

- It’s difficult to maintain consistency of data across sites in the distributed database, and it requires extra measures.

- Updations and backups in distributed databases take time to synchronize data.

### [[Query optimization and processing speed in a distributed database]]

### Conclusion:

Data distribution (vertical and horizontal sharding) across multiple nodes aims to improve the following features, considering that the queries are optimized:

- #Reliability (fault-tolerance)
- Performance
- Balanced storage capacity and dollar costs