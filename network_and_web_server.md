# **Palowan network and web server guru Cheatsheet**

## **Table of Contents**

1. [OSI Model](#osi-model)
2. [Http and Https](#http-and-https)
3. [Web server](#web-server)
4. [Nginx](#nginx)


---
## **OSI Model**
- The abbreviation for Open Systems Interconnection Model
- A conceptual model that describes how computer systems communicate with each other.
- Each layer has well-defined functions.
- 7-layer structure which can be further divided into two parts: media layers and host layers
    - Media layers (hardware)

        1. Physical layer
        2. Data Link layer
        3. Network layer

    - Host layers (software)

        4. Transport layer
        5. Session layer
        6. Presentation layer
        7. Application layer

- Forms of interaction
    - Adjacent-layer interaction: Each layer is served by the lower one and provides services to the next higher one
    - Same-layer interaction: Each layer on one end communicates with the corresponding layer on the other end

- The whole communication between two devices proceeds as follows:
    1. Data is composed at the highest layer.
    2. Then it is passed to the next lower layer, and
    3. encapsulated with header or footer (protocol information) at that layer.
    4. Repeat 2. and 3. until reaching the lowest layer where the data (with protocol information) is transmitted to another device.
    5. The transmitted data is de-encapsulated (removing the protocol information attached by the corresponding layer from the data), and
    6. passed to the next higher layer.
    7. Repeat 5. and 6. until reaching the highest layer where the data is finally consumed.


### **Encapsulation and De-encapsulation**

<!-- TODO -->

---

## **Http and Https**
- Client-server protocol at application layer
    - Data/ messages sent by the client (usually the browser) are requests
    - Data/ messages sent by the server are responses
- Get data such as HTML, CSS and scripts from internet

### **_Http message_**



<!-- TODO -->

### **_CORS_**

<!-- TODO -->

---

## **Web Server**

<!-- TODO -->


### **_Proxy_**

- Forward Proxy

<!-- TODO -->

- Reverse Proxy

<!-- TODO -->

### **_Load Balancing_**

<!-- TODO -->

- Round Robin
- Weighted Round Robin
- Sticky Session
- Least Connections
- IP Hash


### **_Cache_**

<!-- TODO -->

---

## **Nginx**

<!-- TODO -->

### **_Installation_**

<!-- TODO -->

### **_Configuration_**

<!-- TODO -->

---


## References:

1. OSI Model  
    [OSI Model Layers and Protocols in Computer Network](https://www.guru99.com/layers-of-osi-model.html) \
    [OSI Model & Its Layers in Computer Network](https://byjus.com/govt-exams/osi-model-open-systems-interconnection/) \
    [Data Encapsulation, Protocol Data Units (PDUs) and Service Data Units (SDUs)](http://www.tcpipguide.com/free/t_DataEncapsulationProtocolDataUnitsPDUsandServiceDa.htm)


