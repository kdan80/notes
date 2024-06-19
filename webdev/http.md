## 1 - HTTP

HTTP is a client-server, application-layer protocol (Layer 7) built on top of TCP for sending hypermedia and other data. HTTP is a stateless protocol meaning no data is kept between requests. A client, such as a web browser, sends an HTTP request and a web server processes and responds to that request with an HTTP response. HTTP requests are sent entirely in clear text. The request may pass through many proxies, routers, modems, caches etc but these are hidden to the http protocol as they operate at the network and transport layers.

Headers can be hop-by-hop (allowing intemediary nodes e.g. caching, proxy servers) or end-to-end.

The http specification is determined by the Internet Egineering Task Force (IETF) and published in Request For Comments (RFCs).

| Version | Year | Status | Specification |
| ------- | ---- | ------ | ---- |
| HTTP/0.9 | 1991 | Obsolete | No headers, status or error codes. Serial requests only.
| HTTP/1.0 | 1996 | Obsolete | Headers, status & error codes introduced.
| HTTP/1.1 | 1997 | Standard | Chunking, pipelining, re-usable/persistent connections introduced.
| HTTP/2.0 | 2015 | Standard | Parallel/Multiplex requests, compressed headers & server push introduced.
| HTTP/3.0 | 2022 | Standard | QUIC/UDP replaces TCP. TLS is builtin.

**Proxies**  
Between the web browser and server numerous computers exist that may relay or modify the http request. They may be at the network and transport layers but those at the application layer are called proxies. They may perform numerous functions:  

* Caching
* Filtering (parental controls, virus scanners)
* Load balancing
* Authentication
* Logging

**HTTP/0.9**  
Before a client and server can exchange HTTP messages a TCP connection must be established. This requires a 3 way handshake that is time consuming. The initial HTTP spec (since renamed HTTP/0.9) was built on top of this TCP based connection and  required a new TCP connection for each request (HTTP was always initiated by the client). 

Requests consisted of a single line of ASCII characters terminated by a CR LF (carriage return, line feed) pair. Only GET requests were possible and only HTML could be transmitted (errors and other infomation were contained within the HTML documents). Requests are idempotent.

Responses were sent as HTML in a byte stream consisting of ASCII characters. Lines were delimited by an LF or CR LF. After the whole document was sent the server would terminate the connection.

**HTTP/1**  
HTTP was extended with versioning, status codes and headers were introduced for both requests and responses. The Content-Type header meant documents other than HTML could now be transmitted.

To mitigate the need for a new TCP connection for each request HTTP/1.1 introduced "keep-alive"/persistent connections, which could be partially controlled with a conenction header e.g. `Connection: keep-alive`.

HTTP/1.1 also introduced pipelining allowing multiple requests to be sent without having to wait for a response before the next request is sent, however responses had to be received in the same order as the requests were sent. As it was difficult to implement most browsers removed support for pipelining.

Pipelining also suffered from Head-of-line blocking, where if any request was blocked all subsequent requests were also blocked.

**HTTP/2**  
HTTP/2 messages, although following the same format as HTTP/1, are embedded into a binary structure called a frame. This allowed for optimizations like compression and multiplexing. Browsers translate textual data to (and from) this binary format.  The frame header (not to be confused with HTTP headers) includes the length of the frame, frame type etc. Following the frame header is the payload (HTTP headers, body etc).

It also introduced "streams"
(a successor to pipelining) where multiple requests could be sent in tandem on the same TCP connection. Each stream is independent and does not need to be sent in order unlike pipelining. Headers are also compressed.

HTTP Push was also introduced where the server can send new data to the client without requiring a request from the client.

**HTTP/3**  
Uses QUIC (a UDP based protocol) as the transfer protocol and streams are now first-class citizens and operate at the transport layer. Once a QUIC connection/stream has been established no further handshaking is required and all requests can reuse the same connection. Streams are identified by IDs, odd IDs for client initiated streams and even for server initiated streams. A stream is concluded by a "Fin" bit on the last stream frame.

QUIC uses packet numbers to identity when a dropped packet has been retransmitted making it more resilient to reordering and packet loss.

QUIC can also handle a client connection switching networks e.g. from 4G to 5G and keeps the connection alive (with the same QUIC ID) something that is not possible with TCP based connections.

### HTTP Authentication

**Basic**  
Uses the `WWW-Authenticate: Basic` header. Username and password are sent unencrypted but encoded as base64 (If sent over https the header is encrypted). Username and password are concatenated by a colon eg admin:password1. Credentials must be sent with every request. Logging out is achieved by overwriting credentials with invalid credentials. The browser has a built-in prompt/pop-up for entering credentials.

**Digest**  
Uses an md5 hash to encrypt password but otherwise has the same pitfalls as basic. The server sends a one-time number, called a nonce, which is combined with credentials prior to hashing.

With http authentication the web server (apache, nginx etc) is responsible for handling authentication as opposed to application authentication which is handled by the framework (django, laravel etc).

### HTTP Request Methods

HTTP defines methods (verbs) that identify the desired action to be performed;

**GET** - For the retrieval of data.

**POST** - Used for sending new data to the server.

**PUT** - Used for replacing existing data on the server. The client specifies the resource. The entire resource is modified.

**PATCH** - Similar to **PUT** but the resource on the server is only partially modified.

**HEAD** - Used for the retrieval of meta-information, similar to **GET** but no body is returned.

**DELETE** - For deleting data on the server.

**OPTIONS** - Returns what HTTP methods a server supports.

**CONNECT** - A hop-by-hop method used for creating an HTTP tunnel through a proxy server to a desired resource.

**TRACE** - The client request is returned in the body of the response, that way modifications by intermediaries can be understood. Used for debugging.

### HTTP Request

An HTTP request is composed of 3 parts; 
1. A request line.
2. Request headers. 
3. A blank line.
4. An optional body.

**Request line**  
Takes the form METHOD-URI-PROTOCOL;   
`GET /index.html HTTP/1.1`  
The URI may also contain a "?" indicating the start of a query string;  
`GET /index?query=hello.html HTTP/1.1`

**Request Headers**  
A set of header fields that take the form; name:value 
`Host: google.com`   
`Content-Type: application/json`

The header fields allow for additional information to be sent to the server, e.g. authentication data, content type, desired response type etc

**Blank Line**  
The blank line indicates that all metadata has been sent.

**Request Body**  
An optional field (not used by **GET**) used to send additional information to the server e.g. username and password or a blog post. Can contain virtually any typ of data, JSON is commonly used.

**Example HTTP Request**
```
POST /api/login HTTP/1.1
Host: example.com
Content-Type: application/json
Cache-Control: no-cache

{
     "Username": "admin",
     "Password": "password123"
}
```

Note there is a blank line between the header and body.

### HTTP Response

Like a request an http response is composed of 3 parts differing only in the composition of the first line, which is called the status line; 

**Status line**  
The start of the response includes the http protocol and version, a status code and associated text.  
`HTTP/1.1 200 OK`  
`HTTP/1.1 404 Not Found`

**Status Codes**  
The first digit of a response status code defines its class:

`1XX` Informational  
The request was received, continuing process.  
`2XX` Successful  
The request was successfully received, understood, and accepted.  
`3XX` Redirection  
Further action needs to be taken in order to complete the request.  
`4XX` Client error  
The request contains bad syntax or cannot be fulfilled.  
`5XX` Server error  
The server failed to fulfill an apparently valid request.