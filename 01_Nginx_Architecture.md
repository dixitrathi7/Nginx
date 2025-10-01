# 🧱 NGINX Architecture

NGINX is known for its high performance, scalability, and efficient handling of concurrent connections. This is largely thanks to its **Master-Worker process model** and **event-driven architecture**.

---

## ⚙️ Master-Worker Process Model

When you start NGINX, it launches two types of processes:

### 🧑‍✈️ 1. Master Process

- **Runs as `root`** — This is necessary because only privileged users can bind to low-numbered ports (like 80 and 443).
- **Key responsibilities:**
  - **Reads and validates** the NGINX configuration file.
  - **Binds to network ports** (e.g., 80, 443) and opens *listening sockets*.
  - **Shares** those sockets with worker processes.
  - **Spawns and manages** worker processes. You can define the number of workers manually, or leave it as `auto`, which will match the number of CPU cores.
  - Handles **graceful restarts and reloads** without dropping client connections.

> 📌 The Master Process **does not handle any client requests**. Its job is to manage and supervise the workers.

---

### 🧑‍🔧 2. Worker Processes

- **Run as unprivileged users** for security.
- Handle **all HTTP/HTTPS traffic** and client connections.
- All workers are **identical** and run independently of each other.
- The number of workers is set using the `worker_processes` directive in the config, typically equal to the number of CPU cores for best performance.

---

## 🔁 How NGINX Handles Thousands of Requests (Event Loop Explained)

NGINX uses a highly efficient **event-driven**, **asynchronous**, **non-blocking** model—primarily built on OS-level features like `epoll` (Linux), `kqueue` (BSD/macOS), or `select` (older systems).

Here’s a simplified flow of how it works:

---

### 🧩 Step-by-Step: Event Loop in Action

#### 1. 🧠 New Connection Event

- A client initiates a TCP handshake (tries to connect).
- The **OS notifies NGINX** (via `epoll`) that a new connection is ready on a listening socket.

#### 2. 📥 Receiving the Request

- The client sends an HTTP request.
- The OS receives the data and **passes it to NGINX**.
- The worker parses the request:
  - HTTP method (GET, POST, etc.)
  - URI and headers
  - Matches request to the correct `server` and `location` blocks.

#### 3. 🔀 Handling Upstream Communication (Optional)

- If NGINX needs data from a backend (e.g., PHP-FPM, Node.js, etc.):
  - It **opens a new upstream connection** to the backend server.
  - But here's the key: **NGINX doesn’t wait** for the backend to respond.
  - Instead, it returns to the event loop to handle other connections.

> This is what makes NGINX **non-blocking** — it doesn't get stuck waiting on slow backends.

#### 4. 📬 Response from Backend

- Later, when the backend responds:
  - The OS alerts NGINX that the **upstream socket has data ready**.
  - NGINX reads the data, then sends the response back to the client.

#### 5. 🔁 The Cycle Repeats

- The worker goes back to the event loop, ready to handle more events.
- This allows a **single worker process** to manage **thousands of connections** efficiently.

---

## 🧠 Summary

- NGINX's **Master Process** manages configuration, sockets, and workers.
- **Worker Processes** do the heavy lifting — handling requests using an **event loop**.
- This model allows NGINX to be **fast, scalable, and resource-efficient**, even under heavy load.

> 🚀 This is why NGINX is the go-to choice for modern, high-performance web servers.

---
### 📊 Visual Summary: NGINX Architecture (Master-Worker Model)

```text
                    +-----------------------+
                    |     MASTER PROCESS    |
                    |  (runs as root user)  |
                    +-----------------------+
                    | - Reads configuration |
                    | - Binds to ports      |
                    |   (80, 443, etc.)     |
                    | - Spawns workers      |
                    | - Handles reloads     |
                    +----------+------------+
                               |
              (manages & signals worker processes)
                               |
        +----------------------+----------------------+
        |                      |                      |
+-------+--------+    +--------+-------+    +---------+-------+
| WORKER PROCESS |    | WORKER PROCESS |    | WORKER PROCESS  |
| (unprivileged) |    | (unprivileged) |    | (unprivileged)  |
+----------------+    +----------------+    +-----------------+
| * Event Loop   |    | * Event Loop   |    | * Event Loop    |
| * Handles      |    | * Handles      |    | * Handles       |
|   thousands    |    |   thousands    |    |   thousands     |
|   of requests  |    |   of requests  |    |   of requests   |
+----------------+    +----------------+    +-----------------+
        ^                      ^                      ^
        |                      |                      |
        v                      v                      v
   [ Shared Socket :80 ]  [ Shared Socket :80 ]  [ Shared Socket :80 ]
     (OS distributes incoming TCP connections across workers)
