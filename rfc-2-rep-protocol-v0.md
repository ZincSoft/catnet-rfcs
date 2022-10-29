REP Version 0 (REPv0)

# Introduction
The REP (Replicating Envelope Protocol_ Protocol is Catnet's solution for one-to-many broadcasting. Ethernet multicast would not work in global scale routing, so the only solution is to route things through the Catnet protocol.

## Model of Operation
The REP protocol doesn't seem like a very good idea because of it's replicating nature, but abusing it would only be as effective as sending out packets yourself (which you can do on any network), or, if you had major stake in the network, you could slow it down. However, major stake is needed and the slow down may not be drastic.

The basis of the REP protocol is to propagate another protocol contained within it thoughout a network. Sending out a REP packet that contains an IDENT packet will return you IDENT packets from everyone on the network, assuming they are part of a group that allows that. The initial node starts the tree, and the others check the TTL value, and send out more. REP packets are only sent to nodes that probably haven't already received it, and the inner IDENT packet is only processed by those who wish to process it.

## Motivation
One of the first step of integrating a node into a network is to gather information about others in the network. Using Ethernet multicast would work fine for this purpose; listen for IDENT packets and add them to your local cache, and send IDENT packets to request, say, the MAC address of a certain computer.

However, this creates the situation where some nodes possess very incomplete information. Ideally, we want this to be kept to a minimum. Granted, for things like an ARP table on an IP network, this would be fine, but each CP node on a network must have the most up-to-date and complete information (or close to it) to security reasons.

# Specification

## REP Header
The following is a simple diagram showing the makeup of the REP protocol.

~~~
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                      See HHS                      |C| Padding |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Subscriber  Group| Max Sends | Collapse  Token |  NHL  |Padding|
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
~~~

| Field | Bits | Description | RFC |
| :---- | :--: | :---------: | --: |
| See HHS | 26 | Valid under 4 or 5. | 9 |
| C (Collapse) | 1 | Indicates if working down the replicating tree. | N/A |
| Subscriber Group | 31 | Group number that nodes subscribe to to receive the REP packet. | N/A|
| Max Sends | 6 | The maximum number of nodes this node can send the REP packet to. | N/A |
| Dispatch Token | 8 | Token used to match replication tree structures to packets. | N/A |
| New Hop Limit | 8 | The new hop limit. | N/A |

### C (Collapse)
Indicates if the tree is currently collapsing. If it is, the node should reference it's internal table mapping dispatch tokens to CPs, and then relay the REP packet back to them.

### Subscriber Group
A group number that the node reads to know if it has to read the contense of the REP packet. If the node has subscribed to that group, it'll process the inner-packet.

### Max Sends
The maximum number of nodes this node can send a replicated REP packet to.

### Dispatch Token
Token used to match replication tree structures to packets. Only used if C is set to 1.

### NHL (New Hop Limit)
The new hop limit is read instead of the TTL value of the CP protocol. Lower level processes who don't care about anything relates to REP will still cary on as normal using the CP TTL, but only code dedicated to REP packets will use this number.

# Reference

1. Catnet RFC 4
2. Catnet RFC 5
3. Catnet RFC 6

# Authors
Milo Banks

