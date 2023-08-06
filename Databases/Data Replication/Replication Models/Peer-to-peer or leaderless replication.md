All the nodes have equal weightage and can accept reads and writes requests. 

## Example:

DynamoDB by Amazon

![[Pasted image 20230805160403.png]]

## Disadvantage:

Like primary-secondary replication, this replication can also yield inconsistency. This is because when several nodes accept write requests, it may lead to concurrent writes. A helpful approach used for solving write-write inconsistency is called **quorums**.

## Quorums:

Let’s suppose we have three nodes. If at least two out of three nodes are guaranteed to return successful updates, it means only one node has failed. This means that if we read from two nodes, at least one of them will have the updated version, and our system can continue working.

If we have  **N** nodes, then every write must be updated in at least **W** nodes to be considered a success, and we must read from **R** nodes. We’ll get an updated value from reading as long as **<u>W + R > N</u>** because at least one of the nodes must have an updated write from which we can read. Quorum reads and writes adhere to these **r** and **w** values. These **N, W, and R** are configurable in Dynamo-style databases.

![[Pasted image 20230805161436.png]]

