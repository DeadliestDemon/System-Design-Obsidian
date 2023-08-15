In the **statement-based replication** approach, the primary node saves all statements that it executes, like insert, delete, update, and so on, and sends them to the secondary nodes to perform. 

## Example:
MySQL before version 5.1

## Disadvantages:

This type of approach seems good, but it has its disadvantages. For example, any nondeterministic function (such as *NOW()**) might result in distinct writes on the follower and leader. 

*NOW() ->  Returns the current date and time

Furthermore, if a write statement is dependent on a prior write, and both of them reach the follower in the wrong order, the outcome on the follower node will be uncertain.