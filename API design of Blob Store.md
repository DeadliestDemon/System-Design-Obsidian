## API design:

Let’s look into the API design of the blob store. All of the functions below can only be performed by a registered and authenticated user. For the sake of brevity, we don’t include functionalities like registration and authentication of users.

**Create container**

The `createContainer` operation creates a new container under the logged-in account from which this request is being generated.

```
createContainer(containerName)
```

|   |   |
|---|---|
|**Parameter**|**Description**|
|`containerName`|This is the name of the container. It should be unique within a storage account.|

**Upload blobs**

The client’s data is stored in the form of Bytes in the blob store. The data can be put into a container with the following code:

```
putBlob(containerPath, blobName, data)
```

|   |   |
|---|---|
|**Parameter**|**Description**|
|`containerPath`|This is the path of the container in which we upload the blob. It consists of the `accountID` and `containerID`.|
|`blobName`|This is the name of the blob. It should be unique within a container, otherwise our system will give the blob that was uploaded later a version number.|
|`data`|This is a file that the user wants to upload to the blob store.|

> **Note:** This API is just a logical way to spell out needs. We might use a multistep streaming call for actual implementation if the data size is very large.

**Download blobs**

Blobs are identified by their unique name or ID.

```
getBlob(blobPath)
```

|   |   |
|---|---|
|**Parameter**|**Description**|
|`blobPath`|This is the fully qualified path of the data or file, including its unique ID.|

**Delete blob**

The `deleteBlob` operation marks the specified blob for deletion. The actual blob is deleted during garbage collection.

```
deleteBlob(blobPath)
```

|   |   |
|---|---|
|**Parameter**|**Description**|
|`blobPath`|This is the path of the blob that the user wants to delete.|

**List blobs**

The `listBlobs` operation returns a list of blobs under the specified container or path.

```
listBlobs(containerPath)
```

|   |   |
|---|---|
|**Parameter**|**Description**|
|`containerPath`|This is the path to the container from which the user wants to get the list of blobs.|

**Delete container**

The `deleteContainer` operation marks the specified container for deletion. The container and any blobs in it are deleted later during garbage collection.

```
deleteContainer(containerPath)
```

|   |   |
|---|---|
|**Parameter**|**Description**|
|`containerPath`|This is the path to the container that the user wants to delete.|

**List containers**

The `listContainers` operation returns a list of the containers under the specified user’s blob store account.

```
listContainers(accountID)
```

|   |   |
|---|---|
|**Parameter**|**Description**|
|`accountID`|This is the ID of the user who wants to list their containers.|

> Note: The APIs used to retrieve blobs provide metadata containing size, version number, access privileges, name, and so on.