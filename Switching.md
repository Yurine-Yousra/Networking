# Ethernet Frame Structure

## Preamble
- **Length:** 7 bytes  
- **Description:** Alternating 1s and 0s (10101010 repeated 7 times)  
- **Purpose:** Allows devices to synchronize their receiver clocks.

## Start Frame Delimiter (SFD)
- **Length:** 1 byte  
- **Value:** 10101011  
- **Purpose:** Marks the end of the preamble and the beginning of the frame.

## MAC Addresses
- **Source MAC Address:** Indicates the physical address of the sending device (Length: 6 bytes).  
- **Destination MAC Address:** Indicates the physical address of the receiving device (Length: 6 bytes).  

## Type/Length Field
- **Length:** 2 bytes  
- **Description:** 
  - A value of 1500 or less indicates the length of the encapsulated data.
  - A value of 1536 or greater indicates the type of encapsulated packet (e.g., IPv4 or IPv6). The length is determined via other methods.
  - **IPv4:** 0x800 (2048 in decimal) indicates that this field is a type field, not a length field.
  - **IPv6:** 0x86DD (34525 in decimal) indicates that this field is also a type field.

## Frame Check Sequence (FCS)
- **Length:** 4 bytes  
- **Purpose:** Detects corrupted data by running the CRC algorithm over the received data.  
- **Note:** CRC stands for cyclic redundancy check.

## MAC Addresses
- **Length:** 6 bytes  
- **Description:** A physical address assigned to a device when manufactured.  
- **Uniqueness:** Globally unique.
- **Structure:** 
  - First 3 bytes: Organizationally Unique Identifier (OUI) assigned to the manufacturer.
  - Last 3 bytes: Unique to the individual device.

## Types of Frames
- **Unicast Frame:** A frame destined for a single target (e.g., PC M to PC N).  
- **Multicast Frame:** A frame destined for multiple targets in a network (e.g., PC M to several devices).  
- **Broadcast Frame:** A frame destined for all devices in the network.

## Updating the MAC Table of a Switch
- Each device connects to an interface on the switch.
- When a frame is sent, the MAC address of the sending device is stored in the switch’s MAC table with the appropriate interface.
- If the destination MAC address is unknown, the frame is flooded to all interfaces (except the one it was received on).
- Devices with the same MAC address as the destination will accept the frame; others will drop it.

## Flooding
- **Description:** Sending a frame across all interfaces of a switch except the interface it was received from because the destination MAC address is unknown.

## Forwarding
- **Description:** Sending a frame to a specific device because its MAC address is stored in the MAC address table of the switch.

## Frame Types
- **Unknown Unicast Frame:** A frame where the destination device's MAC address is not in the MAC address table.
- **Known Unicast Frame:** A frame where the destination MAC address is known by the switch, so it does not need to flood the frame.