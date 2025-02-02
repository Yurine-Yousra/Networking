

### General Information

- Initially a Cisco proprietary protocol, but now published openly for use on other equipment.
- Considered an advanced distance vector routing protocol.
- Reacts much faster to network changes compared to RIP.
- Does not have the 15-hop count limit of RIP.
- Sends messages using the multicast address **224.0.0.10**.
- The only IGP capable of unequal-cost load balancing by default.

### Equal-Cost Multi-Path (ECMP) Load Balancing

- EIGRP, like RIP, performs ECMP load-balancing over four paths by default.
- Example:
    - A destination can be reached through four different routes learned by the same dynamic routing protocol (e.g., RIP).
    - If two of these routes have the same smallest metric, they will be added to the routing table.
    - The router will load balance traffic over these two routes.
    - The default number of paths that can perfomr ECMP can be modified it can vary from 1-32(i should add the command to change the default number of paths )
- With EIGRP:
    - You can configure the router to load balance traffic to a destination even if routes have different metrics.
    - In this case the traffic will not be load balanced but it will be unequally load balenced
    - Traffic can be proportionally load balanced based on bandwidth. Routes with lower metrics will carry more traffic compared to routes with higher metrics.

---

### Wild Card Masks

#### Definition

- A **/K mask** is written as 1s _K_ times, with the remaining bits as 0.
- A **/K wild card mask** is written as 0s _K_ times, with the remaining bits as 1.

#### Example:

- **/16 Mask**: `11111111.11111111.00000000.00000000`
- **/16 Wild Card Mask**: `00000000.00000000.11111111.11111111`

#### Functionality of Wild Card Masks

- A `0` in the wild card mask indicates **must match** (between the router's interface IP addresses and the EIGRP network command).
- A `1` in the wild card mask indicates **does not have to match** (between the router's interface IP addresses and the EIGRP network command).

#### Example:

- **Command**: `network 172.16.1.0 0.0.0.15` (a /28 wild card mask)
    - Any interface on the router with the first 28 bits matching the first 28 bits of the network entered will have EIGRP activated on it.

---

### EIGRP Configuration

#### Step-by-Step Configuration

1. **Start EIGRP:**
    
    ```
    R1(config)#router eigrp 1
    ```
    
    - `1` is the autonomous system number (must be the same across all routers in the same AS for EIGRP adjacency).
2. **Disable Auto-Summary:**
    
    ```
    R1(config-router)#no auto-summary
    ```
    
    - Auto-summary should be disabled by default, but verify and disable it if enabled.(it depends on the IOS of the Router)
3. **Passive Interface Configuration:**
    
    ```
    R1(config-router)#passive-interface G0/2
    ```
    
    - Stops G0/2 from advertising the routing table of R1 because it does not have any EIGRP neighbors.
4. **Network Commands:**
    
    ```
    R1(config-router)#network 10.0.0.0
    ```
    
    - A classful command that activates EIGRP on all router interfaces with the prefix `10` in the first octet.
    
    ```
    R1(config-router)#network 172.16.1.0 0.0.0.15
    ```
    
    - A /28 wild card mask that enables EIGRP on interfaces with the first 28 bits matching the specified network.
    
    ```
    R1(config-router)#network 172.16.1.0 0.0.0.0
    ```
    
    - A wild card mask of `0.0.0.0` specifies that all bits must match, enabling EIGRP only on the interface with the exact network address.

---

### EIGRP Router-ID

#### Viewing EIGRP Information

- Use the command:
    
    ```
    R1#show ip protocols
    ```
    
- This will generate the EIGRP output.

---

### Notes:

- Insert the screenshot of the EIGRP output here to complete the explanation.
- Ensure all routers in the autonomous system have consistent configuration to establish adjacencies and share route information.
- i should add a screenshot of the eigrp output command