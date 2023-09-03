### Sliding window counter algorithm:

Sliding Window Counter is a hybrid solution that combines Fixed Window Counter and Sliding Window Log.

Fixed Window Counter is not a robust rate-limiting solution because it can’t correctly handle burst requests. But It is a memory-efficient solution. Sliding Window Log is a powerful solution since it enforces a hard limit on every time window. However, It isn’t memory efficient solution.

Sliding Window Counter tries to achieve efficient memory usage and a robust rate-limiting solution. Let’s first rewind the fixed window counter solution.

![[Pasted image 20230903170627.png]]

The fixed window counter algorithm is memory efficient because it only needs to count requests. However, we want to use a sliding window to make the rate limiter more robust.

![[Pasted image 20230903170644.png]]

However, the problem is that since we only know the counter in each fixed time window, the sliding window doesn’t know how many requests are in it.

![[Pasted image 20230903170700.png]]

![[Pasted image 20230903170723.png]]


![[Pasted image 20230903170759.png]]

Here, *R*<sub>p</sub>​ is the number of requests in the previous window, which is 8. *R*<sub>c</sub>​ is the number of requests in the current window, which is 5. The *time frame* is 60 seconds in our case, and *overlap time* is 31.8 seconds.

Rate = 8 * ( (60 - 31.8)/60 ) + 5

#### Essential parameters:

This algorithm is relatively more complex than the other algorithms described above. It requires the following parameters:

- **Rate limit (*R*)** It determines the number of maximum requests allowed per window.
- **Size of the window (*W*):** This parameter represents the size of a time window that can be a minute, an hour, or any time slice.
- **The number of requests in the previous window (*R*<sub>p</sub>​):** It determines the total number of requests that have been received in the previous time window.
- **The number of requests in the current window (*R*<sub>c</sub>​​):** It represents the number of requests received in the current window.
- **Overlap time (*O*<sub>t</sub>​):** This parameter shows the overlapping time of the rolling window with the current window.

#### Advantages:

- The algorithm is also space efficient due to limited states: the number of requests in the current window, the number of requests in the previous window, the overlapping percentage, and so on.
- It smooths out the bursts of requests and processes them with an approximate average rate based on the previous window.

#### Disadvantages:

- This algorithm assumes that the number of requests in the previous window is evenly distributed, which may not always be possible.