## CAP Theorem

In a distributed computer system, you can only support two of the following guarantees:
1. **Consistency -** 
	-> Every read receives the most recent write or an error
	-> Consistency maintained among replicated nodes
	-> Sequential Consistency
	-> Every client receives same view of data 
	
2. **Availability -**
	-> Every request receives a response, without guarantee that it contains the most recent version of the information
	-> Every non-failing node, returns a response for all reads & write the requests in resonable amount of time
	
3. **Partition Tolerance -** 
	-> The system continues to operate despite arbitrary partitioning due to network failures
	-> Partitioned system can be recovered later, once the network in restarted
	

![[Pasted image 20230807193423.png]]

Networks aren't reliable, so you'll need to support partition tolerance. You'll need to make a software tradeoff between consistency and availability.

**CP - consistency and partition tolerance**
Waiting for a response from the partitioned node might result in a timeout error. CP is a good choice if your business needs require atomic reads and writes.
-> Examples: MongoDB, Redis 

**AP - availability and partition tolerance**
Responses return the most readily available version of the data available on any node, which might not be the latest. Writes might take some time to propagate when the partition is resolved.
-> Examples: Amazon Dynamo DB, Google Cloud

AP is a good choice if the business needs to allow for eventual consistency or when the system needs to continue working despite external errors.