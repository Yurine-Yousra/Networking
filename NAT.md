# Static NAT

IPv4 does not provide enough addresses for all devices that need an IP address in modern networks. A long-term solution is to switch to IPv6. There are three short-term solutions:

- CIDR
- Private IPv4 addresses
- NAT

RFC 1918 specifies the following IPv4 address ranges as private:

- **10.0.0.0 /8** (10.0.0.0 to 10.255.255.255)
- **172.16.0.0 /12** (172.16.0.0 to 172.31.255.255)
- **192.168.0.0 /16** (192.168.0.0 to 192.168.255.255)

You are free to use those IPv4 addresses in your network; they don’t have to be globally unique. However, there are two problems:

1. Duplicate IP addresses
2. Private IP addresses can't be used over the internet, so the PCs cannot access the internet

### Network Address Translation (NAT)
NAT is used to modify the source and/or destination IP addresses of packets.

There are various reasons to use NAT, but the most common reason is to allow hosts with private IP addresses to communicate with other hosts over the internet.

For the CCNA, you have to understand source NAT and how to configure it on Cisco routers.

### Static NAT
In the figure, PC1 will send traffic to the server. The router will change the private IP address of PC1 to the IP address of its interface. When the server communicates with PC1, it will use the global inside IP address attributed to PC1 by the router. The router will then translate the global IP address attributed to PC1 back to its original private IP address.

#### Inside Local / Inside Global / Outside Local / Outside Global
Static NAT involves statically configuring one-to-one mappings of private IP addresses to public IP addresses.

- **Inside Local:** The IP address of the inside host, from the perspective of the local network. It is usually the IP address configured on the inside host and is usually private.
- **Outside Local:** The IP address of the outside host, from the perspective of the local network.
- **Outside Global:** The IP address of the outside host from the perspective of the outside network.
- **Inside Global:** The IP address of the inside host from the perspective of the outside hosts. This is the public IP address assigned after NAT.

### NAT Configuration
On R1:
```
R1(config)#interface g0/1
R1(config-if)#ip nat inside  (define the inside interface(s) connected to the internal network)
R1(config-if)#interface g0/0
R1(config-if)#ip nat outside (define the outside interface(s) connected to the external network)
R1(config-if)#exit

R1(config)#ip nat inside source static 192.168.0.167 100.0.0.1  (configures one-to-one IP address mappings)
R1(config)#ip nat inside source static 192.168.0.168 100.0.0.2
```
To show the IP NAT translation on the router, use the command:
```
show ip nat translations
```
To clear the dynamic NAT translations, use the command:
```
clear ip nat translation *
```

### Dynamic NAT
In dynamic NAT, the router dynamically maps inside local addresses to inside global addresses as needed.

An ACL is used to identify which traffic should be translated:
- If the source IP is permitted by the ACL, the source IP will be translated.
- If the source IP is denied by the ACL, the source IP will not be translated, and the traffic will be dropped.

A NAT pool is used to define the available inside global addresses.

On R1:
```
R1(config)#interface g0/1
R1(config-if)#ip nat inside
R1(config-if)#interface g0/0
R1(config-if)#ip nat outside
R1(config-if)#exit

R1(config)#access-list 1 permit 192.168.0.0 0.0.0.255 (define the traffic that should be translated)
R1(config)#ip nat pool POOL1 100.0.0.1 100.0.0.10 prefix-length 24 (or netmask 255.255.255.0)
R1(config)#ip nat inside source list 1 pool POOL1
```
Although they are dynamically assigned, the mappings are still one-to-one (one inside local IP per one inside global IP address).

If there are not enough inside global IP addresses (i.e., all are currently in use), this is called **NAT pool exhaustion**. If a packet from another inside host arrives and needs NAT, but there are no available addresses, the router will drop the packet. The host will be unable to access outside networks until one of the inside global IP addresses becomes available.

Dynamic NAT entries will time out automatically if not used, or you can clear them manually.

### PAT (NAT Overload)
PAT translates both the IP address and the port number (if necessary).

By using a unique port number for each communication flow, a single public IP address can be used by many different internal hosts. Port numbers are 16 bits in length, providing over 65,000 available port numbers.

For example, in the figure:
- **PC1** has an IP address of `192.168.0.167` and is using port `54321`.
- **PC2** has an IP address of `192.168.0.168` and is also using port `54321` (the same port as PC1).

The router will assign the same public IP address but will change the port number of PC2 to `54322` to keep the connection unique.

### PAT Configuration
```
R1(config)#interface g0/1
R1(config-if)#ip nat inside
R1(config-if)#interface g0/0
R1(config-if)#ip nat outside
R1(config-if)#exit

R1(config)#access-list 1 permit 192.168.0.0 0.0.0.255
R1(config)#ip nat pool POOL1 100.0.0.1 100.0.0.10 prefix-length 24
R1(config)#ip nat inside source list 1 pool POOL1 overload
```
This configuration enables NAT overload, allowing multiple devices to share a single public IP address while maintaining unique connections using different port numbers.

