<h1 style = color:red;font-family:TimesNewRoman;>Client</h1> 

###  A client is any device (hardware) or software that sends request to the server to access a service ( Eg : browser)


<h1 style = color:red;font-family:TimesNewRoman;>Server</h1>   

### A server is computer or software that listening for request and respond by provinding data or services. Server can host websites or store files and run application and it manage multiple users.It is designed to run 24/7

Main use of servers

     Centralization 
     Scalability (handle many users)
     Reliability (backups)

 Some types of Servers

      1. Web servers
      2. Databse servers
      3. File server
      4. Email server
      5. Application server

### Example : Swiggy
    Application server for payments
    Database server for user accounts
    File server for images etc.,

<h1 style = color:red;font-family:TimesNewRoman;>HOST</h1>

### A Host is the Network connected device identified by ip address,that can communicate with other devices by sending and recieving data


<h1 style=color:red;font-family:TimesNewRoman;>Packets</h1>

### Packets are known as breaking a large peice of data into small peices to send from source to destination across network.The small peices contain header ,payload data.
### 1.Header contains Source and Destination Ip address
### 2.Payload Contains actual data
### 3.Trailer inform the Destination that data reached and checks error

<h1 style=color:red;font-family:TimesNewRoman;>Bandwidth</h1>

### Bandwidth refers to amount of data transmitted across network communication.It is measured as bits per second.

<h1 style=color:red;font-family:TimesNewRoman;>Latency</h1>

### Latency refers to the time taken by the data to reach destination from source.It is measured in milliseconds.Low latency means data arrives quickly.
<h1 style=color:red;font-family:TimesNewRoman;>Protocol</h1>

### Protocal means a set of rules,that defines how data is transmitted and recieved.

<h1 style=color:red;font-family:TimesNewRoman;>Port</h1>

### Port is the numerical identity used to direct network traffic to specific application/service.
### There are 65535 ports available.

### Most Common ports are 
     80-HTTP
     443-HTTPS
     22-SSH
<h1 style=color:red;font-family:TimesNewRoman;>Socket</h1>

### Socket is the combination of ip address and port number,forming a unique end point network communication.
<h1 style=color:red;font-family:TimesNewRoman;>Ip Address</h1>

### Ip address is the unique numerical lable that is assigned to each device on a network that uses internet protocol for communication.
### There are two verions 
    IPV4-32 bit
    IPV6-128 bit

<h1 style=color:red;font-family:TimesNewRoman;>HTTP/HTTPS</h1>

### HTTP Stands for HyperText Transfer Protocol,It is set of protocol that define how data formatted between client and server.It uses port 80.

### HTTPS is the secure version of HTTP using SSL/TLS encryption layer to protect data being intercepted during transmission.It uses port 443.

<h1 style=color:red;font-family:TimesNewRoman;>FTP</h1>

### FTP stands for file tranfer protocol,it is used for transmiiting files.It uses port 21.

<h1 style=color:red;font-family:TimesNewRoman;>DNS</h1>

### DNS is domain name system is the phonebook of the internet.It acts as a translator between human and server.Humans know only names,server needs ip address,DNS solves this problem.

<p align="center">
<img src="./https flow diagram.jpg" width="800" alt="HTTPS Request Lifecycle">
</p>

<p align="center">
HTTPS Request/Response Life Cycle
</p>

<h1 style=color:red;font-family:TimesNewRoman;>Caching</h1>


### Caching is the Process of storing frequently accessed data in a temprory storage.so that future requests for the same data can be served much faster, without fetching it from the original source again.

### There are 5 common types of caching,

     1.Browser caching-The browser stores static HTML,CSS files on the user computer

     2.CDN caching-The CDN cache stored static contents on server accross the world

     3.Server caching-The application server stores frequently data in the memory

     4.Distributed caching-In large scale application with multiple severs,all servers share same cache

     5.Database caching-It stores the frequently executed SQL querries

<p align="center">
<img src="./Cache flow.jpg" width="800" alt="Cache flow lifecyle">
</p>

<p align="center">
Cache flow  Life Cycle
</p>
     
<h1 style=color:red;font-family:TimesNewRoman;>Load Balancer</h1>

### The load balancer used to split the apllication server into n number of server, used for faster performance without overloading one server.

<h1 style=color:red;font-family:TimesNewRoman;>Scalability</h1>

### Scalability is the process of handle growing amounts of works,traffic or data without sacrficing performance.

### There is two major ways of scalabilty:
     Vertical Scalabilty-It is process of adding more resources such as RAM,CPU,Storage to a single existing server.

     Horizontal Scalability-It is the process of adding more number of servers,systems with same ability to reduce over load of one server.

