IDENT Version 0 (IDENTv0)

# Introduction
The IDENT protocol is a protocol dedicated to allowing nodes to identify themselves. Unlike most other
protocols, the IDENT protocol doesn't stand for anything, except being an shortened version of idenitify.

Like other protocols that wish to integrate some sort of acknolegement functionality, IDENT relies on
the TRAMP protocol (RFC 7) for functionality along those lines.

In reality, IDENT is really only used during the AS Swallow Protocl (RFC 8). Gathering information on
a large number of nodes should not be done via IDENT; instead, it should be encapsulted within a shared
DHDB instance.

## Motivation
As mentioned above, IDENT really only exists for the purpose of the AS Swallow Protocol.

# Specification

## Base Header Format
The following is a simple datagram showing the makeup of the IDENT protocol.

~~~
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                      See HHS                      |  Padding  |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|           TRAMP ID            |      Interface Bandwidth      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                            Reserved                            ->
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
 ->                          Reserved                           |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
~~~

| Field | Bits | Description | RFC |
| :---- | :--: | :---------: | --: |
| TRAMP ID | 16 | TRAMP ID for acknolegment packet. May be all 0s for no reply. | RFC 7 |
| Interface Bandwidth | 16 | The sender's interface bandwidth for weight calculations. | N/A |
| Reserved | 64 | It's a free for all!. | N/A |

### See HHS
See RFC 9.

#### NOTE:
IDENT can over be carried over ATP.

### TRAMP ID
Because IDENT is a TRAMP compatable protocol, a TRAMP ID needs to be sent so an appropriate
acknolegement TRAMP packet can be formulated.

### Interface Bandwidth
The sender's interface bandwidth for weight calculations is needed here. The weight is used
for Network level routing in the future once a node is fully swallowed by an autonomous system.

### Reserved
It's a free for all!

# References
1. Catnet RFC 7
2. Catnet RFC 8
3. Catnet RFC 9

# Authors
Milo Banks

