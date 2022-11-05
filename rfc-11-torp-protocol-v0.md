Tree Oriented Replication Protocol Version 0 (TORPv0)

TODO: Separate the replication tree stuff out of here and put it into it's own protocol.
NOTE: I'm going to bed for the night :)

Distributed Hash Synchronization Protocol Version 0 (DHSPv0)

# Introduction
Distributed hash databases wouldn't be distributed hash databases if they weren't, well,
distributed. In order for this distribution to work, a synchronization protocol exists.
That protocol is DHSP.

DHSP propagates itself through a network by having the original sending node build a tree
that each DHSP packet plays its part in working up. Each DHSP packet has no knowledge of
the tree below it, and if the sending node noticed it hasn't gotten a response from a node
in the network (or rather, the nodes they were suppose to send to), the sender will reconstruct
the tree around the "dead-ndoe" and take note to remove them from the DHSP, as they have left
the network.

## Motivation
If distributed hash databases weren't, well, distributed, they wouldn't be called that.
This distribution requires a synchronization protocol in order to function. DHSP is that
protocol.

# Specification

## Base Header Format
The following is a simple diagram showing the makeup of the DHSP protocol. It is broken up
into three parts, which are YOUHAVE, IPEEK, YOUGIVE.

~~~
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                      See HHS                      |  Padding  |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                     Last Modification Time                    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
~~~

| Field | Bits | Description | RFC |
| :---- | :--: | :---------: | --: |
| Source Port | 16 | The originating port, except for in alias routing. | N/A |

### See HHS
See RFC 9.

#### Note:
DHSP can only be carried over ATP.

## When
One time that a DHSP propagation is instigated is when a node node wants to join a network.
This process is nessesary for making sure that that new node has the most up to date information.

Another way to trigger a DHSP propagation is when a machine's properties update. By properties,
it is refering to any data stored in the shared DHDB.

## Timeout
In order to avoid network congestion, a timeout should be implemented and checked before any
node passes along a DHSP packet. This is up for each node to decide, however, it is required
that the timeout be reported to the offending node via TRAMP.

If a node receives a TRAMP DHSP timeout report, and they are not the original sender, the TRAMP
packet should be relayed back to the original sender, and hopefully they act upon it.
