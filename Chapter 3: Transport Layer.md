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

> UDP at the sender side performs the 1s complement of the sum of all the 16-bit words in the segment, with any overflow encountered during the sum being wrapped around.

<img src="https://lh3.googleusercontent.com/pw/ADCreHd6aGmd6Jk9aEfaOuteJHdj1V8Tl3M-w_eAdWjDwwqOUuDR8hobSh1fjb2PyblDTqlzm8bLz72502F7YGVJF2x5nirudlaEPwHHrIdqbkWzDpvPUb6K3jccQN5bxYKKsbptwUDTjAnvI0uhICFYGpRv=w1286-h1130-s-no" width="550" height="500">

- Thus, the 1s complement of the sum `0100101011000010` is `1011010100111101`,

> If no errors are introduced into the packet, then clearly the sum at the receiver will be 1111111111111111. If one of the bits is a 0, then we know that errors have been introduced into the packet.

> It is useful for the transport layer to provide error checking as a safety measure. Although UDP provides error checking, it does not do anything to recover from an error. Some implementations of UDP simply discard the damaged segment; others pass the dam- aged segment to the application with a warning.

## 3.6 Building Reliable Data Transfer Portocol

**3.6.1 RDT 1.0:**
- **Basic Version:** RDT 1.0 is the most basic version of the Reliable Data Transfer protocol.
- **Key Characteristics:**
  - Sender sends data to the receiver.
  - Receiver simply accepts the data without providing feedback. (unidirectional communication)
  - Assumes a perfectly reliable channel where data is never lost or corrupted.
  - No error detection or correction mechanisms in place.

**3.6.2 RDT 2.0:**
- **Enhanced Reliability:** RDT 2.0 is an enhanced version of the RDT 1.0 protocol.
- **Key Characteristics:**
  - Introduces a basic acknowledgment mechanism.
  - Sender sends data and waits for an acknowledgment (ACK / NAK) from the receiver.
  - Receiver sends an ACK to confirm successful data reception.
  - If ACK is not received, sender retransmits the data.
  - Addresses the issue of lost or corrupted data and ensures basic reliability.

  > The message-dictation protocol uses both positive acknowledgments (“OK”) and negative acknowledgments (“Please repeat that.”). These control messages allow the receiver to let the sender know what has been received correctly, and what has been received in error and thus requires repeating. It is known as ARQ (Automatic Repeat reQuest) protocols.

  > when the sender is in the wait-for-ACK-or-NAK state, it cannot get more data from the upper layer; that will happen only after the sender receives an ACK and leaves this state. This is known as `Stop-and-Wait` protocol.

**3.6.3 RDT 2.1:**
- **Extended Reliability:** RDT 2.1 further improves upon reliability.
- **Key Characteristics:**
  - Adds a sequence number to each frame sent by the sender.
  - Receiver identifies duplicate frames and discards them.
  - If the receiver receives a frame with the wrong sequence number, it discards it.
  - This prevents duplicate frames from being delivered and enhances reliability.

**3.6.4 RDT 2.2:**
- **Enhanced Error Handling:** RDT 2.2 focuses on error handling and retransmission.
- **Key Characteristics:**
  - Similar to RDT 2.1, it uses sequence numbers to handle duplicate frames.
  - Introduces a timeout mechanism.
  - If the receiver doesn't receive an expected frame within a certain time (timeout), it requests retransmission.
  - Sender retransmits the missing frame.
  - Adds the ability to recover from lost frames more efficiently.
