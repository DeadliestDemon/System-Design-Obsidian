#non-functional_requirement 
## What is Scalability
The ability of a system to handle an increasing amount of workload without compromising performance. A search engine, for example, must accommodate increasing numbers of users, as well as the amount of data it indexes.

## Different approaches of scalability
### Vertical scalability - scaling up
#Vertical_Scaling, also known as “**scaling up**,” refers to scaling by providing additional capabilities (for example, additional CPUs or RAM) to an existing device. Vertical scaling allows us to expand our present hardware or software capacity, but we can only grow it to the limitations of our server. The dollar cost of vertical scaling is usually high because we might need exotic components to scale up.
### Horizontal scalability - scaling out
#Horizontal_Scaling, also known as “**scaling out**,” refers to increasing the number of machines in the network. We use commodity nodes for this purpose because of their attractive dollar-cost benefits. The catch here is that we need to build a system such that many nodes could collectively work as if we had a single, huge server.

## **Harvard CS75:**
- VPS → Virtual Private Server
- #Vertical_Scaling → Increasing the computational resources. More cores → more threads/ more requests fulfilled simultaneously.
- #Horizontal_Scaling → Instead of increasing the computational resources of the same device → add similar or lower parallel computing devices.
- #Load_Balancing → Traffic coming from the users can be distributed evenly to all the servers. Expose the IP address o the load balancer… whom the user will request and let it decide the server's IP address for each request. The load balancer checks the server it will allot based on some factors and then forward that server's TCP/IP packet.
- #Caching If a webpage or some static assets is requested in too much frequency → store it in a memory, which has much faster availability.
- [[Data Replication]]
- [[Data Partitioning]] ( #Sharding ) 
## **Scalability for Dummies**
1. #Cloning :
    A user should always get the same results of his request back, independent of what server he “landed on”. This leads to the **first golden rule for scalability:**  “*Every server contains exactly the same codebase and does not store any user-related data, like sessions or profile pictures, on local disc or memory.*”
	
	> Sessions must be stored in a centralized data store accessible to all your application servers. It can be an external database or an external persistent cache, like Redis.   
	
	After *“outsourcing”* your sessions and serving the same codebase from all your servers, you can now create an image file from one of these servers (AWS calls this AMI - Amazon Machine Image.) Use this AMI as a “super-clone” that all your new instances are based upon. Whenever you start a new instance/clone, just do an initial deployment of your latest code and you are ready!
2. #Database_tuning :
	After 1, our servers can now horizontally scale and you can already serve thousands of concurrent requests. But somewhere down the road, your application gets slower and slower and finally breaks down. <mark style="background: #D2B3FFA6;">The reason: your database.</mark> It’s MySQL, isn’t it? 2 paths possible at this point ->
	***Path #1 -*** stick with the same DB and keep it running. Apply #master-slave_replication (read from slaves, write to master) and upgrade your master server by adding RAM, RAM, and more RAM. Apply other database optimization techniques like, #sharding, #denormalization and #SQL_tuning. This may be a quite hectic path.
	
	**Path #2 -** De-normalize the database right from the beginning, and include no more Joins in any database query. You can stay with MySQL, and use it like a NoSQL database. But even if you successfully switch to the latest and greatest NoSQL database and let your app do the dataset joins, soon your database requests will again be slower and slower. You will need to introduce a cache.
3. #Caching :
	After 2, you now have a scalable database solution. World is looking fine. But just for you. Your users still have to suffer slow page requests when a lot of data is fetched from the database. <mark style="background: #D2B3FFA6;">The solution == cache.</mark> To remember - never do file-based caching, it makes cloning and auto-scaling of your servers just a pain instead go for in-memory caching. 2 patterns of caching your data →
	
	**Cached Database Queries —-** Whenever you do a query to your database, you store the result dataset in a cache. A hashed version of your query is the cache key. Issues → hard to delete a cached result when you cache a complex query OR When one piece of data changes you would need to delete all cached queries.
	
	**Cached Objects —-** Let your class assemble a dataset from your database and then store the complete instance of the class or the assembled dataset in the cache. When your class has finished the “assembling” of the data array, directly store the data array, or better yet the complete instance of the class, in the cache! This allows you to easily get rid of the object whenever something did change and makes the overall operation of your code faster and more logical. 
4. #Asychronisizing :
	**Path #1 —-** Doing #Cron_Jobs: Doing the time-consuming work in advance and serving the finished work with a low request time. often this paradigm is used to turn dynamic content into static content. Pages of a website, maybe built with a massive framework or CMS, are pre-rendered and locally stored as static HTML files on every change. 
	
	**Path #2 —-** Implementing #Message_Queues: A user comes to your website and starts a very computing-intensive task that would take several minutes to finish. So the front end of your website sends a job onto a job queue and immediately signals back to the user: your job is at work, please continue to browse the page. The job queue is constantly checked by a bunch of workers for new jobs. If there is a new job then the worker does the job and after some minutes sends a signal that the job was done. The front, which constantly checks for new “job is done” - signals sees that the job was done and informs the user about it.