## Resource estimation:

Let’s estimate the total number of servers, storage, and bandwidth required by a blob storage system. Because blobs can have all sorts of data, mentioning all of those types of data in our estimation may not be practical. Therefore, we’ll use YouTube as an example, which stores videos and thumbnails on the blob store. Furthermore, we’ll make the following assumptions to complete our estimations.

**Assumptions:**

- The number of daily active users who upload or watch videos is five million.
- The number of requests per second that a single blob store server can handle is 500.
- The average size of a video is 50 MB.
- The average size of a thumbnail is 20 KB.
- The number of videos uploaded per day is 250,000.
- The number of read requests by a single user per day is 20.

### Number of servers estimation:

From our assumptions, we use the number of daily active users (DAUs) and queries a blob store server can handle per second. The number of servers that we require is calculated using the formula given below:

![[Pasted image 20230915161715.png]]

### Storage estimation:

Considering the assumptions written above, we use the formula given below to compute the total storage required by YouTube in one day:

![[Pasted image 20230915161746.png]]

Putting the numbers from above into the formula gives us *12.51 TB/day*​, which is the approximate storage required by YouTube per day for keeping a single copy of the uploaded video in a single resolution.

#### Total Storage Required to Store Videos and Thumbnails Uploaded Per Day on YouTube

|   |   |   |   |
|---|---|---|---|
|No. of videos per day|Storage per video (MB)|Storage per thumbnail (KB)|Total storage per day (TB)|
|250000|50|20|12.51|

### Bandwidth estimation:

Let’s estimate the bandwidth required for uploading data to and retrieving data from the blob store.

**Incoming traffic**: To estimate the bandwidth required for incoming traffic, we consider the total data uploaded per day, which indirectly means the total storage needed per day that we calculated above. The amount of data transferred to the servers per second can be computed using the following formula:

![[Pasted image 20230915161858.png]]

#### Bandwidth Required for Uploading Videos on YouTube

|   |   |   |
|---|---|---|
|Total storage per day (TB)|Seconds in a day|Bandwidth (Gb/s)|
|12.51|86400|f1.16|

**Outgoing traffic**: Since the blob store is a read-intensive store, most of the bandwidth is required for outgoing traffic. Considering the aforementioned assumptions, we calculate the bandwidth required for outgoing traffic using the following formula:

![[Pasted image 20230915161924.png]]

#### Bandwidth Required for Downloading Videos on YouTube

|   |   |   |   |
|---|---|---|---|
|No. of active users per day|No. of requests per user|Data size (MB)|Bandwidth required (Gb/s)|
|5000000|20|50|462.96|


![[Pasted image 20230915162027.png]]
