## Algorithms for rate limiting:

The task of a rate limiter is directed by highly efficient algorithms, each of which has distinct advantages and disadvantages. However, there is always a choice to choose an algorithm or combination of algorithms depending on what we need at a given time. While different algorithms are used in addition to those below, we’ll take a look at the following popular algorithms.

- [[Token bucket]]
- [[Leaking bucket]]
- [[Fixed window counter]]
- [[Sliding window log]]
- [[Sliding window counter]]

## A comparison of rate-limiting algorithms:

The two main factors that are common among all the rate-limiting algorithms are:

- **Memory:** This feature refers to the number of states an algorithm requires to maintain for a normal operation. For example, if one algorithm requires fewer variables (states) than the other, it is more space efficient.
    
- **Burst:** This refers to an increase of traffic in a unit time exceeding the defined limit.

## A Comparison of Rate-limiting Algorithms

|   |   |   |
|---|---|---|
|**Algorithm**|**Space efficient**|**Allows burst?**|
|Token bucket|Yes|Yes, it allows a burst of traffic within defined limit.|
|Leaking bucket|Yes|No|
|Fixed window counter|Yes|Yes, it allows bursts at the edge of the time window and can exceed the defined limit.|
|Sliding window log|No, maintaining the log requires extra storage.|No|
|Sliding window counter|Yes, but it requires relatively more space than other space efficient algorithms.|Smooths out the burst|
