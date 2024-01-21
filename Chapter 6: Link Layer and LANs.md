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

  <img src="https://www.myreadingroom.co.in/images/stories/docs/dcn/aloha%20Protocols_Pure%20aloha.JPG" width="50%" height="auto">

#### Slotted ALOHA

- A simple random access protocol where time is divided into time-slots. 
- Nodes are allowed to transmit at slot beginnings. Collisions are detected, and nodes retransmit with a probability. 
- If a station misses out the allowed time, it must wait for the next slot. This reduces the probability of collisions.
- Efficiency is determined by the long-run fraction of successful slots.

  <img src="https://www.myreadingroom.co.in/images/stories/docs/dcn/aloha%20Protocols_Slotted%20aloha.JPG" width="50%" height="auto">


#### Carrier Sense Multiple Access (CSMA)

- CSMA protocols introduce `carrier sensing` to listen before transmitting and `collision detection` to stop transmitting if interference is detected. 
- The possibility of collision still exists because of propagation delay
- Types of CSMA:
  - 1-Persistent CSMA: If station is busy, it will **CONTINUOUSLY** sense the medium and transmit the data again. [Probability=1]
  - Non-persistent CSMA: If station is busy, it will wait for a random time and **RANDOMLY** sense the medium before transmitting again.
  - p-Persistent CSMA: It applies to slotted channels; If station is idle, it it will **CONTINUOUSLY** sense and waits for the slotted time to transmits the data  [Probability=p]

    <img src="https://data-flair.training/blogs/wp-content/uploads/sites/2/2021/12/o-persistent.webp" width="30%" height="auto">

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

