# Everything you need to know about HTTP - CS FYI

[https://cs.fyi/guide/http-in-depth](https://cs.fyi/guide/http-in-depth) 

### What is HTTP?

HTTP is a protocol that powers the entire web. It is built upon TCP/IP and standardises the way clients and servers communicate with one another. It defines how content is requested and transmitted across the internet. 

We should think about HTTP as a layer of abstraction that standardises communication. It relies on TCP/IP and abstracts it. 

### HTTP/0.9

HTTP originally started out as a one liner. It was simply a GET request and once it was completed the connection was closed

### HTTP/1.0

HTTP/0.9 could only deal with HTML. HTTP/1.0 improved functionality as it could now deal with images, video files, plain text, and other file types. It also added the POST and HEAD methods. Other things added include formatting for response and request, HTTP headers, and status codes. Clients also started to send personal information in headers (something that couldn’t be done before as there were no headers).

A major drawback was that you couldn’t have multiple requests per connection. Every time a client needs something from the server, it will have to open a new TCP connection. And after the request is fulfilled, the connection is closed. This is bad if you have multiple files - you could get performance issues as you would need to perform the three-way handshake every time. 

### Three-way Handshake

All TCP connections begin with a Three-way handshake in which the client and server share a series of packets before starting to share the application. 

Client → Sends syn (random number) → Server 

Server → Acknowledges the request, sends an ACK package back with the random number + an increment → Client 

Client → Takes ACK and increments it → Server 

Once the three-way handshake is complete, the data sharing commences. HTTP1.0 tried to fix inefficiencies with the three-way handshake (having to open multiple connections) by including a “Connection: Keep Alive” header. 

### HTTP/1.1

HTTP/1.1 introduced new methods like PUT, PATCH, OPTIONS, and DELETE. A Hostname (previously optional) was now required. It also introduced persistent connection 

A “header connection: close” was made available, thus allowing clients to request for connections to be closed. 

Pipelining was also introduced. Clients could now send multiple requests to their server without waiting for the response from the server on the same connection. The server had to respond in the same sequence in which requests were received. 

Content length header - this allowed for pipelining. Clients could use it to identify where the response ends 

### Chunked Transfers

In the case of dynamic content and when the server is unable to find the content-length, it may start to send content in pieces and add the content-length for each chunk when it is sent 

When all the chunks are sent, it sends an empty chunk (content-length 0) so the client knows that the transmission is over 

HTTP/1.1 was introduced in 1999 and was widely used all over. The problem was that as the web developed it became more data and resource intensive. Simple websites nowadays need more than 30 connections and HTTP/1.1 could not solve this as it can have only up to ONE outstanding connection at a time! 

### SPDY

As mentioned before, HTTP/1.1 started to see performance issues into the 2000s as websites became more and more demanding (load, data required). Google experimented to see what alternative protocols they could introduce to make the web faster and they introduced SPDY in 2009. 

<aside>
💡 One of the things that Google noticed is that increasing the total bandwidth does improve network performance but it will eventually see less and less improvement. Improving latency, however, will see consistent performance gain 

Bandwidth: The amount of data moved per second in the network 
Latency: The total amount of time it takes for the data to move from the source to the destination

</aside>

SPDY features include multiplexing, compression, prioritisation, and security. Eventually Google did not want to have competing protocols so it merged SPDY into HTTP, thus forming HTTP/2 in 2015 

### HTTP/2

HTTP/2 was designed for low latency transport of content. The key differences it introduced include:

1. Binary - data is transferred through binary rather than text 
2. Multiplexing - multiple asynchronous HTTP requests over a single connection
3. Header compression with HPACK 
4. Server Push - multiple server responses for a single connection (pre-empting requests) 
5. Request prioritisation 
6. Security 

### HTTP/2 - Binary

With HTTP/2 came the shift to binary. This meant that HTTP became non-human readable. This allowed it to become much faster. 

The key to this binary are “frames” and “streams”. HTTP messages were no composed or one or more frames (frames include HEADERS, DATA, RST_STREAM etc). 

Every HTTP/2 requests and response is given a unique stream ID and it is divided into frames. Frames are binary pieces of data and a collection of frames is called a Stream. Each frame has a stream id that identifies the stream to which it belongs and each frame has a common header. 

Another type of frame is the RST_STREAM frame. Previously, if you wanted to cancel or stop the server from sending responses to the client you had to close the connection. But created inefficiencies as a new connection had to be opened for future requests. Thus, RST_STREAM stops the stream whilst keeping the connection open 

### HTTP/2 - Multiplexing

Multiplexing is the act of sending many asynchronous requests from the client and the server responding in the same way by sending many asynchronous responses. The client uses stream IDs to match the stream to which the packet is receives is from. 

### HTTP/2 - Header Compression

There is a lot of redundant data that is sent over and over again. HTTP/2 introduced header compression - both the client and server keep a table of repetitive headers (e.g. user agent) 

### HTTP/2 - Server Push

The server kind of knows what the client wants and is going to demand. Thus, they can anticipate this and pre-empt the client demand by sending a PUSH_PROMISE. This tells the client that it has already sent the required data and tells the client NOT to ask for it in a future request 

### HTTP/2 - Request Prioritisation

A client can assign a priority to a stream - it can include the prioritisation information in the HEADERS frame by which a stream is opened. 

Without any priority information, server processes the requests asynchronously (i.e. without any order). If there is a priority assigned to a stream, the server can then decide how much resources should be assigned to process the request 

### HTTP/2 - Security

While there was much discussion, TLS did not become mandatory for HTTP/2. However, most vendors state that they will only support HTTP/2 when it is used over TLS. Thus, while it is not explicitly stated, TLS and encryption has become mandatory by default anyways.