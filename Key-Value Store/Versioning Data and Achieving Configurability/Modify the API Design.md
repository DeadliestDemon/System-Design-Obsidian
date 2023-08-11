The API call to get a value should look like this:

```txt
get(key)
```

|   |   |
|---|---|
|**Parameter**|**Description**|
|`key`|This is the `key` against which we want to get `value`.|

We return an object or a collection of conflicting objects along with a `context`. The `context` holds encoded metadata about the object, including details such as the object’s version.

The API call to put the value into the system should look like this:

```txt
put(key, context, value)
```

|   |   |
|---|---|
|**Parameter**|**Description**|
|`key`|This is the `key` against which we have to store `value`.|
|`context`|This holds the metadata for each object.|
|`value`|This is the object that needs to be stored against the `key`.|

The function finds the node where the value should be placed on the basis of the `key` and stores the value associated with it. The `context` is returned by the system after the `get` operation. If we have a list of objects in `context` that raises a conflict, we’ll ask the client to resolve it.

To update an object in the key-value store, the client must give the `context`. We determine version information using a vector clock by supplying the `context` from a previous read operation. If the key-value store has access to several branches, it provides all objects at the leaf nodes, together with their respective version information in context, when processing a read request. Reconciling disparate versions and merging them into a single new version is considered an update.

![[Pasted image 20230808142525.png]]