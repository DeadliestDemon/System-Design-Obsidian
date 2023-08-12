
In the **logical (row-based) log replication** approach, all secondary nodes replicate the actual data changes. 

## Working:

1. For example, if a row is inserted or deleted in a table, the secondary nodes will replicate that change in that specific table. 

2. The binary log records change to database tables on the primary node at the record level. 

3. To create a replica of the primary node, the secondary node reads this data and changes its records accordingly. 

## Advantage:

Row-based replication doesn’t have the same difficulties as WAL because it doesn’t require information about data layout inside the database engine.