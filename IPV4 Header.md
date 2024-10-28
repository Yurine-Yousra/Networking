
### IPv4 Header Structure

1. **Version (4 bits)**: Indicates the IP version. For IPv4, this is `0100`; for IPv6, it’s `0110`.

2. **IHL (Internet Header Length) (4 bits)**: Specifies the length of the IPv4 header in 4-byte increments. 
   - Minimum value: 5 (20 bytes)
   - Maximum value: 15 (60 bytes)
   - If IHL = 5, the options field is empty.

3. **DSCP (Differentiated Services Code Point) (6 bits)**: Used for Quality of Service (QoS) to prioritize traffic, such as delay-sensitive data (e.g., voice streaming).

4. **ECN (Explicit Congestion Notification) (2 bits)**: Signals potential congestion without dropping packets, providing end-to-end notification of network conditions.

5. **Total Length (16 bits)**: Indicates the total length of the IP packet (header + data) in bytes.
   - Minimum: 20 bytes (header only)
   - Maximum: 65,535 bytes (all 16 bits set to 1).

6. **Identification (16 bits)**: Used to identify fragments of a packet if fragmentation occurs. All fragments of the same packet share the same identification value.

7. **Flags (3 bits)**:
   - Bit 0: Reserved (always 0)
   - Bit 1: Don’t Fragment (DF)
   - Bit 2: More Fragments (MF) – set to 1 if more fragments follow.

8. **Fragment Offset (13 bits)**: Indicates the position of the fragment within the original unfragmented packet, allowing for reassembly of fragments that may arrive out of order.

9. **TTL (Time to Live) (8 bits)**: Specifies the maximum number of hops a packet can take before being discarded, preventing endless circulation in the network. Each router decrements the TTL by 1.

10. **Protocol (8 bits)**: Indicates the protocol used in the transport layer (L4):
    - 6: TCP
    - 17: UDP
    - 1: ICMP
    - 89: OSPF (dynamic routing protocol)

11. **Header Checksum (16 bits)**: A checksum to check for errors in the IPv4 header. If it does not match upon arrival, the packet is dropped.

12. **Source IP and Destination IP (32 bits each)**: Both fields contain the source and destination IP addresses.

13. **Options Field**: Length varies (0 to 320 bits). Present if IHL > 5. Rarely used, it can include options like security, timestamp, and record route.

This structure is essential for the proper functioning of IP networks, facilitating routing, fragmentation, and error-checking processes. Let me know if you need any more details or explanations!