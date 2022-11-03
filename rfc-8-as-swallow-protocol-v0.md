AS Swallow Protocol v0

# Introduction
Unlike most other protocols present in the realm of Catnet, The AS Swallow Protocol (henceforce refered
to as ASP) isn't a protocol that get directy fed through a wire; instead, it is a protocol describing
how nodes should attempt to become part of an autonomous system consisting of over Catnet nodes.

Because this is v0, it is very limited. CPv0 doesn't support DIP (Catnet's version of DHCP), or any
other fancy things you might be accustomed to on IP networks. Instead, it covers the very basic steps
to becoming... swallowed!

## Terminology

### Autonomous System (AS)
An autonomous system is an independent network that route packets internally.

### Multicast
A way to deliver a packet from one source to many receivers. This is in contrast to broadcast, which is
generally considered to be inferior, and is a one-to-all delivery system.

## Motivation
Without being able to become part of a network, it's impossible to communicate with other systems. A
standardized protocol is nessisary to make sure that every node speaks the same language to new-comers
to help them get actuated and connected.

## Scope
As stated in the title of this RFC, this AS Swallow Protocol is only to be used on local networks. As such,
it shouldn't be used in areas that intradomain routing might take place; there is another protocol for that.

The reason why this isn't allowed is because ASP uses multicast (via MUDS, RFC 2) which is a bad idea/just plain
won't work on the wider, public internet. There are also other reasons, such as the initial peer discovery
problem, but coming up with those is an excersize left to the reader :)

## Operation
Below are the steps for a computer to become a peer to others in an autonomous system running Catnet.
Assume Bob wants to become a peer.

1. Bob sends an IDENT packet out of his prefered network device and to a node on the network (Sally).
2. Sally sees Bob's flooded IDENT packet, and responds with her DHDB (Distributed Hash DataBase) via a DHSP return packet.
3. Bob will attempt to make contact with everybody in Sally's DHDB by constructing a relay tree for the RELAY packet.
4. Bob sends out a RELAY packet wrapping a DHSP packet to each 2nd degree joint.
5. By looking at responses, Bob learns of down nodes, and reconstructs the tree to bypass them, updating his copy of the DHDB
in the process.
6. Repeat from step 4 until the DHDB is completely updated.
7. If there any any updates, fetch one of each different one and merge.
8. Bob now has a up-to-date DHDB.

Let's go back for a moment, at look at steps 4-5. In between those, other nodes on the network get Bob's RELAY DHSP packet
and send back a hash of their DHDB, a time last modified value, and if they want to receive an updated DHDB if their's is
found to be out of date. Bob will then send back a new DHDB to everyone who wants one, once he calculates the new database.

## Potential Issues
ASPv0 isn't very efficient becuase it doesn't need to be. At the scales that CPv0 is being tested on, ASPv0 will work just
fine. However, on larger networks, ASPv0 is not flexible at all.

### Huge DHDBs
At some point, the collective DHDB will get large enough it's simply not efficient to transfer everything. This could be improved
by splitting the DHDB into multiple differnet DBs, but this makes this step harder. A problem for a future version!

### Others
This is left as an excerize to the reader.

# Authors
Milo Banks

