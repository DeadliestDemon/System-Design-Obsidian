### Reverse Proxy
A reverse proxy is a web server that _centralizes internal services and provides unified interfaces_ to the public. _Requests from clients are forwarded to a server that can fulfill them_ before the reverse proxy returns the server's response to the client. 

#### Additional benefits:
1. **Increased security** - Hide information about backend servers, blacklist IPs, limit the number of connections per client
2. **Increased scalability and flexibility** - Clients only see the reverse proxy's IP, allowing you to scale servers or change their configuration
3. **SSL termination** - Decrypt incoming requests and encrypt server responses so backend servers do not have to perform these potentially expensive operations
    - Removes the need to install X.509 certificates on each server
4. **Compression** - Compress server responses
5. **Caching** - Return the response for cached requests
6. **Static content** - Serve static content directly like HTML/CSS/JS, Photos, Videos, etc.

#### Loadbalancer vs Reverse Proxy:
- Deploying a load balancer is useful when you have multiple servers. Often, load balancers route traffic to a set of servers serving the same function.
- Reverse proxies can be useful even with just one web server or application server, opening up the benefits described in the previous section.
- Solutions such as NGINX and HAProxy can support both layer 7 reverse proxying and load balancing.