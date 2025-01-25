


## Overview

- It is an industry-standard protocol.
- It is a distance vector IGP (uses the routing by rumor to learn and share routes).
- Uses hop count as a metric where one router = one hop (bandwidth is irrelevant).
- The maximum hop count is equal to 15, which means that if a destination network has a route with more than 15 hops (15 routers), it is considered an unreachable destination.
- RIP has three versions:
    - RIP v1 and RIP v2 are used for IPv4.
    - RIPng (RIP next generation) is used for IPv6.

---

## RIP Messages

RIP uses two types of messages:

1. **Request:** To ask RIP-enabled neighbor routers to send their routing table.
2. **Response:** To send the local router's routing table to neighbor routers.

By default, RIP-enabled routers share their routing tables once every 30 seconds.

---

## RIP v1 (Classful Networks)

- Networks are classful, which means a network with an address of `10.0.12.0 /30` will be considered as `10.0.0.0 /8` because this address belongs to the Class A range.
- A network with an address of `172.16.1.0 /28` will be considered as `172.16.0.0 /16` because this address belongs to the Class B range.
- A network with an address of `192.168.3.0 /29` will be considered as `192.168.3.0 /24` because this address belongs to the range of Class C. This is what is classful.
- The CIDR and VLSM are not supported in this version, which means that RIP v1 cannot be used in modern networks because modern networks work with VLSM and CIDR.

---

## RIP v2 (Classless Networks)

- RIP v2, however, uses the CIDR and VLSM technologies, so the network will be taken with its real subnet mask.

---

## Basic Configuration of RIP Protocol on a Router

1. **Enter RIP configuration mode:**
    
    ```plaintext
    R1(config)#router rip
    ```
    
2. **Use RIP version 2:**
    
    ```plaintext
    R1(config-router)#version 2
    ```
    
3. **Disable automatic summarization:**
    
    ```plaintext
    R1(config-router)#no auto-summary
    ```
    
4. **Define the networks:**
    
    ```plaintext
    R1(config-router)#network 10.0.0.0
    R1(config-router)#network 172.16.0.0
    ```
    

---

## Explanation of the `network` Command

- The `network` command is classful; it will automatically convert to classful networks.
- The address entered after the `network` command will be classed (e.g., if `10.12.3.0` is entered, it will be converted to `10.0.0.0`, so here the prefix is `10`).
- The RIP protocol will be enabled on all the router's interfaces that have an address with the same prefix.
- In the topology:
    - After entering the command `[network 10.0.0.0]`, all the interfaces with an address of prefix `10` will activate the RIP protocol.
    - After entering the command `[network 172.16.0.0]`, all interfaces with an address of prefix `172.16` will activate the RIP protocol.

---

## Summary of the `network` Command

- The `network` command tells the router to:
    1. Look for interfaces with an IP address that is in the specified range.
    2. Activate RIP on the interfaces that fall in the range.
    3. Form adjacencies with concerned RIP neighbors.
    4. Advertise the `[network prefix of the interface]` and NOT the prefix in the `network` command.

**Note:** The `network` command does not tell the router which networks to advertise. It tells the router which interfaces to activate the RIP on, and then the router will advertise the network prefix of those interfaces.

---

## Passive Interface Configuration

- Let's take the example of the given topology:
    
    - In R1, the command `network 172.16.0.0` is executed.
    - Then RIP will be activated on all the interfaces with an address of prefix `172.16`. In this case, it will be activated on `G2/0`.
    - The network that will be advertised is `172.16.1.0 /28` AND NOT `172.16.0.0 /16`.
- The interfaces will form adjacencies with RIP-enabled neighbors.
    
- However, the interface has no RIP neighbors, although R1 will continuously send RIP advertisements out of `G0/2`. This is unnecessary traffic, so `G0/2` should be configured as a passive interface.
    
- The command to do that is:
    
    ```plaintext
    R1(config-router)#passive-interface g0/2
    ```
    

### Explanation:

- The passive interface command tells the router to stop sending RIP advertisements out of the specified interface.
- However, it will continue to advertise the network prefix of the interface (`172.16.1.0 /28`) to its RIP neighbors (`R2`, `R3`).

---

## Advertising the Default Route in RIP

- Let's assume that any packet with an unknown destination should be sent to the internet. So, all the packets with destinations that do not match any entry of the routing table will be sent to the internet via the default route.

### Reminder of the Default Route:

- If any received packet has a destination that does not match any entry of the routing table of the router, it will be sent over the default route.

### Configuration:

1. **Define the default route:**
    
    ```plaintext
    ip route 0.0.0.0 0.0.0.0 [next-hop]
    ```
    
    Example:
    
    ```plaintext
    ip route 0.0.0.0 0.0.0.0 203.0.113.2
    ```
    
2. **Advertise the default route:**
    
    ```plaintext
    R1(config-router)#default-information originate
    ```
    

### Explanation:

- Any packet received on R1 with a destination that does not match any entry of the routing table of R1 will be sent to `203.0.113.2`.
- Then, the command `default-information originate` should be executed to advertise the default route (to tell R2 & R3 that to reach the internet, for example, you send the packet to me).
- R2 and R3 then tell R4 that to reach the internet, you send the packet to me.