In **synchronous replication**, the primary node waits for acknowledgments from secondary nodes about updating the data. After receiving acknowledgment from all secondary nodes, the primary node reports success to the client.

### Har replica update hone ke baad 1 successful write


## Main Advantage:

The advantage of synchronous replication is that all the secondary nodes are completely up to date with the primary node.

## Disadvantage:

If one of the secondary nodes doesn’t acknowledge due to failure or fault in the network, the primary node would be unable to acknowledge the client until it receives the successful acknowledgment from the crashed node. 

This causes high latency in the response from the primary node to the client.

## Use-Case: Banking App