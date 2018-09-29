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

In this case, we can call this "connection switching", the source asks the switch to reserve resources in advance. On each packet, the source writes a unique identifier so that the switch can recognise it. However, if the source goes silent, the resources are still reserved, and other sources cannot use it. We resources are reserved per active connections, and that admission control & forwarding decisions are made per connection.

Packet switching makes an efficient use of ressources, but has a unpredictable performance. On the other hand, "connection switching" has a predictable performance and makes an inefficient use of ressources.  
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