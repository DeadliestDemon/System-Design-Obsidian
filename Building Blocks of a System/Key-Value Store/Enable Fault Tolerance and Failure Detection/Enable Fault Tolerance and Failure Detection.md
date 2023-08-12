
## Handle temporary failures:

We’ll use a sloppy quorum instead of strict quorum membership. Usually, a leader manages the communication among the participants of the consensus. The participants send an acknowledgment after committing a successful write. Upon receiving these acknowledgments, the leader responds to the client. However, the drawback is that the participants are easily affected by the network outage. If the leader is temporarily down and the participants can’t reach it, they declare the leader dead. Now, a new leader has to be reelected. Such frequent elections have a negative impact on performance because the system spends more time picking a leader than accomplishing any actual work.

In the sloppy quorum, the first n healthy nodes from the preference list handle all read and write operations. The n healthy nodes may not always be the first n nodes discovered when moving clockwise in the consistent hash ring.

Let’s consider the following configuration with **n=3**. If node **A** is briefly unavailable or unreachable during a write operation, the request is sent to the next healthy node from the preference list, which is node **D** in this case. It ensures the desired availability and durability. After processing the request, the node **D** includes a hint as to which node was the intended receiver (in this case, **A**). Once node **A** is up and running again, node **D** sends the request information to **A** so it can update its data. Upon completion of the transfer, **D** removes this item from its local storage without affecting the total number of replicas in the system.

This approach is called a **<u>hinted handoff</u>.** Using it, we can ensure that reads and writes are fulfilled if a node faces temporary failure.

![[Pasted image 20230808202846.png]]
### Limitations of Hinted Handoff:

## Handle permanent failures:

In the event of permanent failures of nodes, we should keep our replicas synchronized to make our system more durable. We need to speed up the detection of inconsistencies between replicas and reduce the quantity of transferred data. We’ll use Merkle trees for that.
### [[Merkle Trees]]

## Promote membership in the ring to detect failures:

The nodes can be offline for short periods, but they may also indefinitely go offline. We shouldn’t rebalance partition assignments or fix unreachable replicas when a single node goes down because it’s rarely a permanent departure. Therefore, the addition and removal of nodes from the ring should be done carefully.

Planned commissioning and decommissioning of nodes results in membership changes. These changes form history. They’re recorded persistently on the storage for each node and reconciled among the ring members using a gossip protocol. A **<u>gossip-based protocol</u>** also maintains an eventually consistent view of membership. When two nodes randomly choose one another as their peer, both nodes can efficiently synchronize their persisted membership histories.

Let’s learn how a gossip-based protocol works by considering the following example. Say node **A** starts up for the first time, and it randomly adds nodes **B** and **E** to its token set. The token set has virtual nodes in the consistent hash space and maps nodes to their respective token sets. This information is stored locally on the disk space of the node.

Now, node **A** handles a request that results in a change, so it communicates this to **B** and **E**. Another node, **D**, has **C** and **E** in its token set. It makes a change and tells **C** and **E**. The other nodes do the same process. This way, every node eventually knows about every other node’s information. It’s an efficient way to share information asynchronously, and it doesn’t take up a lot of bandwidth.

Decentralized failure detection protocols use a gossip-based protocol that allows each node to learn about the addition or removal of other nodes. The join and leave methods of the explicit node notify the nodes about the permanent node additions and removals. The individual nodes detect temporary node failures when they fail to communicate with another node. If a node fails to communicate to any of the nodes present in its token set for the authorized time, then it communicates to the administrators that the node is dead.