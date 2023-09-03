### The leaking bucket algorithm:

The **leaking bucket algorithm** is a variant of the token bucket algorithm with slight modifications. Instead of using tokens, the leaking bucket algorithm uses a bucket to contain incoming requests and processes them at a constant outgoing rate. This algorithm uses the analogy of a water bucket leaking at a constant rate. Similarly, in this algorithm, the requests arrive at a variable rate. The algorithm process these requests at a constant rate in a first-in-first-out (FIFO) order.

Let’s look at how the leaking bucket algorithm works in the illustration below:

![[Pasted image 20230903163102.png]]

#### Essential parameters:

The leaking bucket algorithm requires the following parameters.

- **Bucket capacity (*C*):** This determines the maximum capacity of the bucket. The algorithm will discard the incoming requests when the bucket reached its maximum limit of *C*.
- **Inflow rate (*R<sub>in</sub>​):** This parameter shows the inflow rate of requests. This is a varying quantity that depends on the application and nature of requests. We use this parameter to find the initial capacity of the bucket.
- **Outflow rate (*R*<sub>out</sub>​):** This determines the number of requests processed per unit time.

#### Advantages:

- Due to a constant outflow rate (*R*<sub>out</sub>​​), it avoids the burst of requests, unlike the token bucket algorithm.
- This algorithm is also space efficient since it requires just three states: inflow rate (*R*<sub>in</sub>​), outflow rate (*R*<sub>out</sub>​​), and bucket capacity (*C*).
- Since requests are processed at a fixed rate, it is suitable for applications with a stable outflow rate.

#### Disadvantages:

- A burst of requests can fill the bucket, and if not processed in the specified time, recent requests can take a hit.
- Determining an optimal bucket size and outflow rate is a challenge.
