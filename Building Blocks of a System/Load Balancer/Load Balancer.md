### What is Load Balancing?
The job of the load balancer is to fairly divide all clients’ requests among the pool of available servers. Load balancers perform this job to avoid overloading or crashing servers.

First point of contact within a data center after the firewall. A load balancer may not be required if a service entertains a few hundred or even a few thousand requests per second.
![[Pasted image 20230811103606.png]]
### Capabilities/ Services Offered
- #Scalability : By adding servers, the capacity of the application/service can be increased seamlessly. Load balancers make such upscaling or downscaling transparent to the end users.
- #Availability : Even if some servers go down or suffer a fault, the system still remains available. One of the jobs of the load balancers is to hide faults and failures of servers.
- #Performance : Load balancers can forward requests to servers with a lesser load so the user can get a quicker response time. This not only improves performance but also improves resource utilization.
- **Health checking**: LBs use the heartbeat protocol to monitor the health and, therefore, reliability of end-servers. Another advantage of health checking is the improved user experience.
- **TLS termination**: LBs reduce the burden on end-servers by handling TLS termination with the client.
- **Predictive analytics**: LBs can predict traffic patterns through analytics performed over traffic passing through them or using statistics of traffic obtained over time.
- **Reduced human intervention**: Because of LB automation, reduced system administration efforts are required in handling failures.
- **Service discovery**: An advantage of LBs is that the clients’ requests are forwarded to appropriate hosting servers by inquiring about the service registry.
- **Security**: LBs may also improve security by mitigating attacks like denial-of-service (DoS) at different layers of the OSI model (layers 3, 4, and 7).

Hence, provide #flexibility, #reliability, #redundancy, and #efficiency to the overall design of the system.
### Possible Locations for placing Load Balancer
- Between end users of the application and web servers/application gateway.
- Between the web servers and application servers that run the business/application logic.
- Between the application servers and database servers.

### Types of Load balancing
- **[[Global server load balancing (GSLB)]]**: Involves the distribution of traffic load across multiple geographical regions.
- **[[Local load balancing]]**: Achieved within a data center, Focuses on improving efficiency and better resource utilization of the hosting servers in a data center.

### Algorithms used for Load Balancing
1. **Round-robin scheduling**: Each request is forwarded to a server in the pool in a repeating sequential manner.
2. **Weighted round-robin**: Each node is assigned a weight. LBs forward clients’ requests according to the weight of the node. The higher the weight, the higher the number of assignments. higher capability -> higher weight.
3. **Least connections**: LBs keep a state of the number and mapping of existing connections in such a scenario. Newer arriving requests are assigned to servers with fewer existing connections.
4. **Least response time**: The server with the least response time is requested to serve the clients.
5. **IP hash**: hashing the IP address is performed to assign users’ requests to servers.
6. **URL hash**: a client requesting service from a URL is assigned to a certain cluster or set of servers.

### Stateful & Stateless LBs
If the session information isn’t kept at a lower layer (distributed cache or database), load balancers are used to keep the session information. 
#### Stateful LBs
Maintains a state of the sessions established between clients and hosting servers. Incorporates state information in its algorithm to perform load balancing.
Retain a data structure that maps incoming clients to hosting servers. Stateful LBs increase complexity and limit scalability because session information of all the clients is maintained across all the load balancers.

#### Stateless LBs
Maintains no state and is, therefore, faster and lightweight. Use [[Consistent Hashing]] to make forwarding decisions. Problem with consistent Hashing -> cannot handle infrastructure changes. Therefore, a local state may still be  required along with consistent hashing. Hence, state maintained within a load balancer for internal use.


### Types of Load Balancers
Depending on the requirements, load balancing can be performed at the network/transport and application layer of the open systems interconnection (OSI) layers.
1. **Layer 4 load balancers** ->
	- Load balancing performed on the <mark style="background: #D2B3FFA6;">basis of transport protocols</mark> like TCP and UDP.
	- Maintain connection/session with the clients and ensure that the same (TCP/UDP) communication ends up being forwarded to the same back-end server. 
	- Even though TLS termination is performed at layer 7 LBs, some layer 4 LBs also support it.

2. **Layer 7 load balancers** ->
	- Load Balancing <mark style="background: #D2B3FFA6;">based on the data of application layer protocols</mark>.
	- It’s possible to make application-aware forwarding decisions based on HTTP headers, URLs, cookies, and other application-specific data—for example, user ID. 
	- Apart from performing TLS termination, these LBs can take responsibilities like rate limiting users, HTTP routing, and header rewriting.

### Deployment of Load Balancer
Single layer of load balancer is not enough to forward requests, multiple layers of load balancers coordinate to take informed forwarding decisions. 

#### Tier-0 and tier-1 LBs
If DNS can be considered as the tier-0 load balancer, 
then equal cost multipath (ECMP) routers are the tier-1 LBs -> divides incoming traffic on the basis of IP or some other algorithm.
Tier-1 LBs will balance the load across different paths to higher tiers of load balancers. 
Hence, ECMP routers play a vital role in the horizontal scalability of the higher-tier LBs.

#### Tier-2 LBs
Layer 4 load balancers -> make sure that for any connection, all incoming packets are forwarded to the same tier-3 LBs. 
Can be considered the glue between tier-1 and tier-3 LBs. Excluding tier-2 LBs could result in erroneous forwarding decisions in case of failures or dynamic scaling of LBs.

#### Tier-3 LBs
Layer 7 load balancers -> In direct contact with the back-end servers, they perform health monitoring of servers at HTTP level. 
Enables scalability by evenly distributing requests among the set of healthy back-end servers and provides high availability by monitoring the health of servers directly.
Also reduces the burden on end-servers by handling low-level details like <mark style="background: #D2B3FFA6;">TCP-congestion control protocols</mark>, the <mark style="background: #D2B3FFA6;">discovery of Path MTU (maximum transmission unit)</mark>, the difference in application protocol between client and back-end servers, and so on.

>  Tier 1 balances the load among the load balancers themselves. Tier 2 enables a smooth transition from Tier 1 to Tier 3 in case of failures, whereas Tier 3 does the actual load balancing between back-end servers. Each tier performs other tasks to reduce the burden on end-servers.

#### Example -
1. R1​ indicates request 1 coming through one of the ECMP routers (tier-1 LBs).
2. ECMP routers forward R1​ to any of the three available tier-2 LBs using a round-robin algorithm. Tier-2 LBs take a hash of the source IP address (IPc​) and forward the packet to the next tier of LBs.
3. Tier-3, upon receiving the packet, offloads TLS and reads the HTTP(S) data. By observing the requested URL, it forwards the request to the server handling requests for `slides`.

Tier-3 LBs are preconfigured to forward requests to application servers based on the application data.

![[Pasted image 20230811185550.png]]

### Implementation of load balancers
#### Hardware Load Balancers
work as stand-alone devices and are quite expensive. Nonetheless, they have their performance benefits and are able to handle a lot of concurrent users. Configuration of hardware-based solutions is problematic because it requires additional human resources.
**Issues** -> Availability, additional hardware will be required to failover,  higher maintenance/ operational costs, compatibility issues.
#### Software Load Balancers
flexible, programmable, and cost-effective. 
Implemented on commodity hardware; Scale well as requirements grow; Availability won’t be an issue with software LBs because small additional costs are required to implement shadow load balancers on commodity hardware. Provide predictive analysis that can help prepare for future traffic patterns.
#### Cloud Load Balancers
Load Balancers as a Service (LBaaS). cloud owners provide load balancing services.
Cloud-based LBs may not necessarily replace a local on-premise load balancing facility, but they can perform global traffic management between different zones. Primary advantages of such load balancers include ease of use, management, metered cost, flexibility in terms of usage, auditing, and monitoring services to improve business decisions.

### Other Important Stuff
**[[Client-side Load Balancing]]**