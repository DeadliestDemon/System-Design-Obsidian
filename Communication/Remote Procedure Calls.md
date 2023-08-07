# RPC 
**Remote Procedural Calls**
RPC is an <mark style="background: #D2B3FFA6;">interprocess communication</mark> protocol that’s widely used in distributed systems. In the OSI model of network communication, RPC spans the transport and application layers.

Spans the <mark style="background: #ADCCFFA6;">transport layer</mark> and <mark style="background: #ADCCFFA6;">application layer</mark> in Network OSI Model.
## How does RPC work?
When we make a remote procedure call, the calling environment is paused and the procedure parameters are sent over the network to the environment where the procedure is to be executed.

When the procedure execution finishes, the results are returned to the calling environment where execution restarts as a regular procedure call.

To see how it works, let’s take an example of a client-server program. There are five main components involved in the RPC program -> 
![[Pasted image 20230807142007.png]]

## RPC Runtime
The RPC runtime is responsible for transmitting messages between client and server via the network. The responsibilities of RPC runtime also include retransmission, acknowledgment, and encryption.

## Steps of Communication
The client, the client stub, and one instance of RPC runtime are running on the client machine. The server, the server stub, and one instance of RPC runtime are running on the server machine.

During the RPC process, the following steps occur:
1. Client initiates the client stub process by giving parameters as normal. The client stub is stored in the address space of the client.
2. Client stub - converts the parameters into a standardized format, packs them into a message, and then requests the local RPC runtime to deliver the message to the server.
3. The RPC runtime at the client delivers the message to the server over the network. After sending a message to the server, it waits for the message result from the server.
4. RPC runtime at the server receives the message and passes it to the server stub.
5. The server stub unpacks the message, takes the parameters out of it, and calls the desired server routine, using a local procedure call, to do the required execution.
6. After the server routine has been executed with the given parameters, the result is returned to the server stub.
7. The server stub packs the returned result into a message and sends it to the RPC runtime at the server on the transport layer.
8. The server’s RPC runtime returns the packed result to the client’s RPC runtime over the network.
9. The client’s RPC runtime that was waiting for the result now receives the result and sends it to the client stub.
10. The client stub unpacks the result, and the execution process returns to the caller.

![[Pasted image 20230807144143.png]]
