## Overview

OSPF stands for Open Shortest Path First Used the Shortest Path First algorithm of Dutch computer scientist Edsger Disktra There are 3 versions of OSPF:

1. **OSPFv1**: Old, not in use anymore
2. **OSPFv2**: Used for IPv4
3. **OSPFv3**: Used for IPv6 (can also be used for IPv4, but usually OSPFv2 is used for IPv4)

Routers store information about the network in LSAs (Link State Advertisements), which are organized in a structure called LSDB (Link State Database). Routers will flood LSAs until all routers in the OSPF area develop the same map of the network (LSDB).

**Important Note:** For the purpose of CCNA, we will be configuring only OSPFv2 with a single Area.

## LSA Flooding

In the following topology (i will insert a topology):

- OSPF is enabled on R4's G1/0 interface. R4 creates an LSA to tell its neighbors about the network on G1/0. The LSA is flooded throughout the network until all routers have received it. This results in all routers sharing the same LSDB.

The basic format of an LSA is as follows:

- **RID**: Router ID
- **IP**: IP address of the interface that enabled OSPF on it
- **COST**

Each LSA has an aging timer of 30 minutes. The LSA will be flooded again after the timer expires.

## OSPF Area

**What is an OSPF Area?** It is a group of routers that share the same LSDB.

OSPF uses areas to divide up a network. Small networks can be single-area networks without any negative effects on their performance.

In larger networks, a single-area design can have negative effects:

- The SPF (Shortest Path First) calculation takes more time to be calculated.
- The SPF requires exponentially more processing power on the routers.
- The larger LSDB takes up more memory on the router.
- Any small changes in the network cause routers to flood LSAs and run the SPF algorithm again all over the network.

By dividing a large OSPF network into several smaller areas, you can avoid the above negative effects.

- The backbone area (**area 0**) is an area that all other areas must connect to.
- Routers with all interfaces in the same area are called **internal routers**.
- Routers with interfaces in multiple areas are called **Area Border Routers (ABRs)**.
- Routers connected to the backbone are called **backbone routers**.
- An **intra-area route** is a route to a destination inside the same OSPF area.
- An **inter-area route** is a route to a destination in a different OSPF area.

## Rules to Follow When Making OSPF Areas

- OSPF areas should be contiguous (it should not be divided as shown in the figure2 (here I should insert an image that shows separated OSPF areas)).
- All OSPF areas must have at least one ABR connected to the backbone area.
- OSPF interfaces in the same subnet must be in the same area (I should insert an image that shows that OSPF Rule Three).

In the following topology, the interface.2 should be configured to be in **area 0** and not **area 1** because it shares the same subnet mask as area 0.

## Basic OSPF Configuration

```bash
R1(config)#router ospf [processId]
```

The range of the IDs is 1-65535. Here, it is not like EIGRP that necessitates having the same AS on all routers to function properly. The process ID is different from the AS; routers with different OSPF process IDs can operate normally and form OSPF adjacencies.

```bash
R1(config-router)# network [network] [wildcardmask] area [area-number]
```

The network command in OSPF works the same as in EIGRP; however, in OSPF, you have to specify the number of the area.

**Note1 + Reminder:** The network command tells OSPF to:

- Look for any interfaces with an IP address contained in the range specified in the network command.
- Activate OSPF on the interface in the specified area.
- The router will then try to become OSPF neighbors with other OSPF-activated neighbor routers.

**Note2:** You can configure the passive interface the same way as EIGRP. From the router OSPF mode:

```bash
R1(config-router)#passive-interface g2/0
```

If an interface is passive, this tells the router to stop sending OSPF hello messages out of this interface. This means it will not make OSPF neighbors through this interface, but this passive interface has OSPF activated on it, so it will continue to send LSAs to its OSPF neighbors to advertise the subnet configured on the interface.

## Advertise a Default Route into OSPF

You can do this the same way in EIGRP:

- Configure a default route.
- Then, from the OSPF mode, enter the command:

```bash
default-information originate
```

The router will advertise the default route to all its OSPF neighbors, and the LSA of the default route will be sent all over the network, and all routers will be able to reach it.

When configuring a default route on an OSPF router, the router will be an **ASBR (Autonomous System Boundary Router)**, which means that the router can connect the OSPF network to an external network.

## Configuration: How to Configure a Router ID

From the OSPF or EIGRP mode, enter the command:

```bash
R1(config-router)#router-id [IP-address]
```

After entering this command, the ID of the router will not be updated automatically. You should reload or use the command:

```bash
clear ip ospf process
```

This allows the router ID to be modified. However, it is a bad practice because by using this command, OSPF will not work for a short time, and traffic will not be carried.

**By default:** OSPF can load balance traffic over 4 paths. However, it cannot unequally load balance traffic like EIGRP can. You can modify the maximum number of load-balancing paths by entering the command from OSPF mode:

```bash
maximum-paths <number-of-paths>
```

The range of the maximum paths is 1-32.

Here is the structured version of the content with bold titles after the headings:

---

**Heading: OSPF Cost**  
**OSPF's metric is called cost.**

- It is automatically calculated based on the bandwidth (speed) of the interface.
- It is calculated by dividing the **[reference bandwidth]** value by the bandwidth of the interface.

**Different Ethernet Bandwidths**

- Regular Ethernet Interface: 10 Mbps
    
- Fast Ethernet Interface: 100 Mbps
    
- Gigabit Ethernet Interface: 1000 Mbps = 1 Gbps
    
- 10-Gigabit Ethernet Interface: 10000 Mbps = 10 Gbps
    
- **The default reference bandwidth is 100 Mbps.**
    
    - Reference 100 Mbps / Interface: 10 Mbps = cost of 10
    - Reference 100 Mbps / Interface: 100 Mbps = cost of 1
    - Reference 100 Mbps / Interface: 1000 Mbps = cost of 0.1 converted to 1
    - Reference 100 Mbps / Interface: 10000 Mbps = cost of 0.01 converted to 1
- **All values less than 1 are converted to 1.**
    
    - Therefore, FastEthernet, GigabitEthernet, and 10GigabitEthernet have the same cost of 1 by default.

**Changing the Reference Bandwidth**

- Use the following command from OSPF mode:  
    `R1(config-router)#auto-cost reference-bandwidth <megabits-per-second>`
    
- **The range of reference bandwidth is [1 - 4294967].**
    
    - For example:  
        `R1(config-router)#auto-cost reference-bandwidth 100000`
- **The new costs will be as follows:**
    
    - Reference 100000 Mbps / Interface: 10 Mbps = cost of 10000 (Ethernet)
    - Reference 100000 Mbps / Interface: 100 Mbps = cost of 1000 (FastEthernet)
    - Reference 100000 Mbps / Interface: 1000 Mbps = cost of 100 (GigabitEthernet)
    - Reference 100000 Mbps / Interface: 10000 Mbps = cost of 10 (10GigabitEthernet)
- **Recommendation:**
    
    - Configure a reference bandwidth greater than the fastest link in your network.
    - Configure the same reference bandwidth on all OSPF routers in the network.

**The OSPF Cost to a Destination**

- The total cost to a destination is the sum of the 'outgoing/exit interfaces.'
- **Example Topology:**
    - R1 cost to reach 192.168.4.0 /24 (default reference bandwidth):
        - Cost = 100 (R1 g0/0) + 100 (R2 G1/0) + 100 (R4 G1/0)
        - Cost = 100 * 1 + 100 * 1 + 100 * 1
        - Cost = 300

---

**Heading: Changing OSPF Cost**  
**There are three ways to change the OSPF cost:**

1. **Changing the reference bandwidth** on all OSPF routers in the network:  
    `R1(config-router)#auto-cost reference-bandwidth <megabits-per-second>`
    
2. **Changing the bandwidth of the interface** with the `bandwidth` command:
    
    - **Note:** Although the bandwidth matches the interface speed by default, changing the interface bandwidth does not actually change the speed at which the interface operates.
    - The bandwidth is just a value used to calculate OSPF cost and EIGRP metrics.
    - To change the speed at which an interface operates, use the **[speed]** command.
    - **Recommendation:** Because the bandwidth value is used in other calculations, it is not recommended to change it.
3. **Changing the cost of the interface** using the command:  
    `ip ospf cost`
    

---

**Heading: OSPF Neighbors**  
**When OSPF is activated on an interface:**

- The router starts sending OSPF hello messages out of the interface at regular intervals (determined by the hello timer).
- These are used to introduce the router to potential OSPF neighbors.
- **Default hello timer:** 10 seconds on an Ethernet connection (may vary with connection type).
- **Hello messages:** Multicast to `224.0.0.5`.
- OSPF messages are encapsulated in an IP header with the protocol field value of 89.

**Example Process:**

- **Step One: Down State**
    
    - OSPF sends a hello message to `224.0.0.5`.
    - The hello message contains the Router-ID of Router1.
    - The neighbor ID of R2 is set to `0.0.0.0`.
- **Step Two: Init State**
    
    - The hello message is received by R2, and the neighbor ID remains `0.0.0.0`.
    - R2 adds an entry for R1 to its OSPF neighbor table.
- **Step Three: 2-Way State**
    
    - R2 sends a hello packet containing the Router-ID of both routers.
    -  R1 sends a hello packet containing the Router-ID of both routers.
    - Both routers reach the 2-way state.
- **Step Four: Exstart**
    
    - The router with the higher ID becomes the master, and the router with the lower ID becomes the slave.
    - **Master and Slave Selection:**
        - Decided through DBD (Database Description) packet exchange.
        - Let's assume that R1 send the first DBD that indicates that R1 is the Master R2 receives it and then sends another DBD indicating that R2 has the higher Router-Id so it will become the Master Router therefore R1 will be the Slave Router.
- **Step Five: Exchange**
    
    - Routers exchange DBDs, which contain a list of the LSAs in their LSDB.
    - Each router will send DBDs that contains a list of LSAs ,those DBDs don't contain detailed information about the LSAs just basic information .
    - The routers compare the information in the DBD they received to the information in their own LSDB to determine which LSAs they must receive from the neighbor.
- **Step Six: Loading**
    
    - Routers send Link State Requests (LSR) to request LSAs they do not have.
    - The neighbor will send Link State Updated (LSU) as his neighbor requested throught LSR.
    - After receiving the LSU the router sends LSAck to tell the transmitter that it has succesfully received the LSU.
    
- **Step Seven: Full**
    
    - In the full state , the routers have a full OSPF adjacency and identical LSDBs.
    - They continue to send and listen for Hello packets(every 10 seconds by default)
    - Every time the Hello Timer is received the Dead Timer (40 Seconds by default) is Reset.
    - If the dead Timer counts down to 0 and no Hello message is received , the neighbor is removed.
    - The routers will continue to share LSAs as the network changes to make sure each router has a complete and accurate map of the network (LSDB)
  

---

**Heading: Configuring OSPF by Interface**  
**Commands:**

```
R1(config)#int g0/0  
R1(config-if)#ip ospf [ospf-process-id] area [area-number]  
R1(config-if)#ip ospf 1 area 0  
```

---

**Heading: Configuring a Loopback Interface**  
**Commands:**

```
R1(config)#interface loopback <loopback-number>  
R1(config-if)#ip address <ip-address> <subnet-mask>  
```

### Loopback Interface

- It is a virtual interface in the router.
- It is always **up/up** (unless you manually shut it down).
- It is not dependent on a physical interface.
- Therefore, it provides a consistent IP address that can be used to reach/identify the router.

---

### OSPF Network Types

The OSPF network type refers to the type of connection between OSPF neighbors.

There are three main OSPF network types:

1. **Broadcast**
    - Enabled by default on Ethernet and FDDI (Fiber Distributed Data Interfaces) interfaces.
2. **Point-to-Point**
    - Enabled by default on PPP (Point-to-Point Protocol) and HDLC (High-Level Data Link Control) interfaces.
3. **Non-Broadcast**
    - Enabled by default on Frame Relay and X.25 interfaces.

---

### OSPF Broadcast Network Type

- Enabled on Ethernet and FDDI by default.
- Routers dynamically discover neighbors by sending/listening for OSPF Hello messages using multicast address 224.0.0.5.
- A DR (Designated Router) and BDR (Backup Designated Router) must be elected on each subnet (only DR if there are no OSPF neighbors).
- Routers that are not DR or BDR are referred to as DROther.

#### How to Select a DR and BDR

On each subnet:

1. The router with the highest **interface priority** will be the DR.
2. The router with the second-highest **interface priority** will be the BDR.
3. By default, all OSPF interfaces have the same priority set to 1.

- If the priorities are the same, the **Router-ID** will be used to determine the DR and BDR.
- The router with the highest Router-ID in the subnet will be the DR, and the router with the second-highest Router-ID in the subnet will be the BDR.

**Note:** The same router can act as a DR in one subnet, a BDR in another subnet, and a DROther in another subnet.

---

#### Example Scenario

In the following topology, assume that all router interfaces have the same priority equal to 1 and that each router has an ID of **RouterNumber.RouterNumber.RouterNumber.RouterNumber**:

- In subnet **10.0.1.0/24**, **10.0.5.0/24**, **10.0.4.0/24**, and **10.0.3.0/24**, there is only one OSPF neighbor, so it will automatically be the DR.
- In subnet **192.168.1.0/30**, R1 and R2 have the same interface priority (all set to 1). Therefore, the Router-ID will be used:
    - R1's ID is **1.1.1.1** and R2's ID is **2.2.2.2** (higher than R1's ID).
    - R2 will be elected as DR, and R1 will be the BDR.
- The same process applies to subnet **192.168.2.0/29**:
    - R5, with the highest ID of **5.5.5.5**, will be the DR.
    - R4, with an ID of **4.4.4.4**, will be the BDR.
    - Routers R2 and R3 will be DROthers.

---

#### Changing the DR/BDR

For example, to make R2 the DR of subnet **192.168.2.0/29**, you can change R2's **g0/0 interface OSPF priority** using the following command:

```
R2(config)#interface g0/0  
R2(config-if)#ip ospf priority <priority> [priority range: 0–255]  
```

**Note:** If you set the OSPF interface priority to 0, the router **CANNOT** be the DR or BDR of the subnet, no matter what.

The process of electing the DR/BDR is **not preemptive**, which means that even after making changes (e.g., making R2 the DR), R5 will stay the DR of the subnet unless you reload using the command:

```
clear ip ospf process  
```

---

#### Important Note

- When a DR goes down (e.g., R5), the **BDR from the first election** will become the new DR.
- Then, an election is held for the next BDR.

For instance:

- After configuring R2’s interface OSPF priority to be the highest in the subnet, R2 will not become the DR of the subnet immediately.
- Instead, R4 (elected as the previous BDR) will step up to become the DR.
- Since R2 now has the highest OSPF priority, it will become the BDR.


Here’s the text with proper headings added for clarity:

---

### DR/BDR/DROther Relation

- DROthers will only move to the full state with the DR and BDR of the subnet.
- DROthers with other DROthers will move only to the 2-way state.

**Conclusion**:  
In the broadcast network type, routers will form FULL OSPF ADJACENCY with the DR and BDR of the segment.  
Therefore, routers only exchange LSAs with the DR and BDR. DROthers will not exchange LSAs with each other.  
All routers will still have the same LSDB, but this will reduce the amount of LSAs flooding the network.

---

### OSPF Point-to-Point Network Type

- Enabled on serial interfaces using the PPP and HDLC encapsulations by default.
- Routers dynamically discover neighbors by sending/listening for OSPF Hello messages using the multicast address `224.0.0.5`.
- DR and BDR are not elected.

These encapsulations are used for point-to-point connections, which means that there are only two routers in the subnet. Therefore, there is no need to elect a DR and BDR.  
The two routers will make full adjacency with each other and share the same LSDB.

---

### Overview of Serial Connections

- It is between two sides.
- One side of the serial connection functions as a DCE (Data Communications Equipment).
- The other side functions as a DTE (Data Terminal Equipment).

#### DCE Configuration:

- The DCE side needs to specify the clock rate (the speed of the connection).
- The two sides are connected via serial ports (e.g., `S0/1`).

To configure the clock rate on the DCE side, follow these steps:

```
R1(config)#interface s0/1
R1(config-if)#clock rate <clock rate>  (use the `?` to discover the allowed clock rates; generally, 64000 is used)
```

Then, you can enable the interface and configure its IP address.  
You do not have to configure the clock rate on the DTE side.

**Note 1**: Ethernet interfaces use the `speed` command to configure the interface speed, while serial interfaces use the `clock rate` command.  
**Note 2**: The default encapsulation on a serial interface is HDLC. If you change the encapsulation, it must match on both sides.

To change the encapsulation type to PPP, use the following command:

```
R1(config)#interface s0/1
R1(config-if)#encapsulation ppp
```

Using this command, you change the encapsulation type of one side to PPP. You must execute the same command on the other side; otherwise, the interfaces will go down, and they cannot transmit traffic.

---

### Configuring OSPF Network Type

You can configure the OSPF network type on an interface with the following command:

```
ip ospf network <type> [broadcast - non-broadcast - point-to-point]
```

#### When to Change the OSPF Network Type:

For example, if two routers are connected via an Ethernet connection, the OSPF broadcast type is set by default. However, since there is no need to elect a DR and BDR for this subnet, which contains only two ends, you can configure a point-to-point OSPF network.

---

### OSPF Neighbor Requirements

The following are the requirements that must be met for routers to become OSPF neighbors:

- **Same Area**: Routers must belong to the same area.
    
- **Same Subnet**: Interfaces must be in the same subnet.
    
- **OSPF Process Status**: The OSPF process must not be shut down.
    
    _Note_: You can shut down the OSPF process on a router without removing the configuration. When the OSPF process is disabled, the router will have no neighbors and will be removed from the neighbor list of other routers.
    
- **Unique Router ID**: The router ID must be unique.
    
- **Hello and Dead Timers**: These must match.
    
- **OSPF Authentication**: The authentication must match.
    
    You can configure a password for the OSPF process. This password must match on all OSPF neighbors.  
    To configure a password on an OSPF-enabled interface, use the following commands:
    
    ```
    R2(config-if)#ip ospf authentication-key <password>
    R2(config-if)#ip ospf authentication
    ```
    
- **IP MTU Settings**: These must match.
    
    _Note_: If the IP MTUs do not match, OSPF will still be activated but will not function properly.  
    You can change the size of packets transmitted out of an interface using the command:
    
    ```
    R2(config-if)#ip mtu <68-1500> (in bytes)
    ```
    
- **Matching OSPF Network Type**: The OSPF network type must match.
    

---


