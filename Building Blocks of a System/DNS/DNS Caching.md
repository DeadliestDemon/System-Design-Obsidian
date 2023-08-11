## DNS Caching
Refers to the temporary storage of frequently requested resource records. A record is a data unit within the DNS database that shows a name-to-value binding. 
Reduces response time to the user and decreases network traffic. Using caching at different hierarchies, can reduce a lot of querying burden on the DNS infrastructure. 
Can be implemented in the browser, operating systems, local name server within the user’s network, or the ISP’s DNS resolvers.

> Even if there is no cache available to resolve a user’s query and it’s imperative to visit the DNS infrastructure, caching can still be beneficial. The local server or ISP DNS resolver can cache the IP addresses of TLD servers or authoritative servers and avoid requesting the root-level server.