## Causality:

 Apart from having unique identifiers for events, we’re also interested in finding the sequence of these events. Let’s consider an example where Peter and John are two Twitter users. John posts a comment (event A), and Peter replies to John’s comment (event B). Event B is dependent on event A and can’t happen before it. The events are not concurrent here.

We can also have concurrent events—that is, two events that occur independently of each other. For example, if Peter and John comment on two different Tweets, there’s no happened-before relationship or causality between them. It’s essential to identify the dependence of one event over the other but not in the case of concurrent events.

![[Pasted image 20230810183950.png]]

Some applications need the events to have unique identifiers and carry any relevant causality information. An example of this is giving an identifier to the concurrent writes of a key into a key-value store to implement the last-write-wins strategy.

We can either use logical or physical clocks to infer causality. Some systems have additional requirements where we want event identifiers’ causality to map wall-clock time. An example of this is a financial application that complies with the European MiFID regulations. MiFID requires clocks to be within 100 microseconds of UTC to detect anomalies during high-volume/high-speed market trades.

We use time to determine the sequence of events in our life. For example, if Sam took a bath at 6 a.m. and ate breakfast at 7:00 a.m., we can determine that Sam took a bath before breakfast by the time stamps of each event. Time stamps, therefore, can be used to maintain causality.

## Use UNIX time stamps:

UNIX time stamps are granular to the millisecond and can be used to distinguish different events. We have an **ID-generating server** that can generate one ID in a single millisecond. Any request to generate a unique ID is routed to that server, which returns a time stamp and then returns a unique ID. The ability to generate an ID in milliseconds allows us to generate a thousand identifiers per second. This means we can get 

*24(hour)∗60(min/hour)∗60(sec/min)∗1000(ID/sec)=86400000*

IDs in a day.  That’s less than a billion per day.

Our system works well with generating IDs, but it poses a crucial problem. The ID-generating server is a single point of failure (SPOF), and we need to handle it. To cater to SPOF, we can add more servers. Each server generates a unique ID for every millisecond. To make the overall identifier unique across the system, we attach the server ID with the UNIX time stamp. Then, we add a load balancer to distribute the traffic more efficiently. 

### Pros

This approach is simple, scalable, and easy to implement. It also enables multiple servers to handle concurrent requests.

### Cons

For two concurrent events, the same time stamp is returned and the same ID can be assigned to them. This way, the IDs are no longer unique.

Requirements Fulfilled by Each Approach

|   |   |   |   |   |   |
|---|---|---|---|---|---|
||**Unique**|**Sca﻿lable**|**Available**|**64-bit numeric ID**|**Causality maintained**|
|**Using UUID**|✖️|✔️|✔️|✖️|✖️|
|**Using a database**|✖️|✖️|✔️|✔️|✖️|
|**Using a range handler**|✔️|✔️|✔️|✔️|✖️|
|**Using UNIX time stamps**|✖️|**weak**|✔️|✔️|**weak**|

## Twitter Snowflake:

Let’s try to use time efficiently. We can use some bits out of our targeted 64 bits for storing time and the remaining for other information. An overview of division is below:

![[Pasted image 20230810184759.png]]

The explanation of the bits division is as follows:

• **Sign bit**: A single bit is assigned as a sign bit, and its value will always be zero. It makes the overall number positive. Doing so helps to ensure that any programming environment using these identifiers interprets them as positive integers.

• **Time stamp**: 41 bits are assigned for milliseconds. The Twitter Snowflake default epoch will be used. Its value is 12888349746571288834974657, which is equivalent to November 4, 2010, 01:42:54 UTC. We can initiate our own epoch when our system will be deployed, say January 1, 2022, at 12 midnight can be the start of our epoch from zero. The maximum time to deplete this range is shown below:

![[Pasted image 20230810184855.png]]

The above calculations give us 69 years before we need a new algorithm to generate IDs. As we saw earlier, if we can generate 1,000 identifiers per second, we aren’t able to get our target of a billion identifiers per day. Though now, in the Snowflake proposal, we have ample identifiers available when we utilize worker ID and machine local sequence numbers.

• **Worker number**: The worker number is 10 bits. It gives us 210210 = 1,024 worker IDs. The server creating the unique ID for its events will attach its ID.

• **Sequence number**: The sequence number is 12 bits. For every ID generated on the server, the sequence number is incremented by one. It gives us 212212 = 4,096 unique sequence numbers. We’ll reset it to zero when it reaches 4,096. This number adds a layer to avoid duplication.

The following slides show the conversion of the time stamp to UTC.

### Pros

Twitter Snowflake uses the time stamp as the first component. Therefore, they’re time sortable. The ID generator is highly available as well.

### Cons

IDs generated in a **dead period** are a problem. The dead period is when no request for generating an ID is made to the server. These IDs will be wasted since they take up identifier space. The unique range possible will deplete earlier than expected and create gaps in our global set of user IDs.

Another weak point of this system is its reliance on time. NTP can affect the working of this system. If the clock on one of the servers drifts two seconds in the future, other servers are two seconds behind. The NTP clock recognizes it and recalibrates its clock. Now, all serves will be aligned. However, in that drifting process, IDs could have been generated for a time that hasn’t occurred yet, and now we’ll have a pair of possible nonconcurrent events with the same time stamp. Lastly, the causality of our events won’t be maintained.

![[Pasted image 20230810185013.png]]

Having accurate time still remains an issue. We can read a machine’s time-of-day clock with microsecond or even nanosecond resolution. Even with this fine-grained measurement, the risks of NTP remain. Since we can’t rely on physical clocks, let’s put logical clocks to use.

The following table gives an overview of the requirements that have been fulfilled using different design approaches.

Requirements Fulfilled by Each Approach

|   |   |   |   |   |   |
|---|---|---|---|---|---|
||**Unique**|**Sca﻿lable**|**Available**|**64-bit numeric ID**|**Causality maintained**|
|**Using UUID**|✖️|✔️|✔️|✖️|✖️|
|**Using a database**|✖️|✖️|✔️|✔️|✖️|
|**Using a range handler**|✔️|✔️|✔️|✔️|✖️|
|**Using UNIX time stamps**|✖️|**weak**|✔️|✔️|**weak**|
|**Using Twitter Snowflake**|✔️|✔️|✔️|✔️|**weak**|

## Using logical clocks:

### Lamport clocks:

In **Lamport clocks**, each node has its counter. All of the system’s nodes are equipped with a numeric counter that begins at zero when first activated. Before executing an event, the numeric counter is incremented by one. The message sent from this event to another node has the counter value. When the other node receives the message, it first updates its logical clock by taking the maximum of its clock value. Then, it takes the one sent in a message and then executes the message.

Lamport clocks provide a unique partial ordering of events using the happened-before relationship. We can also get a total ordering of events by tagging unique node/process identifiers, though such ordering isn’t unique and will change with a different assignment of node identifiers. However, we should note that Lamport clocks don’t allow us to infer causality at the global level. This means we can’t simply compare two clock values on any server to infer happened-before relationship. Vector clocks overcome this shortcoming.

#### Example:

Absolutely, let's create a simple example with three friends: Alice, Bob, and Carol. They're sending messages to each other, and we'll use Lamport clocks to understand the order of events.

Here are the messages they send along with their Lamport clock values:

1. Alice sends a message (Clock: A1)
2. Bob sends a message (Clock: B1)
3. Carol sends a message (Clock: C1)
4. Carol sends another message (Clock: C2)
5. Alice sends another message (Clock: A2)

Now, let's see how these messages are ordered based on the Lamport clocks:

- Alice's message (A1)
- Bob's message (B1)
- Carol's first message (C1)
- Carol's second message (C2)
- Alice's second message (A2)

However, let's say that we assign different identifiers to the friends, which can change the order:

1. Alice (A) sends a message (Clock: A1)
2. Carol (C) sends a message (Clock: C1)
3. Bob (B) sends a message (Clock: B1)
4. Alice (A) sends another message (Clock: A2)
5. Carol (C) sends another message (Clock: C2)

In this case, the order changes:

- Alice's first message (A1)
- Carol's first message (C1)
- Bob's message (B1)
- Alice's second message (A2)
- Carol's second message (C2)

So, as you can see, even though the messages are the same, the order can be different depending on the identifiers assigned to the friends. This demonstrates how Lamport clocks help establish an order of events, but that order can change based on how we label or identify the participants.

![User](https://chat.openai.com/_next/image?url=https%3A%2F%2Flh3.googleusercontent.com%2Fa%2FAAcHTtfWOdN-9Tt1ASez-9XFO2RUF2om7p3owmqC9bZdp8bUcxg%3Ds96-c&w=96&q=75)

generate slides that explain how TrueTime’s time master servers work with GPS and atomic clocks in multiple data centers.

ChatGPT

I'm sorry, but I cannot directly generate visual content like slides. However, I can provide you with a text-based outline that you can use to create your own slides explaining how TrueTime's time master servers work with GPS and atomic clocks in multiple data centers:

Slide 1: Introduction

- Title: TrueTime's Time Synchronization with GPS and Atomic Clocks in Data Centers
- Brief overview of the importance of accurate time synchronization in distributed systems.

Slide 2: TrueTime Overview

- Brief explanation of TrueTime as a system for providing synchronized time across distributed systems.

Slide 3: Time Master Servers

- Definition of Time Master Servers: Servers responsible for maintaining accurate time.
- Emphasis on their crucial role in maintaining consistency and ordering of events.

Slide 4: GPS Time Synchronization

- Introduction to GPS (Global Positioning System) as a reliable external time source.
- Explanation of how GPS signals are used to determine precise time.
- Diagram showing how GPS signals are received by time master servers.

Slide 5: Atomic Clocks

- Brief explanation of atomic clocks as incredibly accurate timekeeping devices.
- Mention of cesium and rubidium atomic clocks commonly used.
- Importance of atomic clocks for maintaining precise time over extended periods.

Slide 6: Time Master Server Operation

- Overview of how TrueTime's time master servers work:
    - Receive time signals from both GPS and atomic clocks.
    - Analyze and compare incoming time signals.
    - Compensate for signal delays and inconsistencies.

Slide 7: Data Center Deployment

- Explanation of why multiple data centers are needed for redundancy and reliability.
- Diagram showing multiple data centers in different locations.

Slide 8: Challenges and Solutions

- Discussion of challenges:
    - Network latency causing delays in time signal transmission.
    - Signal disruptions due to atmospheric conditions.
- Explanation of how TrueTime mitigates these challenges:
    - Compensating for network latency.
    - Implementing error correction algorithms.

Slide 9: Achieving Synchronized Time

- Description of the ultimate goal: Achieving synchronized time across data centers.
- Emphasis on how synchronized time enables accurate event ordering and data consistency.

Slide 10: Benefits of TrueTime

- List of benefits offered by TrueTime's synchronization approach:
    - Improved data integrity.
    - Better fault tolerance.
    - Enhanced performance in distributed applications.

Slide 11: Conclusion

- Recap of key points discussed.
- Affirmation of the importance of accurate time synchronization in modern distributed systems.

Remember to use visuals, diagrams, and concise bullet points to complement each slide's content. You can expand on each point in your presentation notes to provide more detailed explanations while using the outline as a guide for creating your slides.

![User](https://chat.openai.com/_next/image?url=https%3A%2F%2Flh3.googleusercontent.com%2Fa%2FAAcHTtfWOdN-9Tt1ASez-9XFO2RUF2om7p3owmqC9bZdp8bUcxg%3Ds96-c&w=96&q=75)

what is the dollar cost?

ChatGPT

I'm sorry, but I don't have real-time access to external data or current events, including currency exchange rates or prices. Therefore, I cannot provide you with the current dollar cost of any particular item or service. If you have a specific product or service in mind, I recommend checking with relevant sources such as online retailers, financial websites, or other appropriate platforms to get the most up-to-date pricing information. Is there anything else I can assist you with?

![User](https://chat.openai.com/_next/image?url=https%3A%2F%2Flh3.googleusercontent.com%2Fa%2FAAcHTtfWOdN-9Tt1ASez-9XFO2RUF2om7p3owmqC9bZdp8bUcxg%3Ds96-c&w=96&q=75)

Explain the sentence: The dollar cost of a Spanner-like system is also high due to its elaborate infrastructure needs and monitoring.

ChatGPT

Certainly! That sentence is explaining that a system similar to Google's "Spanner," which is a highly advanced and sophisticated database system, comes with a significant financial expense. This high cost is attributed to the complex and extensive requirements for its infrastructure (the underlying technology and hardware) and the continuous monitoring that's necessary to ensure the system runs smoothly.

In simpler terms, creating and maintaining a system like Spanner is expensive because it requires a lot of specialized equipment, technology setup, and ongoing attention to make sure everything works correctly. The costs come from both setting up the system initially (infrastructure) and keeping an eye on it over time (monitoring).

![User](https://chat.openai.com/_next/image?url=https%3A%2F%2Flh3.googleusercontent.com%2Fa%2FAAcHTtfWOdN-9Tt1ASez-9XFO2RUF2om7p3owmqC9bZdp8bUcxg%3Ds96-c&w=96&q=75)

what is the maximum theoretical frequency of issuing of sequence values

ChatGPT

The maximum theoretical frequency of issuing sequence values depends on various factors, including the implementation of the sequence generator and the underlying hardware. Generally, it's influenced by the speed at which the system can generate unique values and the limitations of the hardware and software components involved.

For example, if we're talking about a simple software-based sequence generator in a modern computer system, the theoretical frequency could be quite high, potentially reaching millions or even billions of sequence values per second. However, the actual achievable frequency may be limited by factors such as the processing power of the computer, the efficiency of the sequence generation algorithm, and any potential contention or synchronization mechanisms used to ensure uniqueness.

On the other hand, if we're considering hardware-based generators, such as those in specialized equipment or dedicated hardware components, the frequency could be even higher due to the optimized design and dedicated resources.

It's important to note that while the theoretical frequency can be high, practical limitations, such as the need to ensure uniqueness, concurrency control, and system stability, often result in systems operating at lower frequencies than their theoretical maximums. Additionally, factors like network latency, storage access times, and synchronization overhead can also affect the actual achievable frequency.

In summary, the maximum theoretical frequency of issuing sequence values is influenced by various technical and design factors, and it can vary widely based on the specific implementation and hardware resources available.

![User](https://chat.openai.com/_next/image?url=https%3A%2F%2Flh3.googleusercontent.com%2Fa%2FAAcHTtfWOdN-9Tt1ASez-9XFO2RUF2om7p3owmqC9bZdp8bUcxg%3Ds96-c&w=96&q=75)

meaning of maximum theoretical frequency of issuing of sequence values

ChatGPT

The "maximum theoretical frequency of issuing sequence values" refers to the highest possible rate at which a system can generate and assign unique sequence values. In computing and databases, a sequence is a set of unique values that are generated in order, typically used for tasks like creating unique identifiers for database records.

This concept is about understanding how fast a system can churn out these unique values. It's important to note that this theoretical frequency might not always be achievable in real-world scenarios due to practical constraints, such as hardware limitations, processing speed, and the need to ensure the uniqueness of the generated values.

In simpler terms, it's like asking: "How quickly can a system generate a series of different numbers one after the other without repeating any of them?" It's a measure of how efficiently and rapidly a system can create these unique identifiers.

### Vector clocks:

Vector clocks maintain causal history—that is, all information about the happened-before relationships of events. So, we must choose an efficient data structure to capture the causal history of each event.

Consider the design shown below. We’ll generate our ID by concatenating relevant information, just like the Twitter snowflake, with the following division:

- **Sign bit**: A single bit is assigned as a sign bit, and its value will always be zero.
    
- **Vector clock**: This is 53 bits and the counters of each node.
    
- **Worker number**: This is 10 bits. It gives us 2^{10} = 1,024 worker IDs.

![[Pasted image 20230810192105.png]]

![[Pasted image 20230810192110.png]]

Our approach with vector clocks works. However, in order to completely capture causality, a vector clock must be at least �n nodes in size. As a result, when the total number of participating nodes is enormous, vector clocks require a significant amount of storage. Some systems nowadays, such as web applications, treat every browser as a client of the system. Such information increases the ID length significantly, making it difficult to handle, store, use, and scale.

Requirements Fulfilled by Each Approach

|   |   |   |   |   |   |
|---|---|---|---|---|---|
||**Unique**|**Sca﻿lable**|**Available**|**64-bit numeric ID**|**Causality maintained**|
|**Using UUID**|✖️|✔️|✔️|✖️|✖️|
|**Using a database**|✖️|✖️|✔️|✔️|✖️|
|**Using a range handler**|✔️|✔️|✔️|✔️|✖️|
|**Using UNIX time stamps**|✖️|**weak**|✔️|✔️|**weak**|
|**Using Twitter Snowflake**|✔️|✔️|✔️|✔️|**weak**|
|**Using vector clocks**|✔️|**weak**|✔️|**can exceed**|✔️|

## TrueTime API:

Google’s TrueTime API in Spanner is an interesting option. Instead of a particular time stamp, it reports an interval of time. When asking for the current time, we get back two values: the earliest and latest ones. These are the earliest possible and latest possible time stamps.

Based on its uncertainty calculations, the clock knows that the actual current time is somewhere within that interval. The width of the interval depends, among other things, on how long it has been since the local quartz clock was last synchronized with a more accurate clock source.

Google deploys a GPS receiver or atomic clock in each data center, and clocks are synchronized within about 7 ms. This allows Spanner to keep the clock uncertainty to a minimum. The uncertainty of the interval is represented as epsilon.

Spanner guarantees that two confidence intervals don’t overlap (that is, A *earliest*​ < A *latest​* < B *earliest*​ < B *latest*​), then B definitely happened after A.

We generate our unique ID using TrueTime intervals. Let’s say the earliest interval is T*e*, the latest is T*l*​, and the uncertainty is `ε`. We use  T*e*​ in milliseconds as a time stamp in our unique ID.

- **Time stamp**: The time stamp is 41 bits. We use  T*e*​ as a time stamp.
    
- **Uncertainty**: The uncertainty is four bits. Since the maximum uncertainty is claimed to be 6–10 ms, we’ll use four bits for storing it.
    
- **Worker number**: This is 10 bits. It gives us 2^10 = 1,024 worker IDs.
    
- **Sequence number**: This is eight bits. For every ID generated on the server, the sequence number is incremented by one. It gives us 2^8 = 256 combinations. We’ll reset it to zero when it reaches 256.

### Pros

TrueTime satisfies all the requirements. We’re able to generate a globally unique 64-bit identifier. The causality of events is maintained. The approach is scalable and highly available.

### Cons

If two intervals overlap, then we’re unsure in what order A and B occurred. It’s possible that they’re concurrent events, but a 100% guarantee can’t be given. Additionally, Spanner is expensive because it ensures high database consistency. The dollar cost of a Spanner-like system is also high due to its elaborate infrastructure needs and monitoring.

The updated table provides the comparison between the different system designs for generating a unique ID.

Requirements Fulfilled by Each Approach

|   |   |   |   |   |   |
|---|---|---|---|---|---|
||**Unique**|**Sca﻿lable**|**Available**|**64-bit numeric ID**|**Causality maintained**|
|**Using UUID**|✖️|✔️|✔️|✖️|✖️|
|**Using a database**|✖️|✖️|✔️|✔️|✖️|
|**Using a range handler**|✔️|✔️|✔️|✔️|✖️|
|**Using UNIX time stamps**|✖️|**weak**|✔️|✔️|**weak**|
|**Using Twitter Snowflake**|✔️|✔️|✔️|✔️|**weak**|
|**Using vector clocks**|✔️|**weak**|✔️|**can exceed**|✔️|
|**Using TrueTime**|✔️|✔️|✔️|✔️|✔️|

## Summary

- We want to avoid duplicate identifiers. Consider what will happen if duplicate payment or purchase orders are generated.
    
- UUIDs provide probabilistic guarantees about the keys’ non-collision. Deterministically getting non-collision guarantees might need consensus among different distributed entities or stores and read from the replicated store.
    
- As key length becomes large, it often causes slower tuple updates in a database. Therefore, identifiers should be big enough but not too big.
    
- Often, it’s desirable that no one is able to guess the next ID. Otherwise, undesirable data leaks can happen, and the organization’s competitors may learn how many orders were processed in a day by simply looking at order IDs. Adding a few random numbers to the bits of the identifier make it hard to guess, although this comes at a performance cost.
    
- We can just use simple counters for generating unique IDs if we don’t want to relate ID to time. Fetching time stamps is slower than simple counters, though this requires that we store generated IDs persistently. The counter needs to be stored in the database. Storage comes with its own issues. These include multiple concurrent writes becoming overwhelming for the database and the database being the single point of failure.
    
- For some distributed databases, such as Spanner, it can hurt to generate monotonically increasing or decreasing IDs. Google reports the following: “In fact, using monotonically increasing (or decreasing) values as row keys does not follow best practices in Spanner because it creates hotspots in the database, leading to a reduction in performance.”

![[Pasted image 20230810192652.png]]