Distributed Hash Synchronization Protocol Version 0 (DHSPv0)

# Introduction
Distributed hash databases wouldn't be distributed hash databases if they weren't, well,
distributed. In order for this distribution to work, a synchronization protocol exists.
That protocol is DHSP.

DHSP propagates itself through a network using the TORP protocol. As such, many DHSP
requests shouldn't be sent in a short period of time, or the risk of timing out other
nodes on the network is run. This propagation only exists in the BOB_HAS amd MARY_SUMMARYS stages
of the DHSP protocol.

## Motivation
If distributed hash databases weren't, well, distributed, they wouldn't be called that.
This distribution requires a synchronization protocol in order to function. DHSP is that
protocol.

# Specification
DHSP is broken up into four parts, which are BOB_HAS, MARY_SUMMARYS, BOB_WANTS, MARY_GIVES. The stages are
named after Bob and Mary; Bob is the computer who originally started with DHSP ordeal, and
Mary is the computer sending him information. Below information and the specification for each
part, or stage, as they will be refered to from now on.

## BOB_HAS (Bob Has)
The first stage of the DHSP protocol. Using TORP, DHSP propages a request to every node
in the network that is known. Each node in the tree will send back an MARY_SUMMARYS.

### Header Specification
~~~
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                      See HHS                      |  Pdn  |Stg|
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                         Last Modified                         |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                         Database Hash                         |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|      Number of Entries        |0 1 1 0 1 1 0 1 0 1 1 0 0 0 1 0|
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
~~~

| Field | Bits | Description | RFC |
| :---- | :--: | :---------: | --: |
| Pdn | 3 | Just a shorter way of saying padding. | N/A |
| Stg | 2 | Stage of the DHSP exchange. Must be 0. | N/A |
| Last Modified | 32 | Unsigned 32-bit integer since the UNIX epoch since DHDB mutation. | N/A |
| Database Hash | 32 | 32-bit MurmurHash3 hash of the DHDB. | N/A|
| Number of Entries | 16 | Number of entries in the DHDB. | N/A |
| "Random" Number | 16 | Serves no functional purpose. | N/A |

#### See HHS
See RFC 9.

##### Note:
DHSP can over be carried over DTP.

#### Stg (Stage)
The stage of the DHSP exchange. This must be 0.

#### Last Modified
The time since the DHDB was last mutated. This should take the form of a 32-bit integer representing
the number of seconds that have passed since the last UNIX epoch.

This is part of the process used to see if the node who received this request has a more up-to-date
version of the DHDB.

#### Database Hash
A hash of the DHDB. It is a 32-bit version of the MurmurHash3 algorithm. To calculate the hash,
the database should be collected into contiguous memory. For more information, see RFC 12.

#### Number of Entries
The number of entries in the DHDB. This is part of the process used to see if the node who received
this request has a more up-to-date version of the DHDB.

#### "Random" Number
This number is not random! It is the binary representation of the ASCII string "mb", which are my
initials. I SHALL BECOME IMORTAL!!!

In all seriousness, if a Catnet implementation doesn't copy these numbers in here, it's not
standards conforming.

### Functionality
When a node receives this packet and has already executed the protocol required by TORP, it
should look at the Database hash, Number of Entries, and the Last Modified fields. If any
two or more of these show a difference between the receiving node and the information in the
packet, it will be considered a notable event and propagated back by TORP via the MARY_SUMMARYS stage.

However, if the database hash is different, there does not have to be a second change along with
it.

To calculate the hash, the database should be collected into contiguous memory. For more
information, see RFC 12.

## MARY_SUMMARYS (Mary Recaps)
After BOB_HAS, the request requests each node and sends back a respond, known as the MARY_SUMMARYS
stage. Of course, this propagated back through the TORP tree. If they don't have a viable
update, then an acknowledgement is sent via TRAMP. Mary is summarizing her DHDB.

If they do, a custom notice is sent back. This isn't sent via TRAMP because of its extreemly
limited data area size. This then triggers BOB_WANTS.

### Header Specification
~~~
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                      See HHS                      |H|M|L|P|Stg|
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                         Last Modified                         |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                         Database Hash                         |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|      Number of Entries        |0 1 1 0 1 1 0 1 0 1 1 0 0 0 1 0|
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
~~~

| Field | Bits | Description | RFC |
| :---- | :--: | :---------: | --: |
| H | 1 | Differing hash. | N/A |
| M | 1 | Differing mutated date. | N/A |
| L | 1 | Differing database length. | N/A |
| P | 1 | Just a shorter way of saying padding. | N/A |
| Stg | 2 | Stage of the DHSP exchange. Must be 1. | N/A |
| Pdg | 3 | Just a shorter way of saying padding. | N/A |
| Last Modified | 32 | Unsigned 32-bit integer since the UNIX epoch since DHDB mutation. | N/A |
| Database Hash | 32 | 32-bit MurmurHash3 hash of the DHDB. | N/A|
| Number of Entries | 16 | Number of entries in the DHDB. | N/A |
| "Random" Number | 16 | Serves no functional purpose. | N/A |

#### See HHS
See RFC 9.

##### Note:
DHSP can over be carried over DTP.

#### H (Differing Hash)
This is set if there is a difference between the hash reported by the receiving node and the hash
reported in the DHSP packet.

#### M (Differing Mutation Date)
This is set if there is a difference between the mutation date reported by the receiving node and
the mutation date reported in the DHSP packet.

#### L (Differing Database Length)
This is set if there is a difference between the length reported by the receiving node and the length
reported in the DHSP packet.

#### Stg (Stage)
The stage of the DHSP exchange. This must be 1.

#### Last Modified
The time since the DHDB was last mutated. This should take the form of a 32-bit integer representing
the number of seconds that have passed since the last UNIX epoch.

This is part of the process used to see if the node who received this request has a more up-to-date
version of the DHDB.

#### Database Hash
A hash of the DHDB. It is a 32-bit version of the MurmurHash3 algorithm. To calculate the hash,
the database should be collected into contiguous memory. For more information, see RFC 12.

#### Number of Entries
The number of entries in the DHDB. This is part of the process used to see if the node who received
this request has a more up-to-date version of the DHDB.

#### "Random" Number
This number is not random! It is the binary representation of the ASCII string "mb", which are my
initials. I SHALL BECOME IMORTAL!!!

In all seriousness, if a Catnet implementation doesn't copy these numbers in here, it's not
standards conforming.

### Functionality
When the original sending computer receives this, it should collect them all into a list. After (or
during for all we care, if you trust your programming abilities) all are collected and TORP is satisfied,
duplicates should be deleted. Which duplicate doesn't matter, so there is room for optimization here.

After, the node that started this whole shebang should send a DHSP packet in the BOB_WANTS stage to all
nodes in that list. This should not be wrapped in TORP, as it is a conventional unicast solution.

## BOB_WANTS (Bob Wants)
The triggering node should gather one of each unique update by looking through all the
MARY_SUMMARYS requests that were received. After a list of unique MARY_SUMMARYS candidates (a node with a
unique update to the DHDB), each is sent a DHSP packet. Bob wants these!

Unlike the two before stages, this one does not use TORP. It is a conventional unicast solution.

### Header Specification
~~~
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                      See HHS                      |  Pdn  |Stg|
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
~~~

| Field | Bits | Description | RFC |
| :---- | :--: | :---------: | --: |
| Pdn | 3 | Just a shorter way of saying padding. | N/A |
| Stg | 2 | Stage of the DHSP exchange. Must be 0. | N/A |

#### See HHS
See RFC 9.

##### Note:
DHSP can over be carried over DTP.

#### Stg (Stage)
The stage of the DHSP exchange. This must be 2.

### Behaviour
Once Mary recieves this packet, she has to go ahead to send her DHDB.

## MARY_GIVES (Mary Gives)
After Mary receives a DHSP packet in the BOB_WANTS stage, Mary sends it right over! No use
in delaying any longer, now scram!

Again, this does not use TORP, and is a conventional unicast solution.

### Header Specification
~~~
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                      See HHS                      |  Pdn  |Stg|
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|         Segment Index         |       Data Area Size          |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
/                             Data                              \
\                              Area                             /
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
~~~

| Field | Bits | Description | RFC |
| :---- | :--: | :---------: | --: |
| Pdn | 3 | Just a shorter way of saying padding. | N/A |
| Stg | 2 | Stage of the DHSP exchange. Must be 0. | N/A |
| Segment Index | 16 | The segment index if the DHDB is too large to fit in one CP packet. | N/A |
| Data Area Size | 16 | Size, in bytes, of the data area. | N/A |
| Data Area | Data Area Size | Data area for DHDB transmission. | N/A |

#### See HHS
See RFC 9.

##### Note:
DHSP can over be carried over DTP.

#### Segment Index
In case the DHDB is too large to be sent over in a single packet, multiple of these packets
in the MARY_GIVES state can be sent. The Segment Index is simply only used for reassembly.
This could be accomplished with using ATP instead of DTP, but that would provide lots of overhead
for something that might not even be needed.

As such, segment order doesn't matter, because Bob will just reassemble them.

#### Data Area Size
How big the data area is, in bytes.

#### Data Area
Where the raw DHDB data should be to be transmitted. May be incomplete with the help of the
Segment Index.

### Functionality
Once Mary receives a DHSP packet in the BOB_WANTS stage, she'll formulate a packet (or multiple,
depending on how big the DHDB is, see the segment index), and send them back to Bob. It's out of
her hands now, and into Bob's.

He should reassemble the packets if needed and apply the changes to his own DHDB copy.

# Authors
Milo Banks

