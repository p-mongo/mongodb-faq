# MongoDB FAQ

## Deployment / Configuration

### Replica Set Node Reachability

Every node in the replica set must be reachable from every other node
and from the client. This means:

- Each node and the client must be able to resolve the hostname of every
other node. [Answer](https://stackoverflow.com/questions/59405838/how-to-connect-to-mongo-database-in-docker-from-another-machine-using-ruby-code/60585071#60585071)
- [Each node and the client must be able to connect to every other node.](https://stackoverflow.com/questions/62448505/connect-on-site-mongodb-replica-instance-to-remote-nodes/62453853#62453853)
  and [here](https://stackoverflow.com/questions/62276291/cannot-connect-to-mongo-atlas-using-vpc-peering-from-gcp-cluster/62276778#62276778)

The hostnames specified in the replica set configuration take precedence
over hostnames specified in the URI/when the client is instantiated.
Meaning, the client must be able to reach all of the replica set nodes
*using the hostnames in the replica set configuration*.

#### Replica Set Nodes Behind NAT

A replica set node generally cannot be behind NAT (network address translation).
For example, it is generally not possible to have a replica set with one
node in a data center (or otherwise somewhere on the internet) and another
node on one's home network.

## Connectivity

### General Connectivity Troubleshooting

- Get the connection working with `mongo` shell before troubleshooting
  connection through your application/using a driver.

### On-Premise Server Connectivity Troubleshooting

This section applies when you are running `mongod` yourself.

- If your database and your application are not on the same machine,
  ensure `bindIp` is appropriately set.

### Atlas Connectivity Troubleshooting

- Ensure whitelist is configured with your IP.
  - Try 0.0.0.0 as the whitelist.
  - If your application is hosted on the internet, you need to whitelist the
    application's IP address. [Heroku IP addresses](https://help.heroku.com/JS13Y78I/i-need-to-add-heroku-dynos-to-our-allowlist-what-are-ip-address-ranges-in-use-at-heroku)
- Follow [general connectivity troubleshooting](#general-connectivity-troubleshooting)

### mongo Shell Does Not Wait For Deployment

Unlike drivers, `mongo` shell does not wait for deployment to become available
and will immediately exit with an error if the deployment is starting up.

[Answer](https://stackoverflow.com/questions/62806782/no-connection-to-mongo-shell)

### Node Driver & queryTxt ETIMEOUT

DNS resolution of TXT records appears to be broken on your machine. You can
use the non-SRV URI to connect to your Atlas deployment instead.

[Answer](https://stackoverflow.com/questions/62758525/getting-timeout-error-while-connecting-mongodb-atlas-with-mongoose/62759942#62759942)

### Client Connects Asynchronously

Drivers establish connections to the database in the background. A successful
client object instantiation does not mean the client successfully connected
to the deployment.

[Answer](https://stackoverflow.com/questions/62484932/mongodb-rust-client-connection-errors/62487393#62487393)

## Operation

### Transaction Support

Transactions require:

- For replica set deployments: MongoDB 4.0+
- For sharded cluster deployments: MongoDB 4.2+
- WiredTiger storage engine

Not supported:

- Standalone servers (of any version)
- Deployments using MMAPv1 storage engine

[Answer](https://stackoverflow.com/questions/62349032/using-mongo-transactions/62351098#62351098)

## Querying

### Case Insensitive Querying

[Answer](https://stackoverflow.com/questions/62576025/case-sensitive-while-querying-in-mongodb)
