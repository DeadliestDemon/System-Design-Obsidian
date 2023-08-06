In **asynchronous replication**, the primary node doesn’t wait for the acknowledgment from the secondary nodes and reports success to the client after updating itself.

### Main DB pr write krke 1 successful write, baki replica kuch inteval mai update ho jaenge

## Main Advantage:

The advantage of asynchronous replication is that the primary node can continue its work even if all the secondary nodes are down.

## Disadvantage:

However, if the primary node fails, the writes that weren’t copied to the secondary nodes will be lost.

## Use-Case: Facebook Comments