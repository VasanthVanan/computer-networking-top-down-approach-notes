# Chapter 3: Transport Layer


> transport layer -- extending the network layer’s delivery service between two end systems to a delivery service between two application layer processes running on the end systems.


## 3.1 Transport-Layer Services

- The transport layer resides between the application and network layers, providing communication services to application processes on different hosts.
- Transport layer protocols enable `logical communication` between application processes on different hosts, abstracting the physical infrastructure.
- It converts application messages into transport layer `segments`, encapsulated within network layer packets (datagrams) for transmission.
- Transport protocols work only within end systems and are not involved in routing or network core activities.
- These protocol provides communication between application **processes**, while network layer protocol provides communication between **hosts**.

-  IP makes its “best effort” to deliver segments between communicating hosts, but it makes no guarantees. (IP is said to be an unreliable service)

> The most fundamental responsibility of UDP and TCP is to extend IP’s delivery service between two end systems to a delivery service between two processes running on the end systems. Extending host-to-host delivery to process-to-process delivery is called transport layer multiplexing and demultiplexing.


- Key Differences in UDP vs TCP Protocols:

| Aspect                      | UDP (User Datagram Protocol)        | TCP (Transmission Control Protocol)      |
|-----------------------------|----------------------------------|-----------------------------------------|
| Service Model               | Unreliable and connectionless    | Reliable and connection-oriented         |
| Application Selection       | Chosen by the application developer | Chosen by the application developer    |
| Terminology                | Uses "datagram" for segments     | Uses "segment" for segments             |
| Network Layer Protocol     | Operates on top of IP (Internet Protocol) | Operates on top of IP (Internet Protocol) |
| Services Provided          | Process to process data delivery and error checking | Reliable data transfer, flow control, sequence numbers, acknowledgments, timers, and congestion control |
| Reliability                 | Unreliable - Does not guarantee data integrity or delivery | Reliable - Ensures data delivery, integrity, and order |
| Congestion Control         | Unregulated - No congestion control | Regulated - Prevents excessive traffic and aims for fair sharing of network resources |
| Complexity                  | Simpler and less complex         | Complex due to additional services and mechanisms |

## 3.2 Multiplexing and Demultiplexing

- **Objective**: Extend host-to-host delivery to process-to-process delivery for applications.

### Demultiplexing

- Transport layer delivers data to an intermediary socket, not directly to a process.
- Each socket has a unique identifier. Fields in a transport layer segment are used to identify these receiving socket.
- Demultiplexing directs the segment to the corresponding socket, ensuring data is delivered to the correct process.
- In the household analogy, this is similar to handing out mail to the right person based on the address.

### Multiplexing

- Multiplexing involves gathering data from different sockets, encapsulating it with header information to create segments, and passing the segments to the network layer.
- The transport layer in intermediate hosts performs both demultiplexing and multiplexing.
- Sockets have unique identifiers. Special fields in segments identify the socket for delivery.
- These fields include `source port number` and `destination port number`.
- Port numbers are 16 bit numbers, ranging from 0 to 65535. The well known port numbers (0 to 1023) are reserved for established application protocols.


## 3.3 Connectionless Multiplexing and Demultiplexing

- UDP sockets are automatically assigned port numbers in the range 1024 to 65535.
- Servers typically assign specific port numbers, while clients often let the transport layer assign them.
- Alternatively, a specific port number can be associated with a UDP socket using the `bind()` method.

```python
clientSocket = socket(AF_INET, SOCK_DGRAM)
clientSocket.bind((’’, 19157))
```

- UDP segments have source and destination port numbers for identification.
- A socket is fully identified by a `destination IP address` and a `destination port number`.
- Segments are directed to the corresponding socket based on the destination port number.
- If different segments have the same destination IP and destination port numbers, they go to the same destination process.
- The source port number in a UDP segment serves as part of the **"return address"**. It allows the recipient to send a response back to the sender.


> A-to-B segment the source port number serves as part of a “return address”—when B wants to send a segment back to A, the destination port in the B-to-A segment will take its value from the source port value of the A-to-B segment. 


## 3.4 Connection Oriented Multiplexing and Demultiplexing

- TCP sockets are identified by a four tuple: (source IP address, source port number, destination IP address, destination port number).
- A connection establishment request includes destination and source port numbers.
- Host operating systems use these values to locate the server process waiting for connections.
- Server and client sockets are established and identified with four tuple values.
- All subsequent segments are demultiplexed based on these four values to the correct socket.

<img src="https://lh3.googleusercontent.com/pw/ADCreHcmMPDSQ4fOctkpXLDYANE30KxCaIIeiv4cNkwy7qP5yrIkfrHwx0vMaMeRo7aSooqns-GiXxoULVwt7bLenIiyAi49A2ZMf4zAg82q2jzIWVFuD6Sx19TF--PFa21b_2xfQVsyIR7oNZDQWG4Vv5S_=w1920-h1060-s-no" width="580" height="350">

- Web servers may have multiple processes or threads to handle connections.
- Each process or thread has its connection socket for receiving HTTP requests and sending responses.
- High performing servers typically use one process and multiple threads or lightweight subprocesses.
- One process can have many connection sockets, each with different identifiers.

### Persistent vs. Non Persistent HTTP

- Persistent HTTP exchanges messages over the same server socket during a connection.
- Non persistent HTTP creates and closes a new TCP connection and socket for every request/response.
- Frequent socket creation and closure can impact the performance of busy Web servers.

## 3.5 UDP (User Datagram Protocol)

- UDP is a minimalistic transport protocol. It provides `multiplexing`/`demultiplexing` and minimal `error checking`. Unlike TCP, it adds very little to IP.

> If the application developer chooses UDP instead of TCP, then the application is almost directly talking with IP layer. UDP takes messages from the application process, attaches source and destination port number fields for the multiplexing/demultiplexing service, adds two other small fields, and passes the resulting segment to the network layer. 

- `Connectionless`: No handshaking between sender and receiver. Often used for `real time`, `low delay`, and `low overhead` applications.
- Some applications are better suited for UDP
  - Allows `finer control over data sent and timing`.
  - `No connection establishment`, minimizing delays.
  - `No connection state,` supporting more active clients.
  - `Small packet header` overhead (UDP's 8 bytes vs. TCP's 20 bytes).
- Reliability can be added to applications using UDP like QUIC protocol, but it's nontrivial. It can be built directly into the application.
- Allows reliable communication without TCP's congestion control limitations.
- Key fields in the UDP header:
  - `Port numbers`: For demultiplexing.
  - `Length`: Specifies the segment length (header + data).
  - `Checksum`: Used for error detection.
- Checks for alterations during data transmission.

> UDP at the sender side performs the 1s complement of the sum of all the 16 bit words in the segment, with any overflow encountered during the sum being wrapped around.

<img src="https://lh3.googleusercontent.com/pw/ADCreHd6aGmd6Jk9aEfaOuteJHdj1V8Tl3M-w_eAdWjDwwqOUuDR8hobSh1fjb2PyblDTqlzm8bLz72502F7YGVJF2x5nirudlaEPwHHrIdqbkWzDpvPUb6K3jccQN5bxYKKsbptwUDTjAnvI0uhICFYGpRv=w1286-h1130-s-no" width="550" height="500">

- Thus, the 1s complement of the sum `0100101011000010` is `1011010100111101`,

> If no errors are introduced into the packet, then clearly the sum at the receiver will be 1111111111111111. If one of the bits is a 0, then we know that errors have been introduced into the packet.

> It is useful for the transport layer to provide error checking as a safety measure. Although UDP provides error checking, it does not do anything to recover from an error. Some implementations of UDP simply discard the damaged segment; others pass the damaged segment to the application with a warning.

## 3.6 Building Reliable Data Transfer Portocol

### 3.6.1 RDT 1.0:
- **Basic Version:** RDT 1.0 is the most basic version of the Reliable Data Transfer protocol.
- **Key Characteristics:**
  - Sender sends data to the receiver.
  - Receiver simply accepts the data without providing feedback. (unidirectional communication)
  - Assumes a perfectly reliable channel where data is never lost or corrupted.
  - No error detection or correction mechanisms in place.

### 3.6.2 RDT 2.0:
- **Enhanced Reliability:** RDT 2.0 is an enhanced version of the RDT 1.0 protocol.
- **Key Characteristics:**
  - Introduces a basic acknowledgment mechanism.
  - Sender sends data and waits for an `acknowledgment` (`ACK` / `NAK`) from the receiver.
  - Receiver sends an ACK to confirm successful data reception.
  - If ACK is not received, sender retransmits the data.
  - Addresses the issue of lost or corrupted data and ensures basic reliability.

  > The message dictation protocol uses both positive acknowledgments (“OK”) and negative acknowledgments (“Please repeat that.”). These control messages allow the receiver to let the sender know what has been received correctly, and what has been received in error and thus requires repeating. It is known as `ARQ (Automatic Repeat reQuest)` protocols.

  > when the sender is in the wait for ACK or NAK state, it cannot get more data from the upper layer; that will happen only after the sender receives an ACK and leaves this state. This is known as `Stop and Wait` protocol.

### 3.6.3 RDT 2.1:
- **Extended Reliability:** RDT 2.1 further improves upon reliability.
- **Key Characteristics:**
  - Adds a `sequence number` to each frame sent by the sender.
  - Receiver identifies duplicate frames and discards them.
  - If the receiver receives a frame with the wrong sequence number, it discards it.
  - This prevents duplicate frames from being delivered and enhances reliability.

### 3.6.4 RDT 3.0:
- **Enhanced Error Handling:** RDT 3.0 focuses on error handling and retransmission.
- **Key Characteristics:**
  - Similar to RDT 2.1, it uses sequence numbers to handle duplicate frames.
  - Introduces a `timeout mechanism`.
  - If the receiver doesn't receive an expected frame within a certain time (timeout), it requests retransmission.
  - Sender retransmits the missing frame.
  - Adds the ability to recover from lost frames more efficiently.
  - RDT 3.0 is a functionally correct protocol but has performance limitations due to its stop and wait behavior.

- **Performance Example:**
  - Consider two hosts on the opposite coasts of the United States with a round trip propagation delay (RTT) of 30 milliseconds.
  - The transmission rate (R) is 1 Gbps, and the packet size (L) is 1,000 bytes.
  - The time needed to transmit a packet into the link is 8 microseconds (dtrans).
  - In a stop and wait scenario, the sender utilizes the channel very inefficiently.
  - Only 0.00027 of the sender's time is spent sending data into the channel, resulting in a low effective throughput, even on a high capacity link.

- **Introducing Pipelining:**
  - The stop and wait protocol's performance is poor due to its sender utilization. It can severely limit the capabilities of high-capacity network links.
  - The sender can improve utilization by transmitting multiple packets before waiting for acknowledgments (`pipelining`).
  - This allows for a more efficient use of the channel and effectively increases sender utilization.
  - Introducing pipelining requires several changes:
    - Expanding the `range of sequence numbers` to account for multiple in transit packets.
    - Both sender and receiver may need to `buffer multiple packets`.
  - Two basic approaches for pipelined error recovery are `Go Back N` and `selective repeat`.

### 3.6.5 Go-Back-N (GBN)

Animation: [Go-Back-N ARQ](https://www2.tkn.tu-berlin.de/teaching/rn/animations/gbn_sr/)

> In a Go-Back-N (GBN) protocol, the sender is allowed to transmit multiple packets (when available) without waiting for an acknowledgment, but is constrained to have no more than some maximum allowable number, N, of unacknowledged packets in the pipeline. 


- **Sender Behavior in GBN:**
  - The window slides forward over the sequence number space, making N the window size.
  - N is referred to as the window size, and GBN is considered a sliding-window protocol.
  - `Flow control` and `congestion control` are some of the reasons for limiting the number of unacknowledged packets to N.

<img src="https://lh3.googleusercontent.com/pw/ADCreHf6riQ0xiX1Y8x0IaZhnnaUd6EYEaffccwXfQry67IIKawSascIKFG6Md8mSFxQh_g33BxvYHcKuYAEE2U9AlpXm-r64b5UlepEhonQYufEGA4w63KF1rhgAlaSNo4jzfsUfamxAi-tT4kk0VtgR3Cj=w1920-h440-s-no" width="750" height="200">

- **Sliding Window Behaviour**
  - Sequence numbers between `0` and `'base-1'` are for sent and acknowledged packets.
  - `'base'` to `'nextseqnum-1'` represents sent but unacknowledged packets.
  - Sequence numbers from `'nextseqnum'` to `'base+N-1'` are available for sending when data arrives.
  - Sequence numbers beyond `'base+N'` can't be used until unacknowledged packets are acknowledged.
  - GBN receiver acknowledges correctly received packets and discards out of order packets.

- **Sender's Actions:**
  - The sender must respond to three types of events: invocation from above, receipt of an ACK, and a timeout event.
  - The sender checks if the window is full before sending a packet. If the window is full, data is returned to the upper layer.
  - If there are lost or delayed packets, a timer is used to recover them. The timer for the oldest transmitted but unacknowledged packet is managed.

- **Receiver's Actions:**
  - The receiver handles correctly received and in-order packets by sending an ACK and delivering the data to the upper layer.
  - Out-of-order packets are discarded, as the receiver must deliver data in order.
  - Throwing away out-of-order packets simplifies receiver buffering.

### 3.6.6 Selective Repeat (SR)

Animation: [Selective Repeat ARQ](https://www2.tkn.tu-berlin.de/teaching/rn/animations/gbn_sr/)

- **Performance Issues with GBN:**
  - GBN allows the sender to fill the pipeline with packets but can suffer from performance problems, especially when the window size and bandwidth-delay product are large.
  - A single packet error can trigger unnecessary retransmissions, potentially filling the pipeline with these redundant packets.
  - Selective-repeat protocols aim to avoid unnecessary retransmissions by having the sender retransmit only suspected lost or corrupted packets.
  - In SR, the sender can receive ACKs for some packets in the window, unlike GBN.

- **SR Sender Events and Actions:**
  1. **Data Received:** Send data if it's within the window, otherwise buffer or return it.
  2. **Timeout:** Use individual timers for packet retransmission.
  3. **ACK Received:** Update the window and transmit new in-window packets.

- **SR Receiver Events and Actions:**
  1. **Packet Received:** Send selective ACKs and deliver consecutive in-window packets to the upper layer.
  2. **Special Packet:** Generate ACK for correctly received packets outside the window.
  3. **Other Cases:** Ignore packets not fitting the above scenarios.

- **Synchronization and Implications:**
  - SR protocols lack synchronization between sender and receiver windows.
  - The finite range of sequence numbers can lead to consequences in scenarios involving packet reordering.

### 3.6.7 Improvements in RDT Protocols 

| Mechanism          | Purpose                                            | Comments |
|--------------------|----------------------------------------------------|-----------|
| Checksum           | Detect bit errors in transmitted packets.         | -       |
| Timer              | Timeout for packet retransmission due to lost packets. | Duplicate packets may occur due to premature timeouts or lost ACKs. |
| Sequence number    | Sequential numbering for packet order and lost packet detection. | Gaps indicate lost packets; duplicates detect duplicate packets. |
| Acknowledgment     | Confirms correct receipt of packets.              | Can be individual or cumulative, depending on the protocol. |
| Negative acknowledgment (NAK) | Notifies sender about incorrectly received packets. | Typically carries the sequence number of the problematic packet. |
| Window and Pipelining | Increases sender utilization by allowing multiple unacknowledged packets in the pipeline. | Window size is determined by the receiver's capacity and network congestion. |


## 3.7 TCP (Transmission Control Protocol)

- **Full-Duplex and Point-to-Point:**
  - TCP connections provide a full-duplex service, allowing data to flow in both directions simultaneously.
  - Each TCP connection is between a single sender and a single receiver, and multicasting is not supported.

- **Data Transfer in TCP:**

<img src="https://lh3.googleusercontent.com/pw/ADCreHftJQQO-Aubels0acw3xS3UmGDa2n1Fnh3DkQRsy122XtlIjFenjTHSv7amS_EDWg84DBERoOp7h6XmPOM9vuNdynFKwDSoA-Tv0pS-TnhEmjybGHv3AM-q4Z0H5GYIX47JiNbCeSzjYUORJBXy3sU=w1920-h808-s-no" width="550" height="300">

  - Application-layer data flows from the client process to the server process.
  - Data is passed through the connection's `send buffer` during data transmission.
  - TCP controls when data is sent based on its convenience, and the `Maximum Segment Size (MSS)` limits data size.

    > MSS is the maximum amount of application-layer data in the segment, not the maximum size of the TCP segment including headers.

    > To fit within a single link-layer frame, set the MSS, considering a typical 40-byte TCP/IP header, to 1460 bytes, as Ethernet and PPP have an MTU of 1500 bytes.
  
  - Each data chunk is encapsulated in a TCP header to form TCP segments and further encapsulated in IP datagrams for network transmission.
  - The receiver places the received data into the connection's `receive buffer`, and the application reads the data from there.

- **TCP Segment Structure:**
  - A TCP segment consists of `header` fields and a `data` field.
  - Key fields include `source port` and `destination port`, `checksum`, `sequence number`, `acknowledgment number`, `receive window`, `header length`, and `flags`.
  - An options field can be included for features like maximum segment size (MSS) negotiation and window scaling.
  
  <img src="https://lh3.googleusercontent.com/pw/ADCreHcSNDV-yVd1BdDU6Nj4ezCL_1_VjcuUWaPSqFQBBYShSAwrC9jlbpw_LlvY8oITjDuOttW31BPxO9ayFockAc8Z9RjoVpQB9ai8_Sem2JD7fyJrLw32MmKF15kWA5VdEYtQSpshhgcUYFYip4ICJUY=w1406-h1130-s-no" width="530" height="400">


- **Sequence Numbers and Acknowledgment Numbers:**
  - Sequence numbers are assigned to bytes in the data stream and are included in the segment header.
  - Acknowledgment numbers represent the `next expected byte` in the opposite direction of data flow.
  - TCP uses `cumulative acknowledgments`, acknowledging the last byte received.
  - Handling out-of-order segments is left to TCP implementation and may involve discarding or waiting for missing bytes.


### 3.7.1 Round-Trip Time Estimation and Timeout in TCP

- **Round-Trip Time Estimation:**
  - TCP uses round-trip time (RTT) estimation to determine the time between sending a segment and receiving an acknowledgment.
  - The sample RTT (`SampleRTT`) is measured for a single segment at a time, typically for one of the `unacknowledged segments`.
  - The SampleRTT is computed as the time between sending the segment and receiving an acknowledgment.
  - Multiple SampleRTT values may fluctuate due to network and load variations.
  - TCP calculates an average RTT, `EstimatedRTT`, using an *exponentially weighted moving average (EWMA)* formula:

    ```
    EstimatedRTT = (1 - alpha) * EstimatedRTT + alpha * SampleRTT (alpha = 0.125)
    ```
  - EstimatedRTT gives more weight to recent samples, reflecting current network conditions.

- **RTT Variation Measurement:**
  - TCP also measures RTT variation (`DevRTT`) to estimate how much SampleRTT deviates from EstimatedRTT.
  - DevRTT is calculated as an EWMA of the difference between `SampleRTT` and `EstimatedRTT`.
  
    ```
    DevRTT = (1 – beta) * DevRTT + beta * | SampleRTT – EstimatedRTT |
    ```

- **Setting the Retransmission Timeout Interval:**
  - The retransmission timeout (TimeoutInterval) should be greater than or equal to EstimatedRTT to avoid unnecessary retransmissions.
  - It shouldn't be much larger than EstimatedRTT to avoid delays when retransmitting lost segments.
  - DevRTT plays a role in determining the timeout interval:
    ```
    TimeoutInterval = EstimatedRTT + 4 * DevRTT
    ```
  - An initial TimeoutInterval of 1 second is recommended. After a timeout, the TimeoutInterval is doubled to prevent premature timeouts for subsequent segments, but it's recomputed based on the formula afterward.
  

### 3.7.2 Reliable Data Transfer

- **TCP Timer Management:**
  - An individual timer for each unacknowledged segment is conceptually simple but can have considerable overhead.
  - Recommended TCP timer management uses only a single retransmission timer, even for multiple unacknowledged segments.
  - TCP reliable data transfer is described in two steps: `timeout-based recovery` and recovery using `duplicate acknowledgments`.

- **Timeout and Retransmission Handling:**

  1. **Data Received from Application Above:**
      - A TCP segment is created with a sequence number.
      - If the timer is not running, start the timer. The timer is associated with the oldest unacknowledged segment.
      - Pass the segment to IP.
      - Timer expiration interval is `TimeoutInterval`, calculated from `EstimatedRTT` and `DevRTT`.

  2. **Timeout Event:**
      - Retransmit the unacknowledged segment with the smallest sequence number.
      - Restart the timer.

  3. **ACK Received:**
      - On ACK with field value y, compare y with SendBase.
      - Update SendBase if y > SendBase.
      - If any unacknowledged segments remain, start the timer.

    <img src="https://lh3.googleusercontent.com/pw/ADCreHfd0XeL8ZXe8ewMBM-9XEzLjEQHHXXBQSAGJXOiPCYUlRUDfrbo1FCzGP5NLdxRoCmTgjI1auHjGNDFbkm3fxXuNQ7cv7k65Ha4DrT7hCo_r_rpKPo1vxmCad40jF1o7lAbrDf-0DV7-9xmkbekxu8=w1920-h616-s-no?authuser=2" width="800" height="300">

- **Scenarios:**
  - `Duplicate ACKs` can trigger a `fast retransmit` (retransmission before timeout).
  - TCP employs exponential back-off for timer intervals.
  - The sender can often detect packet loss before a timeout by observing duplicate ACKs.

  > A duplicate ACK is an ACK that reac- knowledges a segment for which the sender has already received an earlier acknowl- edgment.

  > If the TCP sender receives three duplicate ACKs for the same data, it takes this as an indication that the segment following the segment that has been ACKed three times has been lost. Then, the TCP sender performs a `fast retransmit` retransmitting the missing segment before that segment’s timer expires. 

  - TCP's error recovery mechanism is categorized as a hybrid of `Go-Back-N (GBN)` and `Selective Repeat (SR)` protocols.


### 3.7.3 Flow Control

Animation: [Flow Control](https://www2.tkn.tu-berlin.de/teaching/rn/animations/flow/)

TCP Flow Control ensures that the sender doesn't overwhelm the receiver's buffer. When data arrives at the receiver, it's placed in a receive buffer, which the application reads from. If the application reads slowly, the sender can easily overflow the buffer.

- **Flow Control vs. Congestion Control:**
  - Flow control matches sender rate to receiver reading speed.
  - Congestion control manages sender rate due to network congestion.
  - Although they both throttle the sender, they serve different purposes.

- **TCP Flow Control:**

    <img src="https://lh3.googleusercontent.com/pw/ADCreHdwuKcHW7WDAFNgV855UgY1P66rxQ52f8nFf1Czx7h1J2gZoP60wVYzQRbXRwM6ooQbGPuL0WQxy2XKUZHWMPCoeRNV4p-vCd_dQv9AEn_dxAZ3SxIifgzty24DQQVfFs0ZOvasX8PyUvRoGx3TCfM=w1748-h860-s-no?authuser=2" width="400" height="250">

  - The sender maintains a `receive window variable (rwnd)` to determine available buffer space at the receiver.
  - Full-duplex communication means both sender and receiver have `distinct receive windows`.
  - rwnd is `dynamic`, and Host B informs Host A by including rwnd in its segments.
  
  ```
  rwnd = RcvBuffer - [LastByteRcvd - LastByteRead]
  ```

- **Using rwnd for Flow Control:**
  - Host B's rwnd value reflects its available buffer space.
  - Host A ensures LastByteSent - LastByteAcked ≤ rwnd.
  - If rwnd is zero, TCP mandates that Host A sends segments with one data byte to unblock the connection.
  - This ensures Host A is informed when space becomes available in Host B's receive buffer.

## 3.7.4 TCP Connection Management

<img src="https://lh3.googleusercontent.com/pw/ADCreHerZiPLvIkD639YmnLg4pvsWFpC001vHpsgwfcRnyxsT3JyaW9A4xcLw2SoQIAR4qV6y-8JQ4eMA3WRWUse594rU-mnBwuK6xjDGe5kkqb6JVi7vKdf6pkWXMIDKO0Cr6iuuLM0Pak4vNxgMsWs7_M=w1920-h888-s-no?authuser=2" width="950" height="550">


## 3.8 Congestion Control
