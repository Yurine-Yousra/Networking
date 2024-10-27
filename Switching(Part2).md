
---

In fact, the preamble and the SFD (Start Frame Delimiter) are not considered part of the Ethernet header. The minimum size of an Ethernet frame is 64 bytes (Ethernet header + payload + Ethernet trailer). Since the preamble and SFD are not part of the Ethernet header, the size of the Ethernet header is 6 + 6 + 2 = 14 bytes. The Ethernet trailer is 4 bytes, leading to a total of 18 bytes for the header and trailer combined. Therefore, the size of a packet (payload) is equal to 64 - 18 = 46 bytes (the minimum size of a payload).

A payload consists of the packet received from Layer 3 (data + L4 header + L3 header). If the size of the payload is less than 46 bytes, padding bytes (a sequence of zeros) are added.

**ARP Protocol (Address Resolution Protocol)**

ARP is a protocol that finds the appropriate MAC address of a given known IP address. When we have a frame to be sent with the source MAC address but no destination MAC address, we can use both the source IP address and the destination IP address. By running the ARP protocol, we can determine the destination MAC address using the destination IP address.

**ARP Protocol Request**

An ARP request contains the IP source address, IP destination address, and MAC source address. This request is sent over all the interfaces of the switch (an unknown unicast frame). The switch will add the MAC address of the source machine to its MAC address table. If the IP address of the device matches the IP address of the destination in the ARP request, the MAC address of that device will be added to the ARP request.

**ARP Protocol Response**

After adding the destination MAC address to the ARP request, a response will be sent to the source device (the one that sent the ARP request) from the destination device. Therefore, its MAC address will be added to the MAC address table of the switch. Since the switch has already learned the MAC address of the source device, the response will not be flooded; instead, it will be directly forwarded to the source device. This indicates that the given destination IP address corresponds to the returned destination MAC address. After this, we will be able to send frames from the source device to the destination device.

--- 

