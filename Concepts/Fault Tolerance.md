#non-functional_requirement
## Fault Tolerance
Real-world, large-scale applications run hundreds of servers and databases to accommodate billions of users’ requests and store significant data. These applications need a mechanism that helps with data safety and eschews the recalculation of computationally intensive tasks by avoiding a single point of failure.
Handling [[Failures]] that may occur in a Distributed Systems.
It refers to a system’s ability to execute persistently even if one or more of its components fail. Here, components can be software or hardware. Conceiving a system that is hundred percent fault-tolerant is practically very difficult.

### Fault Tolerance Techniques
#### Replication
One of the most widely-used techniques is replication-based fault tolerance. With this technique, we can replicate both the services and data. We can swap out failed nodes with healthy ones and a failed data store with its replica. A large service can transparently make the switch without impacting the end customers.

Create multiple copies of our data in separate storage. All copies need to update regularly for consistency when any update occurs in the data. Updating data in replicas is a challenging job. Hence, we compromise either on availability or on consistency under failures—a reality that is outlined in the CAP theorem.

#### Checkpointing
A technique that saves the system’s state in stable storage when the system state is consistent. Checkpointing is performed in many stages at different time intervals. The primary purpose is to save the computational state at a given point. When a failure occurs in the system, we can get the last computed data from the previous checkpoint and start working from there.

Checkpointing also comes with the same problem that we have in replication. When the system has to perform checkpointing, it makes sure that the system is in a consistent state, meaning that all processes are stopped.
- **Consistent state**: No communication among the processes when the system performs checkpointing. All the processes are sending or receiving messages before and after checkpointing.
- **Inconsistent state**: Processes communicate through messages when the system performs checkpointing. This indicates an inconsistent state, because when we get a previously saved checkpoint, _Process i_ will have a message (m11​) and _Process j_ will have no record of message sending.

![[Pasted image 20230807191246.png]]
