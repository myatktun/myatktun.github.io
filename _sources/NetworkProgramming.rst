===================
Network Programming
===================

1. `Basics`_
2. `Sockets`_
3. `System Calls`_
4. `Binary Serialisation`_
5. `Data Encapsulation`_
6. `Example Programs`_
7. `Additional Resources`_

`back to top <#network-programming>`_

Basics
======

* `Packets`_, `Ports`_, `Byte Order`_

Packets
-------
    * a packet is encapsulated in a header by the protocol, then encapsulated again by the next
    protocol, then again by the next protocol, and again by the final protocol on the hardware
    layer
    * e.g. [Ethernet(IP[UDP(TFTP[Data])])]
    * at the receiver end, the hardware strips the "Ethernet" header, the kernel strips the "IP"
    and "UDP" headers, and the TFTP program strips the "TFTP" header to get the data
    * OSI model is used as a general, but "Network Access->Internet->Transport->Application" might
    be more consistent with Unix
    * the kernel builds the Transport Layer and Internet Layer, and the hardware does the Network
    Access Layer

Ports
-----
    * 16-bit number that's like a local address for a connection, used by sockets
    * helps differentiate between services on a computer with a single IP address
    * different services on the Internet have different well-known port numbers
    * on Unix can check the ports in ``/etc/services`` file
    * e.g. HTTP:80, SMTP:25, HTTPS:443, SSH:22
    * ports under 1024 are special, and require OS privileges

Byte Order
----------
    * **Network Byte Order**
        - Big-Endian: storing bytes with the big end first, e.g. b34f is stored as b34f
        - used in Motorola 68k
    * **Host Byte Order**
        - Little-Endian: storing bytes in reverse order, e.g. b34f is stored as 4fb3
        - used in Intel x86, some architectures use Big-Endian
    * always assume Host Byte Order is not right, and use a function to set it to Network Byte
    Order
    * e.g. ``htons()`` converts unsigned short integer from Host Byte Order to Network Byte Order
    * convert the numbers to Network Byte Order before going out the wire, and to Host Byte
    Order as they come in

`back to top <#network-programming>`_

Sockets
=======

* `Internet Sockets`_, `Socket Data Types`_, `Manipulating IP Addresses`_
* `Non-blocking Socket`_, `Broadcasting`_
* a way to communicate other programs using standard Unix file descriptors
* Unix programs do I/O by reading or writing to a file descriptor
* file descriptor: an integer associated with an open file, which can be a network connection,
FIFO, pipe , terminal or anything
* there are many kinds of sockets such as DARPA Internet address (Internet Sockets), path names
on a local node (Unix Sockets), and CCITT X.25 addresses (X.25 Sockets)

Internet Sockets
----------------
    * **Stream Sockets**
        - ``SOCK_STREAM``, reliable two-way connected communication streams
        - data arrives in order and will be error-free
        - e.g. used by telnet, ssh, HTTP
        - built on top of TCP for high level data transmission quality
        - need to maintain an open connection
    * **Datagram Sockets**
        - ``SOCK_DGRAM``, sometimes called connectionless sockets
        - data may arrive out of order, built on top of IP and UDP
        - do not need to maintain an open connection
        - e.g. used by tftp, dhcpcd, multiplayer games, streaming audio, video conferencing
        - tftp and similar programs have own protocol on top of UDP that needs to send back
          ACK, or else the sender will re-transmit the packet until ACK

Socket Data Types
-----------------
    * socket descriptor: ``int``
    * every struct as IPv4 and IPv6 variants
    * **addrinfo**
        - struct used to prep the socket address structures for subsequent use
        - also used in host and service name lookups

        .. code-block:: c

           struct addrinfo {
                   int              at_flags;     // e.g. AI_PASSIVE
                   int              ai_family;    // AF_INET, AF_INET6, AF_UNSPEC
                   int              ai_socktype;  // SOCK_STREAM, SOCK_DGRAM
                   int              ai_protocol;  // use 0 for "any"
                   size_t           ai_addrlen;   // size of ai_addr in bytes
                   struct sockaddr* ai_addr;      // stuct sockaddr_in or _in6
                   char*            ai_canonname; // full canonical hostname
                   struct addrinfo* ai_next;      // linked list, next node
           };


    * **sockaddr**
        - holds socket address information for many types of sockets
        - ``sa_data`` contains a destination address and port number for the socket

        .. code-block:: c

           struct sockaddr {
                   unsigned short sa_family;   // address family, AF_xxx
                   char           sa_data[14]; // 14 bytes of protocol address
           };


    * **sockaddr_in**
        - parallel structure of ``sockaddr`` to be used with IPv4
        - pointer to ``sockaddr_in`` ca be cast to a pointer to ``sockaddr`` and vice-versa
        - makes it easy to reference elements of the socket address
        - ``sin_zero`` should be set to all zeros with ``memset()``
        - ``sin_port`` must be in Network Byte Order by using ``htons()``

        .. code-block:: c

           struct sockaddr_in {
                   short int          sin_family;  // address family, AF_INET
                   unsigned short int sin_port;    // port number
                   struct in_addr     sin_addr;    // internet address
                   unsigned char      sin_zero[8]; // same size as struct sockaddr
           };


    * **in_addr**

        .. code-block:: c

           struct in_addr {
                   uint32_t s_addr;
           };


    * **sockaddr_storage**
        - designed large enough to holg both IPv4 and IPv6 structures
        - parallel structure and similar to ``sockaddr``, but larger
        - some calls might fill out ``sockaddr``, so can use this instead
        - check the address family in ``ss_family`` field, and cast it to ``sockaddr_in``

        .. code-block:: c

           struct sockaddr_storage {
                   sa_family_t ss_family; // address family
   
                   // padding, implementation specific
                   char    __ss_pad1[__SS_PAD1SIZE];
                   int64_t __ss_align;
                   char    __ss_pad2[__SS_PAD2SIZE];
           };



Manipulating IP Addresses
-------------------------
    * ``INET_ADDRSTRLEN`` and ``INET6_ADDRSTRLEN`` macros for largest IPv4 and IPv6 address
    * **inet_pton()**
        - presentation to network
        - converts IP address in numbers-and-dots notation to ``in_addr`` or ``in6_addr``
        - return value: 1 on success, 0 if not valid network address , and -1 if not valid
          address family

        .. code-block:: c

           struct sockaddr_in  sa;  // IPv4
           struct sockaddr_in6 sa6; // IPv6
   
           inet_pton(AF_INET, "10.12.110.57", &(sa.sin_addr));             // IPv4
           inet_pton(AF_INET6, "2001:db8:63b3:1::3490", &(sa6.sin6_addr)); // IPv6


    * **inet_ntop()**
        - network to presentation
        - converts ``in_addr`` or ``in6_addr`` to IP address in numbers-and-dots notation

        .. code-block:: c

           char               ip4[INET_ADDRSTRLEN];
           struct sockaddr_in sa;
   
           inet_ntop(AF_INET, &(sa.sin_addr), ip4, INET_ADDRSTRLEN);


    * **Private Networks**
        - firewall translates internal IP addresses to external addresses using NAT (Network
        Address Translation)
        - e.g. "10.x.x.x", "192.168.x.x" (0 <= x <= 255), "172.y.x.x" (16 <= y <= 31)
        - private IPv6 start with "fdXX:", but IPv6 does not really need NAT

Non-blocking Socket
-------------------
    * many functions are allowed to block, e.g. ``accept()``, all ``recv()`` functions
    * the kernel sets the socket descriptor to blocking by default
    * use ``fcntl()`` to set the socket to non-blocking
    * non-blocking socket can be polled for information
    * if there is no data, return -1 and set ``errno`` to ``EAGAIN`` or ``EWOULDBLOCK``
    * polling using non-blocking socket can use much CPU time

    .. code-block:: c

       #include <fcntl.h>
   
       sockfd = socket(PF_INET, SOCK_STREAM, 0);
       fcntl(sockfd, F_SETFL, O_NONBLOCK);



Broadcasting
------------
    * sending data to multiple hosts at the same time
    * only available with UDP and standard IPv4
    * need to set the socket to ``SO_BROADCAST``, and it is the only difference between UDP
    application that can broadcast and one that can't
    * every receiver must go through encapsulated data to find what port the data is for
    * **To Subnet's Broadcast Address**
        - all one-bits set for the host portion, e.g 192.168.1.255
        - bitwise logic: ``network_number OR (NOT net_mask)``
        - can send to remote networks and local network
        - but the packet can be dropped by the destination's router to prevent flooding
    * **To Global Broadcast Address**
        - ``INADDR_BROADCAST``: 255.255.255.255
        - many machines will bitwise AND it with the network number to convert it to a network
          broadcast address
        - routers do not forward this type of broadcast packet off the local network

    .. code-block:: c

       int                broadcast = 1;
   
       setsockopt(sockfd, SOL_SOCKET, SO_BROADCAST, &broadcast,
                  sizeof(broadcast));


`back to top <#network-programming>`_


System Calls
============

* `getaddrinfo()`_, `socket()`_, `bind()`_, `connect()`_, `listen()`_, `accept()`_
* `send()`_, `recv()`_, `sendto()`_, `recvfrom()`_, `close()`_, `shutdown()`_
* `getpeername()`_, `gethostname()`_, `poll()`_, `select()`_

getaddrinfo()
-------------
    * returns a pointer to one or more ``addrinfo`` structures
    * used to get all ``sockaddr`` info, including DNS and service name lookups
    * ``node``: host name or IP address
    * ``service``: service name or port number
    * ``hints``: pointer to ``addrinfo`` with relevant information

    .. code-block:: c

       int getaddrinfo(const char* node, const char* service,
                       const struct addrinfo* hints, struct addrinfo** res);


    * **Listen on local host IP address**

        .. code-block:: c

           // listen on host IP address
           int              status;
           struct addrinfo  hints;
           struct addrinfo* servinfo;
   
           memset(&hints, 0, sizeof(hints)); // make sure struct is empty
           hints.ai_family   = AF_UNSPEC;   // use IPv4 or IPv6
           hints.ai_socktype = SOCK_STREAM;
           hints.ai_flags    = AI_PASSIVE; // use local host address
   
           if ((status = getaddrinfo(NULL, "3490", &hints, &servinfo)) != 0) {
                   fprintf(stderr, "getaddrinfo error: %s\n", gai_strerror(status));
                   exit(1);
           }
   
           freeaddrinfo(servinfo);



socket()
--------
    * returns a socket descriptor to communicate through using ``send()`` and ``recv()``
    * can use ``read()`` and ``write()``, but former ones provide more control over data transmission
    * returns -1 on error, and sets ``errno`` to the error's value

    .. code-block:: c

       int socket(int domain, int type, int protocol);


    * use the values from the results of ``getaddrinfo()``, and fee them to ``socket()``

        .. code-block:: c

           int             sockfd;
           struct addrinfo hints, *res;
   
           getaddrinfo("www.example.com", "http", &hints, &res);
           sockfd = socket(res->ai_family, res->ai_socktype, res->ai_protocol);



bind()
------
    * associate a socket with a port on the machine
    * commonly used to listen for incoming connections on a specific port
    * returns -1 on error, and sets ``errno`` to the error's value
    * can omit to use if binding to any local port is allowed

    .. code-block:: c

       int bind(int sockfd, const struct sockaddr* addr, int addrlen);



connect()
---------
    * make a connection to a socket
    * returns -1 on error, and sets ``errno`` to the error's value

    .. code-block:: c

       int connect(int sockfd, const struct sockaddr* addr, int addrlen);



listen()
--------
    * listens for incoming connections on a socket
    * ``backlog``: number of connections allowed on the incoming queue
    * incoming queue: contains incoming connections until ``accept()``
    * returns -1 on error, and sets ``errno`` to the error's value
    * order of sys calls when listening: ``getaddrinfo``->``socket``->``bind``->``listen``->``accept``

    .. code-block:: c

       int listen(int sockfd, int backlog);



accept()
--------
    * used to get a pending connection from an incoming queue
    * return a new socket file descriptor to be used for the single accepted connection
    * the original descriptor listens for more new connections, and new one is used to ``send()``
    and `recv()`
    * use ``close()`` to prevent more incoming connections
    * returns -1 on error, and sets ``errno`` to the error's value
    * ``addr``: pointer to local ``sockaddr_storage``, will save information about the incoming
    connection
    * ``addr_len``: local int set to ``sizeof(struct sockaddr_storage)``

    .. code-block:: c

       int accept(int sockfd, struct sockaddr* addr, socklen_t* restrict addr_len);



send()
------
    * used to communicate over stream sockets or connected datagram sockets
    * returns number of bytes sent
    * returns -1 on error, and sets ``errno`` to the error's value
    * if return value does not match ``len``, must send the rest of the data
    * ``msg``: pointer to the data to send
    * ``len``: length of data in bytes

    .. code-block:: c

       int send(int sockfd, const void* msg, int len, int flags);


    * **Partial send**
        - kernel might not send all data out in one chunk
        - need to handle the data left in the buffer

        .. code-block:: c

           int sendall(int s, char* buf, int* len)
           {
                   int total     = 0;
                   int bytesleft = *len;
                   int n;
   
                   while (total < *len) {
                           n = send(s, buf + total, bytesleft, 0);
                           if (n == -1) {
                                   break;
                           }
                           total     += n;
                           bytesleft -= n;
                   }
   
                   *len = total;
   
                   return n == -1 ? -1 : 0;
           }




recv()
------
    * used to receive over stream sockets or connected datagram sockets
    * returns number of bytes actually read into the buffer
    * returns -1 on error, and sets ``errno`` to the error's value
    * return 0 means the remote side has closed the connection
    * ``buf``: buffer to read the data into
    * ``len``: maximum length of the buffer

    .. code-block:: c

       int recv(int sockfd, void* buf, int len, int flags);



sendto()
--------
    * used to communicate over unconnected datagram sockets
    * destination address structure is obtained from ``getaddrinfo()``, or from ``recvfrom()`` or
    hardcoded
    * returns number of bytes sent
    * returns -1 on error, and sets ``errno`` to the error's value
    * if return value does not match ``len``, must send the rest of the data
    * ``addr``: pointer to ``sockaddr``, which will be recasted

    .. code-block:: c

       int sendto(int sockfd, const void* msg, int len, int flags,
                  const struct sockaddr* addr, socklen_t addr_len);



recvfrom()
----------
    * used to receive over unconnected datagram sockets
    * returns number of bytes actually read into the buffer
    * returns -1 on error, and sets ``errno`` to the error's value
    * return 0 means the remote side has closed the connection
    * ``addr``: pointer to ``sockaddr``, which will be recasted

    .. code-block:: c

       int recvfrom(int sockfd, void* buf, int len, int flags, struct sockaddr* addr,
                    socklen_t* addr_len);



close()
-------
    * prevent reads and writes to the socket
    * attempting to read or write the socket will receive an error
    * must be used to free a socket descriptor
    * returns 0 on success, -1 on error, and sets ``errno`` to the error's value

    .. code-block:: c

       close(sockfd);



shutdown()
----------
    * have more control over how the socket closes
    * does not actually close the file descriptor, but changes its usability
    * allows to cut off communication in a certain direction, or both ways
    * ``how``: 0 = disallow further receives, 1 = disallow further sends, 2 = disallow further
    sends and receives like `close()`
    * returns 0 on success, -1 on error, and sets ``errno`` to the error's value

    .. code-block:: c

       int shutdown(int sockfd, int how);



getpeername()
-------------
    * get the name of the connected peer socket
    * returns 0 on success, -1 on error, and sets ``errno`` to the error's value
    * after getting the address, can use ``inet_ntop()``, ``getnameinfo()``, or ``gethostbyaddr()``
    to print or get more information
    * ``addr``: holds the information about the other side of the connection

    .. code-block:: c

       int getpeername(int sockfd, struct sockaddr* addr, int* addr_len);



gethostname()
-------------
    * returns the name of the computer the program is running on
    * the name can be used by ``getaddrinfo()`` to determine the IP address of the local machine
    * returns 0 on success, -1 on error, and sets ``errno`` to the error's value
    * ``hostname``: pointer to an array of chars to store the name on function return
    * ``size``: length in bytes of the ``hostname`` array

    .. code-block:: c

       int gethostname(char* hostname, size_t size);



poll()
------
    * allows to monitor sockets at once and handle the ready ones
    * slow for large number of connections, use ``libevent`` for better performance
    * use an array of ``struct pollfd`` with sockets and events to monitor
    * OS will block ``poll()`` until one of the events or a user-specified timeout occurs
    * return number of elements in the array for which events have occurred
    * need to check which elements have events occurred, count the numbers when checking and
    stop when count is equal to the return value
    * make enough space for the array or ``realloc()`` as needed
    * to delete from the array, copy the last element over-top the one to delete, and pass in
    one fewer as the count to `poll()`, or set any `fd` field to a negative number
    * ``nfds``: count of elements in the array
    * ``timeout``: in milliseconds, specify negative value to wait indefinitely
    * ``POLLIN``: alert when data is ready to ``recv()`` on the socket
    * ``POLLOUT``: alert when ready to ``send()`` data to the socket without blocking

    .. code-block:: c

       #include <poll.h>
   
       int poll(struct pollfd fds[], nfds_t nfds, int timeout);
   
       struct pollfd {
               int   fd;      // socket
               short events;  // bitmap of events
               short revents; // when poll() returns, bitmap of events that
                              // occurred
       };



select()
--------
    * allows to monitor sockets at once and handle the ready ones for read, write and sockets
    that raise exceptions
    * slow for large number of connections, use ``libevent`` for better performance
    * monitors sets of file descriptors in ``readfds``, ``writefds``, and ``exceptfds``
    * returns the number of file descriptors in three sets, which are also modified
    * returns -1 on error, and sets ``errno`` to the error's value, and the file descriptor sets
    are unmodified
    * ``nfds``: should be set to highest-numbered file descriptor plus 1
    * ``timeout``: interval to block and wait for file descriptor to be ready, set ``NULL`` to wait
    indefinitely
    * when the function returns, ``timeout`` might be updated to show the time remaining, but
    depends on Unix flavour, do not rely on it to for portability
    * ``FD_SET(int fd, fd_set* set);``: add ``fd`` to the set
    * ``FD_CLR(int fd, fd_set* set);``: remove ``fd`` from the set
    * ``FD_ISSET(int fd, fd_set* set);``: return ``true`` if ``fd`` is in the set
    * ``FD_ZERO(fd_set* set);``: clear the set
    * **Linux Bugs**
        - sometimes ``select()`` can return ready to read, and then not actually be ready, and it
          can block ``read()``
        - to solve this, set ``O_NONBLOCK`` flag on the receiving socket to error with
          ``EWOULDBLOCK``

    .. code-block:: c

       #include <sys/select.h>
   
       struct timeval {
               int tv_set;  // seconds
               int tv_usec; // microsecond
       };
   
       int select(int nfds, fd_set* readfds, fd_set* writefds, fd_set* exceptfds,
                  struct timeval* timeout);


`back to top <#network-programming>`_

Binary Serialisation
====================

* `Encode as Text`_, `Pass Raw Data`_, `Encode as Portable Binary Form`_
* used where a specific byte order is required for communication between systems that may have
different native byte orders
* use serialisation libraries instead of implementing own
* attackers can send badly-constructed packets which will be executed during unpacking

Encode as Text
--------------
    * can easily print and read binary data encoded as text
    * human-readable protocol, such as Internet Relay Chat (IRC), is good for
    non-bandwidth-intensive situation
    * slow to convert, and take more space

Pass Raw Data
-------------
    * take a pointer to the data to send, and call ``send()``
    * not all architectures represent numbers with the same bit representation or the same byte
    ordering

    .. code-block:: c

       // send
       double send_d = 3.4901;
       send(sockfd, &send_d, sizeof(send_d), 0); // non-portable
   
       // receive
       double receive_d;
       recv(sockfd, &receive_d, sizeof(receive_d), 0); // non-portable



Encode as Portable Binary Form
------------------------------
    * pack the data into known binary format and the receiver can unpack it, such as ``htons()``
    and `ntohs()`

    .. code-block:: c

       uint32_t htonf(float f)
       {
               uint32_t p;
               uint32_t sign;
   
               if (f < 0) {
                       sign = 1;
                       f    = -f;
               }
               else {
                       sign = 0;
               }
   
               p = ((((uint32_t)f) & 0x7fff) << 16) |
                   (sign << 31); // whole part and sign
               p |= (uint32_t)((f - (int)f) * 65536.0f) & 0xffff; // fraction
   
               return p;
       }
   
       float ntohf(uint32_t p)
       {
               float f  = ((p >> 16) & 0x7fff);    // whole part
               f       += (p & 0xffff) / 65536.0f; // fraction
   
               if (((p >> 31) & 0x1) == 0x1) {
                       f = -f; // sign bit set
               }
   
               return f;
       }


`back to top <#network-programming>`_

Data Encapsulation
==================

* `Example Packet Structure`_
* encapsulate data in a header and packet structure
* both client and server know how to pack/marshal and unpack/unmarshal

Example Packet Structure
------------------------
    * packet order of ``len name chatdata``
    * ``len``: total length of the packet
    * ``name``: user name, NULL-padded if necessary
    * ``chatdata``: data sent by user
    * each field can have specific size
    * the data muse be completely sent, even if it takes multiple calls to ``send()``
    * always assume only partial packet is received, and call ``recv()`` multiple times
    * **Receive Method 1**
        - since every packet starts with a length, call ``recv()`` to get the packet length
        - call it again specifying exactly the remaining length of the packet
        - only need one buffer per packet, but need to call ``recv()`` at least twice to get all
          data
    * **Receive Method 2**
        - call ``recv()`` with maximum number of bytes in a packet, might get some of the next
          packet
        - use a buffer big enough for two packets, and reconstruct the packets
        - in every ``recv()``, append the data into the buffer, and check if the packet is
          complete by comparing bytes in the buffer with the length specified in the header
        - remove the packet after processing, and move the second packet, maybe partial of it,
          to the front of the buffer
        - can use a circular buffer instead of removing and moving packets

`back to top <#network-programming>`_

Example Programs
================

* `Show IP Address`_, `Stream Client-Server`_, `Datagram Client-Server`_, `Poll Server`_, `Select Server`_
* `Encode into IEEE-754`_

Show IP Address
---------------
    * example program that show IP addresses for given host on the cmd
    * compile and run with ``PROGRAM_NAME HOST_NAME``

    .. code-block:: c

       #include <arpa/inet.h>
       #include <netdb.h>
       #include <netinet/in.h>
       #include <stdio.h>
       #include <string.h>
       #include <sys/socket.h>
       #include <sys/types.h>
   
       int main(int argc, char* argv[])
       {
               struct addrinfo hints, *res, *p;
               int             status;
               char            ipstr[INET6_ADDRSTRLEN];
   
               if (argc != 2) {
                       fprintf(stderr, "usage: showip hostname\n");
                       return 1;
               }
   
               memset(&hints, 0, sizeof(hints));
               hints.ai_family   = AF_UNSPEC;
               hints.ai_socktype = SOCK_STREAM;
   
               if ((status = getaddrinfo(argv[1], NULL, &hints, &res)) != 0) {
                       fprintf(stderr, "getaddrinfo: %s\n", gai_strerror(status));
                       return 2;
               }
   
               printf("IP addresses for %s:\n\n", argv[1]);
   
               for (p = res; p != NULL; p = p->ai_next) {
                       void* addr;
                       char* ipver;
                       if (p->ai_family == AF_INET) {
                               struct sockaddr_in* ipv4 =
                                   (struct sockaddr_in*)p->ai_addr;
                               addr  = &(ipv4->sin_addr);
                               ipver = "IPv4";
                       }
                       else {
                               struct sockaddr_in6* ipv6 =
                                   (struct sockaddr_in6*)p->ai_addr;
                               addr  = &(ipv6->sin6_addr);
                               ipver = "IPv6";
                       }
   
                       inet_ntop(p->ai_family, addr, ipstr, sizeof(ipstr));
                       printf("%s: %s\n", ipver, ipstr);
               }
   
               freeaddrinfo(res);
               return 0;
       }



Stream Client-Server
--------------------
    * client-server can use ``SOCK_STREAM``, ``SOCK_DGRAM`` or anything else, as long as using the
    same thing
    * **Server**

        .. code-block:: c

           #include <arpa/inet.h>
           #include <errno.h>
           #include <netdb.h>
           #include <netinet/in.h>
           #include <stdio.h>
           #include <stdlib.h>
           #include <string.h>
           #include <sys/socket.h>
           #include <sys/types.h>
           #include <sys/wait.h>
           #include <unistd.h>
   
           #define PORT    "3049"
           #define BACKLOG 10
   
           void  sigchld_handler(int);
           void* get_in_addr(struct sockaddr*);
   
           int main(int argc, char* argv[])
           {
                   int                     sockfd, new_fd;
                   struct addrinfo         hints, *servinfo, *p;
                   struct sockaddr_storage their_addr;
                   socklen_t               sin_size;
                   struct sigaction        sa;
                   int                     yes = 1;
                   char                    s[INET6_ADDRSTRLEN];
                   int                     rv;
                   char*                   msg = "hello world";
   
                   memset(&hints, 0, sizeof(hints));
                   hints.ai_family   = AF_UNSPEC;
                   hints.ai_socktype = SOCK_STREAM;
                   hints.ai_flags    = AI_PASSIVE;
   
                   if ((rv = getaddrinfo(NULL, PORT, &hints, &servinfo)) != 0) {
                           fprintf(stderr, "getaddrinfo: %s\n", gai_strerror(rv));
                           return 1;
                   }
   
                   for (p = servinfo; p != NULL; p = p->ai_next) {
                           if ((sockfd = socket(p->ai_family, p->ai_socktype,
                                                p->ai_protocol)) == -1) {
                                   perror("server: socket");
                                   continue;
                           }
   
                           if (setsockopt(sockfd, SOL_SOCKET, SO_REUSEADDR, &yes,
                                          sizeof(int)) == -1) {
                                   perror("setsockopt");
                                   exit(1);
                           }
   
                           if (bind(sockfd, p->ai_addr, p->ai_addrlen) == -1) {
                                   close(sockfd);
                                   perror("server: bind");
                                   continue;
                           }
                           break;
                   }
   
                   freeaddrinfo(servinfo);
   
                   if (p == NULL) {
                           fprintf(stderr, "server: failed to bind\n");
                           exit(1);
                   }
   
                   if (listen(sockfd, BACKLOG) == -1) {
                           perror("listen");
                           exit(1);
                   }
   
                   sa.sa_handler = sigchld_handler;
                   sigemptyset(&sa.sa_mask);
                   sa.sa_flags = SA_RESTART;
   
                   // reap zombie processes that appear as fork()ed child processes exit
                   if (sigaction(SIGCHLD, &sa, NULL) == -1) {
                           perror("sigaction");
                           exit(1);
                   }
   
                   printf("server: waiting for connections on port %s...\n", PORT);
   
                   while (1) {
                           sin_size = sizeof(their_addr);
                           new_fd =
                               accept(sockfd, (struct sockaddr*)&their_addr, &sin_size);
                           if (new_fd == -1) {
                                   perror("accept");
                                   continue;
                           }
   
                           inet_ntop(their_addr.ss_family,
                                     get_in_addr((struct sockaddr*)&their_addr), s,
                                     sizeof(s));
   
                           printf("server: got connection from %s\n", s);
   
                           if (!fork()) {
                                   close(sockfd);
                                   if (send(new_fd, msg, strlen(msg), 0) == -1) {
                                           perror("send");
                                   }
                                   close(new_fd);
                                   exit(0);
                           }
                           close(new_fd);
                   }
   
                   return 0;
           }
   
           void sigchld_handler(int s)
           {
                   int saved_errno = errno;
                   while (waitpid(-1, NULL, WNOHANG) > 0)
                           ;
   
                   errno = saved_errno;
           }
   
           void* get_in_addr(struct sockaddr* sa)
           {
                   if (sa->sa_family == AF_INET) {
                           return &(((struct sockaddr_in*)sa)->sin_addr);
                   }
   
                   return &(((struct sockaddr_in6*)sa)->sin6_addr);
           }


    * **Client**

        .. code-block:: c

           #include <arpa/inet.h>
           #include <errno.h>
           #include <netdb.h>
           #include <netinet/in.h>
           #include <stdio.h>
           #include <stdlib.h>
           #include <string.h>
           #include <sys/socket.h>
           #include <sys/types.h>
           #include <unistd.h>
   
           #define PORT        "3049"
           #define MAXDATASIZE 100
   
           void* get_in_addr(struct sockaddr*);
   
           int main(int argc, char* argv[])
           {
                   int             sockfd, numbytes;
                   char            buf[MAXDATASIZE];
                   struct addrinfo hints, *servinfo, *p;
                   int             rv;
                   char            s[INET6_ADDRSTRLEN];
   
                   if (argc != 2) {
                           fprintf(stderr, "usage: client hostname\n");
                           exit(1);
                   }
   
                   memset(&hints, 0, sizeof(hints));
                   hints.ai_family   = AF_UNSPEC;
                   hints.ai_socktype = SOCK_STREAM;
   
                   if ((rv = getaddrinfo(argv[1], PORT, &hints, &servinfo)) != 0) {
                           fprintf(stderr, "getaddrinfo: %s\n", gai_strerror(rv));
                           return 1;
                   }
   
                   for (p = servinfo; p != NULL; p = p->ai_next) {
                           if ((sockfd = socket(p->ai_family, p->ai_socktype,
                                                p->ai_protocol)) == -1) {
                                   perror("client: socket");
                                   continue;
                           }
   
                           if (connect(sockfd, p->ai_addr, p->ai_addrlen) == -1) {
                                   close(sockfd);
                                   perror("client: connect");
                                   continue;
                           }
                           break;
                   }
   
                   if (p == NULL) {
                           fprintf(stderr, "client: failed to connect\n");
                           return 2;
                   }
   
                   inet_ntop(p->ai_family, get_in_addr((struct sockaddr*)p->ai_addr), s,
                             sizeof(s));
                   printf("client: connecting to %s\n", s);
   
                   freeaddrinfo(servinfo);
   
                   if ((numbytes = recv(sockfd, buf, MAXDATASIZE - 1, 0)) == -1) {
                           perror("recv");
                           exit(1);
                   }
   
                   buf[numbytes] = '\0';
   
                   printf("client: received '%s'\n", buf);
   
                   close(sockfd);
                   return 0;
           }
   
           void* get_in_addr(struct sockaddr* sa)
           {
                   if (sa->sa_family == AF_INET) {
                           return &(((struct sockaddr_in*)sa)->sin_addr);
                   }
   
                   return &(((struct sockaddr_in6*)sa)->sin6_addr);
           }



Datagram Client-Server
----------------------
    * do not need to use ``listen()`` or ``accept()``
    * **Server**

        .. code-block:: c

           #include <arpa/inet.h>
           #include <errno.h>
           #include <netdb.h>
           #include <netinet/in.h>
           #include <stdio.h>
           #include <stdlib.h>
           #include <string.h>
           #include <sys/socket.h>
           #include <sys/types.h>
           #include <unistd.h>
   
               #define port      "4950"
               #define maxbuflen 100
   
               void* get_in_addr(struct sockaddr*);
   
               int main(int argc, char* argv[])
               {
                       int                     sockfd;
                       struct addrinfo         hints, *servinfo, *p;
                       int                     rv;
                       int                     numbytes;
                       struct sockaddr_storage their_addr;
                       char                    buf[maxbuflen];
                       socklen_t               addr_len;
                       char                    s[inet6_addrstrlen];
   
                       memset(&hints, 0, sizeof(hints));
                       hints.ai_family   = af_inet6;
                       hints.ai_socktype = sock_dgram;
                       hints.ai_flags    = ai_passive;
   
                       if ((rv = getaddrinfo(null, port, &hints, &servinfo)) != 0) {
                               fprintf(stderr, "getaddrinfo: %s\n", gai_strerror(rv));
                               return 1;
                       }
   
                       for (p = servinfo; p != null; p = p->ai_next) {
                               if ((sockfd = socket(p->ai_family, p->ai_socktype,
                                                    p->ai_protocol)) == -1) {
                                       perror("server: socket");
                                       continue;
                               }
   
                               if (bind(sockfd, p->ai_addr, p->ai_addrlen) == -1) {
                                       close(sockfd);
                                       perror("server: bind");
                                       continue;
                               }
   
                               break;
                       }
   
                       if (p == null) {
                               fprintf(stderr, "server: failed to bind socket\n");
                               return 2;
                       }
   
                       freeaddrinfo(servinfo);
   
                       printf("server: waiting to recvfrom port %s...\n", port);
   
                       addr_len = sizeof(their_addr);
                       if ((numbytes = recvfrom(sockfd, buf, maxbuflen - 1, 0,
                                                (struct sockaddr*)&their_addr, &addr_len)) ==
                           -1) {
                               perror("recvfrom");
                               exit(1);
                       }
   
                       printf("server: got packet from %s\n",
                              inet_ntop(their_addr.ss_family,
                                        get_in_addr((struct sockaddr*)&their_addr), s,
                                        sizeof(s)));
   
                       printf("server: packet is %d bytes long\n", numbytes);
                       buf[numbytes] = '\0';
                       printf("server: packet contains \"%s\"\n", buf);
   
                       close(sockfd);
   
                       return 0;
               }
   
               void* get_in_addr(struct sockaddr* sa)
               {
                       if (sa->sa_family == af_inet)
                               return &(((struct sockaddr_in*)sa)->sin_addr);
   
                       return &(((struct sockaddr_in6*)sa)->sin6_addr);
           }


    * **Client**

        .. code-block:: c

           #include <arpa/inet.h>
           #include <errno.h>
           #include <netdb.h>
           #include <netinet/in.h>
           #include <stdio.h>
           #include <stdlib.h>
           #include <string.h>
           #include <sys/socket.h>
           #include <sys/types.h>
           #include <unistd.h>
   
           #define SERVERPORT "4950"
   
           int main(int argc, char* argv[])
           {
                   int             sockfd;
                   struct addrinfo hints, *servinfo, *p;
                   int             rv;
                   int             numbytes;
   
                   if (argc != 3) {
                           fprintf(stderr, "usage: client hostname message\n");
                           exit(1);
                   }
   
                   memset(&hints, 0, sizeof(hints));
                   hints.ai_family   = AF_INET6;
                   hints.ai_socktype = SOCK_DGRAM;
   
                   if ((rv = getaddrinfo(argv[1], SERVERPORT, &hints, &servinfo)) != 0) {
                           fprintf(stderr, "getaddrinfo: %s\n", gai_strerror(rv));
                           return 1;
                   }
   
                   for (p = servinfo; p != NULL; p = p->ai_next) {
                           if ((sockfd = socket(p->ai_family, p->ai_socktype,
                                                p->ai_protocol)) == -1) {
                                   perror("client: socket");
                                   continue;
                           }
   
                           break;
                   }
   
                   if (p == NULL) {
                           fprintf(stderr, "client: failed to create socket\n");
                           return 2;
                   }
   
                   if ((numbytes = sendto(sockfd, argv[2], strlen(argv[2]), 0, p->ai_addr,
                                          p->ai_addrlen)) == -1) {
                           perror("client: sendto");
                           exit(1);
                   }
   
                   freeaddrinfo(servinfo);
   
                   printf("client: sent %d bytes to %s\n", numbytes, argv[1]);
                   close(sockfd);
   
                   return 0;
           }



Poll Server
-----------

    .. code-block:: c

       #include <arpa/inet.h>
       #include <netdb.h>
       #include <poll.h>
       #include <stdio.h>
       #include <stdlib.h>
       #include <string.h>
       #include <sys/socket.h>
       #include <sys/types.h>
       #include <unistd.h>
   
       #define PORT    "8080"
       #define BACKLOG 10
   
       void* get_in_addr(struct sockaddr*);
       int   get_socket(void);
       void  add_to_fds(struct pollfd*[], int, int*, int*);
       void  del_from_fds(struct pollfd*[], int, int*);
   
       int main(int argc, char* argv[])
       {
               int                     sockfd, client_fd;
               struct sockaddr_storage client_addr;
               socklen_t               addr_len;
               char                    buf[256];
               char                    s[INET6_ADDRSTRLEN];
   
               int fd_count = 0;
               int fd_size  = 5;
   
               struct pollfd* fds = malloc(sizeof(*fds) * fd_size);
   
               sockfd = get_socket();
   
               if (sockfd == -1) {
                       fprintf(stderr, "server: socket\n");
                       exit(1);
               }
   
               fds[0].fd     = sockfd;
               fds[0].events = POLLIN;
   
               fd_count = 1;
   
               while (1) {
                       int poll_count = poll(fds, fd_count, -1);
   
                       if (poll_count == -1) {
                               perror("server: poll");
                               exit(1);
                       }
   
                       for (int i = 0; i < fd_count; ++i) {
                               if (fds[i].revents & POLLIN) {
                                       if (fds[i].fd == sockfd) {
                                               addr_len  = sizeof(client_addr);
                                               client_fd = accept(
                                                   sockfd,
                                                   (struct sockaddr*)&client_addr,
                                                   &addr_len);
   
                                               if (client_fd == -1) {
                                                       perror("server: accept");
                                               }
                                               else {
                                                       add_to_fds(&fds, client_fd,
                                                                  &fd_count, &fd_size);
                                                       printf(
                                                           "server: new connection "
                                                           "from %s on socket %d\n",
                                                           inet_ntop(
                                                               client_addr.ss_family,
                                                               get_in_addr((
                                                                   struct
                                                                   sockaddr*)&client_addr),
                                                               s, sizeof(s)),
                                                           client_fd);
                                               }
                                       }
                                       else {
                                               int nbytes    = recv(fds[i].fd, buf,
                                                                    sizeof(buf), 0);
                                               int sender_fd = fds[i].fd;
   
                                               if (nbytes <= 0) {
                                                       if (nbytes == 0) {
                                                               printf("server: socket "
                                                                      "%d hung up\n",
                                                                      sender_fd);
                                                       }
                                                       else {
                                                               perror("server: recv");
                                                       }
   
                                                       close(fds[i].fd);
                                                       del_from_fds(&fds, i,
                                                                    &fd_count);
                                               }
                                               else {
                                                       for (int j = 0; j < fd_count;
                                                            ++j) {
                                                               int dest_fd = fds[i].fd;
                                                               if (dest_fd != sockfd &&
                                                                   dest_fd !=
                                                                       sender_fd) {
                                                                       if (send(
                                                                               dest_fd,
                                                                               buf,
                                                                               nbytes,
                                                                               0) ==
                                                                           -1) {
                                                                               perror(
                                                                                   "se"
                                                                                   "rv"
                                                                                   "er"
                                                                                   ": "
                                                                                   "se"
                                                                                   "n"
                                                                                   "d");
                                                                       }
                                                               }
                                                       }
                                               }
                                       } // END handle data from client
                               }         // END got ready-to-read from poll()
                       }                 // END looping through file descriptors
               }                         // END while loop
               return 0;
       }
   
       int get_socket(void)
       {
               int sockfd;
               int yes = 1;
               int rv;
   
               struct addrinfo hints, *servinfo, *p;
   
               memset(&hints, 0, sizeof(hints));
               hints.ai_family   = AF_UNSPEC;
               hints.ai_socktype = SOCK_STREAM;
               hints.ai_flags    = AI_PASSIVE;
   
               if ((rv = getaddrinfo(NULL, PORT, &hints, &servinfo)) != 0) {
                       fprintf(stderr, "server: getaddrinfo %s\n", gai_strerror(rv));
                       exit(1);
               }
   
               for (p = servinfo; p != NULL; p = p->ai_next) {
                       if ((sockfd = socket(p->ai_family, p->ai_socktype,
                                            p->ai_protocol)) == -1) {
                               perror("server: socket");
                               continue;
                       }
   
                       if (setsockopt(sockfd, SOL_SOCKET, SO_REUSEADDR, &yes,
                                      sizeof(int)) == -1) {
                               perror("server: setsockopt");
                               continue;
                       }
   
                       if (bind(sockfd, p->ai_addr, p->ai_addrlen) == -1) {
                               perror("server: bind");
                               close(sockfd);
                               continue;
                       }
   
                       break;
               }
   
               freeaddrinfo(servinfo);
   
               if (p == NULL) {
                       return -1;
               }
   
               if (listen(sockfd, BACKLOG) == -1) {
                       return -1;
               }
   
               return sockfd;
       }
   
       void* get_in_addr(struct sockaddr* sa)
       {
               if (sa->sa_family == AF_INET) {
                       return &(((struct sockaddr_in*)sa)->sin_addr);
               }
               return &(((struct sockaddr_in6*)sa)->sin6_addr);
       }
   
       void add_to_fds(struct pollfd* fds[], int newfd, int* fd_count, int* fd_size)
       {
               if (*fd_count == *fd_size) {
                       *fd_size *= 2;
                       *fds      = realloc(*fds, sizeof(**fds) * (*fd_size));
               }
   
               (*fds)[*fd_count].fd     = newfd;
               (*fds)[*fd_count].events = POLLIN;
   
               ++(*fd_count);
       }
   
       void del_from_fds(struct pollfd* fds[], int i, int* fd_count)
       {
               fds[i] = fds[*fd_count - 1];
               --(*fd_count);
       }



Select Server
-------------

    .. code-block:: c

       #include <arpa/inet.h>
       #include <netdb.h>
       #include <stdio.h>
       #include <stdlib.h>
       #include <string.h>
       #include <sys/select.h>
       #include <sys/socket.h>
       #include <sys/types.h>
       #include <unistd.h>
   
       #define MAXBUFLEN 256
       #define SERVER    "localhost"
       #define PORT      "8080"
       #define BACKLOG   10
   
       void* get_in_addr(struct sockaddr*);
   
       int main(int argc, char* argv[])
       {
               struct addrinfo         hints, *servinfo, *p;
               struct sockaddr_storage client_addr;
   
               fd_set main_fds;
               fd_set read_fds;
               int    fdmax;
   
               int       sockfd, client_fd;
               socklen_t addrlen;
   
               char buf[MAXBUFLEN], s[INET6_ADDRSTRLEN];
               int  recv_bytes;
               int  yes = 1;
               int  i, j, rv;
   
               FD_ZERO(&main_fds);
               FD_ZERO(&read_fds);
   
               memset(&hints, 0, sizeof(hints));
               hints.ai_family   = AF_INET;
               hints.ai_socktype = SOCK_STREAM;
               hints.ai_flags    = AI_PASSIVE;
   
               if ((rv = getaddrinfo(SERVER, PORT, &hints, &servinfo)) != 0) {
                       fprintf(stderr, "server: getaddrinfo %s\n", gai_strerror(rv));
                       exit(1);
               }
   
               for (p = servinfo; p != NULL; p = p->ai_next) {
                       if ((sockfd = socket(p->ai_family, p->ai_socktype,
                                            p->ai_protocol)) == -1) {
                               perror("server: socket");
                               continue;
                       }
   
                       if (setsockopt(sockfd, SOL_SOCKET, SO_REUSEADDR, &yes,
                                      sizeof(int)) == -1) {
                               perror("server: setsockopt");
                               continue;
                       }
   
                       if (bind(sockfd, p->ai_addr, p->ai_addrlen) == -1) {
                               perror("server: bind");
                               close(sockfd);
                               continue;
                       }
                       break;
               }
   
               if (p == NULL) {
                       fprintf(stderr, "server: failed to bind\n");
                       exit(1);
               }
   
               freeaddrinfo(servinfo);
   
               if (listen(sockfd, BACKLOG) == -1) {
                       perror("server: listen");
                       exit(1);
               }
   
               printf("server: listening on port %s...\n", PORT);
   
               FD_SET(sockfd, &main_fds);
               fdmax = sockfd;
   
               while (1) {
                       read_fds = main_fds;
                       if (select(fdmax + 1, &read_fds, NULL, NULL, NULL) == -1) {
                               perror("server: select");
                               exit(1);
                       }
   
                       for (i = 0; i <= fdmax; ++i) {
                               if (FD_ISSET(i, &read_fds)) {
                                       if (i == sockfd) {
                                               addrlen   = sizeof(client_addr);
                                               client_fd = accept(
                                                   sockfd,
                                                   (struct sockaddr*)&client_addr,
                                                   &addrlen);
   
                                               if (client_fd == -1) {
                                                       perror("server: accept");
                                               }
                                               else {
                                                       FD_SET(client_fd, &main_fds);
                                                       if (client_fd > fdmax) {
                                                               fdmax = client_fd;
                                                       }
                                                       inet_ntop(
                                                           client_addr.ss_family,
                                                           get_in_addr((
                                                               struct
                                                               sockaddr*)&client_addr),
                                                           s, sizeof(s));
                                                       printf("server: got connection "
                                                              "from %s\n",
                                                              s);
                                               }
                                       }
                                       else {
                                               if ((recv_bytes =
                                                        recv(i, buf, sizeof(buf),
                                                             0)) <= 0) {
                                                       if (recv_bytes == 0) {
                                                               printf("server: socket "
                                                                      "%d hung up\n",
                                                                      i);
                                                       }
                                                       else {
                                                               perror("recv");
                                                       }
                                                       close(i);
                                                       FD_CLR(i, &main_fds);
                                               }
                                               else {
                                                       for (j = 0; j <= fdmax; ++j) {
                                                               if (FD_ISSET(
                                                                       j, &main_fds)) {
                                                                       if (j !=
                                                                               sockfd &&
                                                                           j != i) {
                                                                               if (send(
                                                                                       j,
                                                                                       buf,
                                                                                       recv_bytes,
                                                                                       0) ==
                                                                                   -1) {
                                                                                       perror(
                                                                                           "send");
                                                                               }
                                                                       }
                                                               }
                                                       }
                                               }
                                       } // END handle data from client
                               }         // END handle new connection
                       }                 // END looping through file descriptors
               }                         // END while loop
   
               return 0;
       }
   
       void* get_in_addr(struct sockaddr* sa)
       {
               if (sa->sa_family == AF_INET) {
                       return &(((struct sockaddr_in*)sa)->sin_addr);
               }
   
               return &(((struct sockaddr_in6*)sa)->sin6_addr);
       }



Encode into IEEE-754
--------------------
    * encode floats and doubles into IEEE-754 format

    .. code-block:: c

       #include <inttypes.h>
       #include <stdint.h>
       #include <stdio.h>
   
       #define pack754_32(f)   (pack754((f), 32, 8))
       #define pack754_64(f)   (pack754((f), 64, 11))
       #define unpack754_32(f) (unpack754((f), 32, 8))
       #define unpack754_64(f) (unpack754((f), 64, 11))
   
       uint64_t pack754(long double f, unsigned bits, unsigned expbits)
       {
               long double fnorm;
               int         shift;
               long long   sign, exp, significand;
               unsigned    significandbits = bits - expbits - 1; // -1 for sign bit
   
               if (f == 0.0) {
                       return 0;
               }
   
               // check sign and begin normalisation
               if (f < 0) {
                       sign  = 1;
                       fnorm = -f;
               }
               else {
                       sign  = 0;
                       fnorm = f;
               }
   
               // get the normalised form of f and track exponent
               shift = 0;
               while (fnorm >= 2.0) {
                       fnorm /= 2.0;
                       ++shift;
               }
   
               while (fnorm < 1.0) {
                       fnorm *= 2.0;
                       --shift;
               }
   
               fnorm -= 1.0;
   
               // calculate binary form (non-float) of significand data
               significand = fnorm * ((1LL << significandbits) + 0.5f);
   
               // get biased exponent
               exp = shift + ((1 << (expbits - 1)) - 1); // shift + bias
   
               return (sign << (bits - 1)) | (exp << (bits - expbits - 1)) |
                      significand;
       }
   
       long double unpack754(uint64_t i, unsigned bits, unsigned expbits)
       {
               long double result;
               long long   shift;
               unsigned    bias;
               unsigned    significandbits = bits - expbits - 1; // -1 for sign bit
   
               if (i == 0) {
                       return 0.0;
               }
   
               // pull significand
               result  = (i & ((1LL << significandbits) - 1)); // mask
               result /= (1LL << significandbits);             // convert back to float
               result += 1.0f;                                 // add one back
   
               // deal with exponent
               bias  = (1 << (expbits - 1)) - 1;
               shift = ((i >> significandbits) & ((1LL << expbits) - 1)) - bias;
               while (shift > 0) {
                       result *= 2.0;
                       --shift;
               }
   
               while (shift < 0) {
                       result /= 2.0;
                       ++shift;
               }
   
               // sign it
               result *= (i >> (bits - 1)) & 1 ? -1.0 : 1.0;
   
               return result;
       }
   
       int main(int argc, char* argv[])
       {
               float    f = 3.1415926, f2;
               double   d = 3.14159265358979323, d2;
               uint32_t fi;
               uint64_t di;
   
               fi = pack754_32(f);
               f2 = unpack754_32(fi);
   
               di = pack754_64(d);
               d2 = unpack754_64(di);
   
               printf("float before: %.7f\n", f);
               printf("float encoded: 0x%08" PRIx32 "\n", fi);
               printf("float after: %.7f\n\n", f2);
   
               printf("double before: %.20lf\n", d);
               printf("double encoded: 0x%08" PRIx64 "\n", di);
               printf("double after: %.20lf\n\n", d2);
   
               return 0;
       }



`back to top <#network-programming>`_

Additional Resources
====================

* `Beej's Guide`_, `OSI Model`_
* `TCP (RFC-793)`_, `IP (RFC-791)`_, `UDP (RFC-768)`_
* `Private Address Allocation (RFC-1918)`_, `Unique Local Ipv6 (RFC-4193)`_
* `Special-Use Domain Names (RFC-6761)`_, ` Reserved Top Level DNS Names (RFC-2606)`_
* `Identification Protocol (RFC-1413)`_, `HTTP (RFC-2616)`_
* `Floating-Point Arithmetic (IEEE-754)`_, `External Data Representation (RFC-4506)`_

`back to top <#network-programming>`_
