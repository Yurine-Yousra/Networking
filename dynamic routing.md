### Network Route vs Host Route

- **Network Route**: A route to a network (mask length < /32).
- **Host Route**: A route to a specific host (mask length = /32).

In **dynamic routing**, the router will remove invalid routes (dead ends). However, in **static routing**, routers are unaware if an end falls down, so they will continue to send traffic.

Routers can use **dynamic routing protocols** to advertise information about the routes they know to other routers. They form "adjacencies" or "neighbor relationships" with adjacent routers to exchange this information. If multiple routes to a destination are learned, the router determines which route is superior and adds it to the routing table. It uses the **metric** of the route to decide which is superior (lower metric = superior).

### Types of Dynamic Routing Protocols

Dynamic routing protocols can be divided into two main categories:

1. **IGP (Interior Gateway Protocol)**: Used to share routes within a single autonomous system (AS), which is a single organization.
2. **EGP (Exterior Gateway Protocol)**: Used to share routes between different autonomous systems.

**IGP** uses the following algorithms:

- **Distance Vector algorithms**:
    - RIP (Routing Information Protocol)
    - EIGRP (Enhanced Interior Gateway Routing Protocol)
- **Link State algorithms**:
    - OSPF (Open Shortest Path First)
    - IS-IS (Intermediate System to Intermediate System)

**EGP** uses:

- **Path Vector algorithm**:
    - BGP (Border Gateway Protocol)

### Distance Vector Routing Protocols

Distance vector protocols operate by sending the following to their directly connected neighbors:

1. Their known destination networks
2. Their metric to reach their known destination networks

This method of sharing routes is often called "routing by rumor" because the router does not know about the network beyond its neighbors. It only knows the information that its neighbors tell it. The protocol is called "distance vector" because the routers only learn the **distance** (metric) and **vector** (direction to the next hop) of each route.

### Link State Protocols

Every router creates a connectivity map of the network. To allow this, each router advertises information about its interfaces (connected networks) to its neighbors. These advertisements are passed along to other routers until all routers in the network develop the same map of the network. Each router independently uses this map to calculate the best routes to each destination.

Link state protocols use more CPU on the router because more information is shared. However, link state protocols tend to be faster in reacting to changes in the network than distance vector protocols.

### Dynamic Routing Protocol Metrics

If there are multiple routes learned to the same destination via the same dynamic routing protocol, the route with the smallest metric will be added to the routing table of the router. If a router learns two or more routes via the same dynamic routing protocol to the same destination and those routes have the same metric, all routes will be added to the routing table, and traffic will be load balanced over all routes. This is called **ECMP (Equal Cost Multi-Path)**.

- **RIP**: Uses the hop count as a metric. Each router in the path counts as one hop. The total metric is the total number of hops to the destination. Links of all speeds are equal (FastEthernet, Ethernet, GigabitEthernet, 10GigabitEthernet) even though they have different speeds.
- **EIGRP**: Uses a metric based on bandwidth and delay (by default). A complex formula can take into account many values. By default, the bandwidth of the slowest link in the route and the total delay of all links in the route are used.
- **OSPF**: Uses cost. The cost of each link is calculated based on the bandwidth. The total metric is the total cost of each link in the route.
- **IS-IS**: Uses cost. The total metric is the total cost of each link in the route. The cost of each link is not automatically calculated; by default, all links have a cost of 10.

### Administrative Distance

In most cases, a company only uses a single IGP. However, in some cases, they might use two. For example, if two companies connect their networks to share information, two different routing protocols might be in use.

Metrics are used to compare routes learned by the same dynamic routing protocol. The **administrative distance (AD)** is used to determine which routing protocol is preferred. A lower AD is preferred and indicates that the routing protocol is considered more trustworthy (more likely to select good routes).

**Rules**:

- If there are different routes to the same destination learned by the same dynamic routing protocol, the **metric** is used to determine the best route.
- If there are different routes to the same destination learned by different dynamic routing protocols, the **administrative distance** is used to determine the best route.

The administrative distance can be changed to prefer a route learned by one dynamic routing protocol over another. For example, static routes have an administrative distance of 1, and OSPF has an administrative distance of 110. If you want to make the route learned by OSPF preferred over the static route, you can increase the administrative distance of the static route by using the following command:

```
ip route [destination IP address] [destination subnet mask] next-hop IP address [administrative distance of the static route]
```

In this case, because the static route will no longer be in use, it is called a **floating static route**. This route will be inactive unless the route learned by the dynamic routing protocol is removed. The route with the smallest administrative distance will be elected as the best route.

**Administrative Distance Table**:

|Route Source|Administrative Distance|
|---|---|
|Connected interface|0|
|Static route|1|
|EIGRP summary route|5|
|External BGP|20|
|Internal EIGRP|90|
|IGRP|100|
|OSPF|110|
|IS-IS|115|
|RIP|120|
|External EIGRP|170|
|Internal BGP|200|
|Unknown source|255 (untrusted)|