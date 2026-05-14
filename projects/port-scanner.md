# Port Scanner

## Purpose
TCP port scanner that checks a given IP address across a port range and displays which ports are open. Built to learn how network connections and sockets work.

## Tools
- Python 3
- `socket` — for creating TCP connections

## Usage
```bash
python3 port_scanner.py 192.168.1.1 1 1024
```

## What I learned
- How the TCP three-way handshake works (SYN → SYN-ACK → ACK)
- Why a closed port returns `Connection refused` instead of just staying silent
- The importance of socket timeouts — without one, the script hangs on filtered ports
- The difference between TCP and UDP scanning

## Problems & Solutions
**Problem:** Scanning was too slow initially  
**Solution:** Added `socket.settimeout()` — reduced wait time from 30s → 1s per port

**Problem:** Code kept throwing an exception on closed ports  
**Solution:** Replaced `connect()` with `connect_ex()` — returns an error code instead of raising an exception, so the script continues cleanly

## Code
```python
import socket

ip = input("Enter IP: ")
for port in range(1, 1025):
    s = socket.socket()
    s.settimeout(0.5)
    result = s.connect_ex((ip, port))
    if result == 0:
        print(f"Port: {port} - OPEN")
    s.close()

```

---
*Completed: 15.05.2026*
