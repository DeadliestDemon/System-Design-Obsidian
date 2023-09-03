### Sliding window log algorithm:

The **sliding window log algorithm** keeps track of each incoming request. When a request arrives, its arrival time is stored in a hash map, usually known as the log. The logs are sorted based on the time stamps of incoming requests. The requests are allowed depending on the size of the log and arrival time.

The main advantage of this algorithm is that it doesn’t suffer from the edge conditions, as compared to the **fixed window counter** algorithm.

![[Pasted image 20230903165329.png]]

#### Essential parameters:

The following parameters are required to implement the sliding window log algorithm:

- **Log size (*L*):** This parameter is similar to the rate limit (*R*) as it determines the number of requests allowed in a specific time frame.
- **Arrival time (*T*):** This parameter tracks incoming requests’ time stamps and determines their count.
- **Time range (*T*<sub>r</sub>​):** This parameter determines the time frame. The time stamps of the old requests are deleted if they do not fall in this range. The start time of the window is defined based on the first incoming request and expires after one minute. Similarly, when another request after the expiry time arrives the window ranges are updated accordingly.

#### Advantages:

- The algorithm doesn’t suffer from the boundary conditions of fixed windows.

#### Disadvantages:

- It consumes extra memory for storing additional information, the time stamps of incoming requests. It keeps the time stamps to provide a dynamic window, even if the request is rejected.