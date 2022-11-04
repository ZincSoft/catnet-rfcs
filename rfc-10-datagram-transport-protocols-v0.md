Datagram Transport Protocols (Datagram Transport Protocols v0)

# Document
Catnet has various datagram transport protocols. Unlike the IP stack, which only has two main protocols in this
regard (TCP and UDP), CP has more. The point of this is to provide distinct protocols that are most suitable for
their own type of data. Below is a table of these protocols.

| Protocol | RFC | Ordered | Error Checked | Acknowledge | Stateful | Handshake |
| :------- | --- | ------- | ------------- | ----------- | -------- | --------: |
| ATP      | 2   | Y       | Y             | Y           | Y        | Y         |
| UTP      | 4   | N       | N             | N           | N        | N         |
| DTP      | 11  | N       | Y             | N           | N        | N         |

## ATP
ATP is CP's version of TCP. To most applications reaping ATP, it is functionally indistinguishable from TCP.
However, it borrows certain ideas and tools from other places outside CP (namely QUIC and various research
papers), which make ATP A) faster, B) less-conversational, and C) simpler.

## UTP
UTP is CP's version of UDP. To most applications reaping UTP, it is functionally indistinguishable from UDP, as
UTP really offers no improvements on UDP. UDP is simple as it is, and bare bones.

UTP also forms as the base for other datagram transport protocols. Because it's so simple, there seems to be no
transport protocol that cannot be build on it, furthering DRY in Catnet.

## DTP
DTP is kind-of a special case; it's used where quick, simple, stateless transfer is needed (such as what UTP
provides), but where error checking is also needed. It follows ATPs algorithm for error checking. This is mainly
used as a platform that other diagnostic and away-from-user protocols build upon.

