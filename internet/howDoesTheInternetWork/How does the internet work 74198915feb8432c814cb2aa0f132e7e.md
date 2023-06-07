# How does the internet work

# If you had to teach this…

First, we need to know **what is the internet**. The internet is a network of computers that are connected together.

**How they are connected together** - data is transferred between computers in packets (small pieces of data). Routers ensure that the packets are sent to the right destination. The right destination is determined by IP Addresses (e.g. 192.168.10.10).

The reason why you don’t have to memorise IP Addresses is because we use a system called **Domain Names and the Domain Name System**. A Domain Name consists of a name and and a postfix (e.g. Google.com). Domain Names are mapped to IP Addresses using the Domain Name System. So whenever you go to your browser and type in a domain name, your web browser sends a request to the Domain Name Server which responds with the appropriate IP Address.

IP Addresses and Domain Names are part of Protocols in the internet. Protocols are crucial because they standardise the way servers and clients communicate with each other on the internet. This is especially important because it ensures that different manufacturers and service providers can all communicate with each other even though clients/services etc are all built from different entities.

Other Protocols include **HTTP** (HyperText Transfer Protocol), which is used to standardise the transfer of information between clients and servers. **HTTPS** (HTTP Secure) is secure because of the **TLS/SSL** protocol (Transfer Layer System, Secure Sockets System), which encrypts data to ensure that sensitive data is securely transferred over the network.

TCP/IP (Transmission Control Protocol and Internet Protocol) are used to ensure that data is transferred to the right destination (IP Addresses) and that the data is transferred in a reliable and secure way (TCP). Applications/Services are assigned a **port number**. This ensures that data is transferred to the right destination. **Sockets** are a combination of IP Addresses and port numbers - this allows for **connections** to be made between devices.

You should also need to know **how the data is transferred over the internet.** Routers require **modems**, which transforms the data sent on our network into data that the infrastructure which the internet relies on the transfer data can handle. For example, a modem is used to transform data into a format which telephone or electrical cables can handle. Data is transferred from your computer to the router, which then transfers it to the modem. **Internet Service Providers** are then relied on to transfer data using special routers that allow them to connect to other routers controlled by other ISPs in the network. These ISPs eventually relay the information to the modem, router, and finally the destination device.

---

[CS FYI - How does the internet work? ](How%20does%20the%20internet%20work%2074198915feb8432c814cb2aa0f132e7e/CS%20FYI%20-%20How%20does%20the%20internet%20work%209efbded4b6be4f7a931861b11bbf32e7.md)

[Developer Mozilla - How does the Internet work? ](How%20does%20the%20internet%20work%2074198915feb8432c814cb2aa0f132e7e/Developer%20Mozilla%20-%20How%20does%20the%20Internet%20work%200aecab1fdcb443ab8711a58dd4847ac8.md)
