## Origin
Similar to a phone book. As the number of contacts grows, we’ll have to use a phone book to keep track of all our contacts. This way, whenever we need to make a call, we’ll refer to the phone book and dial the number we need.

Computers are uniquely identified by IP addresses. We use IP addresses to visit a website hosted on a machine. Since humans cannot easily remember IP addresses to visit domain names we need a phone book-like repository that can maintain all mappings of domain names to IP addresses. 

## What is DNS
<mark style="background: #D2B3FFA6;">Internet’s naming service</mark> that maps human-friendly domain names to machine-readable IP addresses. The service of DNS is transparent to users. When a user enters a domain name in the browser, the browser has to translate the domain name to IP address by asking the DNS infrastructure. Once the desired IP address is obtained, the user’s request is forwarded to the destination web server.

> **Map of an domain name to its host IP Address. and vice versa.**

## How DNS works?
1. The client requests an _ISP DNS server_ for the mapping.
2. This DNS then requests its nearest DNS server for the same … till it gets IP. Continue till the root DNS server.
3. The nearest DNS server returns the IP recursively till reaches the client. And each server in the way, caches the value.
4. The client requests the IP returned for the webpage.

There are two ways to perform a DNS query ->
	**Iterative:** The local server requests the root, TLD, and the authoritative servers for the IP address.
	**Recursive:** The end user requests the local server. The local server further requests the root DNS name servers. The root name servers forward the requests to other name servers.
![[Pasted image 20230807234431.png]]
## Important Details →
- #Name_servers: DNS isn’t a single server. It’s a complete infrastructure with numerous servers. DNS servers that respond to users’ queries are called **name servers**.
- #Resource_records: The DNS database stores domain name to IP address mappings in the form of resource records (RR). The RR is the smallest unit of information that users request from the name servers. There are different types of RRs.
### Resource Records →
- **A record (address)** - Points a hostname to an IP address.
	-> Key( Hostname ) - to -> Value( IP address ) 
	---> Record ( A , *relay1.main.educative.io* , *104.18.2.119* )

- **NS record (name server)** - Specifies the DNS servers for your domain/subdomain.
	-> Key( Domain Name ) - to -> Value( Hostname ) 
	---> Record ( NS , *educative.io* , *dns.educative.io* )

- **CNAME (canonical)** - Provides the mapping from alias to canonical hostname. Points a name to another name or `CNAME` ( example.com to www.example.com ) or to an `A` record.
	-> Key( Hostname ) - to -> Value( Canonical name ) 
	---> Record ( CNAME , *educative.io* , *server1.primary.educative,io* )

- **MX record (mail exchange)** - Provides the mapping of mail server from alias to canonical hostname. Specifies the mail servers for accepting messages.
	-> Key( Hostname ) - to -> Value( Canonical name ) 
	---> Record ( MX , *mail.educative.io* , *mailserver1.backup.educative,io* )

- [[DNS Caching]]: DNS uses caching at different layers to reduce request latency for the user. It plays an important role in reducing the burden on DNS infrastructure because it has to cater to the queries of the entire Internet.
- [[DNS Hierarchy]]: DNS name servers are in a hierarchical form. The hierarchical structure allows DNS to be highly scalable because of its increasing size and query load.

## DNS Routing Algorithms
### Weighted round robin 
A load balancer running on a round robin algorithm won't be able to treat the two servers according to their capacity/ RAM/ CPU. In spite of the two servers' disproportionate capacities, the load balancer will still distribute requests equally. As a result, any Server can get overloaded faster and probably even go down. The Weighted Round Robin is similar to the Round Robin in the sense that the manner by which requests are assigned to the nodes is still cyclical, albeit with a twist. The node with the higher specs will be apportioned a greater number of requests.
    - Prevent traffic from going to servers under maintenance
    - The balance between varying cluster sizes
    - A/B testing
### Latency-based
Use when you have resources in multiple AWS Regions and you want to route traffic to the Region that provides the best latency. You can use latency routing to create records in a private hosted zone. Latency-based routing is based on latency measurements taken over a period of time, and the measurements reflect these changes. A request that is routed to the Oregon Region this week might be routed to the Singapore Region next week.
### Geolocation-based 
Use when you want to route traffic based on the location of your users. You can use geolocation routing to create records in a private hosted zone.

## DNS as Distributed System
**How?**
- Avoids becoming a single point of failure (SPOF).
- Achieves low query latency so users can get responses from nearby servers.
- Gets a higher degree of flexibility during maintenance and updates or upgrades. For example, if one DNS server is down or overburdened, another DNS server can respond to user queries.
### Highly Scalable
Due to its hierarchical nature, DNS is a highly scalable system. Nearly 1,000 replicated instances of 13 root-level servers are spread throughout the world strategically to handle user queries. 

The working labor is divided among TLD and root servers to handle a query and, finally, the authoritative servers that are managed by the organizations themselves to make the entire system work.

### Reliable
1. **Caching:** The caching is done in the browser, the operating system, and the local name server, and the ISP DNS resolvers also maintain a rich cache of frequently visited services.
2. **Server replication:** DNS has replicated copies of each logical server spread systematically across the globe to entertain user requests at low latency. This redundancy improves the reliability of the overall system.
3. **Protocol:** Although many clients use DNS over unreliable user datagram protocol (UDP), UDP has its advantages. UDP is much faster and, therefore, improves DNS performance. 
	Furthermore, Internet service’s reliability has improved since its inception, so UDP is usually favored over TCP. 
	A DNS resolver can resend the UDP request if it didn’t get a reply to a previous one. This request-response needs just one round trip, which provides a shorter delay as compared to TCP, which needs a three-way handshake before data exchange.

### Consistent
DNS uses various protocols to update and transfer information among replicated servers in a hierarchy. DNS compromises on strong consistency to achieve high performance because data is read frequently from DNS databases as compared to writing. 
However, DNS provides eventual consistency and updates records on replicated servers lazily. 
Consistency can suffer because of caching too. Since authoritative servers are located within the organization, it may be possible that certain resource records are updated on the authoritative servers in case of server failures at the organization. Therefore, cached records at the local and ISP servers may be outdated. To mitigate this issue, each cached record comes with an expiration time called <mark style="background: #D2B3FFA6;">time-to-live (TTL)</mark>.