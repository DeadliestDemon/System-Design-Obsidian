### HLD of Distributed Cache

**Functional Requirements ->**
1. **Insert data:** Insert an entry to the cache.
2. **Retrieve data:** Retrieve data corresponding to a specific key.
**Non-Functional Requirements ->**
1. **High performance**: Enable fast retrieval of data. i.e., both the `insert` and `retrieve` operations must be fast.
2. **Scalability:** It should scale horizontally with no bottlenecks on an increasing number of requests.
3. **High availability**: The unavailability of the cache will put an extra burden on the database servers, which can also go down at peak load intervals. We also require our system to survive occasional failures of components and network, as well as power outages.
4. **Consistency:** Data stored on the cache servers should be consistent. For example, different cache clients retrieving the same data from different cache servers (primary or secondary) should be up to date.
5. **Affordability:** Ideally, the caching system should be designed from commodity hardware instead of an expensive supporting component within the design of a system.

### Limitations
**Maintaining cache servers list ->**
1. Maintain a configuration file in each of the service hosts where the cache clients reside. 
	The configuration file will contain the updated health and metadata required for the cache clients to utilize the cache servers efficiently. *"configuration service can be updated through a push service"*. **Problem** => the configuration file will have to be manually updated and deployed.
↓
2. Store the configuration file in a centralized location. 
	The cache clients can use this to get updated information about cache servers. solves the deployment issue, but we still need to manually update the configuration file and monitor the health of each server.
↓
3. Use a configuration service that continuously monitors the health of the cache servers.
	Cache clients will get notified when a new cache server is added to the cluster or removed from the cluster.

**Improving Availability ->**
Cache servers fail -- addition of replica nodes -- adding one primary and two backup nodes in a cache shard -- possibility of inconsistency -- divide cache data among shards so that neither the problem of unavailability arises nor any hardware is wasted.

**Internal Implementation ->**
![[Pasted image 20230814231111.png]]