# Misc Topics

<br>

## Private IP Vs Public IP 

#### Private IP
1. Private IP address of a system is the IP address which is used to communicate within the same network.
2. Our Wifi Router creates a local network of our devices and assigns private IPs to each local device.
3. It can only be accessed from inside of your LAN (unless you setup **Port Forwarding** for remote access).
4. Private IP can be known by entering “ipconfig” on command prompt. 


#### Public IP
1. Public IP address of a system is the IP address which is used to communicate outside the network. 
2. Public IP address is basically assigned by the ISP (Internet Service Provider) for unique identification of each devices. 
3. Public IP can be known by searching “what is my ip” on google.
4. When we connect with mobile data, IP provided will be public.


<br>

## Dynamic IP and Static IP

### Dynamic IPs
1. Dynamic IP addresses are randomly generated IP addresses for users, these IP addresses are by default allotted by network providers to its subscribers. They are dynamic in nature i.e. they changes any time.
2. For businesses, network providers offer a range of IPs. Within this range that business can allow IP addresses to its employees.
3. For dynamic IP allotment, DHCP (Dynamic host control protocol) is used where the user has to put in a range and submit it.
4. Dynamic ip address is easy to designate and maintaining cost of dynamic ip is less than static ip.

#### Why Dynamic IP addresses are used ?
Reason for keeping most of IPs dynamic is that IP address are limited in number, and it is not feasible to assign static IPs to all devices. All the users on the network will not be always active so restricting an IP address for a user is not beneficial for network providers. Every user who connects to the network will avail a random IP from their available pool.

<br>

### Static IP
1. Static IP addresses are fixed IP addresses and they are usually provided to businesses. If a static ip address is provided then it can’t be changed or modified.
2. Network providers charge a premium to allow static IP addresses.
3. The cost to maintain the static ip address is higher than dynamic ip address.

#### Why Static IP addresses are used ?
Static IP addresses are used by businesses for hosting: FTP servers, Email servers, VPN servers, Web hosting services, Remote desktop service, Tracking user activities.

<br>

### Should I use static IP addresses or dynamic IP addresses?
Large scale companies and software companies require static IP addresses because they need all the services that can be easily availed with static IP addresses. All the other companies and users who don’t use these services should use dynamic IP addresses.



<br>

## [VPN](https://www.youtube.com/watch?v=8wDI5_MVdmk)
VPN stands for "Virtual Private Network". A VPN hides your IP address by letting the network redirect through a specially configured remote VPN server. This means that if you surf online with a VPN, the VPN server becomes the source of your data. The VPN acts as a secure tunnel between you and the internet. Your ISP and other third parties cannot detect this tunnel.This means your Internet Service Provider (ISP) and other third parties cannot see which websites you visit or what data you send and receive online.

<br>

### Benefits of a VPN connection
1. Online activities are hidden even on public networks. Hide your IP address from your ISP and other third parties.
2. VPN servers act as our proxies on the internet. Our actual location cannot be determined.
3. With VPN location spoofing, we can get access to content that is retricted in our region.
4. Ensure secure data transfer. 

<br>

### Types of VPN

#### 1. SSL VPN
SSL VPNs enable devices with an internet connection to establish a secure remote access VPN connection with a web browser. Enterprises use SSL VPNs to enable remote users to securely access organizational resources.

#### 2. Site to site VPN
It is designed to hide private intranets and allow users of these secure networks to access each other's resources. It is useful if you have two separate intranets between which you want to send files without users from one intranet explicitly accessing the other. Mainly used in large companies

#### 3. Client to Server VPN
Connecting via a VPN client can be imagined as if you were connecting your home PC to the company with an extension cable. This involves the user not being connected to the internet via his own ISP, but establishing a direct connection through his/her VPN provider.

<br>

## [What is G in 4G ?](http://net-informations.com/q/diff/generations.html)
Simply, the "G" stands for "GENERATION". While you connected to internet, the speed of your internet is depends upon the signal strength that has been shown in alphabets like 2G, 3G, 4G. Each Generation is defined as a set of telephone network standards. The speed increases and the technology used to achieve that speed also changes.

![img](http://net-informations.com/q/diff/img/mobile-generations.png)
