# CS FYI - How does the internet work?

[https://cs.fyi/guide/how-does-internet-work](https://cs.fyi/guide/how-does-internet-work) 

### Introduction to the Internet

****************Network:****************

A network is a group of computers and/or devices that are connected with each other. The internet is made out of many networks. 

****************Origins:****************

The internet originated in the 1960s as a project by the US Department of Defense. They wanted to create a communication network that could withstand a nuclear attack 

### How the Internet Works

The internet works by connecting devices and computer systems together using a standard protocol. These protocols define how information is exchanged and ensures that the information is transmitted securely and reliably. 

### Core of the Internet

The core of the internet is formed by routers. These routers breaks down data into packets and transmits these packets from devices to other routers to the final destination device. 

Internet Protocol (IP) and Transmission Control Protocol (TCP) are protocols that standardise the way the internet communicates. IP ensures routers transmit packages to the right destination. TCP ensures packets are transmitted in a reliably and correct order 

### Basic Concepts and Terminology

**************Packet:************** Packets are small pieces of data that are transferred 

**************Router:************** Routers help to direct the packets of data 

****IP Address:**** IP Addresses are unique identifiers assigned to each device. These IP Addresses ensure that packets are correctly transferred to the right destination 

**********DNS:********** DNS stands for Domain Name System. It’s symbol a mapping of IP Addresses to Human Readable names (e.g. Google.com). 

**********HTTP:********** HTTP stands for HyperText Transfer Protocol. It’s a standardised way of transferring data from clients (e.g. Web Browsers) to Servers and vice versa. 

************HTTPS:************ This is the encrypted/secure version of HTTP 

**************TLS/SSL:************** TLS stands for Transport Layer Security and SSL stands for Secure Sockets Layer. Both are used to provide secure communication in the internet 

### Protocols

Protocols like IP, DNS, HTTP, and TCP are used to standardise communication methods. This is important because it ensures that when devices or servers are created by different producers, they are able to communicate with each other as there is a shared communication standard. 

### IP Addresses + Domain Name

IP Addresses are a route to ensure data is transferred to the right destination. It is usually composed of 4 numbers (e.g. 192.100.8.10). Domain Names are the human readable versions of IP addresses. Domain Names are matched to IPs using DNS (Domain Name System). For example, when you enter a Domain Name into the web browser (e.g. Google.com), your computer sends a DNS query to a DNS Server. This DNS Server will then return the IP address that corresponds to the Domain Name. 

### Introduction to HTTP and HTTPS

HTTP and HTTPs are two of the most commonly used protocols in internet-based apps and services

HTTP is used to transfer data between a client and a server. When you visit a website, your web browser sends an HTTP request to the server, asking for the webpage or other resource you’ve requested. The server then sends an HTTP response back to the client, containing the requested data.

HTTPS is the more secure version of HTTP. It encrypts the data being transmitted using SSL (Secure Sockets Layer) or TLS (Transport Layer Security) encryption. This ensures that you have an additional layer of security - e.g. login credentials, payment information, and other personal data is more secure. 

### Building Applications with TCP/IP

TCP/IP (Transmission Control Protocol/Internet Protocol) is the underlying communication protocol used by most internet-based applications and services. It provides a reliable, ordered, and error-checked delivery of data between applications running on different devices. 

There are a few key concepts to understand:

- **************PORTS:************** Used to identify the application or service running on a device. Each application or service is assigned a unique port number. This allows data to be sent to the correct destination
- ******************SOCKETS:****************** Sockets are a combination of IP Address as well as the Port. It represents a specific endpoint for communication and connections between devices and the transfer of data between applications
- **************************CONNECTIONS:************************** A connection is established between two sockets when two devices want to communicate with each other. The devices will negotiate various parameters such as maximum segment size and window size etc
- ******************************DATA TRANSFER:****************************** Once a connection is established, data can be transferred between the applications running on each device. Data is typically transferred in segments, with each segment containing a sequence number and other metadata to ensure reliable delivery

When building a TCP/IP application, you’ll need to ensure that your app is designed to work with the appropriate ports, sockets, and connections. You’ll also need to be familiar with the various protocols and standards that are commonly used with TCP/IP, such as HTTP, FTP (File Transfer Protocol), and SMTP (Simple Mail Transfer Protocol). 

### Securing Internet Communication with SSL/TLS

SSL (Secure Sockets Layer) and TLS (Transfer Layer Security) is a protocol used to encrypt data being transmitted over the internet. There are several concepts that are important to this:

- ****************************CERTIFICATES:**************************** Certificates are used to create trust between the client and the server. It contains information like the identify of the server and the certificate is signed by a trusted 3rd party.
- **********************HANDSHAKE:********************** During the SSL/TLS handshake process, the client and server exchange information to negotiate the encryption algorithm and other parameters for the secure connection
- ************************ENCRYPTION:************************ Once the secure connection is established, the data is encrypted using an agreed upon algorithm. This data can then be transmitted securely between the client and the server

It’s crucial to have a good understand of SSL/TLS so that your application is designed to use it when transmitting sensitive data such as login credentials, payment information, and other personal data. You will need to do things such as obtaining and maintaining valid SSL/TLS certificates for your servers, configure your SSL/TLS connections and other things.