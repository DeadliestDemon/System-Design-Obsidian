To track changes in the cluster, many distributed data systems need a separate management server like ZooKeeper. 

**Zookeeper** keeps track of all the mappings in the network, and each node connects to ZooKeeper for the information. 

Whenever there’s a change in the partitioning, or a node is added or removed, ZooKeeper gets updated and notifies the routing tier about the change. 

## Examples:
HBase, Kafka and SolrCloud use ZooKeeper.