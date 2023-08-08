## DNS Hierarchy
Complete infrastructure with name servers at different hierarchies.

Mainly four types of servers
1. **DNS resolver** - 
	Initiate the querying sequence and forward requests to the other DNS name servers. Typically, DNS resolvers lie within the premise of the user’s network. However, DNS resolvers can also cater to users’ DNS queries through caching techniques, as we will see shortly. These servers can also be called local or default servers.
2. **Root-level name servers -**
	These servers receive requests from local servers. Root name servers maintain name servers based on top-level domain names, such as .com, .edu, .us, and so on. For instance, when a user requests the IP address of educative.io, root-level name servers will return a list of top-level domain (TLD) servers that hold the IP addresses of the .io domain.
3. **Top-level domain (TLD) name servers -**
	These servers hold the IP addresses of authoritative name servers. The querying party will get a list of IP addresses that belong to the authoritative servers of the organization.
4. **Authoritative name servers -**
	 These are the organization’s DNS name servers that provide the IP addresses of the web or application servers.

![[Pasted image 20230807234315.png]]

