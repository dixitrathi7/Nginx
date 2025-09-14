## 📘What is Nginx?
Nginx is a web server that serves static content (HTML, images, etc.) to web browsers. It also provides additional features like reverse proxy, load balancing, and HTTP caching.

### 💡 Features of Nginx:
- A reverse proxy
- Load balancer
- HTTP cache
- Mail proxy

---
## 📊 Why use Nginx over HTTPd (Apache)?
- Nginx handles many users at once more efficiently than Apache.
- Nginx uses an event-driven model, which is faster and uses less memory.
- Apache (httpd) uses a process/thread-based model, which can be heavier under load.
- Nginx uses less CPU and RAM, making it ideal for high-performance setups.
- Nginx is optimized for serving static files like images, CSS, and HTML quickly.

**Event-Driven Model(Nginx):**
when a worker process initiates an I/O operation (like reading from a disk or network socket), it does not wait for the operation to complete. Instead, it registers a callback and continues to process other tasks. When the I/O operation finishes, the callback is triggered, and the worker process handles the result. This prevents blocking and allows a single worker to manage many connections simultaneously.

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

