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
