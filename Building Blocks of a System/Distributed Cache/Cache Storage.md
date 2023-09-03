### Cache Storage Mechanism
Storing data in the cache isn’t as trivial as it seems because the distributed cache has multiple cache servers. Some of the techniques used to store data in a cache include ->
#### Hash Functions
To Identify the cache server in a distributed cache to store and retrieve data, we can use consistent hashing or its flavors because simple hashing won’t be ideal in case of crashes or scaling.
To Locate cache entries inside each cache server, we can use typical hash functions to locate a cache entry to read or write inside a cache server.
#### Linked Lists
Use a doubly linked list. Adding and removing data from the doubly linked list in our case will be a constant time operation.

![[Pasted image 20230815204751.png]]
#### #Sharding in Cache Clusters:

To avoid SPOF and high load on a single cache instance, we introduce sharding. Sharding involves splitting up cache data among multiple cache servers.
##### Dedicated Cache Clusters
Separate the application and web servers from the cache servers. 

**Advantages ->**

- Flexibility in terms of hardware choices for each functionality

- Allows scaling web/application servers and cache servers separately.

Standalone caching service enables other microservices to benefit from them

![[Pasted image 20230815205134.png]]
##### Co-located Cache Clusters
Embeds cache and service functionality within the same host.
**Advantages ->**  reduction in CAPEX (Capital Expenditure) and OPEX (Operating Expenditure) of extra hardware. With the scaling of one service, automatic scaling of the other service is obtained.

![[Pasted image 20230815205235.png]]

