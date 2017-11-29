# opensips-clusterer
OpenSIPS Clusterer Experiment
## Summary:
This config utilizes the latest opensips-dev module "clusterer" to send custom messages between a cluster of OpenSIPS proxies and get some responses back.

## Objective:
The objective for this config file here is to demonstrate that OpenSIPS in a cluster can exchange user location information over Binary protocol. Previously other methods exist like shared DB, Shared Redis/memcache cluster, RabitMQ, Rest_client etc that enable proxies to share business logic between them but the problem I see is adding more software to assist SIP !? For example sending information over HTTP or MySQL or CACHE or MQ interfaces to exchange info that gets complicated for OpenSIPS to exchange using it's config file.
Now, we are no longer dependent on external components thus making OpenSIPS all-in-one solution.

## Promising Solution:
Clusterer module was capable of exchanging internal info like dialog states, user-location info etc to other nodes in the cluster already, but I wanted to use it from the script to send additional info to the nodes in a particular cluster and hence this ticket was opened: 
https://github.com/OpenSIPS/opensips/issues/1119
OpenSIPS dev team worked on the request and made it all possible. The new OpenSIPS Clusterer module now enables us to send info over to a Cluster of OpenSIPS boxes and in turn receive replies to continue with the script. 

## Example Use cases:
1 - One Proxy detects a hacker/scanner, sends broadcast to cluster to mark it for blockage. Hence saving their detection processing.
2 - A User gets registered on a node, other nodes are notified in realtime about its serving proxy.
3 - Conference calls is initiated on one node, rest of the cluster can direct that conference number's call to that node.
4 - Send Call START-STOP info to a "CDR/accounting" opensips to write a consolidated CDR journal for the whole cluster.

## Do we need it?
I don't know, everyone have their own preferences and methods. With this new module it becomes so simple to spin up an OpenSIPS service, pull the config file, and it auto-magically connects and performs with the cluster without anymore tools installed and cared for.

## Read More about OpenSIPS 2.4
https://blog.opensips.org/2017/11/01/introducing-opensips-2-4/
