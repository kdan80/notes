## 1 - HTTP

HTTP is a client-server, application-layer protocol (Layer 7) for sending hypermedia. HTTP is a stateless protocol meaning no data is kept between requests. A client, such as a web browser, sends an HTTP request and a web server processes and responds to that request with an HTTP response.

Headers can be hop-by-hop (allowing intemediary nodes e.g. caching, proxy servers) or end-to-end.

The http specification is determined by the Internet Egineering Task Force (IETF) and published in Request For Comments (RFCs).

| Version | Year | Status | Specification |
| ------- | ---- | ------ | ---- |
| HTTP/0.9 | 1991 | Obsolete | No headers, status or error codes. Serial requests only.
| HTTP/1.0 | 1996 | Obsolete | Headers, status & error codes introduced.
| HTTP/1.1 | 1997 | Standard | Chunking, pipelining, re-usable/persistent connections introduced.
| HTTP/2.0 | 2015 | Standard | Parallel/Multiplex requests, compressed headers & server push introduced.
| HTTP/3.0 | 2022 | Standard | QUIC/UDP replaces TCP. TLS is builtin.

### HTTP Authentication

Basic - Uses the `WWW-Authenticate: Basic` header. Username and password are sent unencrypted but encoded as base64 (If sent over https the header is encrypted). Username and password are concatenated by a colon eg admin:password1. Credentials must be sent with every request. Logging out is achieved by overwriting credentials with invalid credentials. The browser has a built-in prompt/pop-up for entering credentials.

Digest - Uses an md5 hash to encrypt password but otherwise has same pitfalls as basic. The server sends a one-time number, called a nonce, which is combined with credentials prior to hashing.

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
2. A request header. 
3. An optional body.

**Request line** - Takes the form METHOD-URI-PROTOCOL e.g. `GET /inde.html HTTP/1.1`

**Request Header** - A set of header fields that take the form name:value e.g. `Host: google.com`, `Content-Type: application/json`

**Request Body** - An optional field (not used by **GET**) used to send additional information to the server e.g. username and password or a blog post. Can contain virtually any typ of data, JSON is commonly used.

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