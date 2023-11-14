# Chapter 2: Application Layer

## 2.1 Principle of Network Applications

- **Client-Server Architecture**: In a client server architecture, there is a dedicated server that services requests from multiple client hosts. Clients do not directly communicate with each other but interact with the server. The server has a fixed, well-known IP address. Examples include the Web, FTP, Telnet, and email.
- **Peer-to-Peer (P2P) Architecture**: In a P2P architecture, there is minimal reliance on dedicated servers in data centers. Peers, which are intermittently connected hosts, communicate directly with each other without a dedicated server intermediary.
- **P2P Scalability**: P2P architectures offer self scalability, with peers contributing service capacity by distributing files to other peers. They are cost effective and do not require significant server infrastructure.
- **Challenges of P2P Architectures**: P2P applications face challenges related to security, performance, and reliability due to their highly decentralized structure.
- **Communication Between Processes**: Processes running on different end systems communicate with each other by exchanging messages across the computer network.
- **Socket**: Messages sent between processes must pass through the network using a software interface called a `socket`. A socket is analogous to a door through which a process sends and receives messages.
- **Socket Communication**: Sockets serve as the interface between the application layer and the transport layer within a host. They are the `Application Programming Interface (API)` for building network applications.

<img src="https://lh3.googleusercontent.com/pw/ADCreHfjdzN47NUqgVF6Leej5qKJLDdzI8ZtOIy4Gbq0EoANrQ8oLeWTBbPJ8_oCehHJG19bSWgldXa86Lw0hEMNjGrGleqJBJijilrs61lEfjnfH26Fy_3VD2Qe_voulh7kOk_Q0Fk0nfvo490o_3tOK-zZ=w1920-h940-s-no" width="680" height="350">

- **Addressing Processes**: To send messages between processes, the receiving process's address needs to be specified. This includes the host's IP address and a destination port number. The IP address identifies the host, while the port number identifies the specific receiving process.
- **Port Numbers**: Popular applications are assigned specific port numbers (e.g., Web server on port 80, mail server on port 25)

## 2.2 Transport Services (TCP & UDP)

- **Choosing a Transport Layer Protocol**: When developing an application, you must choose a transport layer protocol that suits your application's needs. The choice is typically based on services such as reliable data transfer, throughput, timing, and security.
- **Reliable Data Transfer**: Reliable data transfer ensures that data sent by one end is delivered correctly and completely to the other end. Some applications require this service to prevent data loss, while others like multimedia apps can tolerate some loss.
- **Throughput**: Throughput is the rate at which bits are delivered from the sender to the receiver in a communication session. Bandwidth sensitive applications need guaranteed throughput.
- **Timing**: Timing guarantees are essential for real time applications like Internet telephony, teleconferencing, and multiplayer games. Low delay is crucial for their effectiveness.
- **Security**: Transport protocols can provide security services like encryption for confidentiality, data integrity, and end-point authentication.
- **UDP and TCP**: The Internet offers two transport layer protocols: UDP and TCP. UDP is lightweight and provides unreliable data transfer, while TCP offers reliable data transfer and connection oriented services.
- **Services Not Provided**: Neither UDP nor TCP provide throughput or timing guarantees, which means the Internet cannot guarantee specific timing or throughput for time sensitive applications.

> the Internet community has developed an enhancement for TCP, called Transport Layer Security (TLS) [RFC 5246]. TCP enhanced with TLS not only does everything that traditional TCP does but also provides critical process to process security services, including encryption, data integrity, and end point authentication.

> In particular, if an application wants to use the services of TLS, it needs to include TLS code (existing, highly optimized libraries and classes) in both the client and server sides of the application. TLS has its own socket API that is similar to the traditional TCP socket API. 

## 2.3 Application Layer Protocol: Web and HTTP

- **HTTP**: `HTTP (HyperText Transfer Protocol)` is the primary application layer protocol of the World Wide Web. It relies on client and server programs that communicate by exchanging HTTP messages. Web pages are composed of objects, which are individual files with unique URLs.
- **Web Page Structure**: A web page typically includes a base HTML file and referenced objects like images, stylesheets, and videos. Objects are identified by URLs, which consist of a `hostname` and a `path name`.
- **HTTP and TCP**: HTTP uses TCP (Transmission Control Protocol) as its underlying transport protocol. Clients initiate TCP connections with servers to exchange HTTP messages.

> The HTTP client first initiates a TCP connection with the server. Once the connection is established, the browser and the server processes access TCP through their socket interfaces. 

> HTTP need not worry about lost data or the details of how TCP recovers from loss or reordering of data within the network. 

- **Stateless Protocol**: HTTP is a stateless protocol, meaning servers `don't store client specific information.` If a client requests the same object multiple times, the server doesn't remember previous requests.
- **HTTP Versions**: `HTTP/1.0` and `HTTP/1.1` are common versions, with HTTP/1.1 supporting persistent connections. Newer versions like HTTP/2 are also emerging.
### 2.3.1 Non Persistent and Persistent Connections 
- Non persistent connections create a new connection for each requested object. Persistent connections allow multiple objects to be sent over the same connection, improving efficiency.

> Although HTTP uses persistent connections in its default mode, HTTP clients and servers can be configured to use non persistent connections instead.

> round trip time (RTT), which is the time it takes for a small packet to travel from client to server and then back to the client. The RTT includes packet propagation delays, packet queuing delays in intermediate routers and switches, and packet processing delays.

<img src="https://lh3.googleusercontent.com/pw/ADCreHcP6O3E33NG7QnDmzX1ZUYsYN4WdmaGZSwLxO79aCwgpc2VRQI5lV8oSDjGyga6BN6nbLTnXzZnfZe49s3o9JvbZT35Z1vqiUG1c97LMJbYwZwMhTgBitpNwl5znilEEFnGID7QpG4z98mGuwdk6xPg=w1456-h1130-s-no" width="520" height="520">

- The three way handshake involves the client and server exchanging messages, taking one round trip time (RTT) for this process.
- After completing the handshake, the client sends an HTTP request message along with an acknowledgment into the TCP connection.
- The server responds by sending the HTML file over the established connection.
- The total response time is approximately two RTTs plus the transmission time for the HTML file.

### 2.3.2 HTTP Message Format: 

- HTTP messages have two types: `request messages` and `response messages`. Request messages include a method (e.g., GET), URL, and HTTP version, followed by header lines. Response messages include a `protocol version`, `status code` (e.g., 200 OK), and `header lines`, followed by the `entity body`. [Click here for more info](https://github.com/VasanthVanan/web-application-hackers-handbook-notes/blob/main/Chapters/Chapter-3%20Web%20Application%20Technologies.md#311-http-requests)

```http
GET /somedir/page.html HTTP/1.1 \r\n
Host: www.someschool.edu \r\n
Connection: close \r\n
User-agent: Mozilla/5.0 Accept-language: fr \r\n
```

```http
HTTP/1.1 200 OK \r\n
Connection: close \r\n
Date: Tue, 18 Aug 2015 15:44:04 GMT \r\n
Server: Apache/2.2.3 (CentOS) \r\n
Last-Modified: Tue, 18 Aug 2015 15:11:03 GMT \r\n
Content-Length: 6821 \r\n
Content-Type: text/html \r\n
\r\n

(data data data data data ...)
```

- **Header Lines**: Header lines provide additional information in HTTP messages. Examples of request headers include `Host`, `Connection`, `User-agent`, and `Accept-language`. Examples of response headers include `Connection`, `Date`, `Server`, `Last-Modified`, `Content-Length`, and `Content-Type`.
- **Status Codes**: Status codes (e.g., 404 Not Found) indicate the result of an HTTP request. [Click here for more info](https://github.com/VasanthVanan/web-application-hackers-handbook-notes/blob/main/Chapters/Chapter-3%20Web%20Application%20Technologies.md#318-status-codes)
- **HTTP Methods**: HTTP includes methods like GET, POST, PUT, and DELETE for different types of requests and actions.
- **Entity Body**: The entity body in HTTP messages contains data related to the request method.

### 2.3.3 Cookies

- There are situations where web sites need to identify users, for security or personalization purposes. HTTP uses cookies to achieve user identification and tracking.
- Cookies have four components: a `cookie header` in HTTP `response` and `request` messages, a `cookie file` on the user's end system, and a `back end database` on the web site.

<img src="https://lh3.googleusercontent.com/pw/ADCreHfFwZxxD_q6lNIW4MotRBCCUUmdnTTAst3aa9ByyF1GoQxQGr9oMpd-3hudmL_VvIqWZhb9sF4Dsw3YYeV3N38qe-HBB9afByGpuvPj1gSfGeATqwiuckHdhRVluCOM_E_NXmqLYBN82wpku2x_BUd1=w1326-h1130-s-no" width="620" height="630">

- Cookies work by sending a unique identification number in a `Set-cookie` header from the server to the user's browser.
- The browser stores the identification number and sends it back to the server in a Cookie header with subsequent requests.
- Cookies are used to track user activity and offer personalized services, like shopping carts or recommendations.
- Users can be identified over multiple sessions by maintaining the same identification number in cookies.
- Cookies are controversial due to potential privacy concerns, as websites can gather and potentially sell user information.

### 2.3.4 Web Caching

- Web caches, or proxy servers, handle HTTP requests on behalf of origin servers.
- Caches store copies of requested objects, reducing the need to fetch them from the origin server.
- Users can configure browsers to direct requests through caches for faster responses.
- Caches serve as both servers (to clients) and clients (to servers) in the caching process.
- Installed by ISPs and institutions, caches enhance performance and lower bandwidth costs.
- Benefits include faster responses, reduced bandwidth upgrades, and decreased overall Internet traffic.
- Content Distribution Networks (CDNs) use distributed caching to localize content delivery.

> An HTTP request message is a so called conditional GET message if (1) the request message uses the GET method and (2) the request message includes an If-Modified-Since: header line.

- `Conditional GET` checks for freshness by comparing the `If-Modified-Since` header with object modification date.
- If an object hasn't changed, a `304 Not Modified response` allows the cache to serve the locally cached object.

> value of the If-modified-since: header line is exactly equal to the value of the Last-Modified: header line that was sent by the server initially.

### 2.3.5 HTTP/2

> The primary goals for HTTP/2 are to reduce perceived latency by enabling request and response multiplexing over a single TCP connection, provide request prioritization and server push, and provide efficient compression of HTTP header fields.

- **HTTP/2 Motivation:** HTTP/1.1's persistent TCP connections caused HOL blocking. Browsers used multiple parallel TCP connections to work around this issue.

> Head of Line (HOL) blocking: occurs when a web page has a large video clip and numerous small objects. With a slow bottleneck link, the video clip causes delays for small objects queued behind it. 

- **HTTP/2 Solution (Framing):** Reduces the need for parallel TCP connections by breaking messages into frames and interleaving them, significantly reducing user perceived delay. Includes binary frame encoding for efficiency.

> The ability to break down an HTTP message into independent frames, inter leave them, and then reassemble them on the other end is the single most important enhancement of HTTP/2.

- **Message Prioritization:** Developers assign weights (1-256) to messages, and the server prioritizes higher weight responses. Clients can specify message dependencies.
- **Server Push:** Enables sending additional objects to the client without explicit requests, reducing latency.
- **HTTP/3 and QUIC:** QUIC, a new transport protocol over UDP and supports features like message multiplexing, is used for HTTP/3. This streamlined design incorporates HTTP/2 features and leverages QUIC's advantages.

## 2.4 Application Layer Protocol: SMTP

> A typical message starts its journey in the sender’s user agent, then travels to the sender’s mail server, and then travels to the recipient’s mail server, where it is deposited in the recipient’s mailbox. Reattempts are often done every 30 minutes

### 2.4.1 Email Components

- **User Agents:** Tools like Microsoft Outlook, Apple Mail, and Gmail, allowing users to manage emails.
- **Mail Servers:** The central infrastructure, hosting mailboxes for recipients like Bob.
- **SMTP (Simple Mail Transfer Protocol):** The principal protocol to send emails between servers.

### 2.4.2 SMTP Basics

- SMTP transfers messages between sender and recipient mail servers at `Port 25`.
- The client (sender's server) initiates a connection to the recipient's server via TCP.

```http
S: 220 hamburger.edu 
C: HELO crepes.fr
S: 250 Hello crepes.fr, pleased to meet you 
C: MAIL FROM: <alice@crepes.fr>
S: 250 alice@crepes.fr ... Sender ok
C: RCPT TO: <bob@hamburger.edu>
S: 250 bob@hamburger.edu ... Recipient ok
C: DATA
S: 354 Enter mail, end with ”.” on a line by itself 
C: Do you like ketchup?
C: How about pickles?
C: .
S: 250 Message accepted for delivery
C: QUIT
S: 221 hamburger.edu closing connection
```

- It introduces the sender and recipient, transmits the message, and uses a persistent connection for multiple messages.

### 2.4.3 Mail Message Structure

- Email messages consist of a `header` and a `body`.
- The header includes sender and recipient information, such as `"From," "To," and "Subject."`. The header lines and the body of the message are separated by a blank line (that is, by CRLF).
- These headers are distinct from SMTP commands used for server handshake communication.

```http
From: alice@crepes.fr
To: bob@hamburger.edu
Subject: Searching for the meaning of life.
```

### 2.4.4 Mail Access Protocols

<img src="https://lh3.googleusercontent.com/pw/ADCreHe0fyv4ULp_uamEsceCVVhVIa89gH-EYeg8F5QQ9MJOB6_HPfvS8QpwFXnBbpd_o5WBJf3SYSGt_KwkgMRBQ0BUtFHZP6lTTLAvPlWIfoypiutfj2fm5ZktaSNOuMKNcfdEXXH7OafVuFbqC-vIcfa4=w1876-h442-s-no" width="680" height="220">

> Bob’s user agent can’t use SMTP to obtain the messages because obtaining the messages is a pull operation, whereas SMTP is a push protocol.

- Users retrieve their email messages from a shared mail server using either HTTP or IMAP.
- HTTP is often used for web based email clients like Gmail, while IMAP is common with clients like Microsoft Outlook.
- Both the HTTP & IMAP approaches allow to manage folders, move messages to folders, delete messages, mark messages as important, and so on.

## 2.5 Application Layer Protocol: DNS

- DNS is an essential service that translates human friendly hostnames into IP addresses.
- It's a distributed database and an application layer protocol, implemented with `DNS servers`, often running `BIND` software and runs over UDP and uses port 53.

> People prefer the more mnemonic hostname identifier, while routers prefer fixed length, hierarchically structured IP addresses.

- DNS services include:
    - **hostname aliasing**: host with a complicated canonical hostname can have one or more alias names.
        ```http
        relay1.west-coast.enterprise.com (canonical/official website name)
        enterprise.com (alias name)
        ```
    - **Mail Server Aliasing**: DNS resolves alias hostnames to canonical forms for mail servers and retrieves corresponding IP addresses.
        ```http
        bob@yahoo.com --> bob@relay1.west-coast.yahoo.com
        ```
    - **Load Distribution**: DNS balances traffic among replicated servers by rotating IP addresses within replies, ensuring even distribution. This technique is also applied to email servers with shared alias names.

### 2.5.1 How DNS Works: High Level Overview

> gethostbyname() is the function call that an application calls in order to perform the translation.

- DNS operates through query and reply messages using UDP datagrams on port 53.
- DNS queries involve multiple servers globally distributed.
- A simple centralized design for DNS is not feasible due to scalability issues.
- Issues with centralized design: `single point of failure,` `high traffic volume`, `distant database`, and `maintenance`.
- DNS uses a hierarchical structure and a distributed database., to handle the vast number of hosts on the Internet.

### 2.5.2 Distributed, Hierarchical Database


<img src="https://lh3.googleusercontent.com/pw/ADCreHfjtoFE2ozufMyfFi_xvvvjhwhvWHxaFcgn2jVCEG50nQsaQlTqhQQovC1HaJrlB0h8La--jGtCdoz8c6RxZVLsou9ISfsTEy7uaS-fjCMZNBiZtfTPnFpbVbYUDbQ49wyFPKQJJNUc-_7h4wmYm2IX=w1920-h710-s-no" width="580" height="220">

- DNS uses three classes of servers: `Root` DNS servers, `top level domain (TLD)` DNS servers, and `authoritative` DNS servers.
- Root DNS servers provide IP addresses for TLD servers. TLD servers provide IP addresses for authoritative DNS servers Authoritative DNS servers store DNS records for specific organizations.
- A `local DNS server`, specific to an ISP, also plays a crucial role in DNS queries. It cache DNS information to reduce query traffic and improve performance.

> When a host makes a DNS query, the query is sent to the local DNS server, which acts a proxy, forwarding the query into the DNS server hierarchy.

- DNS extensively utilizes caching to enhance performance. These are stored temporarily and it allows DNS servers to quickly respond to subsequent queries for the same hostname.

### 2.5.3 Recursive vs Iterative DNS Queries

<img src="https://lh3.googleusercontent.com/pw/ADCreHcDzbK4FY8RExEyQO82XsXu_OFQXOqMXfJx6q8TO_Tx4wYRJt9WaiktQr-4bF482giEXJmGfkLMszdS7Ufn_sjYphuuVOdpKVn7RI6laoHLFKiPUtFpU_kOUnZ_UJN7NjeJdt5Bp65KPlNKkiEpFrIF=w1708-h1044-s-no" width="780" height="520">

### 2.5.4 DNS Records & Messages

- DNS servers store resource records (RRs) in the distributed database.
- A resource record (RR) is a four tuple: `(Name, Value, Type, TTL)`.
- **TTL** (Time to Live) determines when a resource should be removed from a cache.
- Types of resource records:
    - `Type=A`: Maps hostname to IP address.
    - `Type=NS`: Maps a domain to the hostname of an authoritative DNS server.
    - `Type=CNAME`: Provides the canonical name for an alias hostname.
    - `Type=MX`: Maps to the canonical name of a mail server with an alias hostname.

> To obtain the canonical name for the mail server, a DNS client would query for an MX record; to obtain the canonical name for the other server, the DNS client would query for the CNAME record.

- DNS messages have a header section with several fields, including `query/reply` flags, `recursion` flags, and more.
- DNS messages consist of a question section, answer section (resource records), authority section, and additional section.

> A 1 bit query/reply flag indicates whether the message is a query (0) or a reply (1). A 1 bit authoritative flag is set in a reply message when a DNS server is an authoritative server for a queried name. 

> A 1 bit recursion desired flag is set when a client (host or DNS server) desires that the DNS server perform recursion when it doesn’t have the record. 

> A 1 bit recursion available field is set in a reply if the DNS server supports recursion. 

### 2.5.5 Inserting Records to DNS Database

> A registrar is a commercial entity that verifies the uniqueness of the domain name, enters the domain name into the DNS database (as discussed below), and collects a small fee from you for its services.

- To register a domain name, you need to provide registrar with DNS server names and IP addresses. Registrar enters `Type NS` and `Type A` resource records for `authoritative` DNS servers into `TLD` servers.
- Additional resource records, like Type A and Type MX, must be added for Web and mail servers.

