# Chapter 5: Network Layer (Control Plane)

- The Control Plane Network layer is a component in networking that manages the communication and coordination between network devices. It is responsible for handling tasks such as routing, signaling, and configuration. 
- In essence, the control plane determines how data should be forwarded through the network by making decisions based on the network's state and topology.

<img src="https://lh3.googleusercontent.com/pw/ABLVV85gXLe2Oe-ngOZehumdWU7yMgPW3sZDlT7-2vK-Ec_jOTZqntb02OxO4GwOLliURJP5kG9VGDK0RQHj5FLm30CISMkl9HIiPeUaQYRdQO1FIG1bzWcR67s9oBLbZqqeduhFhW5IOzEFF0VGxL736TmP=w1542-h1014-s-no?authuser=3" width="60%" height="auto">

## 5.1 Routing Algorithms

**Distance Vector Algorithm:** Each router maintains a table with distances to destinations, updating based on information exchanged with neighbors (e.g., RIP protocol).

**Link State Algorithm:** Routers share comprehensive information about network link states to build a shortest path based on the complete network topology for efficient path calculations (e.g., OSPF protocol).

| Feature                        | Distance Vector Routing         | Link State Routing             |
|--------------------------------|---------------------------------|--------------------------------|
| Information Exchange           | Exchange routing tables         | Exchange Link State Database   |
| Update Triggers                | Periodic updates or triggered   | Event-driven updates           |
| Hop Count                      | Based on hop count              | Based on shortest path         |
| Path Selection                 | May not always find shortest    | Always finds shortest path     |
| Convergence Time                | Slower convergence              | Faster convergence             |
| Memory Usage                   | Lower memory usage              | Higher memory usage            |
| Bandwidth Usage                | Higher bandwidth usage          | Lower bandwidth usage          |
| Routing Table Size              | Larger routing tables           | Smaller routing tables         |
| Example Protocols               | RIP, EIGRP                      | OSPF, IS-IS                    |
| Loop Prevention                | Split Horizon, Poison Reverse   | SPF Algorithm                  |
| Scalability                    | Limited scalability            | Highly scalable                |
| Fault Tolerance                | Less fault-tolerant             | More fault-tolerant            |
| Example Implementations        | Traditional networks            | Internet-scale networks        |

## 5.2 Intra-AS Routing: OSPF

### Autonomous Systems (ASs)

> An autonomous system is identified by its globally unique autonomous system number (ASN). AS numbers, like IP addresses, are assigned by `ICANN` regional registries.

- **Definition:** Routers organized into Autonomous Systems (ASs), each under the same administrative control.
- **AS Identification:** Globally unique Autonomous System Numbers (ASNs) assigned by ICANN.
- **Routing Algorithm:** Intra-Autonomous System Routing Protocol.

### 5.2.1 OSPF (Open Shortest Path First)

> OSPF is a link-state protocol that uses flooding of link-state information and a Dijkstra’s least-cost path algorithm. With OSPF, each router constructs a complete topological map (that is, a graph) of the entire autonomous system. Each router then locally runs Dijkstra’s shortest-path algorithm to determine a shortest-path tree to all subnets

- **Link-State Protocol:**
  - Utilizes flooding of link-state information.
  - uses Dijkstra’s least-cost path algorithm.
- **Complete Topological Map:**
  - Each router constructs a full topological map of the entire AS.
  - Locally runs Dijkstra’s algorithm to determine shortest-path trees.
- **Link Costs:**
  - Configured by the network administrator.
  - Allows flexibility (e.g., minimum-hop routing or weights based on link capacity).

### 5.2.2 OSPF Features and Advancements

1. **Security:**
   - Authentication to ensure only trusted routers participate.
   - Supports `simple` and `MD5` authentication, the latter providing higher security.
   - Guards against replay attacks using sequence numbers.

2. **Multiple Same-Cost Paths:**
   - OSPF permits the use of multiple paths when several have the same cost.

3. **Integrated Unicast and Multicast Routing:**
   - MOSPF (Multicast OSPF) extends OSPF to support multicast routing.

4. **Hierarchy Support:**
   - OSPF AS can be configured hierarchically into areas.
   - Area Border Routers facilitate routing between areas, with a designated backbone area.

   <img src="https://lh3.googleusercontent.com/pw/ABLVV84X2z2FuF11XHSg5DVPAaJX1KWa8AfEWP3MRClK3DTwVMMU2nJh6UcOXd_1QF5zn3K9jzTG2fDyE3YyfWPedybHOuGE73JZCvCC9X4oFtD8AhVckBFy8KmfXpYJKkGjYPIMWCuZw1yODVzJHKQxgQ7S=w1920-h564-s-no?authuser=3" width="80%" height="auto">


## 5.3 Inter-AS Routing: BGP

BGP (Border Gateway Protocol) is a fundamental inter-autonomous system routing protocol in the Internet. It acts as the glue that binds thousands of ISPs together, facilitating communication across multiple ASs.

### 5.3.1 BGP Responsibilities

- **Destination Representation:** Routes packets to `CIDRized prefixes`, each representing a subnet or a collection of subnets.
- **Advertising Reachability Information:** Allows subnets to advertise their existence across ASs.
- **BGP Connections:**
  - Advertising reachability information through BGP messages.
  - External BGP (eBGP) connections: Between gateway routers in different ASs.
  - Internal BGP (iBGP) connections: Within routers of the same AS.

### Determining the Best Routes

> When a router advertises a prefix across a BGP connection, it includes with the prefix several BGP attributes. In BGP jargon, a prefix along with its attributes is called a route. Two of the more important attributes are `AS-PATH` and `NEXT-HOP`. 

> The AS-PATH attribute contains the list of ASs through which the advertisement has passed. To generate the AS-PATH value, when a prefix is passed to an AS, the AS adds its ASN to the existing list in the AS-PATH. The NEXT-HOP is the IP address of the router interface that begins the AS-PATH.

- **BGP Attributes:**
  - `AS-PATH`: List of ASs through which the advertisement has passed.
  - `NEXT-HOP`: IP address of the router interface starting the AS-PATH.
- **Choosing the Best Route:** Routers choose among various paths based on cost and policy.

<img src="https://lh3.googleusercontent.com/pw/ABLVV87jx9vAP4G_DTrfiu8oIOSr29Rxs8KtvpWAMnxMCDuobDG3fmX2zZ_8QAy81L6rTQ22jgmaAXHeoCqnZV6OTatkhVz_C-GcgifrfteMUK6viSvc7b-2zUhbhHrUkwKwXBVfXRNV8gLO1vskH_RpPbCP=w1650-h690-s-no?authuser=3" width="60%" height="auto">

Three components: NEXT-HOP; AS- PATH; destination prefix.

```
3d; AS3; X --> 2a; AS2 AS3; X
```

### 5.3.2 Hot Potato Routing in BGP

Hot Potato Routing is a fundamental algorithm in BGP (Border Gateway Protocol) used for selecting the best route to a destination prefix. This algorithm prioritizes getting packets out of an Autonomous System (AS) quickly with minimal cost.

- Goal: Quickly move packets out of the AS with the least possible cost.
- Analogy: Similar to passing a burning "hot potato" to another person (AS) as quickly as possible.
- Selfish Algorithm: Focuses on reducing costs within its AS, ignoring other end-to-end costs outside the AS.

### Algorithm Overview

1. **Learn Routes:**
   - Router 1b learns about two possible BGP routes to prefix x.
   
2. **Cost Calculation:**
   - Utilizes intra-AS routing information to find the least-cost intra-AS path to NEXT-HOP routers (2a and 3d).
   - Defines cost as the number of links traversed.

3. **Route Selection:**
   - Selects the route with the smallest of these least-cost paths.
   - Example: Cost to router 2a is 2, cost to router 3d is 3, so router 2a is selected.

4. **Forwarding Table Update:**
   - Consults the forwarding table (configured by intra-AS algorithm) to find the interface (I) on the least-cost path to router 2a.
   - Adds (x, I) to its forwarding table.

5. **Adding Outside-AS Prefix**
    - When adding an outside-AS prefix to the forwarding table, both inter-AS (BGP) and intra-AS (e.g., OSPF) routing protocols are utilized.

