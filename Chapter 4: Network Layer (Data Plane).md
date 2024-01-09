# Chapter 4: Network Layer (Data Plane)

## 4.1 Overview

The network layer's primary role is to move packets from a sending host to a receiving host. Two key functions are involved:

- **Forwarding:**

    > Forwarding refers to the router-local action of transferring a packet from an input link interface to the appropriate output link interface.

  - Moves packets from a router's `input link` to the appropriate `output link`.
  - The primary function in the data plane.
  - Implemented in hardware.
  
- **Routing:**
    > Routing refers to the network-wide process that determines the end-to-end paths that packets take from source to destination. 

  - Determines the route or path packets take from sender to receiver.
  - Implemented in the control plane.
  - Routing algorithms calculate these paths.
  - Implemented in software

- A router's forwarding table is crucial for packet forwarding. It indexes header values to determine the outgoing link interface.
- `Software-Defined Networking` (SDN) separates the control plane from the router. A remote controller computes and distributes forwarding tables to routers. SDN allows for open, software-based implementations.

## 4.2 Router Architecture

#### Components of a Generic Router:

1. **Input Ports:**
   - Terminate incoming physical links.
   - Perform link-layer functions for interoperability.
   - Conduct a lookup function to determine the output port using the forwarding table.
   - Forward control packets (e.g., carrying routing protocol information) to the routing processor.
   - The number of ports varies, from a few in enterprise routers to hundreds in ISP edge routers.

2. **Switching Fabric:**
   - Connects input ports to output ports.
   - Completely contained within the router.
   - a network inside of a network router!

3. **Output Ports:**
   - Store and transmit packets received from the switching fabric.
   - Conduct link-layer and physical-layer functions.
   - Paired with input ports for bidirectional links.

4. **Routing Processor:**
   - Performs control-plane functions.
   - In traditional routers, executes routing protocols, maintains routing tables, and computes forwarding tables.
   - In SDN routers, communicates with the remote controller, receives forwarding table entries, and performs network management functions.
   - Operates at millisecond or second timescales.

<img src="https://lh3.googleusercontent.com/pw/ADCreHdojxc4ewnh58kAQUrzO6NtwOoEEm69fVwJ7RpsndlMvEb5BTNSvChBSXYZrCqd5AOJwDCbNe9dMgebE2AoJU6ffdfqM39fTHTNzn10HW8GZiBi9iGEGSwB_oCQDh4WDxQ_f8euMQKXI8LI5WcaNEt-=w1497-h927-s-no?authuser=2" width="900" height="600">

### 4.2.1 Input Port Processing and Destination-Based Forwarding

#### Forwarding Table Management:

- The forwarding table is either computed and updated by the routing processor, employing a routing protocol to interact with other routers.
- Alternatively, the table may be received from a remote SDN controller.
- A shadow copy at each line card allows local forwarding decisions, reducing the need for centralized processing on a per-packet basis.

#### Handling Scale: Prefix Matching Example

- Brute-force implementation with one entry for every possible destination address is impractical due to scale.
- `Prefix matching` is introduced as an efficient alternative.
- For instance, a forwarding table might look like this for a specific case:

  | Prefix                                | Link Interface  |
  |---------------------------------------|-----------------|
  | 11001000 00010111 00010               | 0               |
  | 11001000 00010111 00011000            | 1               |
  | 11001000 00010111 00011               | 2               |
  | Otherwise                             | 3               |

- The router matches a packet's destination address with the prefixes in the table.
- `Longest prefix matching` rule is used when multiple matches occur.

#### Fast Lookup Challenges:

- At Gigabit transmission rates, lookup must be performed in nanoseconds.
- Hardware implementation is essential, and techniques beyond a simple linear search are required.
- Memory access times are critical, leading to designs with embedded on-chip DRAM and faster SRAM.

#### Input Port Processing:

1. **Match:**
   - Packet's output port determined through lookup.
   - Longest prefix matching rule applied.
   
2. **Action:**
   - Packet sent into the switching fabric.
   - Temporary blocking possible if fabric is currently in use by other input ports.
   - Blocked packets queued at the input port and scheduled for later transmission.

3. **Additional Actions:**
   - Physical- and link-layer processing.
   - Checking packet's version number, checksum, and time-to-live field.
   - Updating counters for network management.

### 4.2.2 Switching Fabric in Router Architecture

The switching fabric serves as the core of a router, facilitating the actual switching (or forwarding) of packets from input ports to output ports. Different methods of switching are used, each influencing the overall performance and throughput.

#### 1. Switching via Memory:

  - Early routers, acting as traditional computers, controlled switching via the CPU (routing processor).
  - Input ports signaled the CPU through interrupts.
  - Packets were copied into processor memory, and the CPU handled destination address lookup and forwarding table processing.
  - Limited forwarding throughput due to shared system bus constraints.

#### 2. Switching via a Bus:

  - Input port transfers a packet directly to the output port over a shared bus without routing processor involvement.
  - A switch-internal label indicates the local output port.
  - Limited by bus speed; only one packet can cross the bus at a time.
  - Common in small local area and enterprise networks.

#### 3. Switching via an Interconnection Network:

  - Uses a sophisticated interconnection network, e.g., a crossbar switch.
  - Crossbar switch consists of 2N buses connecting N input ports to N output ports.
  - Crosspoints controlled by a switch fabric controller, enabling `non-blocking` and `parallel forwarding`.


### 4.2.3 Output Port Processing

- Output port processing involves transmitting packets stored in the output port's memory over the output link.
- Tasks include packet selection (scheduling), de-queuing for transmission, and executing link-layer and physical-layer functions.

#### Input Queueing

- Minimal queuing at input ports when the switching fabric (Rswitch) is N times faster than the line speed (Rline).
- If Rswitch is not fast enough, packet queues form at input ports, causing `head-of-the-line (HOL) blocking` and potential packet loss.

#### Output Queueing

- Even with a fast switching fabric, output port queues may form if packets from all N input ports are destined for the same output port.
- Queued packets can overwhelm the output port's memory, leading to potential packet loss.

#### How Much Buffering Is "Enough?"

- Traditional rule: Buffering (B) equals average round-trip time (RTT) times link capacity (C), i.e., 
   ```B = RTT * C```.
- Modern Rule: B = `RTT C (2N)^1/2`
- Optimal buffer size is a nuanced consideration, balancing decreased packet loss with increased queueing delays.
- Active queue management (AQM) algorithms, like `Random Early Detection (RED)`, help manage buffer dynamics.

#### Bufferbloat and Complexities

- `Bufferbloat` refers to persistent buffering, showcasing the complexity of managing queues.
- Interaction among senders at the network edge and queues within the network can be subtle.

### 4.2.4 Packet Scheduling

<img src="https://lh3.googleusercontent.com/pw/ADCreHcFLP8aO7hejw7Bz_Pi6SDL4Mn_JCH-uqICUcpReaA3zqp-zmofVtV7jExfIroysPj2720oVq_PGQbRkTk7Y02KAYeLl4FzrAJEU22PuhYmyQpY0GKGamDgPNIctL-cHeb23Fzgf3f0BFlZ7a3l3MKi=w1920-h386-s-no?authuser=2" width="900" height="200">

#### Comparison Table

| Discipline            | Description                                | Operation                                                     |
|-----------------------|--------------------------------------------|---------------------------------------------------------------|
| **FIFO**               | Queuing in arrival order; packets leave in the same order they arrived. | Selection based on arrival order; removal after transmission |
| **Priority Queuing**  | Classifies packets into priority classes; serves higher-priority packets first. | Highest priority class with nonempty queue served first      |
| **Round Robin**       | Alternates service among classes; each class served in a circular manner. | Round-robin service pattern                                   |
| **Weighted Fair Queuing (WFQ)** | Generalized round robin with differential service based on weights assigned to each class. | Service based on weighted round-robin pattern                |

## 4.3. IP Addressing

### 4.3.1 IPv4

<img src="https://lh3.googleusercontent.com/pw/ADCreHduJMEOpHZpP3FXB5m6nzbxOzzFUvu3eIBqzrcg70qXQVH4SKc-2Qbq4wvAh4pdCtO7VINEji6qQvC4e944DTm_96sKkCR6Q7PAcOYEKjfzdBDW-eOpdQWuM_EO1jmIKQDPMVqeVflTjRWBuH7DN8Gc=w1352-h948-s-no?authuser=2" width="600" height="400">

| Field                   | Description                                                                                                         |
|-------------------------|---------------------------------------------------------------------------------------------------------------------|
| **Version number**      | Specifies the IP protocol version of the datagram (IPv4 or IPv6).                                                      |
| **Header length**       | Determines the start of the payload in the IP datagram; typically 20 bytes for most datagrams.                        |
| **Type of service**     | Bits used to distinguish different types of IP datagrams based on the required level of service.                      |
| **Datagram length**     | Total length of the IP datagram (header plus data) in bytes; maximum theoretical size is 65,535 bytes.                 |
| **Identifier, flags, fragmentation offset** | Fields related to IP fragmentation, where large datagrams are broken into smaller fragments for transmission.        |
| **Time-to-live**        | Ensures datagrams do not circulate indefinitely; decremented by routers, and dropped if it reaches 0.                  |
| **Protocol**            | Indicates the specific transport-layer protocol for the data portion of the IP datagram (e.g., TCP, UDP).             |
| **Header checksum**     | Aids in detecting bit errors in the received IP datagram; computed using 1s complement arithmetic.                    |
| **Source and destination IP addresses** | Source inserts its IP address, and destination's address is determined, usually via DNS lookup.                    |
| **Options**             | Allow IP header extension for rare use cases, but omitted in IPv6 for simplicity.                                      |
| **Data (payload)**      | Contains the transport-layer segment (e.g., TCP or UDP) or other data, such as ICMP messages.                         |

*Note: An IP datagram typically has a 20-byte header (excluding options). If carrying a TCP segment, each datagram includes a total of 40 bytes of header (20 bytes of IP header plus 20 bytes of TCP header) along with the application-layer message.*

### 4.3.2 CIDR (Classless Interdomain Routing)

CIDR is the Internet's address assignment strategy, allowing the division of IP addresses into two parts, a network portion and a device portion. This approach improves address allocation efficiency.

- The network portion is indicated by the x most significant bits of the address (prefix), reducing the size of forwarding tables in routers.
- The remaining 32-x bits distinguish devices within the organization.
- CIDR allows flexible subnetting, avoiding the constraints of classful addressing (class A, B, C networks).


### 4.3.3 Dynamic Host Configuration Protocol (DHCP)

The Internet Corporation for Assigned Names and Numbers (ICANN) manages IP addresses globally. It allocates addresses to regional Internet registries, which further manage addresses within their regions.

After obtaining a block of addresses, an organization assigns individual IP addresses to host and router interfaces. 
Dynamic Host Configuration Protocol (DHCP) is commonly used for automatic host address assignment.

#### DHCP Process:

1. **DHCP Server Discovery:**
   - The host sends a DHCP discover message to find a DHCP server.
   - The message is broadcast to all nodes on the subnet using IP broadcast.
   > creates an IP datagram containing its DHCP discover message along with the broadcast destination IP address of 255.255.255.255 and a “this host” source IP address of 0.0.0.0. 

2. **DHCP Server Offer(s):**
   - DHCP server(s) respond with offer messages.
   - Offers include the proposed IP address, network mask, and lease time.

3. **DHCP Request:**
   - The client chooses an offer and responds with a DHCP request.

4. **DHCP ACK:**
   - The server acknowledges the request with a DHCP ACK message.
   - The client can use the allocated IP address for the lease duration.

DHCP's plug-and-play capability makes it efficient for network administrators, especially in scenarios with frequent host mobility.

> If no server is present on the subnet, a DHCP relay agent (typi- cally a router) that knows the address of a DHCP server for that network is needed.

### 4.3.4 Network Address Translation (NAT)

- Network Address Translation (NAT) provides a simpler approach to address allocation, especially in scenarios like SOHO networks.

- The addressing within the home network remains as a private network with addresses like 10.0.0.0/24, reserved for private realms. NAT hides the details of the home network from the outside world.

- **Private Address Realm:** The home network computers use private addresses (e.g., 10.0.0.0/24), which are meaningful only within the given network.
- **Single IP Address to the World:** The NAT router represents the home network to the external world with a single IP address (e.g., 138.76.29.7).
- **DHCP Usage:** The router often uses DHCP to get its address from the ISP, and it runs a DHCP server for internal network devices.

> NAT routers use a translation table to keep track of internal hosts and their corresponding ports. When a datagram from an internal host is forwarded to the WAN, the NAT router performs address and port translation, updating the source IP address and port.

### 4.3.5 IPv6

<img src="https://lh3.googleusercontent.com/pw/ADCreHfHs4kPC6QGsL7q-0yCrpp5_QZddnjOJtMVac9UDP0gLPW-DFpAMthbUFsFBDydYEPNwrvqZbClMvtNo-zJotLMOuk_cyi5ozrgos5acXsOVtBJGzUF3cMEvpjBJXqn1NYfOvKmjC-ub6uaXf_t0ONs=w1522-h856-s-no?authuser=2" width="600" height="400">

| Field             | Size (bits) | Description                                                                                                              |
|-------------------|-------------|--------------------------------------------------------------------------------------------------------------------------|
| Version           | 4           | Identifies the IP version number. IPv6 carries a value of 6 in this field.                                                |
| Traffic Class     | 8           | Similar to the TOS field in IPv4, it can prioritize datagrams within a flow or from specific applications.               |
| Flow Label        | 20          | Identifies a flow of datagrams, allowing special handling for quality of service or real-time service.                    |
| Payload Length    | 16          | Unsigned integer indicating the number of bytes in the IPv6 datagram following the fixed-length 40-byte header.          |
| Next Header       | 8           | Identifies the protocol to which the datagram's contents will be delivered (e.g., TCP or UDP).                             |
| Hop Limit         | 8           | Decremented by each forwarding router; if it reaches zero, the datagram is discarded.                                    |
| Source Address    | 128         | IPv6 128-bit address format.                                                                                             |
| Destination Address | 128       | IPv6 128-bit address format.                                                                                             |
| Data              | Variable    | Payload portion of the IPv6 datagram, passed on to the protocol specified in the Next Header field.                      |
| Fragmentation/Reassembly | N/A | IPv6 does not allow fragmentation and reassembly at intermediate routers. "Packet Too Big" ICMP error is sent if needed. |
| Header Checksum   | N/A         | Removed in IPv6, as checksumming is performed by transport and link-layer protocols.                                      |
| Options           | Variable    | No longer part of the standard IP header; included as one of the possible next headers, resulting in a fixed-length header.|

#### Transitioning from IPv4 to IPv6

The most widely adopted approach for IPv4-to-IPv6 transition involves **tunneling**. Tunneling is a concept applicable in diverse scenarios, including all-IP cellular networks. In tunneling, if two IPv6 nodes (e.g., B and E) want to communicate but are connected through intervening IPv4 routers, a tunnel is established.


<img src="https://lh3.googleusercontent.com/pw/ADCreHd2h_OChNHQc4WNwCHNijHEFo39w0IatpNIbkEBUimj97YnwrCBPSOhDnyipU0dBtwJ81Kw_nqllRnhZINyMzV0eZ24O1Vd7PJ5FrLdzOpAq0lJJdHL85DDlWIlejWaHrol4lMMiRTlz35FP31xPwx3=w1442-h1094-s-no?authuser=2" width="600" height="400">

- The IPv6 node on the sending side (B) encapsulates the entire IPv6 datagram in the payload of an IPv4 datagram.
- The IPv4 datagram is addressed to the IPv6 node on the receiving side (E) and sent into the tunnel.
- Intervening IPv4 routers route the IPv4 datagram, unaware that it contains a complete IPv6 datagram.
- The receiving IPv6 node extracts the IPv6 datagram and routes it as if received directly.


[Yet To Complete]
- Match-Plus-Action
- Middleboxes
