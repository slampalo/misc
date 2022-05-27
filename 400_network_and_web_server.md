# **Palowan network and web server basics**

## **Table of Contents**

1. [OSI Model](#osi-model)
2. [Http](#http)
3. [Web server](#web-server)
4. [References](#references) 
---

## **OSI Model**

- The abbreviation for Open Systems Interconnection Model
- A conceptual model that describes how computer systems communicate with each other.
- Each layer has well-defined functions.
- 7-layer structure which can be further divided into two parts: media layers and host layers

  - Media layers (hardware)

    1. Physical layer
       - Transfer raw data: 0s and 1s
       - Hardwares: Network adapters, Ethernet, Repeaters, Networking hubs
    2. Data Link layer
       - Encapsulate packets from Network layer into frames and send them bit by bit over the underlying hardware
       - Receive signals from Physical layer and reassemble them into frames
       - MAC (Media Access Control) 
          - Manage connections between devices
          - Define permissions for transferring data
       - LLC (Logical Link Control)
          - Error checking
          - Synchronize frames
    3. Network layer
       - Divide segments from Transport layer into packets 
       - Reassemble packets from Data Link layer into segments
       - Determine the best path for routing packets
       - Protocols: IP

  - Host layers (software)

    4. Transport layer
       - Divide data from Session layer into segments
       - Reassemble segments from Network layer
       - flow control - 
       - error handling - check if the data received correctly
       - Protocols: TCP
    5. Session layer
       - Manage the communication
         - Create, maintain and close communication channels, called sessions
       - Keep track the progress of data transfer
    6. Presentation layer
       - Receive data from application layer and prepare for session layer
       - Define how two ends encrypt/ encode/ compress
    7. Application layer
       - The layer closest to end-user
       - Provide protocols for transmitting information
       - Protocols: HTTP, HTTPS, FTP, DNS, POP3

- Two types of interaction

  - Adjacent-layer interaction (Vertical): Each layer is served by the lower one and provides services to the next higher one
  - Corresponding-layer interaction (Horizontal): Each layer on one end communicates with the corresponding layer on the other end

- The whole communication between two devices proceeds as follows:
  1. Compose data at the highest layer.
  2. Pass data to the next lower layer, and
  3. Attach header or footer (protocol information) to the data (encapsulation).
  4. Repeat 2. and 3. until reaching the lowest layer where the data (with protocol information) is transmitted to another device.
  5. Remove from the transmitted data the protocol information attached by the corresponding layer (de-encapsulation)
  6. passed to the next higher layer.
  7. Repeat 5. and 6. until reaching the highest layer where the data is finally consumed.

---

## **HTTP**

- Client-server protocol at application layer
- Get data such as HTML, CSS, and scripts from internet

### **_Http message_**

- Two types of HTTP messages:

  - Requests: Data/ messages sent by the client (usually the browser)
  - Responses: Data/ messages sent by the server

- Every message is composed of three parts:

  1. Start-line
  2. Headers

  - Empty line: The divider between metadata and the body

  3. Body (Optional)

**Request Header**

- Start-line

  1. Method
     - GET: Retrieve data
     - HEAD: Same as GET, but without the response body.
     - POST (Create): Non-idempotent
     - PUT (Create or Update): idempotent
     - DELETE
  2. Request target
  3. HTTP version

- Headers: additional information about the request message
- Body (Optional): the transmitted Data

**Response Header**

- Start-line (also called Status-line in response header)

  1. HTTP version
  2. Status code
     - 1xx: Informational response - The request was received, continuing process
     - 2xx: Success - The request was successfully received, understood, and accepted
     - 3xx: Redirection - Further action needs to be taken in order to complete the request
     - 4xx: Client errors - The request contains bad syntax or cannot be fulfilled
     - 5xx: Server errors - The server failed to fulfil an apparently valid request
  3. Status description
     - See [Wiki - HTTP status code](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes)

- Headers: additional information about the response message
- Body (Optional): the transmitted Data

### **_CORS (Cross-Origin Resource Sharing)_**

- Data are prevented from being shared between different origins by the same-origin policy.
- CORS is a HTTP-header based mechanism used by browsers to enable resources sharing between different origins in a secure fashion (especially for request sent through invoking the fetch or the XMLHttpRequest APIs).
- Origin consists of three parts: scheme (HTTP, HTTPS), domain and port.

  - It is a case of cross-origin if there is no exact match.
    - For example:
      - http://domain-a.com and http://domain-b.com,
      - http://domain-a.com and https://domain-a.com,
      - http://domain-a.com and https://subdomain.domain-a.com, and
      - http://domain-a.com:80 and http://domain-a.com:81, all these are considered as cross-origin.

- For enabling CORS, there are several response headers you need to know:
  - `Access-Control-Allow-Origin`: List all origins which are allowed to access the resources.
  - `Access-Control-Allow-Methods`: List all methods which are allowed to access the resources.
  - `Access-Control-Allow-Headers`: List all HTTP headers that the incoming requests are allowed to have.
  - `Access-Control-Allow-Credentials`: Determine whether or not the request can be made with credentials.

You have to add those headers to the response header for enabling the access from other origins.

> Search the library that fits the language of the server.

### **_HTTPS_**
- HTTP + TLS (or SSL) protocol
- 443 is the common port for HTTPS traffic.
 

---

## **Web Server**

### **_Proxy_**

> Below depicts the simplest case where the client and the web server communicate directly.
>
> ----HTTP Request---->
>
> Client -- Web Server
>
> <----HTTP Response----
>
> However, there are also many cases in which the clients and the servers are unable to have a direct communication due to considerations like security, performance, etc. That's why we need proxy server plays as an intermediary here.
> The main job of proxy server is to forward requests from the client to the web server, after receiving the responses from web server, it passes the responses to the client.
>
> -------HTTP Request------->
>
> Client -- Proxy -- Web Server
>
> <------HTTP Response------
>
> In general, there are two types of proxy server: forward and reverse.

- Forward Proxy

  1. Help client forward requests to origin server
  2. Hide the information about client from the origin server

- Reverse Proxy
  1. Pass the request from client to original server, and respond to client as if it were the original server
  2. Hide the information about server from the client

### **_Load Balancing_**

> Setting up multiple servers helps serving a great amount of loads and prevents servers from being slowed down and crashing. Whenever there are more than one web server, we have to define how the incoming network traffic is distributed to those servers, and that's what we call load balancing.

- Round-Robin
  - Requests are distributed to each server in turn.
- Weighted Round Robin
  - Distribute requests proportionally.
- URL Hash Method:
  - Distribute requests based on the defined key
- Source IP Hash Method
  - Distribute requests based on client ip addresses.
- Least Connections
  - Pass the requests to the server with the least number of connections
- Least Response Time
  - Pass the requests to the server with the least response time.
- Random Algorithm
  - Distribute the request to a random server

---

## References:

1. OSI Model
   - [OSI Model Layers and Protocols in Computer Network](https://www.guru99.com/layers-of-osi-model.html)
   - [OSI Model & Its Layers in Computer Network](https://byjus.com/govt-exams/osi-model-open-systems-interconnection/)
   - [Data Encapsulation, Protocol Data Units (PDUs) and Service Data Units (SDUs)](http://www.tcpipguide.com/free/t_DataEncapsulationProtocolDataUnitsPDUsandServiceDa.htm)
   - [Layers of OSI model](https://www.guru99.com/layers-of-osi-model.html#6)

2. HTTP
   - [HTTP messages](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages)
   - [HTTP status code](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes)
   - [The ultimate guide to enabling Cross-Origin Resource Sharing (CORS)](https://blog.logrocket.com/the-ultimate-guide-to-enabling-cross-origin-resource-sharing-cors/)

3. Web Server
   - [Web Server](https://www.serverwatch.com/web-servers)
   - [Web Server & Nginx](https://medium.com/starbugs/web-server-nginx-1-cf5188459108)
   - [differences between Forward proxy and Reverse proxy](https://medium.com/techtofreedom/forward-proxy-and-reverse-proxy-the-differences-8530a195cb2d#:~:text=1%20Forward%20proxy.%20The%20forward%20proxy%20server%2C%20as,a%20client%20knows%20where%20is%20the%20origin%20server.?msclkid=7dac2f66d00f11eca6a9c54c80d373c5)
