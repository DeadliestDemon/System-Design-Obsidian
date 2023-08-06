### Functional requirements

The functional requirements are as follows:

- **Configurable service**: Some applications might have a tendency to trade strong consistency for higher availability. We need to provide a configurable service so that different applications could use a range of consistency models. We need tight control over the trade-offs between availability, consistency, cost-effectiveness, and performance.

- **Ability to always write**: The applications should always have the ability to write into the key-value storage. If the user wants strong consistency, this requirement might not always be fulfilled due to the implications of the CAP theorem.

- **Hardware heterogeneity**: The system shouldn’t have distinguished nodes. Each node should be functionally able to do any task. Though servers can be heterogeneous, newer hardware might be more capable than older ones.


### Non-functional requirements

The non-functional requirements are as follows:

- **Scalable**: Key-value stores should run on tens of thousands of servers distributed across the globe. Incremental scalability is highly desirable. We should add or remove the servers as needed with minimal to no disruption to the service availability. Moreover, our system should be able to handle an enormous number of users of the key-value store.

- **Available**: We need to provide continuous service, so availability is very important. This property is configurable. So, if the user wants strong consistency, we’ll have less availability and vice versa.

- **Fault tolerance**: The key-value store should operate uninterrupted despite failures in servers or their components.

## Assumptions

We’ll assume the following to keep our design simple:

- The data centers hosting the service are trusted (non-hostile).

- All the required authentication and authorization are already completed.

- User requests and responses are relayed over HTTPS.

## API design

Key-value stores, like ordinary hash tables, provide two primary functions, which are `get` and `put`.

Let’s look at the API design.

**The `get` function**

The API call to get a value should look like this:

```txt
get(key)
```

We return the associated value on the basis of the parameter `key`. When data is replicated, it locates the object replica associated with a specific key that’s hidden from the end user. It’s done by the system if the store is configured with a weaker data consistency model. For example, in eventual consistency, there might be more than one value returned against a key.

|   |   |
|---|---|
|**Parameter**|**Description**|
|`key`|It’s the `key` against which we want to get `value`.|

**The `put` function**

The API call to put the value into the system should look like this:

```txt
put(key, value)
```

It stores the `value` associated with the `key`. The system automatically determines where data should be placed. Additionally, the system often keeps metadata about the stored object. Such metadata can include the version of the object.

|   |   |
|---|---|
|**Parameter**|**Description**|
|`key`|It's the `key` against which we have to store `value`.|
|`value`|It's the object to be stored against the `key`.|

The key is often a primary key in a key-value store, while the value can be any arbitrary binary data.

![[Pasted image 20230806155817.png]]

![[Pasted image 20230806155835.png]]