In the **write-ahead log (WAL) shipping** approach, the primary node saves the query before executing it in a log file known as a write-ahead log file. It then uses these logs to copy the data onto the secondary nodes. 

## Examples:
PostgreSQL and Oracle. 

## Disadvantage:

The problem with WAL is that it only defines data at a very low level.

It’s tightly coupled with the inner structure of the database engine, which makes upgrading software on the leader and followers complicated.