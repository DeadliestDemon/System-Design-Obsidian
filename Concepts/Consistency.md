#Concept 
## What is Consistency

- **In a Distributed System ( <mark style="background: #ABF7F7A6;">CAP Theorem</mark> ) -**
	- Each replica node in a distributed system has the same view of data at a given point in time. The other is that each read request gets the value of the recent write. 
	- Each read request gets the value of the recent write.

- **In a Database ( <mark style="background: #ABF7F7A6;">ACID properties</mark> ) -**
	- Each entry in a Table follow a common template. Ensure that the data stored in the database can be related without any anomalies.

These are not the only definitions of consistency, since there are many forms of consistency. Normally, **consistency models** provide us with abstractions to reason about the correctness of a distributed system doing concurrent data reads, writes, and mutations.

**Consistency Spectrum -**
![[Pasted image 20230807151856.png]]

## Consistency Models

### Eventually Consistent   
#Eventually_consistent = **Weakest consistency || High availability**
**When -** The application don’t have strict ordering requirements and don’t require reads to return the latest write choose this model.
**How -** It ensures that all the replicas will eventually return the same value to the read request, but the returned value isn’t meant to be the latest value. However, the value will finally reach its latest state.
**Examples -** #DNS, #NoSQL_Databases  

### Causally Consistent
#Causally_consistent = 
*“Available under Partition”* -> a process can read and write the memory (memory is Available) even while there is no functioning network connection (network is Partitioned) between processes; it is an asynchronous model.
**How -** categorize operations into dependent and independent operations.
	Dependent Operations : causally-related operations - interrelated operations -> input of a function is an output of some function executed previously.
Used to prevent non-intuitive behaviors.
**Examples -** commenting system in many online platforms. For the replies to a comment on a Facebook post, we want to display comments after the comment it replies to. This is because there is a cause-and-effect relationship between a comment and its replies.

### Sequentially Consistent
#Sequentially_consistent = 
Preserves the ordering specified by each client’s program. However, it doesn’t ensure that the writes are visible instantaneously or in the same order as they occurred according to some global clock.
**Examples -** In social networking applications, we usually don’t care about the order in which some of our friends’ posts appear. However, we still anticipate a single friend’s posts to appear in the correct order in which they were created).

### Strictly Consistent | Linearizability
#Strictly_consitent = **Strongest Consistent**
**Synchronous replication**
carry out transactions/operations in sequential, real-time order.
**When -** Applications with strong consistency requirements use techniques like quorum-based replication to increase the system’s availability.
**How -** It ensures that a read request from any replicas will get the latest write value. Once the client receives the acknowledgment that the write operation has been performed, other clients can read that value.
**Examples -** Updating an account’s password requires strict consistency. For example, if we suspect suspicious activity on our bank account, we immediately change our password so that no unauthorized users can access our account.


