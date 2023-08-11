
## Problem Statement:

The following problems can arise:

- **High latency**: The user-perceived latency will be high due to the physical distance from the serving data center. User-perceived latency has many components, such as transmission delays (a function of available bandwidth), propagation delays (a function of distance), queuing delays (a function of network congestion), and nodal processing delays. Therefore, data transmission over a large distance results in higher latency. Real-time applications require a latency below 200 milliseconds (ms) in general. For the Voice over Internet Protocol (VoIP), latency should not be more than 150 ms, whereas video streaming applications cannot tolerate a latency above a few seconds.

- **Data-intensive applications**: Data-intensive applications require transferring large traffic. Over a longer distance, this could be a problem due to the network path stretching through different kinds of ISPs. Because of some smaller path Message Transmission Unit (MTU) links, the throughput of applications on the network might be reduced. Similarly, different portions of the network path might have different congestion characteristics. The problem multiplies as the number of users grows because the origin servers will have to provide the data individually to each user. That is, the primary data center will need to send out a lot of redundant data when multiple clients ask for it. However, applications that use streaming services are both data-intensive and dynamic in nature.

- **Scarcity of data center resources**: Important data center resources like computational capacity and bandwidth become a limitation when the number of users of a service increases significantly. Services engaging millions of users simultaneously need scaling. Even if scaling is achieved in a single data center, it can still suffer from becoming a single point of failure when the data center goes offline due to natural calamity or connectivity issues with the Internet.

## How will we design a CDN?
We’ve divided the design of CDN into six lessons:

1. **[[Introduction to a CDN]]**: We’ll provide a thorough introduction to CDNs and identify the functional and non-functional requirements.
2. **[[Design of a CDN]]**: We’ll explain how to design the CDN. We’ll also briefly describe the API design.
3. **[[In-depth Investigation of CDN Part 1]]**: This lesson explains caching strategies and CDN architecture. Also, we’ll discuss various approaches to finding the nearest proxy server.
4. **[[In-depth Investigation of CDN Part 2]]**: We’ll discuss how to make content consistent in a CDN and the deployment of proxy servers. We’ll also cover the custom and specialized CDN in detail.
5. **[[Evaluation of CDN]]**: This lesson will provide an evaluation of our proposed design.