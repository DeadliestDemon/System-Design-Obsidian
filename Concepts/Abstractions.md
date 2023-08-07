#Concept 
### What is Abstraction
Obfuscation ( Hiding ) of details that we don’t need.
### Database Abstraction
***[[Transactions]]*** in a database hides many problematic outcomes when concurrent users are reading, writing, or mutating the data and gives a simple interface of commit, in case of success, or abort, in case of failure. 
The transaction enables end users to not be bogged down by the subtle corner-cases of concurrent data mutation, but rather concentrate on their business logic.

### Abstraction in Distributed Systems
***[[Microservices]]***
	Every service offers different levels of agreement. The details behind implementing these distributed services are hidden from the users, thereby allowing the developers to focus on the application rather than going into the depth of the distributed systems that are often very complex.

### Network Abstractions
-- ***RPC [[Remote Procedure Calls]]***
	Provide an abstraction of a local procedure call to the developers by hiding the complexities of packing and sending function arguments to the remote server, receiving the return values, and managing any network retries.	
	