

---

### Switches

- **Interfaces**: 
  - Switches typically have between **24 to 48 RJ-45 interfaces** and around **4 SFP interfaces**.
  
- **LAN Formation**:
  - Devices connected via the same switch are considered part of the same **Local Area Network (LAN)**.
  - If we have **Switch 1** and **Switch 2**, each with its own connected devices, and both switches are connected to a router, the devices on Switch 1 form **LAN 1**, while those on Switch 2 form **LAN 2**.
  - If Switch 1 and Switch 2 are connected by a cable (one end to an interface on Switch 1 and the other end to an interface on Switch 2), the connected devices across both switches are considered part of the same LAN.

### Duplex Modes

- **Half Duplex**: 
  - The device cannot send and receive data at the same time. If it is receiving a frame, it must wait before sending another frame.

- **Full Duplex**: 
  - The device can send and receive data simultaneously, without having to wait.

### Hubs

- **Overview**: 
  - A hub is a basic networking device used to connect multiple devices in a **Local Area Network (LAN)**. 
  - It operates at the **physical layer** (Layer 1) of the OSI model, primarily receiving data packets from one device and broadcasting them to all other devices connected to it.

- **Collision Domain**: 
  - Hubs create a single collision domain, meaning only one device can transmit data at a time. If multiple devices send data simultaneously, collisions occur, leading to retransmissions and delays.
  
- **Collision Avoidance**: 
  - To mitigate collision issues in a network using a hub, the **CSMA/CD** (Carrier Sense Multiple Access with Collision Detection) algorithm is employed.
  - **CSMA/CA** (Carrier Sense Multiple Access with Collision Avoidance) is another approach, typically used in wireless networks.

- **CSMA/CD Algorithm Steps**:
  1. Before sending a frame, the device listens to the collision domain to detect if other devices are transmitting.
  2. If it detects a transmission, it chooses an instant **T** from a range \([0, N]\).
  3. If two devices select the same **T** (e.g., both choose **T = 2**), the range extends to \([0, M]\) where **M > N**, and both devices choose again from the new range.
  4. If a collision occurs, the device sends a **jamming signal** to inform other devices that a collision has happened.
  5. The process repeats until successful transmission occurs.

### Speed Auto-Negotiation

- When a device connects to an interface on a switch, the device and the interface negotiate the transmission speed and duplex mode (half/full).
  
- **Capabilities Advertising**: 
  - Interfaces advertise their capabilities to the device, allowing negotiation about duplex and speed.

- **Disabled Auto-Negotiation**: 
  - If auto-negotiation is disabled, the switch attempts to detect the device's operating speed. If this fails, it defaults to the slowest speed:
    - **Ethernet**: 10 Mbps
    - **Fast Ethernet**: 10/100 Mbps (defaulting to 10 Mbps)
    - **Gigabit Ethernet**: 10/100/1000 Mbps (also defaulting to 10 Mbps)

- **Duplex Modes**:
  - Speed **10/100 Mbps**: Half Duplex
  - Speed **1000 Mbps**: Full Duplex

--- 

