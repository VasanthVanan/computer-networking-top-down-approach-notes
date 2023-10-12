# Chapter 2: Application Layer

## 2.1 Principle of Network Applications

- **Client-Server Architecture**: In a client-server architecture, there is a dedicated server that services requests from multiple client hosts. Clients do not directly communicate with each other but interact with the server. The server has a fixed, well-known IP address. Examples include the Web, FTP, Telnet, and email.
- **Peer-to-Peer (P2P) Architecture**: In a P2P architecture, there is minimal reliance on dedicated servers in data centers. Peers, which are intermittently connected hosts, communicate directly with each other without a dedicated server intermediary.
- **P2P Scalability**: P2P architectures offer self-scalability, with peers contributing service capacity by distributing files to other peers. They are cost-effective and do not require significant server infrastructure.
- **Challenges of P2P Architectures**: P2P applications face challenges related to security, performance, and reliability due to their highly decentralized structure.
- **Communication Between Processes**: Processes running on different end systems communicate with each other by exchanging messages across the computer network.
- **Socket**: Messages sent between processes must pass through the network using a software interface called a `socket`. A socket is analogous to a door through which a process sends and receives messages.
- **Socket Communication**: Sockets serve as the interface between the application layer and the transport layer within a host. They are the `Application Programming Interface (API)` for building network applications.

<img src="https://lh3.googleusercontent.com/pw/ADCreHfjdzN47NUqgVF6Leej5qKJLDdzI8ZtOIy4Gbq0EoANrQ8oLeWTBbPJ8_oCehHJG19bSWgldXa86Lw0hEMNjGrGleqJBJijilrs61lEfjnfH26Fy_3VD2Qe_voulh7kOk_Q0Fk0nfvo490o_3tOK-zZ=w1920-h940-s-no" width="680" height="350">

- **Addressing Processes**: To send messages between processes, the receiving process's address needs to be specified. This includes the host's IP address and a destination port number. The IP address identifies the host, while the port number identifies the specific receiving process.
- **Port Numbers**: Popular applications are assigned specific port numbers (e.g., Web server on port 80, mail server on port 25)

## 2.2 Transport Services (TCP & UDP)

- **Choosing a Transport-Layer Protocol**: When developing an application, you must choose a transport-layer protocol that suits your application's needs. The choice is typically based on services such as reliable data transfer, throughput, timing, and security.
- **Reliable Data Transfer**: Reliable data transfer ensures that data sent by one end is delivered correctly and completely to the other end. Some applications require this service to prevent data loss, while others like multimedia apps can tolerate some loss.
- **Throughput**: Throughput is the rate at which bits are delivered from the sender to the receiver in a communication session. Bandwidth-sensitive applications need guaranteed throughput.
- **Timing**: Timing guarantees are essential for real-time applications like Internet telephony, teleconferencing, and multiplayer games. Low delay is crucial for their effectiveness.
- **Security**: Transport protocols can provide security services like encryption for confidentiality, data integrity, and end-point authentication.
- **UDP and TCP**: The Internet offers two transport-layer protocols: UDP and TCP. UDP is lightweight and provides unreliable data transfer, while TCP offers reliable data transfer and connection-oriented services.
- **Services Not Provided**: Neither UDP nor TCP provide throughput or timing guarantees, which means the Internet cannot guarantee specific timing or throughput for time-sensitive applications.

> the Internet community has developed an enhancement for TCP, called Transport Layer Security (TLS) [RFC 5246]. TCP-enhanced-with-TLS not only does everything that traditional TCP does but also provides critical process-to- process security services, including encryption, data integrity, and end-point authenti- cation.

> In particular, if an application wants to use the services of TLS, it needs to include TLS code (existing, highly optimized librar- ies and classes) in both the client and server sides of the application. TLS has its own socket API that is similar to the traditional TCP socket API. 

## 2.3 Application Layer Protocol: Web and HTTP

- **HTTP**: `HTTP (HyperText Transfer Protocol)` is the primary application-layer protocol of the World Wide Web. It relies on client and server programs that communicate by exchanging HTTP messages. Web pages are composed of objects, which are individual files with unique URLs.
- **Web Page Structure**: A web page typically includes a base HTML file and referenced objects like images, stylesheets, and videos. Objects are identified by URLs, which consist of a `hostname` and a `path name`.
- **HTTP and TCP**: HTTP uses TCP (Transmission Control Protocol) as its underlying transport protocol. Clients initiate TCP connections with servers to exchange HTTP messages.

> HTTP need not worry about lost data or the details of how TCP recovers from loss or reordering of data within the network. 

- **Stateless Protocol**: HTTP is a stateless protocol, meaning servers `don't store client-specific information.` If a client requests the same object multiple times, the server doesn't remember previous requests.
- **HTTP Versions**: `HTTP/1.0` and `HTTP/1.1` are common versions, with HTTP/1.1 supporting persistent connections. Newer versions like HTTP/2 are also emerging.
- **Non-Persistent and Persistent Connections**: Non-persistent connections create a new connection for each requested object. Persistent connections allow multiple objects to be sent over the same connection, improving efficiency.

> Although HTTP uses persistent connections in its default mode, HTTP clients and servers can be configured to use non-persistent connections instead.

- **HTTP Message Format**: HTTP messages have two types: `request messages` and `response messages`. Request messages include a method (e.g., GET), URL, and HTTP version, followed by header lines. Response messages include a `protocol version`, `status code` (e.g., 200 OK), and `header lines`, followed by the `entity body`.
- **Header Lines**: Header lines provide additional information in HTTP messages. Examples of request headers include Host, Connection, User-agent, and Accept-language. Examples of response headers include Connection, Date, Server, Last-Modified, Content-Length, and Content-Type.
- **Status Codes**: Status codes (e.g., 404 Not Found) indicate the result of an HTTP request.
- **HTTP Methods**: HTTP includes methods like GET, POST, PUT, and DELETE for different types of requests and actions.
- **Entity Body**: The entity body in HTTP messages contains data related to the request method.

