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


## 4.2.3 Output Port Processing

- Output port processing involves transmitting packets stored in the output port's memory over the output link.
- Tasks include packet selection (scheduling), de-queuing for transmission, and executing link-layer and physical-layer functions.

### Input Queueing

- Minimal queuing at input ports when the switching fabric (Rswitch) is N times faster than the line speed (Rline).
- If Rswitch is not fast enough, packet queues form at input ports, causing `head-of-the-line (HOL) blocking` and potential packet loss.

### Output Queueing

- Even with a fast switching fabric, output port queues may form if packets from all N input ports are destined for the same output port.
- Queued packets can overwhelm the output port's memory, leading to potential packet loss.

## How Much Buffering Is "Enough?"

- Traditional rule: Buffering (B) equals average round-trip time (RTT) times link capacity (C), i.e., 
   ```B = RTT * C```.
- Modern Rule: B = `RTT C (2N)^1/2`
- Optimal buffer size is a nuanced consideration, balancing decreased packet loss with increased queueing delays.
- Active queue management (AQM) algorithms, like `Random Early Detection (RED)`, help manage buffer dynamics.

## Bufferbloat and Complexities

- `Bufferbloat` refers to persistent buffering, showcasing the complexity of managing queues.
- Interaction among senders at the network edge and queues within the network can be subtle.

## 4.2.4 Packet Scheduling

## Comparison Table

| Discipline            | Description                                | Operation                                                     |
|-----------------------|--------------------------------------------|---------------------------------------------------------------|
| **FIFO**               | Queuing in arrival order; packets leave in the same order they arrived. | Selection based on arrival order; removal after transmission |
| **Priority Queuing**  | Classifies packets into priority classes; serves higher-priority packets first. | Highest priority class with nonempty queue served first      |
| **Round Robin**       | Alternates service among classes; each class served in a circular manner. | Round-robin service pattern                                   |
| **Weighted Fair Queuing (WFQ)** | Generalized round robin with differential service based on weights assigned to each class. | Service based on weighted round-robin pattern                |


