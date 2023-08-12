
# Limitations of File storage [[file storage]]

- We can’t offer concurrent management to separate users accessing the storage files from different locations.
- We can’t grant different access rights to different users.
- How will the system scale and be available when adding thousands of entries?
- How will we search content for different users in a short time?

## Definition - A **database** is an organized collection of data that can be managed and accessed easily.

## Advantages:

- **Managing large data**: A large amount of data can be easily handled with a database, which wouldn’t be possible using other tools.
- **Retrieving accurate data (data #consistency)**: Due to different constraints in databases, we can retrieve accurate data whenever we want.
- **Easy updation**: It is quite easy to update data in databases using data manipulation language (DML).
- **Security**: Databases ensure the security of the data. A database only allows authorized users to access data.
- **Data integrity**: Databases ensure data integrity by using different constraints for data.
- **Availability**: Databases can be replicated (using data replication on different servers, which can be concurrently updated. These replicas ensure #availability.
- **Scalability**: Databases are divided (using data partitioning) to manage the load on a single node. This increases #scalability.

## [[Types of Databases]]: 
#### We’ll discuss the different types of databases, their advantages, and their disadvantages.

## [[Data Replication]]: 
#### We’ll discuss what data replication is and its different models with their pros and cons.

## [[Data Partitioning]]: 
#### We’ll discuss what data partitioning is and its different models with their pros and cons.

## [[Cost-benefit analysis]]: 
#### We’ll discuss which database sharding approach is best for different kinds of databases.

