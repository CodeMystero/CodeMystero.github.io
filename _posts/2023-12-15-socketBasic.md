---
layout: single

title: "[Linux] Socket  networking"
excerpt: "transmits data from client to server"

categories:
  - Linux server

tag: [linux, ubuntu, socket, server, client, chatting] 

toc: true
toc_sticky: true

author_profile: false
sidebar:
  nav: "docs"
---

![header](https://capsule-render.vercel.app/api?type=rect&color=20:660099,100:E2231A)


# Hosting server 

To set up a server, remember the following four steps: socket, binding, listen, accept.

## Socket 

>**Socket is file !!!**

```cpp
int serv_sock;
int clnt_sock;

struct sockaddr_in clnt_addr;
int clnt_addr_size;

struct sockaddr_in serv_addr;
serv_sock = socket(PF_INET, SOCK_STREAM, 0);
```

### PF_INET 

PF_INET is to specify IPv4 when socket() is created. 

- PF_INET: Protocol Family Internet (IPv4)
- PF_INET6: Protocol Family Internet version 6 (IPv6)


### SOCK_STREAM

SOCK_STREAM provides sequenced, reliable, two-way, connection-based byte streams. An out-of-band data transmission mechanism may be supported. 

### socket()

>#include &lt;sys/types.h&gt;<br>#include &lt;sys/socket.h&gt;<br><br>int socket(int <u>domain</u>, <u>int type</u>, int <u>protocol</u>);


## Bind

"Binding" is the process of assigning necessary information such as address, port, and protocol to a socket.

```cpp
serv_addr.sin_family = AF_INET;
serv_addr.sin_addr.s_addr = htonl(INADDR_ANY);
serv_addr.sin_port = htons(atoi("8080"));

if(bind(serv_sock,(struct sockaddr*)&serv_addr, sizeof(serv_addr))==-1){
    printf("bind error\n");
}
```

### AF_INET

AF_INET signifies that the socket is configured to use IPv4 addresses. This enables the socket to communicate using the Internet Protocol. It is a fundamental setting commonly employed in network applications.

### INADDR_ANY

By using this keyword, you don't have to manually find and input the IP address of your hardware; instead, this keyword automatically fetches your server's IP address for you.

### htonl()

This keyword is used to convert the IP address (32 bits) from host format to network format for consistency across different hardware architectures, as endianness can vary. Since the size of endianness differs between hardware (big endian, little endian), this keyword ensures a uniform rule for representation.

### atoi()

Convert integer to ASCII type

### htons()

Unlike htonl, this keyword is used when converting short address formats, such as the port (16 bits), to network format.

>#include &lt;sys/socket.h&gt;<br><br>int bind (int <u>sockfg</u>, const struct sockaddr *<u>addr</u>, socklen_t <u>addrlen</u>);

## Listen 

```cpp
if(listen(serv_sock,5) == -1){
      printf("listen error\n");
}
```

The server, represented by serv_sock, is now prepared to accept clients. The constant 5 in the backlog indicates that it can queue up to 5 connections.

## Accept

```cpp
char buff[200];
int recv_len = 0;
while(1){
    //accept 
    clnt_addr_size = sizeof(clnt_addr);
    clnt_sock = accept(serv_sock, (struct sockaddr*)&clnt_addr, &clnt_addr_size);

    while(1){
        recv_len = read(clnt_sock, buff, 200);

        for(int i = 0;i<recv_len;i++){
        printf("%c",(unsigned char)buff[i]);
        }
        printf("\n");
    }
}
```
### accept()

```console
clnt_sock = accept(serv_sock, (struct sockaddr*)&clnt_addr, &clnt_addr_size);
```

- The accept function accepts a client connection and returns a new socket descriptor (clnt_sock) for communication with that client.
- serv_sock: Server socket descriptor.
- &clnt_addr: Pointer to the structure to hold client address information.
- &clnt_addr_size: Size of the client address information structure.

### Data receeption and output

```cpp
recv_len = read(clnt_sock, buff, 200);

for(int i = 0; i < recv_len; i++){
    printf("%c", (unsigned char)buff[i]);
}
printf("\n");
```

- The read function reads data from the client socket into the buff array.
- recv_len: Length of the read data.
- The read data is then iterated through in a loop, and each character is printed to the console.

# Making client 

## Socket Creation

```cpp
sock = socket(PF_INET, SOCK_STREAM, 0);
if(sock == -1) printf("socket() error\n");
```
Creates a socket using the socket function. Prints an error message if the socket creation fails.

## Server address initialization

```cpp
memset(&serv_addr, 0, sizeof(serv_addr));
serv_addr.sin_family = AF_INET;
serv_addr.sin_addr.s_addr = inet_addr("127.0.0.1 ");
serv_addr.sin_port = htons(atoi("8080"));
```

## memset()

'memset' is a C library function that stands for "memory set." It is used to fill a bloack of memory with a particular value. The function is typically used to initialize arrays, structures, or other data structurs with a specific constant value. 

```bash
void *memset(void *ptr, int value, size_t num);
```

- ptr: A pointer to the memory block to be filled.
- value: The value to be set. It is usually of type int and represents the byte value that will be copied to each byte of the memory block.
- num: The number of bytes to be set to the value.

## Connection to server

```bash 
if(connect(sock, (struct sockaddr *)&serv_addr, sizeof(serv_addr)) == -1){
    printf("connect() error");
}
```

Connects to the server using the connect function. Prints an error message if the connection fails.

## Message Sending Loop

```cpp
char greeting[20] = {'h','e','l','l','o'};

while(1){
    write(sock, greeting, 20);
    sleep(1);
}
```
