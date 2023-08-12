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