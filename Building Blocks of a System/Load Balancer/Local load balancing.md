### Local Load Balancer
Reside within a data center. Behave like a [[reverse proxy]] and make their best effort to divide incoming requests among the pool of available servers. Incoming clients’ requests seamlessly connect to the LB that uses a virtual IP address ( **VIP** )

### Need for local load balancers
DNS plays a vital role in balancing the load, but it suffers from the following limitations:

- The small size of the DNS packet (512 Bytes) isn’t enough to include all possible IP addresses of the servers.
- Limited control over the client’s behavior. Clients may select arbitrarily from the received set of IP addresses. which may also include IP of a busy data centers.
- Clients can’t determine the closest address to establish a connection with.
- In case of failures, recovery can be slow through DNS because of the caching mechanism, especially when TTL values are longer.

To solve the above problems, another layer of load balancing in the form of local LB is required.

