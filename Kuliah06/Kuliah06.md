# Network Programming


---

## Outline
### FTP
- **FTP Communication Model**
- **Active vs Passive Mode**
- **Example FTP Communication**
- **FTP Commands & Reply Codes**
- **FTP in Python**

### UDP
- **Port on Client and Server**
- **Socket Types in Python**
- **When to Use UDP**
- **UDP Server & Client in Python**
- **Wireshark Analysis**

---

## FTP Communication Model

### Active Mode
1. **Client initiates command connection** to FTP server on **port 21**.
2. Client sends `PORT` command with its **IP address and port number**.
3. Server initiates **data connection** from **port 20** to client's specified port.
4. **Issue:** Firewalls/NAT might block the server's incoming connection.

### Passive Mode
1. Client opens **command connection** to server.
2. Client sends `PASV` command.
3. Server **opens a random port** and informs the client.
4. Client **initiates the data connection**.
5. **More firewall-friendly** since client initiates all connections.

---

## Example FTP Communication
- **Home Directory on FileZilla Server**
- **Client connects to server**
- **Log records on FileZilla Server**

### FTP Reply Codes
- **1xx** → Positive preliminary reply
- **2xx** → Command successful
- **3xx** → Command accepted, further action needed
- **4xx** → Temporary error
- **5xx** → Permanent error

---

## FTP in Python

### Example: `ftplib` List Directory
```python
from ftplib import FTP

f = FTP('localhost')
print("Welcome:", f.getwelcome())
f.login('hudan')
print("Current working directory:", f.pwd())
names = f.nlst()
print('List of directory:', names)
f.quit()
```

### Example: `ftplib` Download File
```python
from ftplib import FTP

f = FTP('localhost')
f.login('hudan')
fd = open('2003.pdf', 'wb')
f.retrbinary('RETR 2003.pdf', fd.write)
fd.close()
f.quit()
```

### Code Examples:
[GitHub Repository](https://github.com/studiawan/network-programming/tree/master/bab06)

