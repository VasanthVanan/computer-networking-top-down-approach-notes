# Chapter 6: Link Layer and LANs

## 6.1 Link Layer

The Link Layer, part of the OSI model's Layer 2, provides communication between devices in a local network. It includes protocols like Ethernet (IEEE 802.3), Wi-Fi (IEEE 802.11), PPP, HDLC, and ARP, managing tasks like framing, link access(MAC), reliable delivery, and error detection.

- Link layer is implemented on a chip called the network adapter or `network interface controller (NIC)`.
- Integrated into the motherboard chipset or implemented via a dedicated Ethernet chip.
- Implemented in hardware, handling framing, link access, error detection, etc.

## 6.2 Error Detection & Correction Techniques

### 6.2.1 Parity Checks

- `Even Parity:` Ensures that the total number of 1s in a data unit (including the parity bit) is even.
  - If an odd number of bits are in error, the parity check will detect it.
- `Odd Parity:`
  - Requires the total number of 1s to be odd.
- `Forward Error Correction (FEC)` is an error correction technique where extra bits are added to transmitted data, allowing the receiver to correct errors without retransmission.

### 6.2.2 Checksumming Methods

- Checksums involve summing up the values of a set of data units and appending the result to the data being transmitted.
- **Checksum Calculation:**
  - Sender calculates a checksum value based on the data.
  - Sender sends both the data and the checksum.
  - Receiver calculates its checksum on the received data.
  - If the calculated checksum mismatches the received checksum, an error is detected.
- See more on: [Checksums in UDP](https://github.com/VasanthVanan/computer-networking-top-down-approach-notes/blob/main/Chapter%203%3A%20Transport%20Layer.md#35-udp-user-datagram-protocol)

### 6.2.3 Cyclic Redundancy Check (CRC)

- CRC is a more sophisticated error-checking technique.
- **Polynomial Division:**
  - Data is treated as coefficients of a polynomial.
  - Division is performed using a predetermined divisor polynomial.
  - Remainder is appended to the data for transmission.
- **Error Detection:**
  - Receiver performs the same division and checks the remainder.
  - If the remainder is not zero, an error is detected.

## 6.3 Multiple Access Links and Protocols

|                | **Point-to-Point Links** | **Broadcast Links**                            |
|----------------|--------------------------------|------------------------------------------------------|
| **Characteristics** | Connects two devices directly. Dedicated communication link between them. | Single communication channel shared by multiple devices. Data sent by one device is received by all others. |
| **Link Access Control** | Since it's a point-to-point link, there's no contention for the link. No need for a MAC protocol for multiple access. | Requires a MAC protocol to coordinate access among multiple devices. Avoids collisions and ensures fair access. |
| **Efficiency** | Efficient use of bandwidth since only two devices share the link. Full capacity available to each device. | Bandwidth is shared among multiple devices. Efficiency depends on the effectiveness of the MAC protocol. |
| **Examples** | Traditional telephone lines and Point-to-point leased lines. | Ethernet networks (using CSMA/CD or CSMA/CA protocols). Wireless LANs (using protocols like Wi-Fi). |


* Classification of multiple access protocols into three categories: 
  - channel partitioning, 
  - random access, and 
  - taking-turns (controlled access).

### 6.3.1 Channel Partitioning Protocols

#### Time-Division Multiplexing (TDM)

- TDM divides time into time frames and further divides each time frame into N time slots. 
- Each time slot is then assigned to one of the N nodes. TDM allows one node to speak for a fixed period, then another node speaks, and so on. 
- This ensures fairness, but has drawbacks. See Also [Circuit Switching](https://github.com/VasanthVanan/computer-networking-top-down-approach-notes/blob/main/Chapter%201%3A%20Computer%20Networks%20and%20the%20Internet.md#17-circuit-switching).

#### Frequency-Division Multiplexing (FDM)

- FDM divides the R bps channel into different frequencies and assigns each frequency to one of the N nodes, creating N smaller channels of R/N bps. 
- While avoiding collisions, FDM limits a node to a bandwidth of R/N, even when it's the only active node.

#### Code Division Multiple Access (CDMA)

- CDMA assigns a different code to each node. Nodes use their unique codes to encode data bits. 
- CDMA allows simultaneous transmissions with minimal interference. 

### 6.3.2 Random Access Protocols

In a random access protocol, a transmitting node always transmits at the full rate of the channel (R bps). Collisions are handled by retransmitting frames after a random delay.

#### Pure ALOHA

- The first ALOHA protocol was unslotted and fully decentralized. 
- Nodes immediately transmit frames into the channel and waits for acknowledgement. 
- If ACK not received, node waits for random backoff time and re-sends data.
- If a collision occurs, nodes retransmit with a probability. Efficiency is less than slotted ALOHA. 
- If a first bit of a new frame overlaps with the last bit of a nearly finished frame, both frames will be completely destroyed and must be retransmitted later.

  <img src="https://www.myreadingroom.co.in/images/stories/docs/dcn/aloha%20Protocols_Pure%20aloha.JPG" width="60%" height="auto">

#### Slotted ALOHA

- A simple random access protocol where time is divided into time-slots. 
- Nodes are allowed to transmit at slot beginnings. Collisions are detected, and nodes retransmit with a probability. 
- If a station misses out the allowed time, it must wait for the next slot. This reduces the probability of collisions.
- Efficiency is determined by the long-run fraction of successful slots.

  <img src="https://www.myreadingroom.co.in/images/stories/docs/dcn/aloha%20Protocols_Slotted%20aloha.JPG" width="60%" height="auto">


#### Carrier Sense Multiple Access (CSMA)

- CSMA protocols introduce `carrier sensing` to listen before transmitting and `collision detection` to stop transmitting if interference is detected. 
- The possibility of collision still exists because of propagation delay
- Types of CSMA:
  - 1-Persistent CSMA: If station is busy, it will **CONTINUOUSLY** sense the medium and transmit the data again. [Probability=1]
  - Non-persistent CSMA: If station is busy, it will wait for a random time and **RANDOMLY** sense the medium before transmitting again.
  - p-Persistent CSMA: It applies to slotted channels; If station is idle, it it will **CONTINUOUSLY** sense and waits for the slotted time to transmits the data  [Probability=p]

    <img src="https://lh3.googleusercontent.com/pw/ABLVV85KjpQ0qYdwG8uIEdt8Cc9gmX_OuSDW8miulqzMWK6G4hWCye5msmonru0zsxMhdGGPvPA5v2BLjAbqQ8wdiKADibiq3uBky0cCGSA7HyDAtNAiEGnhGPQEKmwed5E1vho35A8BWl89Evxu11cV7nOY=w792-h912-s-no?authuser=1" width="60%" height="auto">

#### CSMA/CD

- CSMA/CD is a variant of CSMA that introduces collision detection.
- **Transmission Period**: The time required for a node to transmit a frame.
- **Idle Period**: The time required for a node to wait before transmitting again.
- **Contention Period**: The time required for a node to sense the medium before transmitting again. 
- **Binary Exponential Backoff**: It uses binary exponential backoff algorithm to handle collisions. 
- Nodes choose a random value from an increasing set for waiting after collisions.
- When a collision is detected, the transmitting nodes immediately stop transmitting and enter a backoff state.


#### CSMA/CA

- Similar to CSMA/CD, CSMA/CA begins with carrier sensing. Nodes listen to the channel to determine if it's busy or idle.
- CSMA/CA introduces virtual carrier sensing to estimate the channel's status. 
- CSMA/CA uses a contention window, which is a range of time during which a node may choose to transmit after sensing the channel is idle.
- The size of the contention window may vary, and it influences the probability of a successful transmission.

### 6.3.3 Taking Turns Protocols (Controlled Access)

#### Reservation

- A station need to make a reservation before sending data.
- In each interval, a reservation frame precedes the data frames sent in that interval.
- If there are N stations in the system, there are exactly N reservation minislots in the reservation frame.
- When a station needs to send a data frame, it makes a reservation in its own minislot.
- The stations that have made reservations can send their data frames after the reservation frame. 

  <img src="https://lh3.googleusercontent.com/pw/ABLVV87UZqcqWAnjvjtJEv4aOyT7zCUGjsXN3IpnbYXROTCMHMKk0MYs_I4YY3UiacY0VLciUZX3UFWBZeQos1iVeAI13UpJi5mpykVtukwb8KrtujF3js3tJY1g-KeteIAkCjYeT6bwj8-N--2Av-TrdgCx=w1920-h422-s-no?authuser=1" width="60%" height="auto">

#### Polling

- The polling protocol requires one of the nodes to be designated as a Master node (Primary station).
- The master node polls each of the nodes in a round-robin fashion.
- The master node can determine when a node has finished sending its frames by observing the lack of a signal on the channel.
- `Functions`: `Poll` (receive) and `Select` (send) functions
- `Drawback`: Polling Delay; If master node fails, channel will be inoperative.

#### Token Passing

- The token passing protocol is similar to polling, but it uses a token to determine when a node has finished sending its frames.
- Decentralised and highly efficient; (No Master Node)
- A small, special-purpose frame known as a token is exchanged among the nodes in some fixed order.
- When a node receives a token, it holds onto the token only if it has some frames to transmit; otherwise, it immediately forwards the token to the next node.
- `Examples`: Physical, Dual, Star, and Bus Ring.

## 6.4 Link Layer Addressing

### 6.4.1 Media Access Control (MAC) Addresses

  - MAC addresses have a flat structure and remain constant for an adapter, analogous to a social security number.
  - Typically 6 bytes (48 bits) long (for most LANs) and expressed in hexadecimal notation.
  - Originally designed to be permanent, but now changeable via software.
  - Managed by IEEE to ensure uniqueness; Companies purchase a chunk of address space (224 addresses) for manufacturing adapters.

### 6.4.2 Address Resolution Protocol (ARP)

  - ARP resolves IP addresses to MAC addresses (between Network and Link-Layer Addresses).
  - Hosts and routers maintain ARP tables mapping IP addresses to MAC addresses.
  - ARP query packet used to obtain MAC address for a given IP address on the same subnet.
  - Plug-and-play; ARP tables get built automatically.
  - ARP table also contains a time-to-live (TTL) value, which indi- cates when each mapping will be deleted from the table. 

    <img src="https://ipcisco.com/wp-content/uploads/2018/10/arp-packet-format-ipcisco.jpg" width="60%" height="auto">

    > A MAC address table, sometimes called a Content Addressable Memory (CAM) table, is used on Ethernet switches to determine where to forward traffic on a LAN.

    | Feature                  | CAM Table                                    | ARP Table                                    |
    |--------------------------|----------------------------------------------|----------------------------------------------|
    | **Purpose**              | Manages MAC (Media Access Control) addresses  | Resolves IP addresses to MAC addresses       |
    | **Function**             | Stores MAC addresses associated with ports   | Stores IP addresses and corresponding MAC addresses |
    | **Scope**                | Limited to the local network or VLAN          | Network-wide, may involve broadcast messages |
    | **Storage Type**          | Typically stored in hardware (CAM)           | Stored in software, often in the OS's ARP cache |
    | **Security Implications** | Vulnerable to MAC address spoofing            | Can be exploited for ARP spoofing attacks    |
    | **Examples**              | Cisco switches, Ethernet switches             | Operating systems (Windows, Linux, etc.)    |



**Sending a Datagram off the Subnet**

  - When a host on a subnet wants to send a datagram to another subnet, ARP is still used.
  - Router interfaces play a crucial role in forwarding the datagram.
  - The destination MAC address is the router interface's MAC address, not the final destination's MAC address.
  - ARP is used to obtain the MAC address of the first-hop router.
  - Router consults a forwarding table to determine the correct interface for forwarding the datagram.
  - ARP is used again to obtain the MAC address of the ultimate destination.

    <img src="https://lh3.googleusercontent.com/pw/ABLVV85ip3uwpFb-_CdsLD7jkOiO3qB8xndr_7sjDFGBl1Bc0LMva75FMgNGtshNuxoLYOVNC1QtRRlhdZS5VMZEwma62vSR_VMyF30dCzCORAaGmYF6Xqzp4pixg7nXi0klaFi1QfMvewAaR9duSc9X1jNU=w1098-h298-s-no?authuser=1" width="60%" height="auto">

### 6.4.3 Ethernet Frame Structure

An Ethernet frame consists of the following components:

1. **Data field (46 to 1,500 bytes):** Carries the IP datagram, with a maximum transmission unit (MTU) of 1,500 bytes.
2. **Destination address (6 bytes):** MAC address of the destination adapter.
3. **Source address (6 bytes):** MAC address of the transmitting adapter.
4. **Type field (2 bytes):** Permits multiplexing of network-layer protocols.
5. **Cyclic redundancy check (CRC) (4 bytes):** Detects bit errors in the frame.
6. **Preamble (8 bytes):** Synchronizes clocks between adapters.


## 6.5 Link Layer Switches

- The primary role of a switch is to receive incoming link-layer frames and forward them to outgoing links. Switches employ buffers in output interfaces to handle potential rate mismatches.
- Switches perform two key functions: `filtering` and `forwarding`. 
- Filtering determines whether a frame should be dropped or forwarded, while forwarding identifies the interfaces to which a frame should be directed. 
- This is achieved through a switch table, containing MAC addresses, associated interfaces, and entry timestamps. Unlike routers, switches forward based on MAC addresses, not IP addresses.

When a frame arrives:
- If no entry exists for the destination address, the switch broadcasts the frame.
- If there's an entry for the destination on the same interface, the frame is discarded.
- If there's an entry for a different interface, the frame is forwarded to that interface.

**Advantages of Switches**
- `Elimination of Collisions`: Switches prevent collisions, maximizing bandwidth.
- `Heterogeneous Links`: Different LAN links can operate at varying speeds and media types.
- `Management`: Switches offer enhanced security and ease of network management.

**Self-Learning Capability**

Switches possess the self-learning property, automatically building and updating their switch tables without manual intervention. The process involves recording MAC addresses, associated interfaces, and timestamps for each incoming frame. Entries are removed after a specified aging time, ensuring the switch table remains accurate.

## 6.6 Virtual Local Area Networks (VLANs)

- Hosts within a VLAN communicate as if connected directly to the switch, creating broadcast domains within each VLAN. 
- VLANs effectively isolate traffic, optimize switch utilization, and simplify user management.

In a port-based VLAN,
- Switch ports are grouped into VLANs by the network manager.
- Each VLAN forms a broadcast domain, isolating broadcast traffic within the VLAN.

**VLAN Switch Configuration**

- The network manager configures port-to-VLAN mappings using switch management software. 
- Switch hardware delivers frames only between ports belonging to the same VLAN. 
- However, this introduces a challenge: How can traffic from one VLAN be sent to another?

**VLAN Trunking**

- To address inter-VLAN communication, VLAN trunking was introduced. A trunk port on each switch, configured to belong to all VLANs, facilitates VLAN interconnection. Trunk links forward frames between switches, ensuring effective communication.

- The IEEE standard 802.1Q defines VLAN trunking. A special Ethernet frame format includes a four-byte VLAN tag in the header, carrying the VLAN identity. The VLAN tag contains a Tag Protocol Identifier (TPID) field, Tag Control Information field with a VLAN identifier, and a priority field.

  <img src="https://lh3.googleusercontent.com/pw/ABLVV86GRpSCpPwDC_6jYwJMrRJN_qv1J9ibbzlXHiLnhqUDrzYnys_mi0UWgCl23AiOvD6pj5uSwmq2vfZCEwBnR6W4jHveYtPYUuApve3oEhqSQwx2EgrTUDipODFighaOcZSCm8vYj43Tc-rwjNgelxXX=w1070-h326-s-no?authuser=1" width="60%" height="auto">

## 6.7 Multiprotocol Label Switching (MPLS)

MPLS enhances IP router's forwarding speed by incorporating a key concept from virtual-circuit networks: `fixed-length labels`. Unlike circuit-switched networks, MPLS is a packet-switched, virtual-circuit network, co-existing with IP infrastructure.

- An MPLS-capable router adds a small MPLS header to a link-layer frame between MPLS-capable devices. This header includes fields like the `label`, `experimental bits`, an S bit indicating the end of a stacked header series, and a `time-to-live` field.
- MPLS-capable routers, known as `label-switched` routers, forward MPLS frames by looking up labels in their forwarding tables, eliminating the need to extract IP addresses for routing decisions.
- MPLS labels and their associations with IP destinations are distributed among MPLS-capable routers. 
- MPLS introduces traffic engineering capabilities, allowing routers to forward packets along paths not determined by standard IP routing protocols. This enables network operators to override normal IP routing for specific traffic management purposes.
- MPLS can be utilized for fast restoration of forwarding paths, implementing virtual private networks (VPNs), and various other purposes.

  <img src="https://lh3.googleusercontent.com/pw/ABLVV85_ezMEMThiI57Cz0Nl6TJ9fqu5Hy_LhUBUIBekDQIscXCMhItu_f5nWHDMaVJbf-jawTk7U1O373NdGqgU4NAKtSjzFaJUXNmikDk12nLxgQlGLXK2n3IPH1zcOhw5aw9MjKkbRu5CfDZmn1Lp_XJe=w908-h558-s-no?authuser=1" width="60%" height="auto">
