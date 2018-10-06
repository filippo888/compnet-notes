# The internet and Computer Networks 

The internet is a computer network that interconnects many devices such as computer, mobile phones, TV, gaming consoles, servers and wearables (and many more!), throughout the world. All these devices are called **hosts** or **end systems**. Hosts are all connected with **communication links** and **packet switches** (most commonly routers and link-layer switches).  
End systems access the internet through **internet service providers** (ISPs).

![](global_view.png)

## What is the internet made of?

All end systems on the network are connected to the internet with direct links and switches. Let's zoom in and explore the direct link between our machine and a network switch.

### Digital Subscriber line

![](dsl.png)

A residence obtains DSL access from the local telephone company. A DSL modem uses the existing telephone line to exchange data with a digital subscriber line access multiplexer (DSLAM). Telephone lines are made of twisted copper wire containing three different channels: a downstream data channel, an upstream data channel and a 2-way phone channel.  
From an infrastructural point of view, we use telephone lines to avoid having to lay down new infrastructure while we can make use of the existing one.

### Cable internet access

Similarly to DSL, cable internet access makes use of the existing cable television infrastructure. However, unlike DSL, there is no direct link to the switch. Instead, each end system is connected to a neighbourhood-level junction using coaxial copper wiring which is then connected to a cable modem termination system (CMTS) with fibre. It is a shared broadcast medium.

### Other access types

There are many other types of service providers. We can mention universities, cellular, satellite, fibre to the home. The key question to ask when trying to provide a population with internet access is whether or not we can use physical infrastructure that is already available. 

## Who owns and manages the internet ?

We know that ISPs provide access to our devices and end-systems. However, the internet's structure is much more complex that having a direct link between the ISP and the host. The goal is to achieve interconnectivity between ISP so that all end systems can communicate with each other. However, linking all ISPs together would be very costly. The structure that is in place today is a three-tier hierarchy, called network structure 5. It consists of access ISPs, regional or global ISPs and tier 1 ISPs. There exists a customer-provider relationship at all levels of the hierarchy: customers pay the provider for access. ISPs at the same level can have a peering agreement so that all the traffic between them passes through a direct link rather than the level above. Also, there are Internet Exchange Points (IXPs) which are meeting points for the ISPs to interconnect. Lastly, there are emerging **content provider networks**, like Google, that connect directly (or via an IXP) to access ISPs to bypass upper tiers.

![](net5.png)

## How does communication happen ?

The internet is organised around a specific layered architecture. Every layer has its protocol(s) and well-defined interfaces to upper/lower layers. Protocols of various layers are called the **protocol stack**.

1. *The application layer* includes protocols such as HTTP, SMTB and FTP. An application-layer protocol is distributed over multiple end systems, with the application in one end system using the protocol to exchange packets of information with the application in another end system. Information exchanged at this layer will be referred to as a **message**.
2. *The transport layer* transports application-layer messages between application endpoints. There are two transport protocols: TCP and UDP. A transport-layer packet is referred to as a **segment**.
3. *The network layer* includes the IP protocol. All Internet components that have a network layer must run the IP protocol. It also includes many routing protocols. It is often called the IP layer. A network-layer packet is referred to as a **datagram**.
4. *The link layer* is responsible to deliver packets from transmitting node to receiving node in a reliable way. A link-layer packet is referred to as a **frame**.
5. *The physical layer* has to move individual bits within the frame from the link layer, from one node to the next. The protocols on this layer depends on the actual transmission medium of the link.

At the sending host, when the application-layer message is passed to the transport layer, that layer takes the message and appends additional information (transport-layer header information) that will be used by the receiving-side transport layer. The transport-layer encapsulates the application-layer message, and then passes it to the network layer which adds network-layer header information, creating a network-layer datagram, and so on. On the receiving side, each layer will decapsulate the corresponding header information.

## How do we communicate over a network ?

In a network application, end systems exchange messages with each other. To send a message to a destination, the source breaks that messages into smaller chunks of data known as **packets**. To reach the destination, each packet travels from the source through communication links and **packet switches**.  
A switch contains a buffer used to store the data and a forwarding table used to store metadata and indicate where to send the data. In order to send incoming packets to their destination efficiently, we must use resource management. 

### Treating packets on demand: packet switching

In this case, incoming packets are stored in the buffer and then sent to their destination. However, when the buffer becomes full, the switch cannot handle more packets, and the incoming packets are lost. We say packets are treated on demand, and that admission control & forwarding decisions are made per packet. 

### Reserve resources in advance

In this case, we can call this "connection switching", the source asks the switch to reserve resources in advance. On each packet, the source writes a unique identifier so that the switch can recognise it. However, if the source goes silent, the resources are still reserved, and other sources cannot use it. We say resources are reserved per active connections, and that admission control & forwarding decisions are made per connection.

Packet switching makes an efficient use of ressources, but has an unpredictable performance. On the other hand, "connection switching" has a predictable performance and makes an inefficient use of ressources.  
Today's internet uses packet switching. It is easier to implement, but requires congestion control.  

**Congestion control** uses statistical multiplexing in order to manage the available resources. It uses statistical data on user activities and predicts their behaviour to know how to allocate resources. 


## Measuring network performances

We use the following metrics to measure the performance of a network.

### Packet loss

Packet loss consists of the fraction of the packets sent from a source that were dropped before reaching their destination.

### Packet delay (latency)

Packet delay consists of the time needed to send a packet from its source to its destination. A packet goes through the following processes:

- Transmission: bits pushed into a link one by one 
- Propagation: bits travel along a link 
- Queuing: packets wait to be processed
- Processing: packet is being processed

During each of the above steps, a delay is introduced. 

#### Transmission delay 

If the length of the packet is L bits, and the **transmission rate** of the link is R bits/sec, then the 
**transmission delay** is L/R. This is the amount of time required to push (transmit) all of the packet's bits into the link. Typically on the order of microseconds to milliseconds.

#### Propagation delay

**Propagation delay** is the time required to push all the bits from the beginning of the link to the next switch. The bit propagates at the propagation speed of the link, which depends on the link's physical medium. The propagation delay is the distance between the two switches divided by the propagation speed of the link.

#### Queuing delay

At the queue, a packet experiences **queuing delay** as it waits to be transmitted on the link. It depends on the number of packets that have arrived earlier. If the queue is empty, the queuing delay will be 0.  
The queuing delay is characterised with statistical measures such as the average queuing delay, the variance of the queuing delay, and the probability that it exceeds a certain value. It also depends on the traffic pattern: the arrival rate at the queue, the nature of incoming traffic and the transmission rate of the outgoing link.  
If the  bit arrival rate is larger than the bit departure rate, then the queuing delay approaches infinity. Otherwise, it depends on burst size.

#### Processing delay

The **processing delay** consists of the time required to examine the packet's header and determine where to direct the packet.

### Throughput

Another critical performance measure is end-to-end throughput. The **instantaneous throughput** at any time is the rate (in bits/sec) at which the host is receiving the file. If the file consists of F bits and and the transfer takes T seconds for the host to receive the file, the the **average throughput** of the file transfer is F/T bits/sec. For a simple two-link network, the throughput is the transmission rate of the **bottleneck link**.

### Security

Here, we measure the ability of the network to react to adversarial behaviour.

#### Malware

Among all the files on the Internet, we are sometimes exposed to **malware**. Once malware infects our device, it can do all kinds of bad things such as deleting files, sending emails, installing spyware...  
Most malware nowadays is self-propagating. It can spread in the form of a virus or of a worm. **Viruses** require some sort of user interaction to infect a device. **Worms** can infect a device without any explicit interaction (i.e from a network application).

#### Denial of Service attacks

A DoS attack renders a network, host or other piece of infrastructure unusable by legitimate users. DoS attacks fall under three categories:

- Vulnerability attack: well crafted messages are sent to a vulnerable application or operating system on a target host. If the right sequence of packets are sent, the service can stop or the host might crash.
- Bandwidth flooding: the attacker sends a deluge of packets to the targeted host so that the target’s access link becomes clogged, preventing legitimate packets from reaching the server.
- Connection flooding: a large number of TCP connections are established at the target host. The host can become so bogged down by the bogus connections that it stops accepting legitimate connections.

In a distributed DoS attack (**DDoS**) the attacker controls multiple sources that each blast traffic at the target. 

#### Sniffing

A packet sniffer is a passive receiver that records a copy of every packet that passes through the network. Because they are passive, sniffers are hard to detect. 

#### Spoofing

Spoofing is the ability to inject packets into the internet with a false source address. To solve this we will need end-point authentication. 

# Application Layer

In a network, two different end systems communicate using processes at the application layer. A **process** is a piece of code that belongs to the application layer.  
Before diving into software coding, one should have a broad architectural plan for a network application: it dictates how to application is structured over different end systems.  

## Network applications architectures

An application developer will likely draw on one of the two predominant architectural paradigms used in modern applications: the **client-server** architecture or the **peer-to-peer** architecture. 

### The client-server architecture 

![](client_server_architecture.png)

In a client-server architecture, there is an always-on host, called the **server**, which services requests from many other hosts called **clients**. With this architecture, the clients don't directly communicate with each other. Another characteristic is that the server has a fixed IP address. Since the server is always on and has a fixed address, the client can always contact the server by sending a packet to the server's IP address.

Often in a client-server application, the server is incapable of keeping up with all the requests from clients. For this reason, a **data center**, housing a large number of hosts, is often used to create a powerful virtual server. 

### The peer-to-peer architecture

In a peer-to-peer architecture there is very little or no reliance on dedicated servers in data centres. Instead the application exploits direct communication between pairs of intermittently connected hosts, called **peers**. The peers are not owned by service providers, but are instead desktops and laptops controlled by users, with most of the peers residing in homes, universities or offices.

Most of today's traffic-intensive applications are based on P2P architectures: file sharing (BitTorrent), internet telephony (Skype), IPTV...

One of the most compelling features of a peer-to-peer architecture is self-scalability. In a P2P file-sharing application, even though each peer generates workload by requesting files, each peer also adds service capacity to the system by distributing files to other peers. Also, P2P architectures are cost effective since they don't require significant server infrastructure. 

Future P2P applications face three major challenges:

1. **ISP Friendly**: most residential ISPs have been dimensioned for asymmetrical bandwidth usage, that is, for much more downstream traffic than upstream traffic. But P2P video streaming and file distribution applications shift upstream traffic from servers to residential ISPs, thereby putting significant stress on the ISPs.
2. **Security**: because of their highly distributed and open nature, P2P applications can be a challenge to secure.
3. **Incentives**: the success of future P2P applications also depends on convincing users to volunteer bandwidth, storage, and computation resources to the applications, which is the challenge of incentive design.


## Network application transport services

Processes on two different end systems communicate with each other by exchanging messages across the computer network. A sending process creates and sends messages into the network; a receiving process receives these messages and possibly responds by sending messages back.
 
When a process delivers or receives a message to/from the transport layer, some delivery guaranties are needed: this is the goal of a transport-layer protocol.
How to choose among the available transport-layer protocols? We need to study the services offered by that protocol and then pick the one that best matches our application's needs.

We can classify the possible services along four dimensions: reliable data transfer, throughput, timing, and security.

### Reliable data transfer

As seen earlier, a packet can get lost within a network. For many applications such as document transfers, financial transactions and e-mail, data loss can have devastating consequences. To support these applications, something has to be done to guarantee that the data sent by one end of the application is delivered correctly and completely to the other end of the application. If a protocol does so, we say it offers **reliable data transfer**. 
 
When a transport-layer protocol doesn't offer reliable data transfer, some data sent by the sending process may never arrive and the receiving process. This may be acceptable for loss-tolerant applications such as conversational audio/video or online streaming where lost data might result in a small glitch in the audio/video—not a crucial impairment.

### Throughput

Multiple sessions will be sharing the bandwidth along the network path, and because these other sessions will be coming and going, the available throughput can fluctuate with time. These observations led to another natural service that a transport-layer protocol could provide, namely, guaranteed available throughput at some specified rate. Such a guaranteed throughput service would appeal to many applications that have throughput requirements, known as **bandwidth-sensitive applications**.

### Timing

A transport-layer protocol can also provide timing guarantees. Such a service would be appealing to interactive real-time applications, such as Internet telephony, virtual environments, teleconferencing, and multiplayer games, all of which require tight timing constraints on data delivery in order to be effective. For non-real-time applications, lower delay is always preferable to higher delay, but no tight constraint is placed on the end-to-end delays.

### Security

A transport protocol can provide an application with one or more security services: providing confidentiality between the two processes, data integrity and end-point authentication.

## Transport services provided by the internet

The Internet makes two transport protocols available to applications, UDP and TCP. When an application developer creates a new network application for the Internet, one of the first decisions he has to make is whether to use UDP or TCP. 

### TCP services

The TCP service model includes a **connection-oriented** service and a **reliable data transfer** service. When an application invokes TCP as its transport protocol, the application receives both of these services from TCP.

**Connection oriented service**: TCP has the client and server exchange transport-layer control information with each other before the application-level messages begin to flow. This so-called **handshaking procedure** alerts the client and server, allowing them to prepare for an onslaught of packets. When the application finishes sending messages, it must tear down the connection. We say TCP is "statefull", in a way it maintains state on communicating processes.

### UDP services


UDP is a no-frills, lightweight transport protocol, providing minimal services. It is **connectionless** or "stateless": there is no handshaking before the two processes start to communicate. UDP provides an **unreliable data transfer** service.

### Services not provided by UDP and TCP

TCP and UDP provide no performance guarantees. Concerning security guarantees, it's complicated. We use a **Secure Socket Layer** (**SSL**),  a library that implements a bunch of security ready functions (encrypt, decrypt, check integrity).

## Example: the web

The **HyperText Transfer Protocol** (**HTTP**), the Web’s application-layer protocol, is at the heart of the Web. It is implemented in two programs: a client program and a server program. The client program and server program, executing on different end systems, talk to each other by exchanging HTTP messages. HTTP defines the structure of these messages and how the client and server exchange the messages. 

### Web pages

A webpage is a collection of objects. An object is just a file (it can be an HTML file, a Java applet, a JPEG image...) addressable by a single URL. Most web pages consist of a **base HTML file** and several referrenced objects. 

HTTP defines how Web clients request Web pages from Web servers and how servers transfer Web pages to clients. There are few possible types of HTTP requests, 

- GET: client requests to download a file
- HEAD: client requests file metadata
- POST: client provides information
- PUT: client requests to upload a file
- DELETE: client requests to delete a file

and a few possible types of HTTP responses: OK, Not Found, Moved permanently, Bad request. Because an HTTP server maintains no information about the clients, HTTP is said to be a **stateless protocol**.

### Non-Persistent and Persistent Connections

When a client-server interaction is taking place over TCP, the application developer needs to make an important decision––should each request/response pair be sent over a separate TCP connection, or should all of the requests and their corresponding responses be sent over the same TCP connection? In the former approach, the application is said to use **non-persistent connections**; and in the latter approach, **persistent connections**.

In the case of **non-persistent connections**, a TCP connection is required to transfer one object (one request message and one response message). After the object is received, the connection is closed. 

![](time_to_request_receive_html_file.png)

The time needed to send one object over the network is two Round Trip Time (RTT) plus the transmission time at the server of the HTML file.

In the case of **persistent connections**, the server leaves the TCP connection open after sending a response. Subsequent requests and responses between the same client and server can be sent over the same connection: an entire webpage can be sent over the same persistant TCP connection. Requests for objects can be made back-to-back, without waiting for replies to pending requests. Typically, the HTTP server closes a connection when it isn’t used for a certain time (a configurable timeout interval).

The default mode of **HTTP uses persistent connections with pipelining**. 

### User-Server Interaction: Cookies

We mentioned that an HTTP server is stateless. This simplifies server design and has permitted engineers to develop high-performance Web servers that can han- dle thousands of simultaneous TCP connections.  
However, it is often desirable for a website to identify users, either because the server wishes to restrict user access or because it wants to serve content as a function of the user identity. For this purpose, HTTP uses **cookies**. Cookies allow sites to keep track of users.

![](cookies.png)

Cookies can thus be used to create a user session layer on top of stateless HTTP. Although cookies often simplify the Internet experience for the user, they are controversial because they can also be considered as an invasion of privacy.

### Web Caching 

A **web cache**—also called a **proxy server**—is a network entity that satisfies HTTP requests on the behalf of an origin web server. The web cache has its own disk storage and keeps copies of recently requested objects in this storage.

Suppose a browser is requesting an object. Here is what happens:

1. The browser establishes a TCP connection to the Web cache and sends an HTTP request for the object to the web cache.
2. The web cache checks to see if it has a copy of the object stored locally. If it does, the web cache returns the object within an HTTP response message to the client browser.
3. If the web cache does not have the object, the Web cache opens a TCP connec- tion to the origin server. The Web cache then sends an HTTP request for the object into the cache-to-server TCP connection. After receiving this request, the origin server sends the object within an HTTP response to the web cache.
4. When the web cache receives the object, it stores a copy in its local storage and sends a copy, within an HTTP response message, to the client browser (over the existing TCP connection between the client browser and the Web cache).

Web caching is usefull for two reasons. First, it can substantially reduce the response time for a client request. If there is a high-speed connection between the client and the cache, as there often is, and if the cache has the requested object, then the cache will be able to deliver the object rapidly to the client. Second, Web caches can substantially reduce traffic on an institution’s access link to the Internet.

Unfortunately, the copy of an object residing in the cache can be stale. HTTP has a mechanism that allows a cache to verify that its objects are up to date. This mechanism is called the **conditional GET**.
