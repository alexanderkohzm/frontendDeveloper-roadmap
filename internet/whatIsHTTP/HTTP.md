# HTTP

# If you had to teach this to someone …

HTTP is a protocol that is used to standardise the way clients and servers communicate and send data to each other. It is built upon the TCP/IP protocol

Initially, HTTP started out as a very basic GET request (to ask for data) and once the request was completed the connection between the client and server closed.

The beauty of HTTP is that it is extensible through its headers. Over time, from HTTP/0.9 to HTTP/1.1, more and more functionality was added to HTTP. For example, the POST, PUT, PATCH, DELETE, OPTIONS methods were added. Furthermore, HTTP/1.1 introduced keeping the connection alive between the client and server. This means that the client and server could send multiple sequences of request/response without closing the connection, thus reducing inefficiencies by reducing the number of times the three-way handshake needed to be made

Over time, however, the web became more and more demanding (in terms of data and connections it required) and HTTP/1.1 struggled to keep up. For example, by 2009, a simple website needed more than 30 connections and HTTP/1.1 could only keep one connection alive. Some web developers tried to find ways around it (sending HUGE JavaScript files) but this was not sustainable.

Google experimented and had a new protocol to improve network performance. Their improvement, SPDY, was integrated into HTTP as HTTP/2 as Google did not want competing protocols. HTTP/2 introduced things such as converted HTTP into Binary rather than text, multiplexing, header compression, server push, prioritisation, and security.

The key part of HTTP/2 is converting the text format to binary. Now HTTP requests and responses are made out of Streams, which themselves are made out of Frames. Frames include HEADER (metadata) and DATA (payload) amongst others (RST_STREAM to cancel stream). Each frame is associated with a stream via a stream id.

Multiplexing involves the asynchronous sending of frames. This means that once the TCP connection is open, the client sends frames of requests asynchronously which is also processed asynchronously by the server. This solves the head-of-line blocking issue that existed in HTTP/1.x

Header compression involves omitting repetitive headers while server push involves the server pre-empting requests it knows it will have to receive and sending it ahead of time in a PUSH_PROMISE. Requests can be given a priority which affects how much resources a server allocates to it.

[Everything you need to know about HTTP - CS FYI](HTTP%2009952debc2734ef39c743ff8435af4b9/Everything%20you%20need%20to%20know%20about%20HTTP%20-%20CS%20FYI%20fa3d14524b614d4bace4fb6970391524.md)
