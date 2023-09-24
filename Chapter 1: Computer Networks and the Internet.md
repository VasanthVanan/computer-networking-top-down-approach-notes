# Chapter 1: Computer Networks and the Internet

## 1.1 A Nuts-and-Bolts Description (Infrastructure based Internet)

- The Internet connects billions of computing devices worldwide, including traditional computers, smartphones, and various nontraditional devices.
- The term "**hosts**" or "**end systems**" refers to all connected devices. Each End systems are interconnected by a network of communication links and packet switches.
- Packet switches, such as routers and link-layer switches, forward packets to their destinations.
- The sequence of links and switches a packet traverses is called a **route** or **path** through the network.
- Packet-switched networks are compared to transportation networks, with packets analogous to trucks and communication links to highways and roads.
- End systems access the Internet through various types of **Internet Service Providers (ISPs)**, including residential, corporate, university, WiFi, and cellular data ISPs.
- ISPs connect to content providers, and lower-tier ISPs are interconnected through national and international upper-tier ISPs.
- Internet protocols, such as TCP and IP, govern data transmission within the Internet, collectively known as TCP/IP.
Internet standards, developed by the ```Internet Engineering Task Force (IETF)``` and documented as requests for comments (**RFCs**), ensure interoperability.
- There are nearly 9000 RFCs defining various Internet protocols, and other bodies specify standards for network components, such as the **IEEE** 802 LAN Standards Committee for Ethernet and WiFi.

## 1.2 A Services Description (Service based Internet)

- The Internet can be viewed as an infrastructure that serves applications.
- Internet applications include various services like messaging, mapping, streaming, social media, etc.
- Internet applications are distributed and run on end systems, not within packet switches.
- To create an Internet application, you need to write programs for end systems.
- End systems use a **socket interface** to instruct the Internet to deliver data to other end systems. It has rules that must be followed.

    A socket interface that specifies how a program running on one end system asks the Internet infrastructure to deliver data to a specific destination program running on another end system.
    
    A protocol defines the format and the order of messages exchanged between two or more communicating entities, as well as the actions taken on the transmission and/or receipt of a message or other event.

## 1.3 The Network Edge

- End systems, which include computers, smartphones, and other devices, are found at the network edge.
- End systems are also known as hosts and run application programs.
- Hosts can be divided into clients (e.g., desktops, laptops, smartphones) and servers (more powerful machines).
- Servers store and distribute content, often in large data centers.
- Large companies like Google have multiple data centers worldwide with millions of servers.

## 1.4 Access Networks

    Access Network — the network that physically connects an end system to the first router (also known as the “edge router”) on a path from the end system to any other distant end system.

### 1.4.1 Home Access: DSL 

- The two most common broadband residential access types are **DSL** and **cable**.
- DSL is often provided by the local telephone company, acting as `both telco and ISP`.
- DSL connections involve DSL modems at homes communicating with a DSLAM at the telco's central office.
- DSL allows data and telephone signals to share the same line using frequency-division multiplexing.
- Splitters at the customer's end and DSLAMs at the telco end manage signal separation.
- DSL offers varying transmission rates, both downstream and upstream, with the potential for high-speed access.
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

- Fiber to the home (FTTH) is a technology providing high-speed residential broadband access via optical fiber.
- FTTH can offer gigabit-per-second internet speeds.
- Optical distribution networks for FTTH include direct fiber and shared fibers.
- Shared fiber networks use two architectures: **active optical networks (AON)** and **passive optical networks (PON)**.
- PON is used in Verizon's FiOS service and involves optical **network terminators (ONTs)** connected to a neighborhood splitter.
- The splitter combines multiple homes onto a shared optical fiber, which connects to an **optical line terminator (OLT)** in the central office.
- OLT connects to the internet via a telco router, and users connect their home routers to the ONT for internet access.
- In the PON architecture, all packets sent from OLT to the splitter are replicated at the splitter.


<img src="https://lh3.googleusercontent.com/pw/ADCreHfDt9ikXrQ0-2QZvqlKumBfhaRU9zh5meb36d0NOPJIinODENCl24OWvCxFxU_FUoMLI08IbvQ-Goz8aEDXwKgqzvfaXZiwa5biqMD0mD9Nhf7fprl9Gp3_emoohUaZmqKwOrufobgbJCbGEDJIJprt=w1808-h902-s-no" width="700" height="350">

### 1.4.4 Home Access: 5G Fixed Wireless

- 5G fixed wireless is an emerging technology for high-speed residential access.
- It eliminates the need for **costly** and **unreliable cabling** from the telco's central office (CO) to homes.
- Data is sent wirelessly from a provider's base station to a modem in the home using beam-forming technology.
- A WiFi wireless router is connected to the modem, similar to cable or DSL modem setups.