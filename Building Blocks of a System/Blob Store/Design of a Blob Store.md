## High-level design:

Let’s identify and connect the components of a blob store system. At a high level, the components are clients, front-end servers, and storage disks.

![[Pasted image 20230915162801.png]]

The client’s requests are received at the front-end servers that process the request. The front-end servers store the client’s blob onto the storage disks attached to them.

### [[API design of Blob Store]]

## Detailed design:

We start this section by identifying the key components that we need to complete our blob store design. Then, we look at how these components connect to fulfill our functional requirements.

### Components:

Here is a list of components that we use in the blob store design:

- **Client:** This is a user or program that performs any of the API functions that are specified.
- **Rate limiter:** A rate limiter limits the number of requests based on the user’s subscription or limits the number of requests from the same IP address at the same time. It doesn’t allow users to exceed the predefined limit.
- **Load balancer:** A load balancer distributes incoming network traffic among a group of servers. It’s also used to reroute requests to different regions depending on the location of the user, different data centers within the same region, or different servers within the same data center. **DNS** load balancing can be used to reroute the requests among different regions based on the location of the user.
- **Front-end servers:** Front-end servers forward the users’ requests for adding or deleting data to the appropriate storage servers.
- **Data nodes:** Data nodes hold the actual blob data. It’s also possible that they contain a part of the blob’s data. Blobs are split into small, fixed-size pieces called **chunks**. A data node can accommodate all of the chunks of a blob or at least some of them.
- **Master node:** A master node is the core component that manages all data nodes. It stores information about storage paths and the access privileges of blobs. There are two types of access privileges: private and public. A _private_ access privilege means that the blob is only accessible by the account containing that blob. A _public_ access privilege means that anyone can access that blob.

![[Pasted image 20230915163057.png]]

- **Metadata storage:** Metadata storage is a distributed database that’s used by the master node to store all the metadata. Metadata consists of account metadata, container metadata, and blob metadata.
    - **_Account metadata_** contains the account information for each user and the containers held by each account.
    - **_Container metadata_** consists of the list of the blobs in each container.
    - **_Blob metadata_** consists of where each blob is stored. The blob metadata is discussed in detail in the next lesson.
- **Monitoring service:** A monitoring service monitors the data nodes and the master node. It alerts the administrator in case of disk failures that require human intervention. It also gets information about the total available space left on the disks to alert administrators to add more disks.
- **Administrator:** An administrator is responsible for handling notifications from the monitoring services and conducting routine checkups of the overall service to ensure reliability.

### Workflow:

We describe the workflow based on the basic operations we can perform on a blob store. We assume that the user has successfully logged in and a container has already been created. A unique ID is assigned to each user and container. The user performs the following operations in a specific container.

**Write a blob**

1. The client generates the upload blob request. If the client’s request successfully passes through the rate limiter, the load balancer forwards the client’s request to one of the front-end servers. The front-end server then requests the master node for the data nodes it should contact to store the blob.
2. The master node assigns the blob a unique ID using a unique ID generator system ([[Sequencer]]). It then splits the large-size blob into smaller, fixed-size chunks and assigns each chunk a data node where that chunk is eventually stored. The master node determines the amount of storage space that’s available on the data nodes using a **free-space management system**.
3. After determining the mapping of chunks to data nodes, the front-end servers write the chunks to the assigned data nodes.
4. We replicate each chunk for redundancy purposes. All choices regarding chunk replication are made at the master node. Hence, the master node also allocates the storage and data nodes for storing replicas.
5. The master node stores the blob metadata in the metadata storage. We discuss the blob’s metadata schema in detail in the next lesson.
6. After writing the blob, a fully qualified path of the blob is returned to the client. The path consists of the user ID, container ID where the user has added the blob, the blob ID, and the access level of the blob.

**Reading a blob**

1. When a read request for a blob reaches the front-end server, it asks the master node for that blob’s metadata.
2. The master node first checks whether that blob is private or public, based on the path of the blob and whether we’re authorized to transfer that blob or not.
3. After authorizing the blob, the master node looks for the chunks for that blob in the metadata and looks at their mappings to data nodes. The master node returns the chunks and their mappings (data nodes) to the client.
4. The client then reads the chunk data from the data nodes.

![[Pasted image 20230915163449.png]]

**Deleting a blob** Upon receiving a delete blob request, the master node marks that blob as deleted in the metadata, and frees up the space later using a garbage collector. We learn more about garbage collectors in the next lesson.