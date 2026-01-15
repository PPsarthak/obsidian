#system-design #networking #http #osi 

Hyper Text Transfer Protocol(HTTP) is an application layer protocol that is used to access and transfer data
HTTP is a client-server protocol that runs on top of the TCP/IP family of protocols and uses the request/response protocol.

---
### Features of HTTP

- **HTTP is connectionless:** After serving a single HTTP request, the client-server connection is closed and that same connection is never used again.
- **HTTP is media independent:** It means that HTTP can send any sort of data as long as both the client and the server understand how to process the data.
- **HTTP is stateless:** The client and server only know about each other during the current request, and when the connection is disconnected, both the client and the server forget about each other.

---
### HTTP Request-Response Flow

1. Establish the connection
2. Send the Request
3. Send the Response
4. Close the connection

---
### HTTP Methods:

- **GET**: Retrieve data from a server (e.g., fetching a webpage).
- **POST**: Send data to the server, commonly used in form submissions or data uploads.
- **PUT**: Update an existing resource on the server.
- **DELETE**: Remove a specified resource on the server.
- **HEAD**: Similar to GET, but only retrieves headers (no body) to check resource availability or metadata.
- **OPTIONS**: Describe communication options for the target resource.
- **PATCH**: Partially update an existing resource.

![[Pasted image 20241030195000.png]]

---
### HTTP Status Codes:

- Indicates the result of a client’s request
    - **1xx**: Informational (e.g., `100 Continue`)
    - **2xx**: Success (e.g., `200 OK`, `201 Created`)
    - **3xx**: Redirection (e.g., `301 Moved Permanently`, `302 Found`)
    - **4xx**: Client Errors (e.g., `400 Bad Request`, `404 Not Found`)
    - **5xx**: Server Errors (e.g., `500 Internal Server Error`, `503 Service Unavailable`)

---
### HTTP Headers:

Metadata in HTTP requests and responses, providing information about the request or response or about the object sent in the message body.

Common Headers:
- **Host**: Specifies the domain name of the server.
- **User-Agent**: Identifies the client making the request (e.g., browser type).
- **Content-Type**: Describes the media type of the resource (e.g., `application/json`).
- **Authorization**: Carries credentials for authentication.
- **Cache-Control**: Defines caching policies.

---
### Cookies:

- Small pieces of data stored by the client for session tracking, maintaining user-specific information across HTTP requests.
- Cookies allow session persistence, user preferences, or tracking in otherwise stateless HTTP.
---
