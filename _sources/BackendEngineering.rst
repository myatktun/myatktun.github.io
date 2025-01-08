===================
Backend Engineering
===================

1. `Communication Patterns`_
2. `Protocols`_
3. `HTTPS`_
4. `Execution Patterns`_
5. `Proxies`_
6. `Load Balancers`_
7. `Designing Software`_
8. `Request Journey`_

`back to top <#backend-engineering>`_

Communication Patterns
======================

* `Request & Response`_, `Synchronous & Asynchronous`_, `Push`_, `Polling`_, `Long Polling`_
* `Server-Sent Events`_, `Publish & Subscribe`_, `Multiplexing & Demultiplexing`_
* `Stateful & Stateless`_, `Sidecar`_

Request & Response
------------------
    * **Basic Scenario**
        - client send a request
        - server need to understand the request, parse it, process it and send a response
        - client parse the response and consume it
    * to know the beginning and the end of a request/response is important
    * the cost of parsing a request is not cheap
    * parsing the request and processing/executing the request is not same
    * parsing XML is more expensive than parsing JSON, but parsing JSON is still slow
    * some use protocol buffers as quick parser
    * **Usage**
        - almost everywhere
        - web, HTTP, DNS, SSH, RPC, SQL, Database Protocols, APIs
    * **Structure**
        - structure is defined by both client and server
        - has a boundary
        - defined by a protocol and message format
    * **Sending large files**
        - simple way is to send a large request with the file, but if fail, the sent data is
          deleted
        - chunk the file and send a request per chunk, can be resumable if fail
    * some such as notification service or chatting app do not work well with request/response
      model, e.g., a request must be sent every interval to see if there is a notification
    * try ``curl -v --trace out.txt http://google.com`` and check

Synchronous & Asynchronous
--------------------------
    * **Synchronous I/O**
        - caller send a request and is blocked, caller cannot execute anything
        - context switching: CPU taking out the blocked process and giving time to other
          processes that can execute
        - when receiver responds, the caller is unblocked
        - caller and receiver are in sync
        - e.g., program asking OS to read from the disk
    * **Asynchronous I/O**
        - caller send a request and can work until it get a response
        - caller can check if response is ready by epolling
        - or caller knows when the receiver call back when it's done
        - or caller can create a new thread that gets block and does the work
        - caller and receiver are not necessarily in sync
        - e.g., program create secondary thread that reads from the disk and is blocked by OS,
          main program still run and execute, thread finish and call back the main thread
        - example workloads: async programming such as promises and futures, async backend
          processing, async commits in postgres, async I/O in Linux such as epoll and
          io_uring, async replication, async OS fsync
    * synchronicity is a client property, most modern client libraries are asynchronous
    * synchronous communication: caller waits for response from receiver, e.g., asking a
      question in meeting
    * asynchronous communication: response can come at any time, caller and receiver can do
      anything in between, e.g., emailing someone
    * **Asynchronous backend processing**
        - when the backend receives a request and need to execute long running process, the
          client moves on, being async, while backend is synchronous, knowing someone is
          waiting for a response, the whole system can be considered synchronous
        - can change to asynchronous by using queues in the backend
        - when client sends a request, the backend will not promise to execute immediately,
          instead put it in a queue and respond with a job ID/promise, as it might be handling
          other requests
        - the client can check back if the job is done or not in various ways such as push,
          poll, publish/subscribe

Push
----
    * one of the famous patterns for fast response, ideal model for real time notification from
      backend or chat apps
    * **Basic Scenario**
        - client connects to server, server sends data to client
        - client does not need to request anything, only a connection is necessary
        - protocol must be bidirectional
        - e.g., RabbitMQ
    * **Drawbacks**
        - client must be connected/online
        - clients might not be able to handle, the server only pushes, it does not know if the
          client can manage the data, client can crash because of multiple pushes
        - requires bidirectional protocol
    * for smaller/lighter clients, polling is preferred

Polling
-------
    * common, easy to implement and popular pattern, also called short polling
    * ideal for request that takes a long time to process and want it to execute asynchronously
      on the backend
    * **Basic Scenario**
        - client sends a request, server immediately responds with a handle, usually in the
          form of unique id
        - request may not be processed immediately, server will execute it when it is free
        - client can check the status with the handle, server responds if ready or not
        - multiple short request/response as polls
    * the idea of polling is request/response, but the whole system is based on asynchronous
      execution with short polling
    * one big request/response is broken down into multiple request/response as polls
    * server will only send the processed response when it is finished and the client polls, in
      this way client can be disconnected
    * if server send the processed response without client polling, the data may be lost if the
      client is disconnected
    * **Drawbacks**
        - multiple requests must be sent to check status
        - does not scale well, increases network bandwidth
        - waste backend resources as it has to check polls, instead can be used to serve
          requests

Long Polling
------------
    * after sending a request that will take a long time, client polls and wait until server
      responds when it's ready
    * ideal for long processed requests or backend to send notification
    * used by Kafka
    * **Basic Scenario**
        - client sends a request, server immediately responds with a handle, usually in the
          form of unique id
        - request may not be processed immediately, server will execute it when it is free
        - client can check the status with the handle and wait
        - server keep the connection open and responds only if it has a response ready
    * client can be disconnected, and does not need to poll very often
    * some long polling variations have timeouts, not to poll forever
    * **Drawbacks**
        - may not be real time, if client delay/not polling often, when there is a new
          response from server

Server-Sent Events
------------------
    * one request, very very long response
    * **Basic Scenario**
        - client sends request, server sends logical events as part of response
        - client keeps getting response/data
        - client is smart enough to understands and parses chunks/streams of response
        - server never writes the end of the response, since response has start and end
        - server responds with special header ``Content-Type: text/event-stream``
    * ideal for real time notification or where push works but is restrictive
    * SSE works with request/response, any HTTP server supports it
    * can be considered as a request and an unending response
    * **Drawbacks**
        - client must be online, as server is always sending an event
        - client may not be able to handle the streams of response
        - for lighter client, polling might be better
        - HTTP/1.1 problem in browser, where only maximum 6 connections are allowed to one
          domain, other requests might starve when all 6 connections are SSE

Publish & Subscribe
-------------------
    * publisher can publish on a server and client/subscriber can consume it
    * design for services to talk to each other, a service mesh
    * **Basic Scenario**
        - when a client uploads a file to upload service, the client's job is done when it is
          finished, no waiting for other services to complete as well, so client is not blocked
        - the upload service then upload/publish the file into a message queue/broker or
          topic for other services/channels to consume
        - other services will subscribe and consume from the queue, and may publish again
        - a consumer can also be a publisher for another
    * **Topic**
        - a group of published things consumers can subscribe to
        - main component in pub/sub model
        - how published subjects is delivered to consumers varies, e.g., push or long polling
        - RabbitMQ pushes and Kafka long polls
    * scales well with multiple receivers
    * ideal for microservices as it is loose coupling
    * publisher's work is done when published, consumer can consume it any time it wants
    * **Drawbacks**
        - message delivery issues, message can even be consumed more than once
        - two generals problem: publisher can't know if consumer/subscriber get the message
        - adding another broker, scaling can be complex
        - can have network congestion between broker and consumers if multiple polls

Multiplexing & Demultiplexing
-----------------------------
    * **Multiplexing**
        - combining multiple incoming signals into one
        - in HTTP/1.1, incoming requests will be piped individually, e.g., 3 requests 3 pipes
        - in HTTP/2, incoming requests will be multiplexed, e.g., 3 requests multiplexed into
          1 pipe
    * **Demultiplexing**
        - splitting single incoming signal into multiple
        - single multiplexed HTTP/2 requests will be demultiplexed into individual connections
        - e.g., single 3 multiplexed requests demultiplexed into 3 pipes
        - HTTP/1.1 in browser can only demultiplex up to 6 connections
    * in demultiplexing, each connection can have its own rule, flow control and congestion
      control without affecting each other
    * **Connection Pooling**
        - spinning up multiple free connections prior and giving to each incoming requests
        - will have a pool of connections instead of establishing on a request or multiple
          requests competing for one connection
        - can be considered as demultiplexing style
        - a request/connection will be blocked if all connections are busy in the pool

Stateful & Stateless
--------------------
    * whether state is stored in the application/backend or not and does the app depend on it
    * **Stateful**
        - stores state about client in memory, or somewhere else
        - depends on the information for the backend to function properly
        - entire system can be stateful but particular application can be stateless, if it is
          read from another application such as database
    * **Stateless**
        - client is responsible to transfer the state with every request
        - state may be stored but can also lose it, as backend does not depend on it
        - stateless backend can still store data somewhere else, such as database, as long as
          it's functionality doesn't rely on the state being stored in it
        - backend remain stateless but system is stateful
        - can restart whenever, and clients can connect back and finish the remaining work
    * **Stateful Backend Drawbacks**
        - if state is stored on the backend and it restarts and lost the data, it will break
        - will not be able to verify client data if it is not stored/rely on database
        - with load balancing, client data such as cookies, cannot be checked as load balancer
          will point to different servers
    * **Stateful vs Stateless protocols**
        - protocols can be designed to store state
        - TCP is stateful as information about client and server, such as sequences and
          connection file descriptor, is stored in both the client and the server
        - UDP is stateless, even if it stores file descriptor, it isn't really necessary
        - DNS send queryID in UDP to identify queries, DNS client server can be considered
          stateful but the protocol isn't, as it uses UDP
        - QUIC is stateless if it uses UDP, it has to send connectionID in every request to
          identify connection
        - can build a stateless protocol on top of stateful one and vice versa, e.g., HTTP on
          top of TCP, QUIC on top of UDP
    * **Complete Stateless System**
        - very rarely seen, state has to be carried with every request
        - complete stateless backend service will have to rely on the input
        - JWT (JSON Web Token) is stateless

Sidecar
-------
    * every protocol requires a library, still a library even if it's built into the language
    * the library parses the request for server to understand
    * mostly app and library should be same language and become entrenched
    * changing or adding features to the library is hard and requires testing and backward
      compatibility
    * the idea is to delegate communication to other app, such as proxy which has rich library
      and can talk to any language/protocol, and now the client will have a thin library and a
      sidecar proxy
    * **Sidecar Proxy**
        - client->client sidecar proxy->server sidecar reverse proxy->server
        - configure a proxy in the application for all HTTP requests to direct into sidecar
        - called a sidecar as it literally lives within the same machine
        - sidecar proxy will forward the request to another sidecar of the destination
        - the proxy can secure the request, even if the client sends insecure one
        - how proxies communicate is not client's responsibility
        - sidecar proxies can be upgraded independently, client and server code do not need to
          change
        - used in service meshes such as Envoy, Istio, Linkerd
        - can be sidecar proxy container, must be Layer 7 proxy
    * using sidecar can be language agnostic, upgradable protocol, secure, trace and monitor,
      service discovery and cache
    * **Drawbacks**
        - system can be complex
        - debugging can be hard
        - have latency as requests need to be rewritten and send

`back to top <#backend-engineering>`_

Protocols
=========

* `OSI Model`_, `Internet Protocol`_, `UDP`_, `TCP`_, `TLS`_
* `HTTP/1.1`_, `HTTP/2`_, `HTTP/3`_, `WebSockets`_, `gRPC`_, `WebRTC`_
* protocol is a system of rules that allows two parties to communicate
* designed with a set of properties based on it's purpose to solve a problem
    * **Data Format**
        - human readable text based such as plain text, JSON, XML
        - binary such as protobuf, RESP, h2, h3
    * **Transfer Mode**
        - message based, where message has a start and an end, e.g., UDP, HTTP
        - stream of bytes such as TCP, WebRTC
    * **Addressing System**
        - DNS, IP, MAC
    * **Directionality**
        - bidirectional such as TCP
        - unidirectional such as HTTP
        - full/half duplex
    * **State**
        - stateful such as TCP, gRPC, Apache Thrift
        - stateless such as UDP, HTTP
    * **Routing**
        - proxies, gateways
    * **Flow & Congestion Control**
        - is it controllable like in TCP
        - no control like UDP
    * **Error Control**
        - error code, retries and timeouts

OSI Model
---------
    * Open Systems Interconnection Model, conceptual 7 layers, with each describing specific
      networking component
    * it is important to understand on which layer does the application live, especially when
      the app is a bridge between other apps
    * a communication model is required for applications to be agnostic
    * without standard model
        - apps need to understand all the underlying network to communicate
        - difficult to upgrade physical network equipments
        - not possible to do work in each layer without affecting other parts of the system as
          it is not decoupled
    * ``Layer 7, Application``: HTTP, FTP, gRPC, backend engineers do not interact with Layer 7
      directly, instead use libraries
    * ``Layer 6, Presentation``: encoding, serialization
    * ``Layer 5, Session``: connection establishment, where state sets place, TLS
    * ``Layer 4, Transport``: datagram, segments, UDP, TCP
    * ``Layer 3, Network``: packets, IP address
    * ``Layer 2, Data Link``: frames, MAC address, Ethernet
    * ``Layer 1, Physical``: electric signals, fiber, radio waves
    * **Sender (from 7 to 1)**
        - 7: POST with JSON to HTTPS server
        - 6: serialize JSON to flat byte strings
        - 5: request to establish TCP connection/TLS
        - 4: send SYN request to target port
        - 3: add SYN, source and destination IPs to packets
        - 2: each packet into a single frame and add source/dest MAC addresses
        - 1: each frame becomes string of bits and converted to some physical form
    * **Receiver (from 1 to 7)**
        - 1: receive signal and convert to bits
        - 2: bits into frame
        - 3: frames into packet
        - 4: packets into segments/datagrams, congestion control, flow control or
          retransmission if TCP, if segment is SYN, no need to go other layers as connection
          request is still processing
        - 5: establish/identify connection session, only come this layer when necessary
        - 6: deserialize byte strings to JSON for app to consume
        - 7: app understands JSON POST and event is triggered
    * for example, switch works at Layer 2, router and VPN at Layer 3, firewall at Layer 4 and
      CDN at Layer 7
    * OSI model has too many layers and can be hard to understand
    * TCP/IP model deals with Layers 5, 6 and 7 as one layer, application

Internet Protocol
-----------------
    * **IP Address**
        - Layer 3 property, can be assigned auto or statically
        - has network and host portion, 4 bytes or 32 bits in IPv4
        - e.g., 192.168.100.0/24, first 24 bits are network, 2<sup>24</sup> networks, and the
          rest are hosts, 32 - 24 = 8, 2<sup>8</sup> hosts
    * **Subnet Mask**
        - 192.168.100.0/24 is also called a subnet and has a mask 255.255.255.0
        - used to determine if an IP is in the same subnet or not
        - if same subnet, can use MAC address to send a packet directly
        - if not same subnet, need to find someone who knows the route, router or gateway
        - e.g., request to a database in different subnet can have delay if the router is
          congested
    * **Default Gateway**
        - most networks have hosts and default gateway
        - gateway has at least two network interfaces
        - hosts in same subnet communicate directly, otherwise talk to gateway
        - gateway has an IP address and each host should know its gateway
    * **IP Packet**
        - has headers, 20 bytes or 60 bytes if available, and data sections, which can be up
          to 65536 bytes
        - | source IP | data | destination IP |
        - a packet should fit into a single frame
        - if a packet is larger than MTU, maximum transmission unit, either fail or packet can
          be fragmented, e.g., 1 packet fragmented into 2 frames and sent
        - every packet has a single byte that represents a counter, Time To Live, so that it
          does not travel infinitely
    * **ICMP**
        - Internet Control Message Protocol, one of the most critical Layer 3 protocols
        - for informational message, such as host/port unreachable, fragmentation required,
          packet expired
        - uses IP directly, e.g., ping, traceroute
        - does not require listeners or ports to be opened
        - some firewalls block ICMP, and ping may not work if blocked
        - disabling ICMP can cause issues with connection establishment, when fragmentation is
          required message cannot be sent back
    * more information at [Datatracker](https://datatracker.ietf.org/)

UDP
---
    * User Datagram Protocol, Layer 4 protocol built on top of IP
    * can address processes in a host using ports
    * stateless and prior communication not required, has 8 bytes header
    * usually used to avoid overheads of TCP, such as video streaming, VPN, DNS, WebRTC
    * does not guarantee delivery and some frames can get dropped, useful for sending huge
      inconsistent data
    * sender multiplex requests from different apps into UDP and receiver demultiplex
    * require source and destination ports to identify app or process
    * **UDP Datagram**
        - 8 bytes header in IPv4, ports are 16 bits
        - datagram is added as data into IP packet
    * simple protocol, use less bandwidth as header and datagrams are small
    * stateless, consume less memory and low latency
    * **Drawbacks**
        - no acknowledgement, delivery not guaranteed, cannot know if data reaches or not
        - cannot be used for database connections
        - connection less and anyone can send data without prior connection
        - used in DNS flooding attacks, more dangerous than TCP flooding
        - no flow control, although can be built at Layer 7 for it
        - no congestion control, packets are unordered and can easily be spoofed

TCP
---
    * Transmission Control Protocol, Layer 4 protocol
    * can address processes in a host using ports
    * controls the transmission, not like UDP
    * stateful and requires handshake, has 20 or up to 60 bytes header
    * reliable and guarantee deliver, used for remote shell, database connections and any
      bidirectional communication
    * **TCP Connection**
        - Layer 5 agreement between client and server
        - sometimes called socket or file descriptor
        - must create a connection to send data
        - identified by IPs and ports of source and destination
        - OS hashes the identifiers and check against it to verify connection
        - if there is no connection, fail/drop the segment or return ICMP message
        - segments are sequenced, ordered and acknowledged
        - lost segments are retransmitted
    * sender multiplex requests from different apps into TCP segments and receiver demultiplex
    * **TCP 3-way Handshake**
        - client sends SYN to server to synchronize sequence numbers
        - server sends SYN/ACK to client to synchronize its sequence numbers
        - client ACKs server's SYN
        - handshake is complete, both client and server now have file descriptors/state to
          verify connection when data is send
    * **Sending Data**
        - data is encapsulated in a segment and sent
        - receiver acknowledges the segment
        - sender can send new segments before ACK of old segment arrives, but there is a limit
          of flow control and congestion control
        - can ACK multiple segments with one ACK
    * **Closing Connection**
        - 4-way handshake, require to close not to leak memory
        - when client wants to close connection, it sends FIN to server
        - server ACK and sends FIN
        - client ACK and connection is closed
        - server deletes the file descriptor, but client will not
        - time-wait state: client still having closed connection file descriptor to clean
          segments for the connection if they arrive
    * **TCP Segment**
        - 20 or up to 60 bytes header, ports are 16 bits
        - segment is added into IP packet as data
        - has 9 bit flags for NS, CWR, ECE, URG, ACK, PSH, RST, SYN and FIN
        - segment size depends on MTU of network, usually 512 bytes and can be up to 1460
    * **Drawbacks**
        - memory usage increases with more connections as it stores states
        - hashing to get file descriptors can also increase CPU usage
        - if source and destination IPs and ports are known, can send a fake request to close
          connection

TLS
---
    * Transport Layer Security, basic fundamental way to encrypt communication
    * used to encrypt data with symmetric key, authenticate server and other extensions
    * symmetric key is faster than asymmetric
    * in HTTPS, handshake is performed first to share a symmetric key on both client and server
    * **TLS 1.2** <a id="tls-12"></a>
        - server has public, which can be shared, and private RSA asymmetric keys
        - after client has opened TCP connection, it will send TLS client hello to request
          encryption
        - server replies with certificate, which has a public key
        - client encrypts a premastered symmetric key with server's public key and send it
          with change cipher FIN
        - server decrypt with it's private key to get the symmetric key and replies change
          cipher FIN
        - anyone between the connection cannot decrypt it as they do not have the private key
        - client and server both now have symmetric key and use it to encrypt data
    * **Diffie Hellman**
        - require two private keys and one public key to get a symmetric key
        - client and server generate their own private keys, which cannot be shared, but
          public keys can be
        - let g and n be prime number public keys, and x and y be private keys
        - (g<sup>x</sup> % n) or (g<sup>y</sup> % n) is unbreakable
        - (g<sup>x</sup> % n)<sup>y</sup> will get (g<sup>xy</sup> % n), which is the symmetric key, similarly for (g<sup>y</sup> % n)<sup>x</sup>
        - TLS 1.2 has an option to use Diffie Hellman or Elliptic Curve Diffie Hellman
    * **TLS 1.3** <a id="tls-13"></a>
        - client generates public and private keys, and encrypts the private key with the
          public key
        - client sends the public and encrypted keys to the server during TLS client hello
        - server generates its own private key with the encrypted key from client
        - server get the symmetric by using its private key and client's encrypted key
        - server encrypts its private key with client's public key and sends it
        - client get the symmetric by using its private key and server's encrypted key
        - client and server both now have symmetric key and use it to encrypt data
    * TLS 1.2 can have stages to negotiate whether to use Diffe Hellman or not, but not in TLS
      1.3, which removes the overhead of negotiating, and thus faster

HTTP/1.1
--------
    * an HTTP request consists of method, path, protocol, headers and body
    * an HTTP response consists of protocol, code, code text, headers and body
    * built on top of TCP
    * **HTTP**
        - open connection with 3-way TCP handshake
        - send request and get response
        - keep the connection open or close if not needed anymore
    * **HTTPS**
        - open connection with 3-way TCP handshake
        - TLS handshake with symmetric key
        - send request and get response
        - keep the connection open or close if not needed anymore
        - all data are encrypted
    * **HTTP/1.0**
        - open connection with 3-way TCP handshake
        - send request and get response
        - close connection immediately to save memory
        - need to open and close on every request/response
        - has latency as new connection is required every time
        - buffering, transfer-encoding doesn't exist
        - has to send the entire response, cannot send in chunks
        - cannot have multi-homed websites, as client does not set host header
    * in HTTP/1.1, client can tell server to keep connection alive to send many requests
    * low latency and CPU usage, streaming with chunked transfer
    * has pipelining, proxying and multi-homed websites
    * header compression is disabled
    * **HTTP/1.1 Pipelining**
        - instead of waiting for a response before sending another request, multiple requests
          can be sent altogether
        - server will process all requests and send multiple responses at the same time
        - does not guarantee responses will be in order, as one request might take longer than
          others to process
        - disabled by default
        - can build to have IDs on requests and responses on application level
    * **HTTP Request Smuggling**
        - server getting confused of start and end of a request
        - attackers can add their own content-length and transfer-encoding
        - server can get different requests based on parsing
        - more details at [PortSwigger](https://portswigger.net/web-security/request-smuggling)

HTTP/2
------
    * invented by Google, used to be called SPDY
    * **Concurrent Requests**
        - can send concurrent requests on same TCP connection, instead of creating multiple
          connections like in HTTP/1.1
        - every request is tagged with unique stream ID, odd for client and even for server
        - server can respond multiple responses in same TCP connection, do not need to be in
          order
    * **HTTP/2 with Push**
        - server pushes resources that it thinks client might need, which impacts performance
        - client might not understand why server is pushing resources, and building one is
          complex
        - deprecated as it is not scalable
        - replaced with early hints, which is a header that allows client to fetch more
          information about resources that might be needed
    * support headers and body compression
    * multiplexing over single connection save resources
    * secure by default because of [Protocol Ossification](https://en.wikipedia.org/wiki/Protocol_ossification)
    * protocol negotiation during TLS with NPN/ALPN
    * **Drawbacks**
        - has [TCP head of line blocking](https://en.wikipedia.org/wiki/Head-of-line_blocking), cannot innovate at the application layer
        - server push wasn't practical
        - high CPU usage for parsing streams
        - HTTP/1.1 might be more suitable if multiplexing is not used

HTTP/3
------
    * replaces TCP with QUIC, which is built on top of UDP
    * has streams and allows multiplexing on same connection
    * if one datagram of a stream is lost and other datagrams are delivered, only the stream
      with missing one will fail
    * connection setup and TLS are in one handshake
    * has congestion control at stream level
    * as UDP is stateless and connection ID is sent with every packet, ID can be used to
      migrate connection when IP address changes
    * main reason HTTP/2 over QUIC is not used is because of header compression algorithm
    * **Drawbacks**
        - has security limits in connection migration as connection ID is in plain text
        - high CPU usage for parsing streams, connection ID, congestion control, stream window
          sizes etc.,
        - UDP can be blocked
        - IP fragmentation is a problem

WebSockets
----------
    * bidirectional communication on the web, built on top of HTTP to access underlying TCP
    * **WebSocket Handshake**
        - ``ws://`` or ``wss://`` for websocket secure
        - open a connection, client sends GET 1.1 UPGRADE
        - server responds with 101, Switching Protocols
        - connection is now WebSocket, no longer HTTP
        - client header has ``Upgrade: websocket``, ``Connection: Upgrade`` and websocket key,
          protocol and version
        - server header has ``Upgrade: websocket``, ``Connection: Upgrade`` and websocket accept
          and protocol
        - client header's websocket key is used on server to ensure that not anyone can
          upgrade the connection
    * useful for chat apps, live feed, multiplayer gaming and client progress/logging
    * websockets are full-duplex, no polling required, HTTP comaptible, firewall friendly
    * **Drawbacks**
        - proxying is complex and Load Balancers have timeouts, as it is Layer 7
        - stateful and difficult to horizontally scale
        - established connection must be used, cannot request to open new one like in HTTP
        - long polling or SSE might be more suitable in some cases, instead of websockets

gRPC
----
    * communication protocols need a language client library such as SOAP or HTTP client
      library
    * it is hard to maintain and patch client libraries for new features, security etc.,
    * gRPC was invented as one client library for popular languages
    * build a protocol buffer definition file and specific stubs for library
    * certain APIs are exposed, use HTTP/2 and protocol buffers as message format
    * **Unary RPC**
        - basically request/response
        - client send a request and server send response back
    * **Server Streaming RPC**
        - client send a request and server will stream content, such as downloading large
          files, logging and progress of events
    * **Client Streaming RPC**
        - client send a stream of messages and server barely talks to it, such as uploading a
          large file
    * **Bidirectional Streaming RPC**
        - both client and server send a stream of messages simultaneously
    * fast, compact, one client library, can build many apps through progress feedback, can
      cancel HTTP/2 request and protocol buffer is powerful message format
    * **Drawbacks**
        - maintaining schemas can be annoying
        - still a thick client library
        - hard to implement proxies
        - no native error handling and browser support
        - can have timeouts during streams
    * example Todo App gRPC in JavaScript

        .. code-block:: protobuf

           // todo.proto
   
           syntax = "proto3"; // specify syntax
   
           package todoPackage; // define package name, can include multiple services
   
           // need to define methods in service
           service Todo {
   
               rpc createTodo(TodoItem) returns (TodoItem);
   
               rpc readTodos(NoParam) returns (TodoItems);
   
               rpc readTodosStream(NoParam) returns (stream TodoItem);
   
           }
   
           // need to define a void message as methods need to have one parameter
           message NoParam {}
   
           message TodoItem {
               int32 id = 1;
               string text = 2;
           }
   
           message TodoItems {
               repeated TodoItem items = 1;
           }


        .. code-block:: js

           // server.js
           const grpc = require("grpc");
           const protoLoader = require("@grpc/proto-loader");
           const packageDef = protoLoader.loadSync("todo.proto", {});
           const grpcObject = grpc.loadPackageDefinition(packageDef);
           const todoPackage = grpcObject.todoPackage;
   
           const server = new grpc.server();
           server.bind("0.0.0.0:30000", grpc.ServerCredentials.createInsecure());
           server.addService(todoPackage.Todo.service,
               {
                   "createTodo": createTodo,
                   "readTodos": readTodos,
                   "readTodosStream": readTodosStream
               });
   
           server.start();
   
           const todos = []
           function createTodo (call, callback) {
               const todoItem = {
                   "id": todo.length + 1,
                   "text": call.request.text
               };
   
               todos.push(todoItem);
   
               callback(null, todoItem);
           }
   
           function readTodos (call, callback) {
               callback(null, {"items" : todos}); // need to match schema
           }
   
           function readTodosStream (call, callback) {
               todos.forEach(t => call.write(t));
               call.end();
           }


        .. code-block:: js

           // client.js
           const grpc = require("grpc");
           const protoLoader = require("@grpc/proto-loader");
           const packageDef = protoLoader.loadSync("todo.proto", {});
           const grpcObject = grpc.loadPackageDefinition(packageDef);
           const todoPackage = grpcObject.todoPackage;
   
           const client = new todoPackage.Todo("localhost:30000", grpc.credentials.createInsecure());
   
           client.createTodo(
               {
                   "id": -1,
                   "text": "Do Something"
               }, (err, response) => {
                   console.log(JSON.stringify(response));
               });
   
           client.readTodos({}, (err, response) => {
               console.log(JSON.stringify(response));
           });
   
           const call = client.readTodosStream();
           call.on("data", item => {
               console.log(JSON.stringify(item));
           });
           call.on("end", e => console.log("server done"););



WebRTC
------
    * Web Real-Time Communication, peer to peer path to exchange video and audio in efficient
      and low latency
    * standardized API that enables rich communications in browsers, mobile and IoT devices
    * **Basic Scenario**
        - A wants to connect to B
        - A finds out all possible ways the public can connect to it and B does the same
        - A and B signal the session information in other ways such as QR, WebSockets, HTTP
          Fetch etc.,
        - A connects to B through the optimal path
        - A and B also exchange supported media and security
    * **NAT**
        - Network Address Translation
        - when a client does not have public IP and doesn't know how to directly connect to
          a server, it sends the packet to the gateway or router
        - the router replaces client's IP with it's public IP and sends the request
        - the mapping of private/internal IPs and public/external IP is kept on the router
        - when the router receives a response, it remaps to the private IP and forward the
          response to the client
    * **One to One NAT**
        - Full-Cone NAT
        - packets to external IP:port on the router always maps to internal IP:port without
          exception
        - does not check where the packet is coming from
    * **Address Restricted NAT**
        - packets to external IP:port on the router always maps to internal IP:port as long as
          source address from packet matches the table, regardless of the port
        - checks and allows if communicated with host before
    * **Port Restricted NAT**
        - packets to external IP:port on the router always maps to internal IP:port as long as
          source address and port from packet matches the table
        - checks and allows if communicated with host:port before
    * **Symmetric NAT**
        - packets to external IP:port on the router always maps to internal IP:port as long as
          source address and port from packet matches the table
        - checks and allows only if both external IP:port and host IP:port pair fully matches
        - WebRTC does not work with Symmetric NAT
    * **STUN**
        - Session Traversal Utilities for NAT
        - can tell public IP and port through NAT
        - works with Full-Cone, Port/Address restricted NAT
        - does not work with Symmetric NAT
        - STUN server usually runs on port 3478 and 5439 for TLS
        - easy to maintain, can be used from light weight docker container
        - client sends a request to STUN server through router, which does NAT and forwards
          the request
        - STUN server puts the NAT public information in a packet and sends it back
        - even though the router does NAT again to forward the response to client, the public
          IP and port is still in the response packet and client can now know it
        - in Full-Cone NAT, two machines can get their public info from STUN and easily
          connect directly
        - in Address/Port restricted NAT, two machines should attempt to establish connection
          with each other first, with the public info from STUN
        - in Symmetric NAT, since each machine only create NAT entry with STUN server, they
          cannot communicate with each other
    * **TURN**
        - Traversal Using Relays around NAT, just a server that relays packets
        - to work with Symmetric NAT
        - TURN server usually runs on port 3478 and 5439 for TLS
        - expensive to maintain and run
        - in Symmetric NAT, each machine only establish connection with TURN server and
          communicate through it
    * **ICE**
        - Interactive Connectivity Establishment
        - collects all available ICE candidates such as local IP addresses, reflexive
          addresses/STUN and relayed addresses/TURN
        - collecting candidates takes time
        - collected addresses are sent to remote peer via SDP, Session Description Protocol
    * **SDP**
        - Session Description Protocol
        - a format that describes ICE candidates, network options, media options, security
          options etc.,
        - most important concept in WebRTC
        - main goal is to send the SDP generated by a user to other party in any way
    * **SDP Signaling**
        - sending generated SDP to other party to communicate with
        - wait to collect all ICE candidates and necessary data, generate SDP and send it
        - can be done via tweet, QR code, Whatsapp, WebSockets or HTTP
        - the only important thing is to get SDP to the other party, not how it is sent
    * P2P is low latency for high bandwidth content
    * WebRTC provide standardized API and does not need to build new one
    * **Drawbacks**
        - need to maintain STUN and TURN servers
        - P2P can fail when multiple participants involved, as everyone need to be connected
          to everyone
    * **Some Useful APIs**
        - media: ``getUserMedia`` to access mic, camera, RTCPConnection.addTrack(stream)
        - ``onicecandidate``: maintain connection as candidates change, tells user there is new
          candidate after SDP is created, candidate is signaled and sent to other party
        - ``addicecandidate``: used by other party to add candidate to SDP
    * can create own STUN and TURN servers with [COTURN](https://github.com/coturn/coturn)
    * Google also provides public STUN servers
    * example WebRTC in Browser

        .. code-block:: js

           // FIRST BROWSER, initiator
           const lc = new RTCPeerConnection(); // local connection, can have custom config
           const dc = lc.createDataChannel("channel"); // data channel
   
           dc.onmessage = e => console.log("Message received: " + e.data);
   
           dc.onopen = e => console.log("Connection opened!");
   
           lc.onicecandidate = e => console.log("New ICE Candidate! Reprinting SDP " +
           JSON.stringify(lc.localDescription));
   
           lc.createOffer().then(o => lc.setLocalDescription(o))
           .then(e => console.log("Description set successfully!"));
   
           // after receiving answer from second browser
           const answer = {"ANSWER FROM SECOND BROWSER"};
   
           lc.setRemoteDescription(answer);
   
           dc.send("Hello second browser"); // can now communicate
   
           // SECOND BROWSER, reciever
           const offer = {"DESCRIPTION FROM FIRST BROWSER"};
           const rc = new RTCPeerConnection(); // remote connection
   
           rc.onicecandidate = e => console.log("New ICE Candidate! Reprinting SDP " +
           JSON.stringify(rc.localDescription));
   
           rc.ondatachannel = e => {
               rc.dc = e.channel; // data channel received
               rc.dc.onmessage = e => console.log("Message from client: " + e.data);
               rc.dc.onopen = e => console.log("Connection opened!");
           };
   
           rc.setRemoteDescription(offer).then(a => console.log("Offer set!"));
   
           rc.createAnswer().then(a => rc.setLocalDescription(a))
           .then(a => console.log("Answer created!"));
   
           rc.dc.send("Hello first browser"); // can now communicate


`back to top <#backend-engineering>`_

HTTPS
=====

* `Over TCP with TLS 1.2`_, `Over TCP with TLS 1.3`_, `Over QUIC`_
* `Over TFO with TLS 1.3`_, `Over TCP with TLS 1.3 0RTT`_, `Over QUIC 0RTT`_
* establish a connection, encryption, send data and close connection when nothing to send more
* only HTTP/1.1 and HTTP/2 support HTTPS over TCP

Over TCP with TLS 1.2
---------------------
    * establish connection with TCP 3-way Handshake, SYN, SYN/ACK and ACK
    * **Connection Encryption**
        - client and server need to agree on same symmetric key
        - Client Hello sends supported symmetric key encryption and key exchange algorithms
        - Server Hello responds with desired configurations
        - encryption is done after Client FIN and Server FIN
    * connection encryption requires two round trips since [TLS 1.2](#tls-12) is used
    * connection is now encrypted and can send data safely

Over TCP with TLS 1.3
---------------------
    * establish connection with TCP 3-way Handshake, SYN, SYN/ACK and ACK
    * connection encryption is done in one round trip, Client Hello and Server Hello, as it is
      [TLS 1.3](#tls-13)
    * connection is now encrypted and can send data safely

Over QUIC
---------
    * use QUIC, HTTP/3, which is on top of UDP
    * client sends QUIC handshake, server responds it and handshake now ends
    * connection establishment and encryption, TLS handshake, happen in the same round trip
    * connection is now encrypted and can send data safely

Over TFO with TLS 1.3
---------------------
    * uses TFO, TCP Fast Open, which uses cookies to resume an existing connection
    * if client has TFO cookie, it sends SYN + TFO and then sends Client Hello
    * server responds with SYN/ACK and then Server Hello
    * client will end the connection establishment and handshake with ACK
    * connection is now encrypted and can send data safely
    * might not be used since TFO is not built for security

Over TCP with TLS 1.3 0RTT
--------------------------
    * establish connection with TCP 3-way Handshake, SYN, SYN/ACK and ACK
    * client sends Client Hello with TLS extension pre-shared key and then sends data/request
    * if server accepts the pre-shared key, it responds with Server Hello and encrypted
      response
    * Client FIN can be sent
    * no need to wait for TLS encryption round trips before sending requests, as handshake and
      sending request happen same time

Over QUIC 0RTT
--------------
    * use QUIC, HTTP/3, which is on top of UDP
    * client sends QUIC handshake, which includes connection establishment and encryption, with
      a pre-shared key and then sends data/request
    * if server accepts the pre-shared key, it responds with QUIC handshake and encrypted
      response
    * QUIC handshake now ends
    * hard to implement, [Cloudflare blog](https://blog.cloudflare.com/even-faster-connection-establishment-with-quic-0-rtt-resumption/) for more information

`back to top <#backend-engineering>`_

Execution Patterns
==================

* `Process vs Thread`_, `Connection Establishment`_, `Reading/Writing Data`_
* `Single Listener/Single Worker Thread`_, `Single Listener/Multiple Worker Threads`_
* `Single Listener/Multiple Worker Threads with Load Balancing`_, `Multiple Threads/Single Socket`_
* `Multiple Listeners on same Port`_, `Idempotency`_, `Nagle's Algorithm`_

Process vs Thread
-----------------
    * **Process**
        - a set of instructions to be executed serially
        - has isolated memory and PID
        - scheduled in CPU
    * **Thread**
        - lightweight process
        - shares memory with the parent process and has a thread ID
        - scheduled in CPU
    * **Single-Threaded Process**
        - process with a single thread
        - simple but can be hard to write
        - e.g NodeJS
    * **Multi-Process**
        - app with multiple processes
        - each with its own memory, can also have a shared memory pool
        - take advantage of multiple cores
        - use more memory but isolated
        - e.g NGINX, Postgres
    * **Multi-Threaded**
        - single process with multiple threads
        - threads compete for shared memory
        - take advantage of multiple cores
        - less memory than multi-process
        - can have race conditions and need locks and latches
        - e.g SQL Server, Apache, Envoy
    * multiple processes/threads on single core CPU will cause context switches, which can be
      expensive
    * having same number of cores and processes can be optimal

Connection Establishment
------------------------
    * server will listen on both port and all network interfaces addresses by default
    * should specify which network interface to listen on, as it might be exposed to public
    * server app tells the kernel the IP and port on which it is listening, and the kernel
      creates necessary socket and SYN and Accept queues
    * when a client connects, kernel does the handshake to create a connection
        - client sends SYN, kernel adds it to SYN queue and replies with SYN/ACK
        - client replies with ACK and if it matches the SYN from the queue, kernel finishes
          the connection
        - the connection is moved to the Accept queue and the SYN is removed from the queue
        - backend process calls ``accept()`` to move the connection from the kernel's Accept
          queue to its own process
        - file descriptor for the connection is created, which is used for operations
    * **Problems**
        - if backend doesn't accept the connections fast enough, the queues will get full and
          new connections will not be established
        - clients can use SYN flood attack by not sending ACK back, and SYN queue in the
          backend will get full
        - having small back log, SYN and Accept queues sizes, can cause then to be full quick

Reading/Writing Data
--------------------
    * **Receive Buffer**
        - created by the kernel for data
        - when client sends data on a connection, the kernel puts it in a receive buffer
        - kernel will ACK when enough data is received
        - if client sends more data than the buffer space, packets will be dropped
        - when data is ready, backend application is notified and will call ``read()`` to copy
          data into its own process memory
        - copying can be expensive and kernel developers are trying to optimize it
        - copied data, in raw bytes, is handled by the library used in the application
        - the library will use necessary methods to decrypt and parse the data
    * **Send Buffer**
        - created by the kernel for data
        - the library in the backend prepares the data and when it is ready, ``send()`` is
          called and data is copied into send buffer in the form of raw bytes
        - kernel takes over and will send only when it has enough information, such as maximum
          segment size is filled
        - once data is sent, it is ACK and released from the buffer
    * **Problems**
        - if backend does not read fast enough, the receive buffer will be full and the kernel
          cannot accept more data
        - packets sent from client will be dropped and thus client suffers

* Listener: thread/process that calls ``listen()``, by passing address and port, and gets back
  the socket ID
* Acceptor: has access to the socket ID and calls ``accept()`` on it, to get descriptors for
  connections
* Reader: reads/writes the connection for requests

Single Listener/Single Worker Thread
------------------------------------
    * single listener, acceptor and reader
    * single threaded applications such as NodeJS
    * single process is responsible for listening, accepting connections and reading from
      connections asynchronously, as in NodeJS using epoll
    * simple architecture as one process do all the jobs
    * **Problems**
        - process might not be able to handle massive multiple connections optimally
        - the queues in the kernel may become full as the process cannot keep up
    * can spin up multiple simple processes, instead of handling multiple threads

Single Listener/Multiple Worker Threads
---------------------------------------
    * single listener and acceptor, and multiple readers
    * single process both listens and accepts, and spins up multiple threads to read
    * multi-threaded applications such as Memcached
    * threads will be assigned connections to read by the main process, load balancing may be
      used
    * reader threads can become worker threads to handle the request or spin new threads to
      handle it
    * **Problems**
        - one thread might be given heavy processing connection and others lightweight
          connections
        - there is no true load balancing

Single Listener/Multiple Worker Threads with Load Balancing
-----------------------------------------------------------
    * single listener, acceptor, and reader
    * single process listens, accepts and read, but spins up multiple threads to handle
      requests, with load balancing
    * multi-threaded applications such as RAMCloud
    * threads are given actual requests to process, instead of raw connections
    * main process handles everything and passes the requests to the threads for execution
    * requests can now be load balanced before given to the threads
    * **Problems**
        - single point of failure as one process handles everything

Multiple Threads/Single Socket
------------------------------
    * single listener and multiple acceptors and readers, such as NGINX
    * single listener as main worker process with actual socket
    * multiple acceptor and reader threads have access to the socket object on the main process
    * each thread call ``accept()`` on the same socket object, with mutex being handled
    * a thread that accepts a connection owns it

Multiple Listeners on same Port
-------------------------------
    * socket reuse option, ``SO_REUSEPORT``, can be used to share a socket between multiple
      processes
    * OS will create multiple necessary queues for each process and load balance connections to
      each queues
    * can spin up multiple threads, with each being listener, acceptor and reader
    * each thread gets unique socket ID, with own sets of queues, and no conflict will occur
    * default in applications such NGINX, Envoy, HAProxy
    * also called socket sharding
    * **Problems**
        - a process/thread can use ``SO_REUSEPORT`` options to hijack the port
        - a thread can get heavy processing connection as there is no true load balancing
        - architecture can become powerful but complex
    * OS can prevent port hijacking if a key is specified
    * can spin up worker threads only just for reading, and thus load balancing can be achieved

Idempotency
-----------
    * resending the request without changing the state of the backend
    * **Basic Scenario**
        - client lost the connection when sending a request
        - the request is sent and processed at the backend
        - client might attempt to send it again without knowing it has reached the backend
        - multiple same requests may be sent again
        - the backend is not idempotent if the same request is processed again
        - e.g multiple same payments might happen
        - need to identify the request uniquely and do not execute if it is replayed, in
          certain cases
    * **Idempotency Token**
        - easiest implementation is to attach a request ID, usually UUID, with each request
        - skip if the request ID has been processed
        - can be expensive as IDs have to be stored and searched
    * GET requests are idempotent by default, as it doesn't change state with multiple GETs
    * browsers and proxies treat GET as idempotent, and will send/retry without notice
    * never use GET to change state, as browsers might send the request multiple times
    * POST is not idempotent, but can be made

Nagle's Algorithm
-----------------
    * client delayed to reach MSS, instead of sending a few bytes of data, plus 40 bytes header
    * **Basic Scenario**
        - MSS is 1460 bytes, and client sends 500 bytes
        - client is delayed to fill the segment
        - client sends another 960 bytes
        - the segment is now filled and sent
        - the delay only happens if the data needs to be ACK
        - the first 500 bytes will be send immediately if the data doesn't need to be ACK
    * **Problems**
        - sending large data causes delay
        - after segmenting large data, the segment that doesn't reach MSS will not be sent
        - the segment will be sent only when ACK is received
        - there will be a delay as the segment waits for the ACK
        - either disable the algorithm or fill the segment with data, which is hard to do
    * most clients today disable Nagle's Algorithm, by enabling ``TCP_NODELAY``
    * curl disables it by default because TLS handshake slows down

`back to top <#backend-engineering>`_

Proxies
=======

* `Proxy`_, `Reverse Proxy`_, `Tunnel Proxy`_
* proxy and reverse proxy can be used at the same time, but client will only know it as a proxy
* not recommended to use proxy instead of VPN for anonymity, as VPN operates at Layer 3, and
  proxy operates at Layer 4 and above
* HTTP proxy is the most popular

Proxy
-----
    * server that makes requests on client's behalf
    * TCP connection is established with the proxy, not the destination
    * the proxy establish new connection between itself and the destination
    * the destination only knows the IP of the proxy, not the client
    * content transmitted from client is completely rewritten by the proxy
    * some proxies add headers, such as ``X-Forwarded-For``, and client can be known from layer seven
    * usually in proxy configuration, client knows the server but not vice versa
    * proxies can provide anonymity, caching, logging, blocking sites, microservices, debugging
    * most HTTPS proxies sometimes decrypt the traffic
    * from Layer 4, proxy is the final destination for client
    * from Layer 7, client knows the true final destination

Reverse Proxy
-------------
    * client doesn't know the final destination server
    * acts like the true destination, but sends requests to actual backend server
    * reverse proxy server is also called front-end server or edge server
    * load balancer is reverse proxy, but not all reverse proxies are load balancers
    * TCP connection between client and destination, which is reverse proxy, and another TCP
      connection between reverse proxy and the actual destination
    * used for caching, load balancing, ingress, canary deployment, microservices
    * both from Layer 4 and 7, client only knows the reverse proxy as true final destination

Tunnel Proxy
------------
    * HTTP proxy, where client can ask the proxy to open connection to certain server
    * the proxy becomes a pipe, it cannot see the content
    * can do TLS connection in end to end fashion

`back to top <#backend-engineering>`_

Load Balancers
==============

* `Layer 4 Load Balancer`_, `Layer 7 Load Balancer`_
* also known as fault tolerance system
* can replace load balancers with reverse proxy, which does not have balancing logic
* load balancer is reverse proxy, but not all reverse proxies are load balancers

Layer 4 Load Balancer
---------------------
    * warm up by opening TCP connections to the backend servers
    * when client connects, it needs to choose one server
    * load balancer will tag a state, as Layer 4 is stateful, to only one connection to the
      backend
    * all segments from client will only have to go to one connection
    * load balancer has connection to clinet and to backend server
    * **Rewrite Mode**
        - segments from client connection have to be rewritten to the backend connection
    * **NAT Mode**
        - makes into single TCP connection
        - the load balancer acts like a router, becomes gateway of client
        - only changes the destination IP to the backend server
    * **Pros**
        - simple load balancing, efficient as it doesn't lookup data
        - more secure, works with any protocol
        - work with one TCP connection in NAT mode
    * **Cons**
        - no smart load balancing, cannot apply to microservices
        - no load balancing per connection, no caching
        - protocol unaware and can by pass rules
        - reserved connection cannot be used for other clients

Layer 7 Load Balancer
---------------------
    * warm up by opening TCP connections to the backend servers
    * when client connects, it becomes protocol specific
    * logical requests are forwarded to new backend server
    * read and buffer the data, need to decrypt data if encrypted
    * need to have secure connection to decrypt data
    * **Pros**
        - smart load balancing, caching, applicable to microservices
        - API gateway logic, authentication available
    * **Cons**
        - expensive as data lookup is done, terminate TLS for decryption
        - use two TCP connections, must share TLS certificate
        - need to buffer and understand protocol

`back to top <#backend-engineering>`_

Designing Software
==================

* `Workflow Document`_, `Design Overview`_, `Component Design`_, `Design Overview Diagram`_
* code-first approach: can be lost when viewing the code later, can forget things about system
* diagram-first approach: can be too many detailed ones or too high level, may need companion
  documents
* the real way to design software is to spec it out all in writings
* keep away distractions. e.g email, notification
* use clutter-free text editors, e.g VIM, go dark mode if necessary
* produce collection of documents for each type of stake-holder, can be high level or technical

Workflow Document
-----------------
    * do not add a feature if there is no use case, might be fine for personal projects but
      commercial software must be approached pragmatically
    * workflow and use cases can help minimize scope and link back to customer requirement
    * write a workflow of how the software will be use in detail
    * finalize the workflow step by using the questions raised, if any
    * final workflow will be available for non-technical people who are interested
    * workflow document must be clear, can be creative, use UI elements and explain in high
      level
    * anyone reading the workflow document must know what and who the software if for

Design Overview
---------------
    * explain how user will interact, technical representation of the workflow
    * write about different components of the software and how they interact with each other
    * reference the workflow document if applicable
    * do not link back non-functional requirements to workflow, e.g. async jobs, health checks
      that does not require direct user input
    * include any technical problems that might occur, e.g. protocol, database, reverse proxy,
      scaling
    * review it with technical stake-holders

Component Design
----------------
    * make document per component if require
    * can go in details, e.g what the component is, its interface, computation, output
    * be technical as if reading the design is like reading the actual source code
    * some components can be described entirely in the design overview
    * writing component design will give insight of each, may find some so big that they can be
      their own projects

Design Overview Diagram
-----------------------
    * diagram to show how components communicate with each other
    * can use simple tools to design it
    * can have multiple diagrams for each component based on complexity

* it takes time to make documents and keep them up to date
* if owner leaves, next designer might not use same approach
* as there are many documents and diagrams, can be hard to understand for participants who are
  not familiar with

`back to top <#backend-engineering>`_

Request Journey
===============

* `Accept`_, `Read`_, `Decrypt`_, `Decode`_, `Process`_
* understanding the steps of request before processed allows to make appropriate designs
* need to make sure each step does not become a bottleneck
* can use only one thread, a thread or each step, combine steps or split a step further

Accept
------
    * requests are sent on connections, which are needed to be accepted by the backend
    * accept queue: listener queue where connection is placed after performing 3 way handshake
      by server kernel when client connects to it
    * backend is responsible to invoke ``accept()`` syscall on listener socket to create file
      descriptor that represents the connection
    * Accept step can be a bottleneck if backend is slow in accepting connections
    * backlog: parameter to specify the size of accept queue when listening on port
    * for speed, most backend assign one thread only to accept connections
    * can use multi-thread for accepting connections, but threads block each other when
      accepting on same socket
    * use ``SO_REUSEPORT`` option to create multiple listener sockets, multiple accept queues,
      on the same port with each thread owning a socket queue, e.g NGINX, HAProxy

Read
----
    * client can send requests to the backend after connection is established
    * a request is just a series of bytes with clear start and end defined by the protocol used
    * client and backend must agree on the protocol, mostly HTTP
    * client can encrypt the request if TLS is used, compress the body if request compression
      is supported and serialize data type, such as JSON/protobuff, and write the raw bytes
    * bytes sent from client go to NIC, OS kernel and into connection receive queue
    * packets remain in receive queue until backend application invoke ``read()`` or ``rcv()``,
      which move data from receive queue to process user space memory
    * reading of the raw bytes can be done in own thread or in the same thread as accept

Decrypt
-------
    * as the bytes received are decrypted, SSL library is invoked to encrypt
    * only after decryption, content such as headers and other metadata are available
    * decryption is CPU bound operation
    * can be done in own thread or same thread as read and accept

Parse
-----
    * plain text readable bytes are parsed by the library used, based on the protocol agreed
    * in HTTP/1.1, read plaint text and look for start and end of request based on definition
      of HTTP spec, e.g content-length, transfer encoding
    * in HTTP/2 or HTTP/3, more work is done to parse as there are more metadata
    * parsing is CPU bound, and can bottleneck the backend if not used properly
    * can be done in own thread or same thread as others
    * can be a hidden expensive step if not careful

Decode
------
    * request using JSON or protobuf is deserialized
    * turning raw bytes into language structures has cost and memory footprint
    * even in JavaScript, need to ``JSON.parse()``, cannot just read a JSON string
    * bytes of text encoded in UTF8 also need to be decoded, as UTF8 use up to 4 bytes to
      represent some characters
    * if necessary, need to do request decompression, some large POST request body are sent
      compressed

Process
-------
    * can be processed once the request is fully understood
    * may require database query, reading from disk or other computations
    * can be done in the same thread as others
    * recommended to have dedicated worker for processing, worker pool pattern is suitable

`back to top <#backend-engineering>`_
