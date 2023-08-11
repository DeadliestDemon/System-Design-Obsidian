
### Retrieve (proxy server calls to origin server):

If the proxy servers request content, the `GET` method retrieves the content through the `/retrieveContent` API below:

```
retrieveContent(porxyserver_id, content_type, content_version, description)
```

Let’s see the details of the parameters:

Details of Parameters

|   |   |
|---|---|
|**Parameter**|**Description**|
|`porxyserver_id`|This is a unique ID of the requesting proxy server.|
|`content_type`|This data structure will contain information about the requested content. Specifically, it will contain the category (audio, video, document, script, and so on), the type of clients it’s requested for, and the requested quality (if any).|
|`content_version`|This represents the version number of the content. For the `/retrieveContent` API, the `content_version` will contain the current version of the content residing in the proxy server. The `content_version` will be `NULL` if no previous version is available at the proxy server.|
|`description`|This specifies the content detail—for example, the video's extension, resolution detail, and so on if the `content_type` is video.|

### Deliver (origin server calls to proxy servers):

The origin servers use this API to deliver the specified content, the updated version, to the proxy servers through the distribution system. We call this the `/deliverContent` API:

```
deliverContent(origin_id, server_list, content_type, content_version, description)
```

Details of new parameters:

|   |   |
|---|---|
|**Parameter**|**Description**|
|`origin_id`|This recognizes each origin server uniquely.|
|`server_list`|This identifies the list of servers the content will be pushed to by the distribution system.|
|`content_version`|This represents the updated version of the content at the origin server. The proxy server receiving the content will discard the previous version.|

### Request (clients calls to proxy servers)

The users use this API to request the content from the proxy servers. We call this the `/requestContent` API:

```
requestContent(user_id, content_type, description)
```

Details of Parameter

|   |   |
|---|---|
|**Parameter**|**Description**|
|`user_id`|This is the unique ID of the user who requested the content.|

The specified proxy server returns the particular content to the requested users in response to the above API.

### Search (proxy server calls to peer proxy servers)

Although the content is first searched locally at the proxy server, the proxy servers can also probe requested content in the peer proxy servers in the same PoP through the `/searchContent` API. This could flood the query to all proxy servers in a PoP. Alternatively, we can use a data store in the PoP to query the content, though proxy servers will need to maintain what content is available on which proxy server.

The `/searchContent` API is shown below:

```
searchContent(porxyserver_id, content_type, description)
```

### Update (proxy server calls to peer proxy servers)

The proxy servers use the `/updateContent` API to update the specified content in the peer proxy servers in the PoP. It does so when specified isolated scripts run on the CDN to provide image resizing, video resolution conversion, security, and many more services. This type of scripting is known as serverless scripting.

The `/updateContent` API is shown below:

```
updateContent(porxyserver_id, content_type, description)
```

Details of Parameter

|   |   |
|---|---|
|**Parameter**|**Description**|
|`porxyserver_id`|This recognizes the proxy server uniquely in the PoP to update the content.|

![[Pasted image 20230809144837.png]]

![[Pasted image 20230809145225.png]]