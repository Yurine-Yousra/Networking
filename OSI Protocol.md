# What Is a Networking Model?

Networking models categorize and provide a structure for networking protocols and standards.

### Protocols
Protocols are a set of rules defining how network devices and software should work.
### Networking Protocols
Networking protocols are a set of rules that define how network devices should communicate between each other.

## OSI Model

- **Open Systems Interconnection Model**: A conceptual framework that categorizes and standardizes the different functions in a network.
- **Created by**: International Organization for Standardization (ISO).
- **Layers**: Functions are divided into seven layers.

### OSI Model Layers

#### Application Layer
- Closest to the end user.
- Interacts with software applications, such as web browsers.

#### Presentation Layer
- Translates data from the application layer into a network format for transmission over the network.

#### Session Layer
- Controls sessions between communicating hosts.
- Establishes, manages, and terminates connections between local applications (e.g., web browsers) and remote applications (e.g., YouTube).

#### Transport Layer
- Segments and reassembles data for communication between end hosts.
- Breaks larger pieces of data into smaller segments, making them easier to transmit and reducing the likelihood of transmission errors.
- Adds a Layer 4 header to the segmented data.

#### Network Layer
- Provides connections between end hosts on different networks (when devices are not on the same LAN).
- Routers operate at Layer 3 of the OSI model.
- Adds a Layer 3 header, and the segment here is called a packet.

#### Data Link Layer
- Adds a Layer 2 header and trailer to the packet, resulting in a frame.
- Detects and possibly corrects physical layer errors.
- Uses Layer 2 addressing, which is separate from Layer 3 addressing.
- Switches operate at Layer 2.

#### Physical Layer
- Converts the bits of the frame into electrical signals (for copper cabling) or light signals (for fiber optic cabling).

### Protocol Data Units (PDUs)
- **Layer 4 PDU**: Segment (data + Layer 4 header)
- **Layer 3 PDU**: Packet (data + Layer 4 header + Layer 3 header)
- **Layer 2 PDU**: Frame (data + Layer 4 header + Layer 3 header + Layer 2 header + Layer 2 trailer)

## TCP/IP Suite

The TCP/IP suite is a conceptual model that structures networking protocols. It is known as TCP/IP because these are the foundational protocols of the suite, developed by the United States Department of Defense.

### TCP/IP Suite Layers
- **Application Layer**: Equivalent to the Application, Presentation, and Session layers in the OSI model.
- **Transport Layer**: Corresponds to the Transport layer of the OSI model.
- **Internet Layer**: Corresponds to the Network layer of the OSI model.
- **Link Layer**: Combines the Data Link and Physical layers of the OSI model.