A transaction in the distributed database depends on the type of query, number of sites (shards) involved, communication speed, and other factors, such as underline hardware and the type of database used. However, as an example, let’s assume a query accessing three tables, `Store`, `Product`, and `Sales`, residing on different sites.

The number of attributes in each table is given in the following figure:

Database schema consisting of three tables: Store, Product, and Sales

Let’s assume the distribution of both tables on different sites is the following:

- The `Store` table has 10,000 tuples stored at site A.
- The `Product` table has 100,000 tuples stored at site B.
- The `Sales` table has one million tuples stored at site A.

Now, assume that we need to process the following query:

```
Select Store_key from (Store JOIN Sales JOIN Product)where Region= 'East' AND Brand='Wolf';
```

The above query performs the join operations on the `Store`, `Sales`, and `Product` tables and retrieves the `Store_key` values from the table generated in the result of join operations.

Next, assume every stored tuple is 200 bits long. That’s equal to 25 Bytes. Furthermore, estimated cardinalities of certain intermediate results are as follows:

- The number of the `Wolf` brand is 10.
- The number of `East` region stores is 100,000.

Communication assumptions are the following:

- Data rate == 50M bits per second
- Access delay == 0.1 second

#### Parameters assumption

Before processing the query using different approaches, let’s define some parameters:

a= Total access delay

b= Data rate

v= Total data volume

Now, let’s compute the total communication time, �T, according to the following formula:

T= a + v/b​

Let’s try the following possible approaches to execute the query.

#### Possible approaches

- Move the `Product` table to site A and process the query at A.
    
    ![[Pasted image 20230805191404.png]]
    
    Here, 0.1 is the access delay of the table on site A, and 100,000 is the number of tuples in the `Product` table. The size of each tuple in bits is 200, and 50,000,000 is the data rate. The 200 and 50,000,000 figures are the same for all of the following calculations.
    
- Move `Store` and `Sales` to site B and process the query at B:
	
    ![[Pasted image 20230805191428.png]]
    Here, 0.2 is the access delay of the `Store` and `Product` tables. The numbers 10,000 and 1,000,000 are the number of tuples in the `Store` and `Product` tables, respectively.
    
- Restrict `Brand` at site B to `Wolf` (called projection) and move the result to site A:
    
	 ![[Pasted image 20230805191447.png]]
    
    Here, 0.1 is the access delay of the `Product` table. The number of the `Wolf` brand is 10, hence the number of tuples.
    

When we compare the three approaches, the third approach provides us the least latency (0.1 seconds). This example shows that careful query optimization is also critical in the distributed database.