# Multi Threaded Proxy Server (With & Without Cache)

A multi-threaded HTTP proxy server implemented in **C**, supporting both cached and non-cached request handling.  
HTTP request parsing follows standard proxy server behavior.

---

## 📑 Index

| Section |
|--------|
| Project Theory |
| How to Run |
| Demo |
| Contributing |

---

## 📘 Project Theory

### Introduction
A proxy server acts as an intermediary between the client and the destination server.  
It receives requests from the client, forwards them to the target server, and returns the server’s response back to the client.

---

### Basic Working Flow

| Step | Description |
|-----|------------|
| 1 | Client sends HTTP request to proxy |
| 2 | Proxy parses the request |
| 3 | Cache lookup (if enabled) |
| 4 | Cache hit → return cached response |
| 5 | Cache miss → forward request to server |
| 6 | Receive response from server |
| 7 | Send response to client and store in cache |

---

### Multi-Threading Implementation

- Each client request is handled by a separate thread  
- **Semaphores** are used instead of condition variables  
- `pthread_join()` and `pthread_exit()` are avoided  
- Synchronization is handled using:
  - `sem_wait()`
  - `sem_post()`

**Why Semaphores?**
- No need to track thread IDs
- Simpler synchronization model for this use case

---

### Motivation / Need of the Project

This project helps in understanding:

- Request flow from local machine to remote server
- Handling multiple clients concurrently
- Locking and synchronization mechanisms
- Cache behavior similar to modern browsers

---

### What a Proxy Server Can Do

| Capability | Description |
|-----------|------------|
| Performance | Reduces server load and speeds up responses |
| Caching | Stores responses for repeated requests |
| Security | Hides client IP from destination server |
| Control | Can restrict access to specific websites |
| Extensibility | Can be modified to encrypt requests |

---

### OS Concepts Used

| Component | Usage |
|---------|-------|
| Threads | Handle multiple clients |
| Locks | Ensure data consistency |
| Semaphores | Thread synchronization |
| Cache | Implemented using LRU algorithm |

---

### Cache Details

- Cache implemented using a linked list
- **LRU (Least Recently Used)** replacement policy
- Fixed-size cache elements

---

### Limitations

- Websites opening multiple connections may store responses separately
- Cached response may be incomplete in such cases
- Fixed cache size limits large websites
- Not suitable for very large or dynamic content

---

### Possible Extensions

- Use multiprocessing instead of multithreading
- Implement website filtering
- Add support for HTTP POST requests
- Improve cache handling for large responses

---

## ▶ How to Run

```bash
git clone https://github.com/Lovepreet-Singh-LPSK/MultiThreadedProxyServerClient.git
cd MultiThreadedProxyServerClient
make all
./proxy <port_number>
