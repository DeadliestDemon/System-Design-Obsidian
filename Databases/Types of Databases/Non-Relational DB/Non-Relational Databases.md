
## Why to use?

- **Simple design:** 
Unlike relational databases, NoSQL doesn’t require dealing with the impedance mismatch—for example, storing all the employees’ data in one document instead of multiple tables that require join operations. This strategy makes it simple and easier to write less code, debug, and maintain.

- **Horizontal [[Scaling]]:** 
Primarily, NoSQL is preferred due to its ability to run databases on a large cluster. This solves the problem when the number of concurrent users increases. NoSQL makes it easier to scale out since the data related to a specific employee is stored in one document instead of multiple tables over nodes. NoSQL databases often spread data across multiple nodes and balance data and queries across nodes automatically. In case of a node failure, it can be transparently replaced without any application disruption.

- **Availability:** 
To enhance the #availability of data, node replacement can be performed without application downtime. Most of the non-relational databases’ variants support data replication to ensure high #availability and disaster recovery.

- **Support for unstructured and semi-structured data:**
Many NoSQL databases work with data that doesn’t have schema at the time of database configuration or data writes. For example, document databases are structureless; they allow documents (JSON, XML, BSON, and so on) to have different fields. For example, one JSON document can have fewer fields than the other.

- **Cost:** 
Licenses for many RDBMSs are pretty expensive, while many NoSQL databases are open source and freely available. Similarly, some RDBMSs rely on costly proprietary hardware and storage systems, while NoSQL databases usually use clusters of cheap commodity servers.

## Types of NoSQL DB:

![[Pasted image 20230805020541.png]]

- ### [[Key-Value Database]]
- ### [[Document Database]]
- ### [[Graph Database]]
- ### [[Columnar Database]]

## Drawbacks:

#### Lack of standardization

NoSQL doesn’t follow any specific standard, like how relational databases follow relational algebra. Porting applications from one type of NoSQL database to another might be a challenge.

#### Consistency
Non-relational/ NoSQL databases follow #Eventually_consistent  
NoSQL databases provide different products based on the specific trade-offs between consistency and availability when failures can happen. We won’t have strong data integrity, like primary and referential integrities in a relational database. Data might not be strongly consistent but slowly converging using a weak model like eventual consistency.