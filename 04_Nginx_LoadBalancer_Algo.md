# Section 4: ⚖️ Understanding Load Balancing in Nginx

## 🔍 What is Load Balancing in Nginx?

Load balancing is the process of distributing incoming network traffic (client requests) across multiple backend servers to:

- Improve performance

- Increase availability

Prevent any single server from being overloaded

---

## 📊 Load Balancing Algorithms in Nginx

Nginx supports multiple algorithms to balance the load of application:


### 🔁 1. Round Robin (default)

Requests are distributed evenly to each server in the order they are listed.

Example:
- Request 1 → Server A
- Request 2 → Server B
- Request 3 → Server C
- Request 4 → Server A (repeats)
 It is best for simple cases where all backend servers have roughly the same processing power and handle requests of a similar nature.

How to configure:
```bash
upstream mysite {
    server localhost:3000;
    server localhost:3001;
}
server {
# server configuration that we disussed above just have to change this only
# into the proxy pass use algorithms array name 
# server_name mysite;
}
```
### ⚖️IP Hash (```ip_hash```)

NGINX uses a hash function on the client's IP address to determine which server receives the request. This ensures that a client is consistently routed to the same backend server for the duration of their session, which is crucial for stateful applications.

Ideal for applications that store session data locally on a server and cannot easily share state across multiple servers.

How to configure ```ip_hash```:
```bash
upstream mysite {
    ip_hash;
    server localhost:3000;
    server localhost:3001;
}
server {
# server configuration that we disussed above just have to change this only
# into the proxy pass use algorithms array name 
# server_name mysite;
}
```

### 🎯 2. Least Connections/Time (```least_time```)

These algorithms dynamically evaluate the state of the servers before distributing requests to avoid overloading any single server.

The incoming request is sent to the server that currently has the fewest active connections. This helps prevent a busy server from becoming overwhelmed while other servers are idle.

Useful when backend servers take different amounts of time to handle each request.

How to configure ```least_time```:
```bash
upstream mysite {
    least_conn;
    server localhost:3000;
    server localhost:3001;
}

server {
# server configuration that we disussed above just have to change this only
# into the proxy pass use algorithms array name 
# server_name mysite;
}
```
