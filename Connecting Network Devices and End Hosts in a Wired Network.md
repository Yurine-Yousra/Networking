

To connect network devices and end hosts in a wired network, we use cables between the interfaces of each device.

## Ethernet Interfaces/Cables

- **Ethernet Interface/Cables:** 10 Mb/s
- **Fast Ethernet Interface/Cables:** 10/100 Mb/s
- **Gigabit Ethernet Interface/Cables:** 10/100/1000 Mb/s
- **10 Gig Ethernet/Cables:** 10/1000/1000 Mb/s (10 Gb/s)

All cables have a maximum length of 100 meters. One of the most popular Ethernet interfaces is RJ-45 (Registered Jack).

## What Is Ethernet?

Ethernet is a collection of network protocols and standards.

### What Is a Protocol?

A protocol is a set of rules and steps that allow two or more network devices to communicate with each other.

### IEEE

IEEE stands for the Institute of Electrical and Electronics Engineers.

## Copper UTP Connections (Sending Electrical Signals)

### UTP Cables (Unshielded Twisted Pair)

- **Unshielded:** The pairs do not have a metallic shield, making them vulnerable to electrical interference.
- **Twisted Pair:** Protects against electromagnetic interference.

Ethernet cables and Fast Ethernet cables use all 2 pairs (4 pins). Gigabit cables and 10 Gigabit Ethernet cables use all pairs (4 pairs and 8 pins).

### UTP Cables (10BASE-T / 100BASE-T)

- A PC interface transmits (Tx) data on pins 1 & 2 and receives (Rx) data on pins 3 & 6.
- A router interface transmits (Tx) data on pins 1 & 2 and receives (Rx) data on pins 3 & 6.
- A firewall interface transmits (Tx) data on pins 1 & 2 and receives (Rx) data on pins 3 & 6.
- A switch interface transmits (Tx) data on pins 3 & 6 and receives (Rx) data on pins 1 & 2.

### Straight-Through Cabling

To connect a PC and a switch, we use straight-through cabling (pin M connects to pin M). It is full duplex, meaning devices can send and receive data simultaneously without collisions, as separate pairs are used for receiving and transmitting data for each device. Using straight-through cabling, we can connect a PC to a switch, a PC to a router, or a router to a switch. However, we cannot connect a PC to a PC, a router to a router, or a switch to a switch because they transmit and receive data on the same pins at both ends, which can cause data collisions. This is where we use crossover cabling.

### Crossover Cabling

To connect a switch to another switch, two PCs, two routers, or a router to a PC, we use crossover cabling. It is called crossover because pin M is not connected to pin M. For example, to connect a PC to a router, the PC transmits data on pins 1 & 2 (Tx) and the router receives it on pins 3 & 6 (Rx). Similarly, the router transmits on pins 1 & 2 (Tx) while the PC receives on pins 3 & 6 (Rx).

### Auto MDI-X

In newer devices, this technology is involved. For example, if two switches are connected, one device can detect which pins the other device is using for transmitting and receiving, adjusting its pins accordingly. If Switch 1 Tx is at 3 & 6 and Rx at 1 & 2, and Switch 2 is the same, Switch 1 will detect that Switch 2 is Tx at 3 & 6 and Rx at 1 & 2, and it will adjust its pins to Tx at 1 & 2 and Rx at 3 & 6.

### UTP Cables (1000BASE-T / 10GBASE-T)

All pairs are used, and each pair is bidirectional, allowing devices to receive and transmit data on the same pins, which is why they are faster.

## Fiber Optic Connections (Sending Light Over Glass Fibers)

### SFP Transceiver (Small Form-Factor Pluggable)

SFP transceivers are used to connect fiber-optic cables. At each end, there are Tx and Rx.

### Types of Fiber-Optic Cables

- **Multimode Cables:**
    
    - Core diameter is wider than single-mode fiber.
    - Allows multiple angles of light waves to enter the fiber glass core.
    - Allows longer cable lengths than UTP but shorter than single-mode fiber-optic cables.
    - Cheaper than single-mode fiber because it uses less expensive LED-based SFP transmitters.
- **Single Mode Cables:**
    
    - Core diameter is narrower than multimode.
    - Light enters at a single angle from a laser-based transmitter.
    - Allows longer cable lengths.
    - More expensive than multimode cables. 