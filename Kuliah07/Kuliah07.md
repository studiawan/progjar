# UDP Socket
---

## UDP Communication

### Port on Client and Server
- **Client** → Uses random port
- **Server** → Uses a fixed port

### Socket Types in Python
- **UDP** → `SOCK_DGRAM`
- **TCP** → `SOCK_STREAM`

### When to Use UDP
- Low latency applications (e.g., **VoIP, video streaming**)
- Connectionless communication
- Does not require reliability

---

## UDP Server in Python

```python
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
s.bind(('127.0.0.1', 5000))

while True:
    data, address = s.recvfrom(4096)
    print(data, address)
    s.sendto(data, address)
    s.close()
```
- **No `listen()` and `accept()`**
- Uses `recvfrom()` and `sendto()`

---

## UDP Client in Python

### UDP Client 1 (Without `connect()`)
```python
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
s.sendto(b"Hello", ('127.0.0.1', 5000))
data, address = s.recvfrom(4096)
s.close()
print(data, address)
```
- Uses `sendto()` and `recvfrom()`
- Must specify server address in `sendto()`

### UDP Client 2 (With `connect()`)
```python
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
s.connect(('127.0.0.1', 5000))
s.send(b"Hello from connect")
data = s.recv(4096)
print(data)
```
- Uses `connect()`
- No need to specify server address in `send()` and `recv()`

---

## Wireshark Analysis
- **Analyzing server-udp.py and client-udp1.py**
- **Analyzing server-udp.py and client-udp2.py**
- **Analyzing server-udp2.py and client-udp3.py**

### Code Examples:
[GitHub Repository](https://github.com/studiawan/network-programming/tree/master/bab07)

---

