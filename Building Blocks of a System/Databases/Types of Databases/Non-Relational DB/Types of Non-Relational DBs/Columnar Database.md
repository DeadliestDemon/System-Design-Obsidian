**Columnar databases** store data in columns instead of rows. 

They enable access to all entries in the database column quickly and efficiently.

### Examples:

Cassandra, HBase, Hypertable, and Amazon SimpleDB

### Use-Case:

Columnar databases are efficient for a large number of aggregation and data analytics queries.

It drastically reduces the disk I/O requirements and the amount of data required to load from the disk. 

For example, in applications related to financial institutions, there’s a need to sum the financial transaction over a period of time. Columnar databases make this operation quicker by just reading the column for the amount of money, ignoring other attributes of customers.

![[Pasted image 20230805023056.png]]


