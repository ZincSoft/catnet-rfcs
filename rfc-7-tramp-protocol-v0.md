TRAMP Version 0 (TRAMPv0)

# Introduction
Think of TRAMP (TRAnsport layer Message Protocol) like CNMP, but for the transport layer
instead of for the network layer. It's main purpose is to provide a way to send acknolegement
packets. Instead of the solution found in IP, for example, where most protocols each have
their own original way of communicating an acknowledgement, CP uses TRAMP.

TRAMP is desinged to be abstract enough to cover almost all use cases, while not being too
abstract it becomes a pain to implement, or has a measurable performance impact. If a protocol
decides that it doesn't want to use TRAMP, that's fine; TRAMP is just a handy tool that can be
used.

Protocols must either use 100% TRAMP or roll their own solution.

## Motivation
As mentioned above, many protocols send very simple information back to their sender. Catnet
is a huge proponent of DRY, hence TRAMP.

# Vocabulary

## Sending Packet/Protocol
The packet that triggered the TRAMP packet to be sent.

# Specification

## Base Header Format
The following is a simple datagram showing the makeup of the TRAMP protocol.

~~~
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                      See HHS                      |   Class   |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|           TRAMP ID            |           Reserved            |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
+                           Data Area                           +
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
~~~

| Field | Bits | Description | RFC |
| :---- | :--: | :---------: | --: |
| Class | 6 | The class of TRAMP packet (e.g. what it is communicating). | N/A |
| TRAMP ID | 16 | Unique ID linking TRAMP back to specific packet. | N/A|
| Reserved | 20 | Now that's a lotta oil! | N/A |
| Data Area | 64 | Free use data area if required. | N/A |

### See HHS
See RFC 9.

#### Note:
TRAMP can only be carried over DTP.

### Class
The class of TRAMP packet, also known as what the TRAMP packet is trying to communicate.
Below is a list of possibilities.

| Class | Name | Description |
| :---- | :--: | ----------: |
| 0 | Acknowlegement | The packet was received. |
| 1 | Chiari | The packet was malformed in some way. |

### TRAMP ID
The unique ID linking a TRAMP packet back to a specific packet. For example, a TRAMP
packet triggered by the receivement of an IDENT packet will take on the IDENT packet's
TRAMP ID.

### Reserved
Reserved!

### Data Area
A data area that is used however the sending protocol sees fit. Typically, the schema
of this area is defined in the RFC of the sending protocol.

# References
1. Catnet RFC 9

# Authors
Milo Banks

