
## **Atomicity:**

### Ho to pura ho ya toh bilkul na ho

A transaction is considered an atomic unit. Therefore, either all the statements within a transaction will successfully execute, or none of them will execute. If a statement fails within a transaction, it should be aborted and rolled back.

## **Consistency:** 

### Before and after transaction, consistency maintained rahe. Ex) X acc se -50rs and Y acc mei +50rs hokr sum of both acc should be same as before

At any given time, the database should be in a consistent state, and it should remain in a consistent state after every transaction. For example, if multiple users want to view a record from the database, it should return a similar result each time.


## **Isolation:** 

### Do simultaneous payment ek dusre mai ched chad na kare

In the case of multiple transactions running concurrently, they shouldn’t be affected by each other. The final state of the database should be the same as the transactions were executed sequentially.

## **Durability:** 

### System fail ho jaye fir bhi mera paisa na chude

The system should guarantee that completed transactions will survive permanently in the database even in system failure events.