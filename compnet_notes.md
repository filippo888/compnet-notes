
# Computer networks

## Introduction 

The internet is a computer network that interconnects many devices such as computer, mobile phones, TV, gaming consoles, servers and wearables (and many more!), throughout the world. All these devices are called **hosts** or **end systems**. Hosts are all connected with **communication links** and **packet switches** (most commonly routers and link-layer switches).  
End systems access the internet through **internet service providers** (ISPs).

![](global_view.png)

### What's underneath the internet ?

All end systems on the network are connected to the internet with direct links and switches. Let's zoom in and explore the direct link between our machine and a network switch.

#### Digital Subscriber line

![](dsl.png)

A residence obtains DSL access from the local telephone company. A DSL modem uses the existing telephone line to exchange data with a digital subscriber line access multiplexer (DSLAM). Telephone lines are made of twisted copper wire containing three different channels: a downstream data channel, an upstream data channel and a 2-way phone channel.  
From an infrastructural point of view, we use telephone lines to avoid having to lay down new infrastructure while we can make use of the existing one.

#### Cable internet access

Similarly to DSL, cable internet access makes use of the existing cable television infrastructure. However, unlike DSL, there is no direct link to the switch. Instead, each end system is connected to a neighbourhood-level junction using coaxial copper wiring which is then connected to a cable modem termination system (CMTS) with fibre. It is a shared broadcast medium.

#### Other types of access types

There are many other types of service providers. We can mention universities, cellular, satellite, fibre to the home. The key question to ask when trying to provide a population with internet access is whether or not we can use physical infrastructure that is already available. 

### Who owns the infrastructure ?

We know that ISPs provide access to our devices and end-systems. However, the internet's structure is much more complex that having a direct link between the ISP and the host. The goal is to achieve interconnectivity between ISP so that all end systems can communicate with each other. However, linking all ISPs together would be very costly. The structure that is in place today is a three-tier hierarchy, called network structure 5. It consists of access ISPs, regional or global ISPs and tier 1 ISPs. There exists a customer-provider relationship at all levels of the hierarchy: customers pay the provider for access. ISPs at the same level can have a peering agreement so that all the traffic between them passes through a direct link rather than the level above. Also, there are Internet Exchange Points (IXPs) which are meeting points for the ISPs to interconnect. Lastly, there are emerging **content provider networks**, like Google, that connect directly (or via an IXP) to access ISPs to bypass upper tiers.

![](net5.png)

### How does it work ?

The internet is organised around a specific layered architecture. Every layer has its protocol(s) and well-defined interfaces to upper/lower layers. Protocols of various layers are called the **protocol stack**.

1. *The application layer* includes protocols such as HTTP, SMTB and FTP. An application-layer protocol is distributed over multiple end systems, with the application in one end system using the protocol to exchange packets of information with the application in another end system. Information exchanged at this layer will be referred to as a **message**.
2. *The transport layer* transports application-layer messages between application endpoints. There are two transport protocols: TCP and UDP. A transport-layer packet is referred to as a **segment**.
3. *The network layer* includes the IP protocol. All Internet components that have a network layer must run the IP protocol. It also includes many routing protocols. It is often called the IP layer. A network-layer packet is referred to as a **datagram**.
4. *The link layer* is responsible to deliver packets from transmitting node to receiving node in a reliable way. A link-layer packet is referred to as a **frame**.
5. *The physical layer* has to move individual bits within the frame from the link layer, from one node to the next. The protocols on this layer depends on the actual transmission medium of the link.

At the sending host, when the application-layer message is passed to the transport layer, that layer takes the message and appends additional information (transport-layer header information) that will be used by the receiving-side transport layer. The transport-layer encapsulates the application-layer message, and then passes it to the network layer which adds network-layer header information, creating a network-layer datagram, and so on. On the receiving side, each layer will decapsulate the corresponding header information.

