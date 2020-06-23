# MongoDB FAQ

## Replica Set Node Reachability

Every node in the replica set must be reachable from every other node
and from the client. This means:

- Each node and the client must be able to resolve the hostname of every
other node.
- Each node and the client must be able to connect to every other node.

The hostnames specified in the replica set configuration take precedence
over hostnames specified in the URI/when the client is instantiated.
Meaning, the client must be able to reach all of the replica set nodes
*using the hostnames in the replica set configuration*.

### Replica Set Nodes Behind NAT

A replica set node generally cannot be behind NAT (network address translation).
For example, it is generally not possible to have a replica set with one
node in a data center (or otherwise somewhere on the internet) and another
node on one's home network.
