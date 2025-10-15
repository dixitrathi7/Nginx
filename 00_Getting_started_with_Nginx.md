# 📘 Section 1: Nginx Overview 

## What is a Web Server?
A web server is a software application that stores, processes, and delivers web content (such as web pages, images, videos, or other files) to users over the internet. It responds to requests from clients, typically web browsers, using the Hypertext Transfer Protocol (HTTP) or its secure variant (HTTPS).


## What is Nginx?
Nginx is a web server that serves static content (HTML, images, etc.) to web browsers. It also provides additional features like reverse proxy, load balancing, and HTTP caching.

### 💡 Features of Nginx:
- A reverse proxy
- Load balancer
- HTTP cache
- Security (SSL/TSL)

---
## Working of Apache HTTPd:
Apache HTTPd (often just called Apache) is a traditional web server that follows a process-driven or thread-based architecture. This means it handles incoming requests by creating separate processes or threads for each connection, providing strong isolation but at the cost of resource efficiency.
**Connection Management:** Supports keep-alive (persistent connections) to reuse processes/threads for multiple requests from the same client, reducing overhead, After handling the request (or a series via keep-alive), the process/thread may idle or terminate based on configuration.

## 📊 Why use Nginx over HTTPd (Apache)?
- Nginx handles many users at once more efficiently than Apache.
- Nginx uses an event-driven model, which is faster and uses less memory.
- Apache (httpd) uses a process/thread-based model, which can be heavier under load.
- Nginx uses less CPU and RAM, making it ideal for high-performance setups.
- Nginx is optimized for serving static files like images, CSS, and HTML quickly.

**Event-Driven Model(Nginx):**
when a worker process initiates an I/O operation (like reading from a disk or network socket), it does not wait for the operation to complete. Instead, it registers a callback and continues to process other tasks. When the I/O operation finishes, the callback is triggered, and the worker process handles the result. This prevents blocking and allows a single worker to manage many connections simultaneously.

### The Core Problem Nginx Solve: The C10K Problem

**The C10K Problem**, This was the challenge of handling ten thousand concurrent connections on a single server. Traditional web servers (like Apache in its initial prefork mode) used a "one process per connection" or "one thread per connection" model. Creating a process/thread for each connection is expensive in terms of memory and CPU context-switching. At ten thousand connections, the server would grind to a halt.

Nginx's Answer: The Event-Driven, Asynchronous, Non-Blocking Architecture
Nginx elegantly solves the C10K problem by using a event-driven architecture. Instead of a process per connection, it uses a small number of highly efficient worker processes that handle thousands of connections simultaneously.

---

## 🧰 Common DevOps Use Cases for NGINX

| Use Case                              | Example                                                                 |
|--------------------------------------|-------------------------------------------------------------------------|
| Web server                           | Serving static React/Angular apps                                      |
| Reverse proxy                        | Forwarding requests to backend apps (Node.js, Python, Java)            |
| Load balancer                        | Distributing load between multiple backend servers                     |
| SSL termination                      | Handling HTTPS at the edge                                             |
| Caching                              | Reducing load on upstream services                                     |
| Ingress controller (Kubernetes)      | Managing traffic inside Kubernetes clusters                            |
| Rate limiting & security enforcement | Protecting APIs from abuse or bots                                     |


## 🛠️ Installing NGINX

### 🐧 On Ubuntu/Debian
```bash
sudo apt update
sudo apt install nginx -y
```
## 📁 NGINX File Structure (Linux)

| File/Directory        | Purpose                                      |
|-----------------------|----------------------------------------------|
| `/etc/nginx/nginx.conf` | Main configuration file                     |
| `/etc/nginx/sites-available/` | Stores virtual host (server block) configs |
| `/etc/nginx/sites-enabled/`   | Symlinks to active site configs         |
| `/var/www/html`       | Default web root directory                   |
| `/var/log/nginx/`     | Contains access and error logs               |
