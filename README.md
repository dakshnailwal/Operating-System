# Multi-Threaded Proxy Server in C (With and Without Cache)

This project implements a **Multi-Threaded HTTP Proxy Server** using the C programming language.  
The proxy server is capable of handling multiple client requests concurrently and can be executed **with cache** or **without cache**.

When caching is enabled, the proxy uses an **LRU (Least Recently Used)** cache replacement algorithm to store and retrieve HTTP responses efficiently.

The project demonstrates practical usage of **Operating System concepts**, **network programming**, and **HTTP request parsing**.

---

## Table of Contents

- Project Description
- Working Flow
- Multi-Threading Implementation
- Motivation and Need
- Proxy Server Capabilities
- Operating System Concepts Used
- Cache Design
- Limitations
- Possible Extensions
- How to Run
- Running Without Cache
- Demo Behavior
- Notes

---

## Project Description

A proxy server acts as an intermediary between a client (web browser) and a remote web server.  
In this implementation, the client sends its HTTP request to the proxy server instead of directly contacting the destination server.

The proxy server:
- Parses the HTTP request
- Forwards the request to the target server
- Receives the response
- Sends the response back to the client

If caching is enabled, the proxy stores server responses and serves repeated requests directly from the cache, reducing server load and response time.

---

## Working Flow

| Step | Description |
|-----|------------|
| 1 | Client sends an HTTP request to the proxy |
| 2 | Proxy parses the HTTP request |
| 3 | Cache is checked (if enabled) |
| 4 | Cache hit → response served from cache |
| 5 | Cache miss → request forwarded to server |
| 6 | Server sends response to proxy |
| 7 | Proxy forwards response to client |
| 8 | Response stored in cache (if enabled) |

---

## Multi-Threading Implementation

- Each client connection is handled using a **separate thread**
- Synchronization is achieved using **semaphores**
- Condition variables and `pthread_join()` are not used

### Reason for Using Semaphores

| Method | Limitation |
|------|------------|
| pthread_join() | Requires tracking thread IDs |
| Condition Variables | More complex synchronization |
| Semaphores | No thread ID tracking, simpler control |

Semaphores use `sem_wait()` and `sem_post()` to manage concurrent access to shared resources.

---

## Motivation and Need

This project helps in understanding:

- How HTTP requests travel from a client to a server
- Handling multiple clients concurrently
- Locking and synchronization in multi-threaded programs
- Cache implementation similar to browser behavior
- Practical application of Operating System concepts

---

## Proxy Server Capabilities

| Capability | Description |
|----------|-------------|
| Request Forwarding | Forwards client requests to server |
| Multi-Client Handling | Handles multiple clients simultaneously |
| Caching | Stores server responses (optional) |
| Traffic Reduction | Reduces repeated server requests |
| Response Speed | Faster response on cache hits |
| Access Control | Can be extended to restrict websites |

---

## Operating System Concepts Used

| OS Concept | Usage |
|----------|-------|
| Threads | Handle concurrent clients |
| Semaphores | Synchronization |
| Locks | Prevent race conditions |
| Cache | Store HTTP responses |
| LRU Algorithm | Cache eviction policy |

---

## Cache Design

- Cache is implemented using a **linked list**
- Each cache entry contains:
  - Requested URL
  - Corresponding HTTP response
- When cache is full, the **least recently used** entry is removed

---

## Limitations

| Limitation | Explanation |
|----------|-------------|
| Multiple internal requests | Each response cached separately, may cause incomplete page load |
| Fixed cache size | Large websites may not fit |
| Request type | Only HTTP GET requests supported |
| Chunked responses | Partial responses may be cached |

---

## Possible Extensions

| Extension | Description |
|---------|-------------|
| Multi-processing | Replace threads with processes |
| POST support | Handle HTTP POST requests |
| Website filtering | Restrict access to specific domains |
| Encryption | Encrypt client requests |
| Dynamic cache | Variable cache size |

---

## How to Run

```bash
git clone https://github.com/Lovepreet-Singh-LPSK/MultiThreadedProxyServerClient.git
cd MultiThreadedProxyServerClient
make all
./proxy <port_number>

