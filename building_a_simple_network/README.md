# Building a simple network

Network building exercise

## Components
### For the local network

* 1 router
* 1 switch
* 3 PCs

### To simulate internet access

* 1 external router
* 1 web server
* 1 switch

### To simulate website access

* 1 DNS server

## Step-by-step configuration

### Link the PC together

The 3 PCs are linked to the switch's first 3 ports (FastEthernet 0/1 0/2 0/3) through their FastEthernet0 port. I've attributed the ips/subnets masks to the PCs'interfaces as instrucet and made sure the interfaces were up on both of them and the switch.


**Addressing table :**

| Devices | LAN | IP | Mask |
|---------|-----|----|------|
| PC-Robert | Eth | 192.168.1.10 | 255.255.255.0 | 
| PC-Camille | Eth | 192.168.1.11 | 255.255.255.0 |
| PC-Renaud | Eth | 192.168.1.12 | 255.255.255.0 |

At this points the PCs can already communicate between themselves. 

## Add the router through which the internet would be provided

the locala router's GigabitEthernet 0/0 port is linked to the switch's GigabitEthernet 0/1 port.
the router's interface is configured with the following ip/subnet mask : 192.168.1.254 255.255.255.0.
Again I made sure the interfaces are up and running.
The router's IP can now be set as the default gateway for the 3 PCs.

## Simulate access to the internet by creating another network with a webserver we'll need to reach

The router's interface is configured with the following ip/subnet mask : 192.168.3.254 / 255.255.255.0.

The server's interface is configured with the following ip/subnet mask : 192.168.3.1 / 255.255.255.0 and its default gateway is 192.168.3.254.

The external router can now ping the webserver.

## Enable communication between the networks so we can reach the webserver

The routers are linked together through their Serial 0/1/0 port.
the local router's inteface is configured with the following ip/subnet mask : 192.168.2.1 255.255.255.0
the external router's interface is configured with the following ip/subnet mask : 192.168.2.2 255.255.255.0
the routers can now ping each other. But at this point the PCs can't communicate with the webserver or even the external router so adding route is necessary.
First on the local router with a route to reach network 192.168.3.0 with the mask 255.255.255.0 through next hop which is 192.168.2.2.
Then on the external router with a route to reach network 192.168.1.0 with the mask 255.255.255.0 through next hop which is 192.168.2.1.
The PCs can now reach the webserver on another network. If we use a browser on a PC and try to reach 192.168.3.1, the default cisco packet tracer webpage will be displayed.

But users mostly use domain names. Let's say the domain name of this is www.cisco.com. If we try to reach it we get a "Host Name Unresolved" error. It's because our network doesn't know which ip is behind this domain name. So we're going to add a local DNS server to help us.

## Setup the DNS Server

The DNS server will be linked through its FastEthernet0 port to the switch's fastEthernet 0/1 port.
It is configured with the following ip/subnet mask : 192.168.3.1 / 255.255.255.0 . It's default gateway is 192.168.3.254
The DNS server can now be reached by other end devices.
I've enabled the DNS service on the DNS server and I've added an entry with the name www.cisco.com whitch has the address 192.168.3.15
then i've configured all the devices with the 192.168.3.1 as the DNS server address.

Now www.cisco.com can be reached through the browser of the PCs.


