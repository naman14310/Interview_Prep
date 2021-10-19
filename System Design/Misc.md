# Misc Design Terminologies

<br>

## @ Virtual IP and VRRP protocol

[Best Explaination](https://www.youtube.com/watch?v=bXQ0HvsWI60)

A virtual IP address (VIP or VIPA) is an IP address that doesn't correspond to an actual physical network interface. Uses for VIPs include network address translation (especially, one-to-many NAT), fault-tolerance, Availability and mobility.

VRRP is an open standard protocol, which is used to provide redundancy in a network. It is a network layer protocol (protocol number-112). The number of routers (group members) in a group acts as a virtual logical router. If one router goes down, one of the other group members can take place for the responsibilities for forwarding the traffic.

<br>

## @ ZooKeeper
ZooKeeper is an open source Apache project that provides a centralized service for providing configuration information, naming, synchronization and group services over large clusters in distributed systems. The goal is to make these systems easier to manage with improved, more reliable propagation of changes.
