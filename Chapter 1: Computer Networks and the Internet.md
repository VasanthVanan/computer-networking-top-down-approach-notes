# Chapter 1: Computer Networks and the Internet

## 1.1 A Nuts-and-Bolts Description (Infrastructure based Internet)

- The Internet connects billions of computing devices worldwide, including traditional computers, smartphones, and various nontraditional devices.
- The term "**hosts**" or "**end systems**" refers to all connected devices. Each End systems are interconnected by a network of communication links and packet switches.
- Packet switches, such as routers and link layer switches, forward packets to their destinations.
- The sequence of links and switches a packet traverses is called a **route** or **path** through the network.
- Packet switched networks are compared to transportation networks, with packets analogous to trucks and communication links to highways and roads.
- End systems access the Internet through various types of **Internet Service Providers (ISPs)**, including residential, corporate, university, WiFi, and cellular data ISPs.
- ISPs connect to content providers, and lower tier ISPs are interconnected through national and international upper tier ISPs.
- Internet protocols, such as TCP and IP, govern data transmission within the Internet, collectively known as TCP/IP.
Internet standards, developed by the ```Internet Engineering Task Force (IETF)``` and documented as requests for comments (**RFCs**), ensure interoperability.
- There are nearly 9000 RFCs defining various Internet protocols, and other bodies specify standards for network components, such as the **IEEE** 802 LAN Standards Committee for Ethernet and WiFi.

## 1.2 A Services Description (Service based Internet)

- The Internet can be viewed as an infrastructure that serves applications.
- Internet applications include various services like messaging, mapping, streaming, social media, etc.
- Internet applications are distributed and run on end systems, not within packet switches.
- To create an Internet application, you need to write programs for end systems.
- End systems use a **socket interface** to instruct the Internet to deliver data to other end systems. It has rules that must be followed.

> A socket interface that specifies how a program running on one end system asks the Internet infrastructure to deliver data to a specific destination program running on another end system.
    
> A protocol defines the format and the order of messages exchanged between two or more communicating entities, as well as the actions taken on the transmission and/or receipt of a message or other event.

## 1.3 The Network Edge

- End systems, which include computers, smartphones, and other devices, are found at the network edge.
- End systems are also known as hosts and run application programs.
- Hosts can be divided into clients (e.g., desktops, laptops, smartphones) and servers (more powerful machines).
- Servers store and distribute content, often in large data centers.
- Large companies like Google have multiple data centers worldwide with millions of servers.

## 1.4 Access Networks

> Access Network — the network that physically connects an end system to the first router (also known as the “edge router”) on a path from the end system to any other distant end system.

### 1.4.1 Home Access: DSL 

- The two most common broadband residential access types are **DSL** and **cable**.
- DSL is often provided by the local telephone company, acting as `both telco and ISP`.
- DSL connections involve DSL modems at homes communicating with a DSLAM at the telco's central office.
- DSL allows data and telephone signals to share the same line using frequency division multiplexing.
- Splitters at the customer's end and DSLAMs at the telco end manage signal separation.
- DSL offers varying transmission rates, both downstream and upstream, with the potential for high speed access.
- DSL access can be asymmetric, with different rates for downstream and upstream.
- Achievable rates may be lower due to distance, line quality, and provider limitations.
- DSL is best suited for short distances from the central office, typically within 5 to 10 miles.

<img src="https://lh3.googleusercontent.com/pw/AIL4fc8Mjg-m_XwafZaE4lRMp6fpu9DwIJgZkiExxghXGh9Qgxsim6HBe-fnkxqSo6HH8bP_kVqIlEti8LEkvk9E0hzqz0Z2aJ_SfQlOVK21jFqxO92JFJ_Iq5vBibrrl7OcXmegiq0N0dLzvQhKRgf0xcSt=w1920-h860-s-no" width="700" height="300">

### 1.4.2 Home Access: Cable 

- Cable Internet uses the existing cable TV infrastructure.
- Fiber optics connect the cable head end to neighborhood junctions, then coaxial cable reaches individual homes.
- Because both fiber and coaxial cable are employed in this system, it is often referred to as hybrid fiber coax (HFC).
- Cable Internet requires cable modems to connect to home PCs via Ethernet.
- The cable modem termination system (CMTS) turns analog signals from cable modems into digital format.
- Cable Internet has downstream and upstream channels; downstream typically has higher transmission rates.
- Cable Internet is a shared broadcast medium, leading to shared bandwidth.
- Simultaneous usage can reduce individual user rates, but Web surfing is less affected.
- The upstream channel is also shared, requiring a multiple access protocol to avoid collisions.

<img src="https://lh3.googleusercontent.com/pw/ADCreHfdQzhpcW3Nqdx6s9Q_Z5hjTLqeWEM9okcmdCuwfbBvUoPaEZ4n9cJw4rR8iMS4wxuMWCzvzSd_4JsilUCHJ906VUKs6ayT5EcW8mkt8PXaPpphzNAfghV24ssJjVHHY3DrBNzuB5XYRlWXl2JznOOK=w1920-h934-s-no" width="700" height="300">

### 1.4.3 Home Access: FTTH 

- Fiber to the home (FTTH) is a technology providing high speed residential broadband access via optical fiber.
- FTTH can offer gigabit per second internet speeds.
- Optical distribution networks for FTTH include direct fiber and shared fibers.
- Shared fiber networks use two architectures: **active optical networks (AON)** and **passive optical networks (PON)**.
- PON is used in Verizon's FiOS service and involves optical **network terminators (ONTs)** connected to a neighborhood splitter.
- The splitter combines multiple homes onto a shared optical fiber, which connects to an **optical line terminator (OLT)** in the central office.
- OLT connects to the internet via a telco router, and users connect their home routers to the ONT for internet access.
- In the PON architecture, all packets sent from OLT to the splitter are replicated at the splitter.


<img src="https://lh3.googleusercontent.com/pw/ADCreHfDt9ikXrQ0-2QZvqlKumBfhaRU9zh5meb36d0NOPJIinODENCl24OWvCxFxU_FUoMLI08IbvQ-Goz8aEDXwKgqzvfaXZiwa5biqMD0mD9Nhf7fprl9Gp3_emoohUaZmqKwOrufobgbJCbGEDJIJprt=w1808-h902-s-no" width="700" height="350">

### 1.4.4 Home Access: 5G Fixed Wireless

- 5G fixed wireless is an emerging technology for high speed residential access.
- It eliminates the need for **costly** and **unreliable cabling** from the telco's central office (CO) to homes.
- Data is sent wirelessly from a provider's base station to a modem in the home using beam forming technology.
- A WiFi wireless router is connected to the modem, similar to cable or DSL modem setups.

### 1.4.5 Enterprise & Home Access: Ethernet, WiFi

- Ethernet is the predominant LAN technology, using twisted pair copper wire with access speeds from 100 Mbps to tens of Gbps.
- Wireless LAN users connect to an access point, which is linked to the enterprise's network (usually via wired Ethernet).
- IEEE 802.11 (WiFi) is widely used for wireless LAN access with shared transmission rates exceeding 100 Mbps.
- Ethernet and WiFi are used not only in enterprise settings but also in home networks.
- Home networks often combine broadband residential access with wireless LAN technologies.

## 1.5 Physical Media

- Network access technologies in the Internet use various physical media, including fiber cable, coaxial cable, copper wire, and radio spectrum.

>  Physical media fall into two categories: guided media and unguided media. With guided media, the waves are guided along a solid medium, such as a fiber optic cable, a twisted pair copper wire, or a coaxial cable. With unguided media, the waves propagate in the atmosphere and in outer space, such as in a wireless LAN or a digital satellite channel.

- Installation labor costs for physical links can be significantly higher than material costs, motivating builders to install multiple types of media to save on future wiring expenses.
- **Twisted pair copper wire** is a common guided transmission medium, widely used in telephone networks and LANs. It consists of two insulated copper wires twisted together to **reduce interference**. Data rates for LANs range from 10 Mbps to 10 Gbps.
- Twisted pair technology, like category 6a cable, can achieve data rates of 10 Gbps for short distances, making it a dominant solution for high speed LAN networking.
- **Coaxial cable** consists of `two concentric copper conductors` and is commonly used in cable television systems. Coupled with cable modems, it provides high speed Internet access at rates of hundreds of Mbps.
- Coaxial cable can serve as a guided shared medium, allowing multiple end systems to connect directly to the cable and receive signals sent by other end systems.

> In cable television and cable Internet access, the transmitter shifts the digital signal to a specific frequency band, and the resulting analog signal is sent from the transmitter to one or more receivers.

- **Optical fibers** conduct light pulses as bits and offer high data rates, low attenuation, and resistance to interference.
- Fiber optics are ideal for long distance transmission but costly for short haul applications.

- **Terrsetial Radio channels** are wireless and versatile, influenced by propagation environment and distance.
- Three categories of terrestrial radio channels: `short range,` `local area`, and `wide area`.
- Radio channel characteristics include `path loss`, `shadow fading`, `multipath fading`, and `interference`.

- **Satellite communication** involves `geostationary` and `low earth orbiting (LEO)` satellites.
- Geostationary satellites stay fixed above one spot on Earth but introduce signal propagation delay.
- LEO satellites are closer to Earth, move in orbits, and may require multiple satellites for continuous coverage.
- Satellite links offer high speeds and serve areas without DSL or cable based Internet access.

## 1.6 Packet Switching (Store and forward Transmission)

- Most packet switches use **store and forward** transmission.
- Packet switch must receive the entire packet before transmission.

> Because the router employs store and forwarding, at this instant of time, the router cannot transmit the bits it has received; instead it must first buffer (i.e., “store”) the packet’s bits. Only after the router has received all of the packet’s bits can it begin to transmit (i.e., “forward”) the packet onto the outbound link

- `Delay = 2L/R` for a simple source destination network.
- General delay formula for N links each of rate R: `d = N * (L/R)`.

- **Queuing Delays and Packet Loss:** Packet switches have output buffers (output queues).
- Queuing delays occur when the link is busy.
- Packet loss can happen when the buffer is full due to congestion.

> If, during a short interval of time, the arrival rate of packets to the router (when converted to bits per second) exceeds 15 Mbps, congestion will occur at the router as packets queue in the link’s output buffer before being transmitted onto the link.

<img src="https://lh3.googleusercontent.com/pw/ADCreHfX55Xjkoab8kOtoXWB6Pz5F-CWrWrw5L4EFCw6OkzWdqMTgG54E0ZuEkveIh6dnKOvYNbfjwErwsKZvKgvWt-PqDh9Baiy-U_naufuJ4_MSsowqxExy_HUJVPfTrVjW2q_9N7vcgW3QgqDMD4RF2zK=w1806-h1130-s-no" width="700" height="400">

- **Forwarding Tables and Routing Protocols:** Routers use forwarding tables to determine outbound links.
- IP addresses are used for destination routing.
- Routers consult forwarding tables based on destination addresses.
- Internet uses routing protocols to configure forwarding tables automatically.

## 1.7 Circuit Switching

> In circuit switched networks, the resources needed along a path (buffers, link transmission rate) to provide for communication between the end systems are reserved for the duration of the communication session between the end systems.

- Circuit switched networks reserve resources (buffers, link transmission rate) for the entire communication session, while packet switched networks use resources on demand and may involve queuing.
- Traditional telephone networks are examples of circuit switched networks, where circuits are established and transmission rate is reserved for the entire connection.
- **Multiplexing in circuit switched networks** can use `frequency division multiplexing (FDM)` or `time division multiplexing (TDM)`.

<img src="https://lh3.googleusercontent.com/pw/ADCreHcnymJYAUF4Fn1TBsyIZwgZJ6JIBFewHZxR1y6w26eel_YuQjD32j5gqkJZ-NqMjqr32WXra6HCaP_Z9-HXc6oKcrzDifgPrrmuGzS7ec1I4rjT_R5SEviCfBTv0nLAHwlFcVUsi1hTgfto2b6-cU4q=w1482-h1088-s-no" width="600" height="400">

>  FDM, the frequency domain is segmented into four bands, each of bandwidth 4 kHz. For TDM, the time domain is segmented into frames, with four time slots in each frame.

- Packet switching is seen as more efficient because it doesn't reserve resources during idle periods, while circuit switching does.
- Packet switching allows better sharing of transmission capacity and is simpler and more cost effective than circuit switching.
- Packet switching allocates link use on demand, while circuit switching pre allocates link use regardless of demand.

## 1.8 Networks of Networks

- **Network Structures:** Various network structures are discussed, including global transit ISPs, regional ISPs, and tier 1 ISPs.
- **Peering:** ISPs can peer, enabling direct traffic exchange without payment. Tier 1 ISPs also peer.
- **IXPs:** Internet Exchange Points facilitate ISPs' direct connections, enhancing efficiency.
- **Content Provider Networks:** Large providers like Google create their networks for control and cost reduction.

<img src="https://lh3.googleusercontent.com/pw/ADCreHfe3VrXVOkt6FxFDHy4qRsByO6EugEHMcLkEvVuXtz2Bz3iZW28h32W2rJs7faFriHzgxJIfbOPV2Gd5L1_0e9PqKnc_pCtjmqNN0OxeH15auTYBd17kowR13spgnTFSgBjINghCV3Ew7gDsBC8lIh5=w1920-h926-s-no" width="600" height="300">

## 1.9 Delay, Loss, Throughput

<img src="https://lh3.googleusercontent.com/pw/ADCreHfL-nbDiTeiYVwKehMcZPCWS4vbdLyINcp3vKml8sDHzFSTYe1VwuexfmBm7WMuOcHHtZM3jBvRVSjVQPA5Nf3_UIDoQ0kpJrBEbwNwm-aBnnEFXz7mxgiKweHMy39iecCa53_7H2PxFA-5-pKN45Dq=w1920-h812-s-no" width="700" height="300">

- **Types of Delays:** Several types of delays affect packets during their journey, including nodal processing delay, queuing delay, transmission delay, and propagation delay.
- **Processing Delay:** Part of the processing delay includes examining the packet's header and checking for bit level errors. High speed routers have processing delays typically in microseconds.
- **Queuing Delay:** Queuing delay occurs as packets wait in a queue before being transmitted onto the link. It depends on the number of packets ahead in the queue and can range from microseconds to milliseconds.
- **Transmission Delay:** It is the time required to push all of a packet's bits into the link, determined by the packet's length and the transmission rate of the link. It is typically in microseconds to milliseconds.
- **Propagation Delay:** It is the time for a bit to propagate from one router to the next, dependent on the distance between routers and the propagation speed of the link. It can be in milliseconds for wide area networks.
- **Comparing Transmission and Propagation Delay:** Transmission delay is about pushing out the packet, while propagation delay is about bit propagation. They can be thought of as the time it takes for a caravan of cars to travel between tollbooths on a highway.
- **Traffic Intensity:** The traffic intensity (La/R) plays a vital role in estimating queuing delay. If it exceeds 1, queuing delay can approach infinity. It varies based on the rate of packet arrivals and the transmission rate.

<img src="https://lh3.googleusercontent.com/pw/ADCreHd2b-31EKHc-W0Ah_HtMogmvFSs7LCK5p-2s6gK7o4h2dv7yao9fDUTvge6xrDnoPiVzIlly5XMmSi-l-sLOQST4M6NFGPxk7GSChBXEqVrqqEJD_ZESJ2vL-SSSzsXRw8dMtxXjAUJUIHuei8cA2j7=w1092-h888-s-no" width="600" height="400">

> **Trace Route**: measure end to end delay in a computer network by tracing the route taken by packets and measuring round-trip delays to routers.

> If the source receives fewer than three messages from any given router (due to packet loss in the network), Traceroute places an asterisk just after the router number and reports fewer than three round trip times for that router.

## 1.10 Throughput

> If the file consists of F bits and the transfer takes T seconds for Host B to receive all F bits, then the average throughput of the file transfer is F/T bits/sec.

- **Instantaneous vs. Average Throughput**: It explains the difference between instantaneous throughput (bits/sec at a given moment) and average throughput (bits/sec over the entire transfer).
- **Two Link Network**: In a simple network with two links, the throughput is determined by the slower link, which acts as the **bottleneck**. `min{Rc, Rs}`
- **Multiple Links**: In a network with multiple links between source and destination, the throughput is limited by the slowest link along the path.
- **Access Network**: In today's Internet, the access network is often the bottleneck for throughput due to over provisioned core network links.
- **Intervening Traffic**: Throughput can be affected by other data flows sharing the same link, even if the link has a high transmission rate.
- **General Dependence**: Throughput depends not only on link transmission rates but also on intervening traffic, making it a more complex concept.

## 1.11 Protocol Layers

- **Protocol Layering**: Network protocols are organized into layers, with each layer providing specific services and using services from the layer below. This structure helps in the design and modularity of network protocols.
- **Implementation of Layers**: Protocols can be implemented in software, hardware, or a combination of both. Application and transport layer protocols are typically implemented in software, while physical and data link layers are often implemented in hardware.
- **Advantages of Layering**: Protocol layering offers modularity, making it easier to update components. It provides a structured way to discuss system components.
- **Drawbacks of Layering**: Some argue against layering due to potential duplication of functionality and information dependencies between layers.
- **Protocol Stack**: The protocols of different layers collectively form the protocol stack. The Internet protocol stack consists of five layers: physical, link, network, transport, and application layers.
- **Encapsulation**: Data is encapsulated as it moves down the protocol stack. Each layer adds its header information to the data received from the layer above, creating a packet with header fields and a payload field.

## 1.12 Networks Under Attack

- **Malware Consequences:** Malware can have severe consequences, including file deletion, data theft (e.g., passwords and personal information), and turning compromised hosts into part of botnets used for malicious purposes.
- **Self Replicating Malware:** Many malware types are self replicating, meaning they can infect one host and then spread to other hosts over the Internet, leading to exponential growth in infections.
- **Types of DoS Attacks:** DoS attacks can be categorized into three types: `vulnerability attacks`, `bandwidth flooding`, and `connection flooding`. These attacks disrupt services or crash hosts.
- **Distributed DoS (DDoS) Attacks**: In a DDoS attack, attackers control multiple sources to launch attacks, making them harder to detect and defend against.
- **Cryptography Defense**: Cryptography is one of the defenses against packet sniffing. It can help secure communication channels and protect against eavesdropping.
- **IP Spoofing**: Attackers can forge source addresses on packets, allowing them to masquerade as someone else. This is known as IP spoofing and presents a security challenge.
- **End Point Authentication**: To address IP spoofing and ensure message authenticity, end point authentication mechanisms are needed. These mechanisms verify the source of messages.

