### Why Cache
As the number of users increases, the database queries also increase. And as a result, the service providers are overburdened, resulting in slow performance. In such cases, a cache is added to the system to deal with performance deterioration. 
Improves page load time and can reduce the load on the server and databases. The Dispatcher will first Look-up if the request was made previously and try to find the previous result to return, in order to save the actual execution. Putting a cache in front of a database can help absorb uneven loads and spikes in traffic, thereby avoiding the possibility of a bottleneck.
### What is Cache
A temporary data storage that can serve data faster by keeping data entries in memory.  Stores only the most frequently accessed data. When a request reaches the serving host, it retrieves data from the cache <mark style="background: #D2B3FFA6;">(cache hit)</mark> and serves the user. However, if the data is unavailable in the cache <mark style="background: #D2B3FFA6;">(cache miss)</mark>, the data will be queried from the database. Also, the cache is populated with the new value to avoid cache misses for the next time.

A nonpersistent storage area used to keep repeatedly read and written data, which provides the end user with lower latency. Therefore, a cache must serve data from a storage component that is fast, has enough storage, and is affordable in terms of dollar cost as we scale the caching service. 
![[Pasted image 20230811222840.png]]

### What is Distributed Cache?
A caching system where multiple cache servers coordinate to store frequently accessed data. Distributed caches are needed in environments where a single cache server isn’t enough to store all the data. At the same time, it’s scalable and guarantees a higher degree of availability.

Uses the locality of reference principle.
**Benefits ->**
1. Minimize user-perceived latency by pre-calculating results and storing frequently accessed data.
2. Pre-generate expensive queries from the database.
3. Store user session data temporarily.
4. Serve data from temporary storage even if the data store is down temporarily.
5. Reduce network costs by serving data from local resources.

### Why Distributed Cache?
When the size of data required in the cache increases, storing the entire data in one system is impractical. This is because of these three reasons:
- It can be a potential single point of failure (SPOF).
- A system is designed in layers, and each layer should have its caching mechanism to ensure the decoupling of sensitive data from different layers.
- Caching at different locations helps reduce the serving latency at that layer.

### Caching at Different Layers of a System
1. **Web Layer ->** 
	 HTTP cache headers, web accelerators, key-value store, CDNs, and so on
	 Accelerate retrieval of static web content, and manage sessions

2. **Application Layer ->** 
	 Local cache and key-value data store
	 Accelerate application-level computations and data retrieval

3. **Database Layer ->**
	 Database cache, buffers, and key-value data store
	 Reduce data retrieval latency and I/O load from database

### Possible Ways to implement Caching
1. **Client Caching —** Located at the client side (OS or Browser), or in a distinct cache layer. 
2. **CDN Caching —** CDN === Caching, for faster delivery of web-services  
3. **Web Server caching —** Web servers can also cache requests, and can serve static and dynamic content directly, without having to contact application servers.
4. **Database Caching —**  
5. **Application Caching —** In-memory caches such as Memcached and Redis are key-value stores between your application and your data storage. Since the data is held in RAM, it is much faster than typical databases where data is stored on disk. RAM is more limited than disk, so c*ache invalidation algorithms such as least recently used (LRU) can help invalidate 'cold' entries and keep 'hot' data in RAM.* Generally, you should try to avoid file-based caching, as it makes cloning and auto-scaling more difficult.
    1. **Caching at database query level —** Whenever you query the database, hash the query as a key and store the result in the cache. Issues: Hard to delete a cached result with complex queries, If one piece of data changes such as a table cell, you need to delete all cached queries that might include the changed cell.
    2. **Caching at object level —** Similar to what you do with your application code. Have your application assemble the dataset from the database into a class instance or a data structure(s): Remove the object from the cache if its underlying data has changed; Allows for asynchronous processing: workers assemble objects by consuming the latest cached object

### How is Distributed Cache built
#### Writing Policies 
1. **Client-aside ( lazy loading ):** Application responsible for reading & writing from storage ⇒ Cache does not directly interact with storage. 
	**Steps -** Look for an entry in the cache, resulting in a cache miss → Load entry from the database → Add entry to cache → Return entry 
	**Example —** Memcached. Subsequent reads of data added to the cache are fast. Only requested data is cached, which avoids filling up the cache with data that isn't requested.

2. **Write-through cache:** Writes on the cache as well as on the database. Writing on both storages can happen concurrently or one after the other. This increases the write latency but ensures strong consistency between the database and the cache.
	**Steps -** Application adds/updates entry in cache → Cache synchronously writes entry to the database → Return the value.
	Slow overall operation due to write operation, but the subsequent write operations are faster. Users are generally more tolerant of latency when updating data than reading data. Data in the cache is not stale.

3. **Refresh-ahead:** Configure the cache to automatically refresh any recently accessed cache entry prior to its expiration. can reduce latency than client-aside if the cache is successfully able to predict the items to be used in the near future.
4. **Write-back cache:** Data is first written to the cache and asynchronously written to the database. Although the cache has updated data, inconsistency is inevitable in scenarios where a client reads stale data from the database.
5. **Write-around cache:** Data is written to the database only. Later, when a read is triggered for the data, it’s written to cache after a cache miss. The database will have updated data, but such a strategy isn’t favorable for reading recently updated data.
#### Eviction Policies
Remove less frequently accessed data from the cache. Various Strategies:
1. Least recently used (LRU)
2. Most recently used (MRU)
3. Least frequently used (LFU)
4. Most frequently used (MFU)

#### Cache Invalidation
Some data residing in the cache may become stale or outdated over time. Such cache entries are invalid and must be marked for deletion.
Solution -> maintaining a time-to-live (TTL) value to deal with outdated cache items.
Implementation Strategies:
- **Active expiration**: This method actively checks the TTL of cache entries through a daemon process or thread.
- **Passive expiration**: This method checks the TTL of a cache entry at the time of access.
#### Storage Mechanism
Storing data in the cache isn’t as trivial as it seems because the distributed cache has multiple cache servers. Some of the storage strategies are discussed in [[Cache Storage]]

#### Client Cache
A piece of code residing in hosting servers that do (hash) computations to store and retrieve data in the cache servers. Also, cache clients may coordinate with other system components like monitoring and configuration services. All cache clients are programmed in the same way so that the same PUT, and GET operations from different clients return the same results.
All clients can use well-known transport protocols like TCP or UDP to talk to the cache servers.

### [[HLD of a Distributed Cache]]