I will study it from my brother in Christ Tanenbaum because Halsall love to make basic mistakes when it comes to writing a book.

I will solely write about the parts that the professor wants to know, with some added stuff because he isn't clear in what he wants... I dislike him frfr.

# CHAPTER 1: Introduction
This is pretty much an introduction, so I'll write only the important stuff.

A **computer network** is a collection of autonomous computers interconnected by a single technology, and the connection can be **wired** or **wireless**. They also come in many sizes, shapes and forms and networks are connected to create a bigger network, like the **Internet**. 
Note → a distributed system is a software system built on the network. So, the Internet is the network while the **World Wide Web** is the distributed system.

There are many reasons why we use computer networks:
1) Businesses use them to share resources through the **client-server model**, to communicate through emails and calls and other reasons;
2) We use them at home since we can connect to so much stuff through the Internet in the WWW, for **peer-to-peer** communication, like instant messaging and social network applications.
   
There are other reasons but I don't want to explore them right now. If you want, you can read the book that I pushed with the notes of the course.

## Network Hardware
There are two types of transmission technology that are widespread:
1) **Broadcast** links, where the communication network is shared by all the machines on the network. Some broadcast systems also support transmissions to a subset of machines, and it's called **multicasting**;
2) **Point-to-point** links, which connect individual pairs of machines. Point-to-point transmission with exactly one sender and one receiver is called **unicasting**.

We can classify networks also based on the scale:
1) **Personal Area Network**, which lets devices communicate over the range of a person. Examples: a computer connected to its peripherals, with wires or wireless, or **Bluetooth**, which allows wireless connections. The Bluetooth works with a master-slave paradigm.    We also have **embedded medical devices**, like pacemakers, insulin pumps etc.;
2) **Local Area Network**, which is a privately owned network that operates in a single building. There is a standard for wireless LANs called **IEEE 802.11**, also known as **Wi-Fi**, while the standard for wired LANs is **IEEE 802.3**, known as **Ethernet**. (The rest seems more like curiosity things, I won't write about them);
3) **Metropolitan Area Network**, which covers a city. Example: cable television networks;
4) **Wide Area Network** spans over a large geographical area, like a country or a continent, and can be perceived as a connection of LANs... Feels a lot similar to Internetwork, in fact most WANs are also Internetwork;
5) **Internetwork**, which is the connection of two or more networks, like the Internet.

## Network Software

We need to structure also the software for our networks to make it work nowadays. Most of them are organized as a stack of **layers** or **levels**, each one built upon the one below it. The purpose of each layer is to provide services to the higher layers without the details of the implementation of those services.

A **protocol** is an agreement between communicating parties on how communication is to proceed. To send and receive information we need to go below layer 1, where we find the **physical medium** through which communication occurs. There can also happen virtual communication(but for now it is not shown).

Between adjacent layers we have an **interface**, that defines which primitive operations and services the lower layer makes available to the upper one.

A set of layers and protocols is called a **network architecture**, and its specifications must contain enough information to build or write each layer so that it follows the correct protocol. The implementation and specification of the layers is not part of the network architecture. A list of protocols used by a certain system is called **protocol stack**. 

We need to make our network reliable, but:
1) There is a chance that some of the bits received will be damaged. We use **error detection**, which will transmit  the message again until it is received correctly, or **error correction** where we recover the correct message from the possibly incorrect bits. They are used in lower layers to protect packets sent over, and in higher layers to check that the right contents were received;
2) We need to find a working path through a network. Often there are multiple paths between a source and destination, and in a large network, there may be some links or routers that are broken. We do this decision with **routing**;
3) Over time, networks grow larger and new designs emerge that need to be connected to the existing network with the help of **protocol layering**. Every layer needs a mechanism for identifying the senders and receivers involved in that particular message, by **addressing** or **naming** them, for the lower and higher layers respectively;
4) An aspect of growth is that different network technologies often have different limitations. This leads to mechanisms for disassembling, transmitting, and then reassembling messages, known as **internetworking**;
5) Another problem is **resource allocation**. Networks provide a service to hosts from their underlying resources, such as the capacity of transmission lines. Many designs share network bandwidth dynamically rather than by giving each host a fixed fraction of the bandwidth that it may or may not use. This design is called **statistical multiplexing**;
6) When we have a fast sender we don't want fro it to swamp a slow receiver with data. We can solve it with **flow control**. We can also have a **congestion** when too many computers send data and the network cannot deliver it all;
7) The last major design issue is to secure the network by defending it against different kinds of threats, like eavesdropping on communications and mechanisms that provide confidentiality defend against this threat, and they are used in multiple layers. Mechanisms for authentication prevent someone from impersonating someone else.

## Connection-Oriented VS Connectionless Service

Layers can offer two different types of service to the layers above them: **connection-oriented** and **connectionless**.

**Connection-oriented network service**, which the professor calls the **circuit switch**, is modeled after the telephone system: the service user first establishes a connection, uses the connection, and then releases the connection, just like in a telephone system. The essential aspect of a connection is that it acts like a tube.
In some cases when a connection is established, the sender, receiver, and subnet conduct a **negotiation** about the parameters to be used.
A **circuit** is another name for a connection with associated resources, like **fixed bandwidth**.

**Connectionless network service**, even though the professor called it **packet switch**, is modeled after the postal system. Each message carries the full destination address and each one is routed through the intermediate nodes inside the system independent of all the subsequent messages. 

A **packet** is a message at the network layer. While the message travels through intermediate nodes, it can be stored and sent or it can be sent by the node, which can be a switch or a router, before being completely received. These methods are called respectively **store-and-forward switching** and **cut-through switching**.

Each kind of service can further be characterized by its reliability:
1) A typical situation where a reliable connection-oriented service is appropriate is **file transfer**. 
   The owner of the file wants to be sure that all the bits arrive correctly and in the same order they were sent. Reliable connection-oriented service has two minor variations: **message sequences**, where message boundaries are preserved, and **byte streams**, simply a stream of bytes. For some applications though, the transit delays introduced by acknowledgements are unacceptable. One such application is digitized voice traffic for **voice over IP**;
2) Not all applications require connections. In fact, unreliable (meaning not acknowledged) connectionless service is often called **datagram** service:  all that is needed is a way to send a single message that has a high probability of arrival, but no guarantee.  
   What we look for is the convenience of not having to establish a connection to send one message is desired. 
   An **acknowledge datagram** service provides the reliability, and similar to this is the **request-reply** service, commonly used in client-server communication.

A service is formally specified by a set of **primitives** (operations) available to user processes to access the service.  If the protocol stack is located in the operating system, as it often is, the primitives are normally system calls. 
The set of primitives available depends on the nature of the service being provided, and the primitives for connection-oriented service are different from those of connectionless service.
Suffice it to say that having a reliable, ordered byte stream between processes is sometimes very convenient with the primitives.

We need to distinguish  what is a **service** and what is a **protocol**, and also how they collaborate:
- A **service** is a set of primitives (operations) that a layer provides to the layer above it. The service defines what operations the layer is prepared to perform on behalf of its users, but it says nothing at all about how these operations are implemented. 
  A service relates to an interface between two layers, with the lower layer being the service provider and the upper layer being the service user;
- A **protocol**, in contrast, is a set of rules governing the format and meaning of the packets, or messages that are exchanged entities within a layer, and use protocols to implement their service definitions. 
  They are free to change their protocols at will, provided they do not change the service visible to their users.
  
## Reference Models

We will discuss two important network architectures: the **ISO OSI reference model** and the **TCP/IP reference model**.

The **ISO OSI model** has seven layers:

1. **Physical** → is concerned with transmitting raw bits over a communication channel. 
   In this layer we deal with mechanical, electrical and timing design issues, as well as the physical transmission medium;
   
2. **Data link** → it transforms a raw transmission into a line that appears free of undetected transmission errors.  It accomplishes this task by having the sender break up the input data into **data frames** and transmit the frames sequentially. If the service is reliable, the receiver confirms correct receipt of each frame by sending back an **acknowledgement frame**.
   A problem we may deal with in this layer is how to keep a fast transmitter to drown a slow receiver, and we have the solution: a traffic regulation mechanism.
   Broadcast networks have an additional issue in the data link layer: how to control access to the shared channel, and yet again we have a solution: A special sublayer of the data link layer, the **medium** **access** **control** sublayer, deals with this problem;
   
3. **Network** → controls the operation of the subnet. A key design issue is determining how packets are routed from source to destination. The routes can be static, or more often, can be updated automatically, to avoid failed components for example, or can be highly dynamic, determined anew for each packet. If too many packets are present in the subnet at the same time, they will get in one another’s way, forming a **bottleneck**, which the network layer needs to take care of. More generally, the network layer takes care of the quality of service provided;
   
4. **Transport** → accepts data from the layers above, splits it up into smaller units if needed, pass these down, and ensure that the pieces arrive correctly at the other end. All this must be done efficiently and in a way that isolates the upper layers from the inevitable changes in the hardware technology over the course of time. 
   The transport layer also determines what type of service to provide to the **session** **layer**, and, ultimately, to the users of the network. 
   There are various types of transport connection, the ones discussed in the reliability of a service in the previous paragraph. The transport layer is a **true end-to-end layer**; it carries data all the way from the source to the destination;
   
5. **Session** → it allows users on different machines to establish sessions between them, and offers various services, including **dialog control**, **token management** and **synchronization**;
   
6. **Presentation** → is concerned with the syntax and semantics of the information transmitted. In order to make it possible for computers with different internal data representations to communicate, the data structures to be exchanged can be defined in an abstract way, along with a standard encoding to be used "on the wire";
   
7. **Application** → contains a variety of protocols that are commonly needed by users. 
   One widely used application protocol is **HTTP** (HyperText Transfer Protocol), which is the basis for the World Wide Web. When a browser wants a Web page, it sends the name of the page to the server hosting the page using HTTP; the server then sends the page back. 
   Other application protocols are used for file transfer, electronic mail, and network news.
   
The principles that were applied to arrive at the seven layers can be briefly summarized as follows:
1) A layer should be created where a different abstraction is needed;
2) Each layer should perform a well-defined function;
3) The function of each layer should be chosen with a clear definition of internationally standardized protocols;
4) The boundaries between layers should be chosen to minimize the information flow across the interfaces;
5) The number of layers should be large enough that distinct functions are not thrown together in the same layer, and small enough so the architecture does not become unwieldy.
The **ISO OSI model** itself is not a network architecture, it just tells simple standards for all the layers, although these are not part of the reference model itself. 

Now we'll see the **TCP/IP Reference Model**, known for the two primary protocols, **TCP** and **IP**.  We want connections to remain intact as long as the source and destination machines are functioning, even if some of the machines or transmission lines in between were suddenly put out of operation; while also planning a flexible architecture. Because of this we have a connectionless packet-switching network that runs across different networks.  We  will only talk about four layers and their protocols:

1) **Link** **layer** → describes what links such as serial lines and classic Ethernet must do to meet the needs of this connectionless internet layer. It is not really a layer at all, in the normal sense of the term, but rather an interface between hosts and transmission links;
   
2) **Internet** **layer** → holds the whole architecture together: its job is to permit hosts to inject packets into any network and have them travel independently to the destination (potentially on a different network). They may even arrive in a completely different order than they were sent, in which case it is the job of higher layers to rearrange them. 
   Note: ‘‘internet’’ is used here in a generic sense, even though this layer is present in the Internet, our favourite network I guess.
   The internet layer defines an official packet format and protocol called **IP** (Internet Protocol), plus a companion protocol called **ICMP** (Internet Control Message Protocol) that helps it function. The job of the internet layer is to deliver IP packets where they are supposed to go;
   
3) **Transport** **layer** →  is designed to allow peer entities on the source and destination hosts to carry on a conversation, just as in the OSI transport layer. Two end-to-end transport protocols have been defined here: **TCP** (Transmission Control Protocol) and **UDP** (User Datagram Control).
   **TCP**  is a reliable connection-oriented protocol that allows a byte stream originating on one machine to be delivered without error on any other machine in the internet. The byte stream will be first divided and passed to the packets and then sent to the receiving machine, where the receiving TCP process reassembles the messages into the output byte stream. TCP also handles flow control to make sure a fast sender cannot swamp a slow receiver with more messages than it can handle.
   **UDP** is the exact opposite: unreliable, connectionless protocol for applications that do not want TCP’s sequencing or flow control;
   
4) **Application** **layer** → we don't have a session and presentation layer since applications simply include any session and presentation functions that they require. Experience with the ISO OSI model has proven this view correct: these layers are of little use to most applications. Here we have all the high-end protocols like **HTTP** (HyperText Transfer Protocol), **SMTP** (Simple Mail Transfer Protocol), **RTP** (Real-time Trasport Protocol) and **DNS** (Domain Name System).

The model we will study is a mix of the ISO OSI model and the TCP/IP protocols, so we will only talk about five layers:

1. **Physical** **Layer** → specifies how to transmit bits across different kinds of media as electrical or other analog signals;
   
2. **Link** **Layer** → is concerned with how to send messages between directly connected computers with specified levels of reliability. Ethernet and 802.11 are examples of link layer protocols;
   
3. **Network** **Layer** → deals with how to combine multiple links into networks, and multiple networks into internetworks, so that we can send packets between distant computers. This includes the task of finding the path along which to send the packets. IP is the main example protocol we will study for this layer;
   
4. **Transport** **Layer** → strengthens the delivery guarantees of the network layer, usually with increased reliability, and provide delivery abstractions, such as a reliable byte stream, that match the needs of different applications. TCP is an example of transport protocol;
   
5. **Application** **Layer** → contains programs that make use of the network. Many networked applications have user interfaces. However, our concern is with the portion of the program that uses the network, like the HTTP protocol used in the case of the Web browser. There are also important support programs in the application layer, such as the DNS, that are used by many applications.

The rest of this chapter is about example networks, I'll simply read it from the book.

----
# CHAPTER 2: The physical layer
In this chapter we will look at the lowest layer in our protocol model, the **physical** **layer**. It defines the electrical, timing and other interfaces by which bits are sent as signals over channels. 
The physical layer is the foundation on which the network is built. The properties of different kinds of physical channels determine the performance like the **throughput**, **latency** and **error** **rate** so it is a good place to start our journey into networkland.

(The book introduces the Fourier analysis but we didn't do it so I won't write about it, still an interesting read for those who are interested)

The **bandwidth**, described as the width of the band of frequencies that are passed, is a physical property of the transmission medium that depends on the construction, thickness, and length of a wire or fiber. Filters are often used to further limit the bandwidth of a signal, which let more signals share a given region of spectrum, improving the overall efficiency of the system. 
Signals that run from 0 up to a maximum frequency are called **baseband** signals. Signals that are shifted to occupy a higher range of frequencies, as is the case for all wireless transmissions, are called **passband** signals.
At least, this is the description of an **analog** **bandwidth**, while for us the **digital** **bandwidth** is the maximum **data** **rate**, basically how many bits are sent per second. **BUT**, that data rate is the end result of using the analog bandwidth of a physical channel for digital transmission, so the two are related.

In the early 20th century it was already derived an equation expressing the maximum data rate for a finite-bandwidth *noiseless channel*, which is:
$$maximum \space data \space rate = 2B \log_2 V (bits/sec)$$
Where B is our bandwidth, while V consist of levels... not so sure what it means in the book, but since we'll transmit binary signals, we will have 2 levels.
There is a **huge** problem: there is always random (thermal) noise present due to the motion of the molecules in the system. The amount of thermal noise present is measured by the ratio of the **signal** **power**(S) to the **noise** **power**(N), called the **SNR** (Signal-to-Noise Ratio $S/N$). The ratio is expressed on a log scale so that the variations don't vary on a tremendous range. The unit of the log scale $10 \space log_{10} \space S/N$ is called **decibels (dB)**. So now we have an ever more important formula to express the maximum **capacity** of a noisy channel:
$$ maximum \ number \ of \ bits/sec = B \ log_2 \ (1+ S/N)$$
This formula tells us the capacity a real channel can have.

(I've written this part pretty badly :/, I'll check it better later)

## Guided transmission media

The purpose of the physical layer is to transport bits from one machine to another. Various physical media can be used for the actual transmission and each one has its own niche in terms of bandwidth, delay, cost, and ease of installation and maintenance. 
The media are grouped into **guided media**, such as copper wire and fiber optics, and **unguided media**,  like terrestrial wireless, satellite and lasers through the air.

In this section we see the guided media:

1. **Magnetic media** → One of the most common ways to transport data from one computer to another is to write them onto magnetic tape or removable media (e.g., recordable DVDs), physically transport the tape or disks to the destination machine, and read them back in again. It is often more cost effective, especially for applications in which high bandwidth or cost per bit transported is the key factor;
   
2. **Twisted pairs** → Consists of two insulated copper wires, typically about 1 mm thick. The wires are twisted together in a helical form. A signal is usually carried as the difference in voltage between the two wires in the pair, thus providing better immunity to external noise because the noise tends to affect both wires the same, leaving the differential unchanged.
   Twisted pairs can be used for transmitting either analog or digital information, and due to their adequate performance and low cost, twisted pairs are widely used and are likely to remain so for years to come.
   Twisted-pair cabling comes in different varieties, and different LAN standards may use the twisted pairs differently: links that can be used in both directions at the same time, are called **full**-**duplex** links. In contrast, links that can be used in either direction, but only one way at a time are called **half**-**duplex** links. A third category consists of links that allow traffic in only one direction, called **simplex** links;
   
3. **Coaxial cable** → It has better shielding and greater bandwidth than unshielded twisted pairs, so it can span longer distances at higher speeds. A coaxial cable consists of a stiff copper wire as the core, surrounded by an insulating material. The insulator is encased by a cylindrical conductor, often as a closely woven braided mesh. The outer conductor is covered in a protective plastic sheath. The construction and shielding of the coaxial cable give it a good combination of high bandwidth and excellent noise immunity. The bandwidth possible depends on the cable quality and length;
   
4. **Power lines** → Deliver electrical power to houses, and electrical wiring within houses distributes the power to electrical outlets.
   Power lines have been used by electricity companies for low-rate communication, but in recent years there has been renewed interest in high-rate communication over these lines, both inside the home as a LAN and outside the home for broadband Internet access;
   
5. **Fiber optics** → Is used for long-haul transmission in network backbones, high speed LANs and high-speed Internet access. An optical transmission system has **three** **key** **components**: the **light** **source**, the **transmission** **medium**, an ultra-thin fiber of glass and the **detector**, which generates an electrical pulse when light falls on it. 
   So if we attach a light source to one end and a detector to the other, we have a **unidirectional** data transmission system that accepts an electrical signal, converts and transmits it by light pulses, and then is reconverted to an electrical signal at the receiving end.
   
   This transmission system would leak light and be useless were it not for an interesting principle of physics: when a light ray passes from one medium to another the ray is **refracted** (bent) at the boundary, and the amount of refraction depends on the **indices of refraction** of the two media. This is why for angles of incidence above a certain critical value, the light is refracted back internally and can be propagated for kilometers with virtually no loss.
   We'll have many different rays bouncing around at different angles, each one with a different mode; so a fiber having this property is called a **multimode** **fiber**.
   However, if the fiber’s diameter is reduced to a few wavelengths of light, the fiber acts like a wave guide and the light can propagate only in a straight line, yielding a **single**-**mode** **fiber**. Single-mode fibers are more expensive but are widely used for longer distances.
   
   How are fiber cables made? At the center is the **glass** **core** through which the light propagates. In multimode fiber the core is typically 50 microns in diameter, while in single-mode fiber the core is 8 to 10 microns. Then the core is surrounded by a **glass** **cladding** with a lower index of refraction than the core, to keep all the light in the core. Next comes a **thin** **plastic** **jacket** to protect the cladding. Fibers are typically grouped in bundles, protected by an outer **sheath**. (I think I said enough about fiber optics, I'm dying here grandpa)

-----
# CHAPTER 3: The data link layer

In this chapter we will study the design principles for the second layer in our model, the **data** **link** **layer**. This study deals with algorithms for achieving reliable, efficient communication of whole units of information called **frames** between two machines connected by a communication channel that acts conceptually like a wire.
You think that studying this layer would be easy like the physical layer? Hah, the book is like "go fuck yourself". So much happens in a communication channel and we're the ones who have to deal with it. But first, we need to see its functions:
1.  Provide a well-defined service interface to the network layer;
2. Deal with transmission errors;
3. Regulate the flow of data so that slow receivers are not swamped by fast senders.

To accomplish these goals, the data link layer takes the packets it gets from the network layer and encapsulates them into frames for transmission. Each frame contains a **frame** **header**, a **payload** **field** for holding the packet, and a **frame** **trailer**. 

The function of the data link layer is to provide services to the network layer, and the principal service is transferring data from the network layer on the source machine to the network layer on the destination machine. The services offered vary from protocol to protocol, and we can have three reasonable possibilities for our service:
1) **Unacknowledged connectionless service** → consists of having the source machine send independent frames to the destination machine without having the destination machine acknowledge them. 
   
   A good example is the **Ethernet**, and this class is appropriate when the error rate is very low, and also appropriate for real-time traffic;
   
2) **Acknowledged connectionless service** → there are still no logical connections used, but each frame sent is individually acknowledged, so the sender knows whether a frame has arrived correctly, and do nothing, or been lost, and can be sent again. This service is useful for unreliable channels, like wireless systems. **802.11** (**WiFi**) is a good example.
   
   The trouble with this strategy is that it can be inefficient, and this is why link layers have a strict maximum frame length imposed by the hardware, and known propagation delays;
   
3) **Acknowledged connection-oriented service** → the source and destination machines establish a connection before any data are transferred. Each frame sent over the connection is numbered, and the data link layer guarantees that each frame sent is indeed received. Furthermore, it guarantees that each frame is received exactly once and that all frames are received in the right order. It is the equivalent of a reliable bit stream. 
   
   It is appropriate over long, unreliable links such as a satellite channel or a long-distance telephone circuit. 

To provide service to the network layer, the data link layer must use the service provided to it by the physical layer, but if the channel is noisy there will be errors, and the data link layer has to take care of it. 
The usual approach is for the data link layer to break up the bit stream into discrete frames, compute a short token called a **checksum** for each frame, and include the checksum in the frame when it is transmitted. When a frame arrives at the destination, the checksum is recomputed. If the newly computed checksum is different from the one contained in the frame, the data link layer knows that an error has occurred and takes steps to deal with it.

Breaking up the bit stream into frames is more difficult than it at first appears: a good design must make it easy for a receiver to find the start of new frames while using little of the channel bandwidth. We will look at four methods:

1. **Byte count** → uses a field in the header to specify the number of bytes in the frame. When the data link layer at the destination sees the byte count, it knows how many bytes follow and hence where the end of the frame is. 
   
   The trouble with this algorithm is that the count can be garbled by a transmission error and will be unable to locate the end of the current frame and the start of the new frame. Even worse, this problem can hardly be solved by retransmission, so it is rarely used by itself;
   
2. **Flag bytes with byte stuffing** → gets around the problem of the byte count by having each frame start and end with special bytes. Often the same byte, called a **flag** **byte**, is used as both the starting and ending delimiter, so two consecutive flag bytes indicate the end of one frame and the start of the next. However, there is a problem we have to solve: it may happen that the sequence of the flag byte occurs in the data, interfering with the framing.
   
   We solve the problem above by having the sender’s data link layer insert a special **escape byte** (**ESC**) just before each accidental flag byte in the data. Thus, a framing flag byte can be distinguished from one in the data by the absence or presence of an escape byte before it, and the data link layer on the receiving end removes the escape bytes before giving the data to the network layer. This method is called **byte stuffing**. 
   
   An escape byte can be stuffed with another escape byte, since the same problem with an accidental flag byte can happen. We have a problem: it is tied to the use of 8-bit bytes;
   
3. **Flag bits with bit stuffing** → gets around the disadvantage of byte stuffing by framing at the bit level, so frames can contain an arbitrary number of bits made up of units of any size. The **bit stuffing** is analogous to byte stuffing and it also ensures a minimum density of transitions that help the physical layer maintain synchronization. **USB** (**Universal Serial Bus**) uses bit stuffing for this reason. 
   
   With both bit and byte stuffing, a side effect is that the length of a frame now depends on the contents of the data it carries: if there are no flag bytes in the data, 100 bytes might be carried in a frame of roughly 100 bytes. If, however, the data consists solely of flag bytes, each flag byte will be escaped and the frame will become roughly 200 bytes long;
   
4. **Physical layer coding violations** → we use some reserved signals to indicate the start and end of frames. In effect, we are using ‘‘coding violations’’ to delimit frames, but because they are reserved signals, it is easy to find the start and end of frames and there is no need to stuff the data. This method would seem like the best, but many data links protocol use a mix of this and the last 3.

Solved the problem of the markings of the frame, we need to make sure all frames are eventually delivered to the network layer at the destination and in the proper order.
The usual way to ensure reliable delivery is to provide the sender with some **feedback**: the protocol calls for the receiver to send back special control frames with positive, known as **ACK**, or negative, **NACK**, acknowledgements about the frames that arrived already. If the sender receives a positive acknowledgement about a frame, it knows the frame has arrived safely; while a negative acknowledgement means that something has gone wrong.

An additional complication comes from the possibility that hardware troubles may cause a frame to vanish. This possibility is dealt with by introducing **timers** into the data link layer, activated when a frame is sent. The timer is set to expire after an interval long enough for the frame to reach the destination, be processed there, and have the acknowledgement propagate back to the sender. Normally, the frame will be correctly received and the acknowledgement will get back before the timer runs out, in which case the timer will be canceled.

When frames may be transmitted multiple times there is a danger that the receiver will accept the same frame two or more times and pass it to the network layer more than once. To prevent this from happening, it is generally necessary to assign **sequence numbers** to outgoing frames, so that the receiver can distinguish retransmissions from originals.

Another important design issue that occurs in the data link layer is a **bottleneck**: a sender that systematically wants to transmit frames faster than the receiver can accept them. This situation can occur when the sender is running on a fast, powerful computer and the receiver is running on a slow, low-end machine.
Two approaches are commonly used:

1. **Feedback-based flow control** → the receiver sends back information to the sender giving it permission to send more data, or at least telling the sender how the receiver is doing. They are seen at both the link and higher layers;
2. **Rate-based flow control** → the protocol has a built-in mechanism that limits the rate at which senders may transmit data, without using feedback from the receiver. They are seen  as a part of the transport layer.

The physical layer process and some of the data link layer process run on dedicate hardware called a **NIC** (**Network Interface Card**). The rest of the link layer process and the network layer process run on the main CPU as part of the operating system, with the software for the link layer process often taking the form of a device driver.

Under no circumstances is a frame header ever given to a network layer: we keep the network and data link protocols completely separate, and as long as the network layer knows nothing about the data link protocol or the frame format, they can be changed without requiring changes to the network layer’s software. 

Providing a rigid interface between the network and data link layers greatly simplifies the design task because communication protocols in different layers can evolve independently.
In the header we have 3 fields, while the field *info* indicates the actual data:
1) *kind* → tells whether there are any data in the frame, because some of the protocols distinguish frames containing only control information from those containing data as well;
2) *seq* → used of sequence numbers, to distinguish retransmission from originals;
3) *ack* → used of acknowledgements.

It is important to understand the relationship between a packet and a frame: the network layer builds a packet by taking a message from the transport layer and adding the network layer header to it. This packet is then passed to the data link layer in the info field of an outgoing frame. When the frame arrives at the destination, the data link layer extracts the packet from the frame and passes the packet to the network layer. 
In this manner, the network layer can act as though machines can exchange packets directly.

We have 3 types of simplex protocols when we handle frames:

1. **Utopian simplex protocol** → is a protocol that is as simple as it can be because it does not worry about the possibility of anything going wrong. Data are transmitted in one direction only and both the transmitting and receiving network layers are always ready. 
   
   Processing time can be ignored, infinite buffer space is available, and best of all, the communication channel between the data link layers never damages or loses frames. This thoroughly unrealistic protocol will be nicknamed "Utopia".
   
   Only the *info* field of the frame is used by this protocol, because the other fields have to do with error and flow control and there are no errors or flow control restrictions here;
   
2. **Simplex stop-and-wait protocol for an error-free channel** → is a protocol preventing the sender from flooding the receiver with frames faster than the latter is able to process them. This situation can easily happen in practice so being able to prevent it is of great importance, but the communication channel is still assumed to be error free and the data traffic is still simplex.
   
   One solution is to build the receiver to be powerful enough. However, this is a worst-case solution since it requires dedicated hardware and can be wasteful of resources if the utilization of the link is mostly low. 
   
   A more general solution to this problem is to have the receiver provide feedback to the sender. After having passed a packet to its network layer, the receiver sends a little dummy frame back to the sender which, in effect, gives the sender permission to transmit the next frame. This delay is a simple example of a flow control protocol, and these protocols are called **stop-and-wait**.
   
   Although data traffic in this example is simplex, frames do travel in both directions, and so, the communication channel between the two data link layers needs to be capable of bidirectional information transfer. 
   However, this protocol entails a strict alternation of flow, though a half-duplex physical channel would suffice here;
   
1. **Simplex stop-and-wait protocol for a noisy channel** → the normal situation of a communication channel that makes errors: frames may be either damaged or lost completely. However, we assume that if a frame is damaged in transit, the receiver hardware will detect this when it computes the checksum. 
   
   If the frame is damaged in such a way that the checksum is nevertheless correct—an unlikely occurrence—this protocol (and all other protocols) can fail.
   At first glance it might seem that a variation of protocol 2 would work by adding a timer, but the network layer on the receiver has no way of knowing that a packet has been lost or duplicated, so the data link layer must guarantee that no combination of transmission errors can cause a duplicate packet to be delivered to a network layer. 
   
   The receiver needs to distinguish a frame that it is seeing for the first time from a retransmission, and the way is to have the sender put a sequence number in the header of each frame it sends. Then the receiver checks the sequence number of each arriving frame to see if it is a new frame or a duplicate to be discarded. It must carry sequence numbers that are large enough for the protocol to work correctly.
   
   Protocols in which the sender waits for a positive acknowledgement before advancing to the next data item are often called **ARQ** (**Automatic Repeat reQuest**) or **PAR** (**Positive** **Acknowledgement with Retransmission**).
   
   After transmitting a frame, the sender starts the timer running. If it was already running, it will be reset to allow another full timer interval. The interval should be chosen to allow enough time for the frame to get to the receiver, for the receiver to process it in the worst case, and for the acknowledgement frame to propagate back to the sender. If the timeout interval is set too short, the sender will transmit unnecessary frames. While these extra frames will not affect the correctness of the protocol, they will hurt performance.

In most practical situations, there is a need to transmit data not in simplex, but in **duplex**. One way to achieve full-duplex is to run two instances of one of the previous protocols, each using a separate link for simplex data traffic (in different directions). Each link is then comprised of a ‘‘forward’’ channel (for data) and a ‘‘reverse’’ channel (for acknowledgements). 
In both cases the capacity of the reverse channel is almost entirely wasted, so a better idea is to use the same link for data in both directions and by looking at the *kind* field in the header of an incoming frame, the receiver can tell whether the frame is data or an acknowledgement.

Although interleaving data and control frames on the same link is a big improvement over having two separate physical links, yet another improvement is possible: when a data frame arrives, instead of immediately sending a separate control frame, the receiver restrains itself and waits until its network layer passes the next packet.  The acknowledgement is then attached to the outgoing data frame by using the *ack* field in the frame header. 

In effect, the acknowledgement gets a free ride on the next outgoing data frame. The technique of temporarily delaying outgoing acknowledgements so that they can be hooked onto the next outgoing data frame is known as **piggybacking**. 
There is a huge disadvantage: if the data link layer waits longer than the sender’s timeout period, the frame will be retransmitted. So if a new packet arrives quickly, the acknowledgement is piggybacked onto it. Otherwise, if no new packet has arrived by the end of this time period, the data link layer just sends a separate acknowledgement frame.

The next protocols are bidirectional protocols that belong to a class called **sliding** **window** protocols: their essence is that at any instant of time, the sender maintains a set of sequence numbers corresponding to frames it is permitted to send. These frames fall within the **sending** **window**, while the receiver maintains a **receiving** **window** corresponding to the set of frames it is permitted to accept. The sender’s window and the receiver’s window don't need to have the same lower and upper limits or even have the same size. In some protocols they are fixed in size, but in others they can grow or shrink over the course of time as frames are sent and received.

The sequence numbers within the sender’s window represent frames that have been sent or can be sent but are as yet not acknowledged. Whenever a new packet arrives from the network layer, it is given the next highest sequence number, and the upper edge of the window is advanced by one. When an acknowledgement comes in, the lower edge is advanced by one.

Until now we have made the tacit assumption that the transmission time required for a frame to arrive at the receiver plus the transmission time for the acknowledgement to come back is negligible. Sometimes this assumption is clearly false, and the solution lies in allowing the sender to transmit up to *n* frames before blocking, instead of just 1. With a large enough choice of *n* the sender will be able to continuously transmit frames since the acknowledgements will arrive for previous frames before the window becomes full, preventing the sender from blocking.

To find an appropriate value for *n* we need to know how many frames can fit inside the channel as they propagate from sender to receiver. This capacity is determined by the **bandwidth**(**B**) in bits/sec multiplied by the **one-way transit time**(**D**), also known as the **bandwidth-delay product** of the link. This quantity can be divided by the number of bits in a frame as to express it as a **number of frames**. 
So our $n$ should be set at the maximum $2BD +1$, since the data link layer will keep on sending frames until the ACK for the first frame arrives from the receiver, and the $+1$ is because an ACK will not be sent until after a complete frame is received. So we finally have found a proper upper bound where:
$$ link \ utilization \leq \frac{n}{1+2BD}$$
The equation shows the need for having a large window *n* whenever the bandwidth-delay product is large. If the delay is high, the sender will rapidly exhaust its window even for a moderate bandwidth. If the bandwidth is high, even for a moderate delay the sender will exhaust its window quickly unless it has a large window. We can call this **pipelining**, but pipelining frames over an unreliable communication channel raises some serious issues, those typical of an unreliable communication channel, yay.

One option, called **go-back-N**, is for the receiver simply to discard all subsequent frames, sending no acknowledgements for the discarded frames. This strategy corresponds to a receive window of size 1.  The data link layer refuses to accept any frame except the next one it must give to the network layer. If the sender’s window fills up before the timer runs out, the pipeline will begin to empty. Eventually, the sender will time out and retransmit all unacknowledged frames in order, starting with the damaged or lost one.  This approach can waste a lot of bandwidth if the error rate is high.

The other general strategy is called **selective repeat**. When it is used, a bad frame that is received is discarded, but any good frames received after it are accepted and buffered, so when the sender
times out, only the oldest unacknowledged frame is retransmitted. If that frame arrives correctly, the receiver can deliver to the network layer, in sequence, all the frames it has buffered. 
Selective repeat corresponds to a receiver window larger than 1. This approach can require large amounts of data link layer memory if the window is large.
Selective repeat is often combined with having the receiver send a **NAK** when it detects an error, and since they stimulate retransmission before the corresponding timer expires, there is an improvement in performance.

These two alternative approaches are trade-offs between efficient use of bandwidth and data link layer buffer space. Depending on which resource is scarcer, one or the other can be used.

----
# CHAPTER 4: The medium access control sublayer

We already studied **Pont-to-point** connections, and now we'll study **broadcast channels** and their protocols.

In any broadcast network, the key issue is how to determine who gets to use the channel when there is competition for it. When only a single channel is available, it is much harder to determine who should go next; but at least many protocols for solving the problem are known.
Broadcast channels are sometimes referred to as **multiaccess channels** or **random access channels**.
The protocols used to determine who goes next on a multiaccess channel belong to a sublayer of the data link layer called the **MAC** (**Medium Access Control**) sublayer. The **MAC** sublayer is especially important in LANs, particularly wireless ones because wireless is naturally a broadcast channel. WANs, in contrast, use point-to-point links, except for satellite networks. 

**How to allocate a single broadcast channel among competing users**? We can use two schemes:

**Static allocation schemes** → these have huge shortcomings, and so are rarely used. The traditional way is more or less like this: if there are N users, the bandwidth is divided into N equal-sized portions, with each user being assigned one portion. 
Since each user has a private frequency band, there is now no interference among users. When there is only a small and constant number of users, and each has a steady stream or a heavy load of traffic, this division is a simple and efficient allocation mechanism. A wireless example is FM radio stations.
However, if the number of users is large and varying or the traffic is bursty (what does this adjective mean, please enlighten me) is when a huge mess and waste happen.
   
**Dynamic schemes** → We had the allocation model, and it was solved thanks to 5 key points with which we'll have to work, and possibly find better solutions: 
1. **Independent traffic** → the model has N **independent stations**, each with a program or user that generates frames for transmission. Once a frame has been generated, the station is blocked and does nothing until the frame has been successfully transmitted;
2. **Single channel** → a single channel is available for all communication: all stations can transmit on it and all can receive from it. The stations are assumed to be equally capable, though protocols may assign them different roles;
3. **Observable collisions** → if two frames are transmitted simultaneously, they overlap in time and the resulting signal is garbled, causing a **collision**, detected by all stations. A collided frame must be transmitted again later;
4. **Continuous or slotted time** → time may be assumed **continuous**, in which case frame transmission can begin at any instant. Alternatively, time may be slotted or divided into discrete intervals called **slots**;
5. **Carrier sense or no carrier sense** → with the carrier sense assumption, stations can tell if the channel is in use before trying to use it and no station will attempt to use the channel while it is sensed as busy. If there is no carrier sense, it will be the opposite, and only later can they determine whether the transmission was successful.

The professor only talked about **CSMA** (**Carrier Sense Multiple Access**) protocols, so I'll write only those. Obviously it is a **carrier sense protocol**, but there are many different versions:

**1-persistent CSMA** → the protocol is called 1-persistent because the station transmits with a probability of 1 when it finds the channel idle. You'd expect that this scheme avoids collisions except for the rare case of simultaneous sends, but it in fact it does not. The propagation delay has an important effect on collisions. 
There is a chance that just after a station begins sending, another station will become ready to send and sense the channel. If the first station’s signal has not yet reached the second one, the latter will sense an idle channel and will also begin sending, resulting in a collision. 
This chance depends on the number of frames that fit on the channel, or the **bandwidth-delay product** of the channel. If only a tiny fraction of a frame fits on the channel, which is the case in most LANs since the propagation delay is small, the chance of a collision happening is small. The larger the bandwidth-delay product, the more important this effect becomes, and the worse the performance of the protocol.

**Non-persistent CSMA** → In this protocol, a conscious attempt is made to be less greedy than in the previous one: if the channel is already in use, the station does not continually sense it for the purpose of seizing it immediately upon detecting the end of the previous transmission. Instead, it waits a random period of time and then repeats the algorithm. Consequently, this algorithm leads to better channel utilization but longer delays than 1-persistent CSMA.

**p-persistent CSMA** →  it applies to slotted channels and works as follows: when a station becomes ready to send, it senses the channel. If it is idle, it transmits with a probability $p$. With a probability $q = 1 − p$, it defers until the next slot. If that slot is also idle, it either transmits or defers again, with probabilities $p$ and $q$ respectively. 
This process is repeated until either the frame has been transmitted or another station has begun transmitting. In the latter case, the unlucky station waits a random time and starts again. If the station initially senses that the channel is busy, it waits until the next slot and applies the above algorithm. *Note*: IEEE 802.11 uses a refinement of p-persistent CSMA.

There is an improvement over the CSMA: **CSMA/CD**, CSMA with collision detection. CSMA/CD quickly detects the collision and abruptly stop transmitting, saving time and bandwidth.
This protocol is the basis of the classic **Ethernet LAN**. 
It is important to realize that collision detection is an **analog(physical) process**: the station’s hardware must listen to the channel while it is transmitting, and if the signal it reads back is different from the signal it is putting out, it knows that a collision is occurring. 
The implications are that a received signal must not be tiny compared to the transmitted signal, difficult for wireless connections, and that the modulation, or better yet voltage, must be chosen to allow collisions to be detected.
CSMA/CD, as well as many other LAN protocols, uses this conceptual model: a station has finished transmitting its frame, and now any other station having a frame to send may now attempt to do so. If two or more stations decide to transmit simultaneously, there will be a collision. If a station detects a collision, it aborts its transmission, waits a random period of time,
and then tries again.
There is an important problem: **How long will it take to detect the collision?** It will depend on the worst case scenario it takes for the propagation signal to reach the station that sent the frame.

### I need to find a good source for Manchester Encoding... I hate the way the professor explains I swear

Now it is time to see how these principles apply to real systems. Many of the designs for personal, local, and metropolitan area networks have been standardized under the name of **IEEE 802**. A few have survived but many have not; and the most important survivors are **802.3** (**Ethernet**) and **802.11** (**wireless LAN**). 

Two kinds of Ethernet exist: **classic Ethernet**, which solves the multiple access problem using the techniques seen in this chapter; and **switched Ethernet**, in which devices called **switches** are used to connect different computers. It is important to note that, while they are both referred to as Ethernet, they are quite different: classic Ethernet is the original form, while switched Ethernet is what Ethernet has transformed in **fast Ethernet**(100 Mbps), **gigabit Ethernet**(1000 Mbps), and **10 gigabit Ethernet**(10000 Mbps). In practice, only switched Ethernet is used nowadays.
(Halsall is even out of production, why does the professor make us study from it? It's fucking obsolete dang)