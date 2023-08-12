### Cache Storage Mechanism
Storing data in the cache isn’t as trivial as it seems because the distributed cache has multiple cache servers. Some of the techniques used to store data in a cache include ->
#### Hash Functions
To Identify the cache server in a distributed cache to store and retrieve data, we can use consistent hashing or its flavors because simple hashing won’t be ideal in case of crashes or scaling.
To Locate cache entries inside each cache server, we can use typical hash functions to locate a cache entry to read or write inside a cache server.
#### Linked Lists
Use a doubly linked list. Adding and removing data from the doubly linked list in our case will be a constant time operation.
<mark style="background: #D2B3FFA6;">Bloom filters</mark> can be used for quickly finding if a cache entry doesn’t exist in the cache servers. We can use bloom filters to determine that a cache entry is definitely not present in the cache server, but the possibility of its presence is probabilistic. 
#### #Sharding in Cache Clusters
To avoid SPOF and high load on a single cache instance, we introduce sharding. Sharding involves splitting up cache data among multiple cache servers.
##### Dedicated Cache Clusters
Separate the application and web servers from the cache servers. 
**Advantages ->** flexibility in terms of hardware choices for each functionality; Allows scaling web/application servers and cache servers separately.
Standalone caching service enables other microservices to benefit from them
![[Pasted image 20230812140321.png]]
##### Co-located Cache Clusters
Embeds cache and service functionality within the same host.
**Advantages ->**  reduction in CAPEX and OPEX of extra hardware. With the scaling of one service, automatic scaling of the other service is obtained.
![[Pasted image 20230812140334.png]]

