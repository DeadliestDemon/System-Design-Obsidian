
**Key-value stores** are distributed hash tables (DHTs). A key is generated by the hash function and should be unique. In a key-value store, a key binds to a specific value and doesn’t assume anything about the structure of the value. A value can be a blob, image, server name, or anything the user wants to store against a unique key.

![[Pasted image 20230805192011.png]]

## Use-Case:

Such as storing user sessions in a web application and building NoSQL databases.

Examples of key-value store usage include bestseller lists, shopping carts, customer preferences, session management, sales rank, and product catalogs.

Key-value stores can be used to power real-time recommendations and advertising because the stores can swiftly access and present fresh recommendations.

![[Pasted image 20230805225435.png]]

## How will we design a key-value store?

1. [[Design a Key-value Store]]: We’ll define the requirements of a key-value store and design the API.
2. [[Ensure Scalability and Replication]]: We’ll learn to achieve scalability using consistent hashing and replicate the partitioned data.
3. [[Versioning Data and Achieving Configurability]]: We’ll learn to resolve conflicts that occur due to changes made by more than one entity, and we’ll make our system more configurable for different use cases.
4. [[Enable Fault Tolerance and Failure Detection]]: We’ll learn to make a key-value store fault tolerant and how to detect failures in the system.





