# Some Basic Networking Commands

<br>

### ping 
1. The ping command is used to test the ability of source computer to reach a specified destination computer. It's usually used to verify that a computer can communicate over the network with another computer or network device.
2. The ping command **operates by sending Internet Control Message Protocol (ICMP) Echo Request messages** to the destination computer and waiting for a response. How many of those responses are returned, and how long it takes for them to return, are the two major pieces of information that the ping command provides.

Syntax: `ping <IP> or <website_name>`

<br>

### netstat
1. netstat (network statistics) displays list of all active TCP connections which, for each one, will show the local IP address (your computer), the foreign IP address (the other computer or network device), along with their respective port numbers, as well as the TCP state.
2. It is used for finding problems in the network and to determine the amount of traffic on the network as a performance measurement.

<br>

### ipconfig
The output of this command contains the IP address, network mask, and gateway for all physical and virtual network adapters.

<br>

### tracert
1. Consider a situation when you are not able to access a website and can access other websites. You would want to know if this is a problem with your network, some intermediate network or with web server. How do you figure out? You can use Traceroute.
2. It shows you the complete route to a destination address. It also shows the time taken (or delays) between intermediate routers.

Syntax: `tracert <website_name>`

<br>

**How does traceroute make sure that a packet is dropped at i’th hop?**

It uses TTL (Time to Live) field for this purpose. TTL is set as 1 for first packet(s), then 2 and so on until destination is reached.

<br>

**How is total time estimated?**

When a packet is dropped, the router sends an ICMP Time Exceeded message back to the source. That is how source figures out total time.

<br>

### nslookup
1. Nslookup (stands for “Name Server Lookup”) is a useful command for getting information from DNS server.
2. It is used to obtain domain name or IP address mapping or any other specific DNS record. It is also used to troubleshoot DNS related problems.

Syntax: `nslookup <ip> or <website_name>`

<br>

### route
1. route command used to set up static routes to specific hosts or networks via an interface. 
2. It is used for showing or update the IP/kernel routing table.
3. It is used to add or remove default gateway configuration.

<br>

### pathping
1. This is a more advanced version of the Ping, which performs a ping to each hop along the route to the destination (unlike Ping, which just pings from the originating device to the destination device).
2. It is extremely useful in diagnosing packet loss, and can help with diagnosing slow speed faults.

<br>

### netdiag
1. The Netdiag diagnostic tool helps to isolate networking and connectivity problems by performing a series of tests to determine the state of network client.

<br>

### hostname
1. It is used to obtain the DNS(Domain Name System) name and set the system’s hostname or NIS(Network Information System) domain name.

<br>

### arp
1. arp command manipulates the System’s ARP cache. It also allows a complete dump of the ARP cache. 
2. The primary function of this protocol is to resolve the IP address of a system to its mac address, and hence it works between level 2(Data link layer) and level 3(Network layer).
