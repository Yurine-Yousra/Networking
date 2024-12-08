

## **Broadcast Domain**

- A **broadcast domain** is a group of devices that receive a broadcast frame (destination MAC address `FF:FF:FF:FF:FF:FF`) sent by any member of the domain.

---

## **Problems Resolved by VLANs**

1. **Performance**: Reduces unnecessary broadcast traffic, thereby improving network performance.
2. **Security**: Ensures data is sent only to devices within the specified VLAN.

---

## **Default VLANs**

- VLANs **1, 1002, 1003, 1004, and 1005** exist by default on a switch and **cannot be deleted**.

---

## **Types of Switch Ports**

- **Access Port**: A switch port that belongs to a single VLAN. Typically connects to end devices like PCs.
- **Trunk Port**: A switch port that carries traffic for multiple VLANs.

---

## **Trunking Protocols**

### 1. **ISL (Inter-Switch Link)**

- Cisco proprietary protocol.
- Developed before industry standards like IEEE 802.1Q.

### 2. **IEEE 802.1Q**

- Industry-standard protocol.
- Adds a **4-byte VLAN tag** between the source MAC address and type field in the Ethernet header.

---

## **Structure of the IEEE 802.1Q Tag**

- **Total Length**: 4 bytes
- **Fields**:
    1. **TPID (Tag Protocol Identifier)**:
        - 16 bits.
        - Set to `0x8100`, indicating a 802.1Q-tagged frame.
    2. **TCI (Tag Control Information)**:
        - 16 bits, divided into three subfields:
            - **PCP (Priority Code Point)**:
                - 3 bits.
                - Used for class of service (CoS), prioritizing important traffic.
            - **DEI (Drop Eligible Indicator)**:
                - 1 bit.
                - Indicates whether a frame can be dropped during network congestion.
            - **VID (VLAN ID)**:
                - 12 bits.
                - Identifies the VLAN to which the frame belongs.
                - **Valid VLAN IDs**: 1–4094 (IDs 0 and 4095 are reserved).

---

## **VLAN Ranges**

1. **Normal VLANs**: 1–1005
2. **Extended VLANs**: 1006–4094
    - Note: Some older devices may not support the extended VLAN range, but modern switches generally do.

---

## **Native VLAN**

- **Default VLAN**: VLAN 1 on all trunk ports.
- Frames in the **native VLAN** are **not tagged** with an 802.1Q header.
- Can be manually reconfigured using:
    
    ```
    switchport trunk native vlan <vlan-id>
    ```
    

---

## **Router on a Stick (ROS)**

- **Purpose**: Enables inter-VLAN routing using a single physical interface on the router.
- **Method**: The physical interface is divided into multiple logical subinterfaces, each configured with:
    - **VLAN Tag**
    - **IP Address**

### **Configuration Example**

```plaintext
interface g0/0
switchport mode trunk

interface g0/0.10
encapsulation dot1q 10
ip address 192.168.10.1 255.255.255.0

interface g0/0.20
encapsulation dot1q 20
ip address 192.168.20.1 255.255.255.0
```