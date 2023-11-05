### Functional requirements:

- **Create a container**: The users should be able to create containers in order to group blobs. For example, if an application wants to store user-specific data, it should be able to store blobs for different user accounts in different containers. Additionally, a user may want to group video blobs and separate them from a group of image blobs. A single blob store user can create many containers, and each container can have many blobs, as shown in the following illustration. For the sake of simplicity, we assume that we can’t create a container inside a container.

![[Pasted image 20230915161333.png]]

- **Put data:** The blob store should allow users to upload blobs to the created containers.
- **Get data:** The system should generate a URL for the uploaded blob, so that the user can access that blob later through this URL.
- **Delete data:** The users should be able to delete a blob. If the user wants to keep the data for a specified period of time (retention time), our system should support this functionality.
- **List blobs:** The user should be able to get a list of blobs inside a specific container.
- **Delete a container:** The users should be able to delete a container and all the blobs inside it.
- **List containers:** The system should allow the users to list all the containers under a specific account.
![[Pasted image 20230915161404.png]]

### Non-functional requirements:

- **Availability:** Our system should be highly available.
- **Durability:** The data, once uploaded, shouldn’t be lost unless users explicitly delete that data.
- **Scalability:** The system should be capable of handling billions of blobs.
- **Throughput**: For transferring gigabytes of data, we should ensure a high data throughput.
- **Reliability:** Since failures are a norm in distributed systems, our design should detect and recover from failures promptly.
- **Consistency:** The system should be strongly consistent. Different users should see the same view of a blob.
![[Pasted image 20230915161512.png]]

### [[Resource Estimation of Blob Store]]

### Building blocks we will use:
We use the following building blocks in the design of our blob store system:

Building blocks for the design of a task scheduler

- **[[Rate Limiter]]**: A rate limiter is required to control the users’ interaction with the system.
- **[[Load balancer]]**: A load balancer is needed to distribute the request load onto different servers.
- **[[Database]]**: A database is used to store metadata information for the blobs.
- **[[Monitoring]]**: Monitoring is needed to inspect storage devices and the space available on them in order to add storage on time if needed.