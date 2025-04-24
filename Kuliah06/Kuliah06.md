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
# **FTP (File Transfer Protocol)**
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


# UDP (User Datagram Protocol) 
UDP adalah protokol transport layer yang ringan dan tidak menggunakan koneksi (connectionless). Berbeda dengan TCP, UDP tidak melakukan proses handshake sebelum mengirim data, dan tidak menjamin data sampai atau urutannya benar. Karena itu, UDP cocok digunakan untuk aplikasi yang butuh kecepatan tinggi dan bisa mentoleransi kehilangan data sesekali.

---

## Port pada Client dan Server

- **Port Server:** Server UDP mendengarkan (listen) pada port tertentu untuk menerima data. Port ini harus diketahui oleh client agar bisa mengirim data ke server.
- **Port Klien:** Biasanya menggunakan port acak (ephemeral), tapi bisa juga ditentukan secara manual jika dibutuhkan.

Contoh:

**Server:** `192.168.1.10:5005`

**Client:** `192.168.1.20:<port_acak>`

## Tipe Socket di Python
Untuk menggunakan UDP di Python, kamu bisa pakai modul `socket` dan memilih tipe `SOCK_DGRAM`.

```python
import socket

# Create a UDP socket
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
```
Penjelasan:
- `AF_INET` : IPv4

- `SOCK_DGRAM` : UDP

---

## Kapan Sebaiknya Menggunakan UDP

Gunakan UDP saat:

- Kecepatan lebih penting daripada keandalan.

- Kehilangan paket sesekali dapat diterima.

- Kita ingin broadcast atau multicast data.

- Aplikasimu bersifat real-time, seperti:

  - Streaming video
  
  - Game online
  
  - VoIP (Voice over IP)
  
  - Pencarian DNS

## Contoh UDP Server & Client di Python
**Contoh Server UDP:** 
```python
import socket

server_address = ('127.0.0.1', 5000)
server_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
server_socket.bind(server_address)

data, client_address = server_socket.recvfrom(1024)
server_socket.sendto(data, client_address)
print('data:', data, 'client address', client_address)
print('sock name', server_socket.getsockname())
```

**Contoh Client UDP:** 
```python
import socket

server_address = ('127.0.0.1', 5000)
client_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

message = b'Hi ...'
client_socket.sendto(message, server_address)

recv_message, server_address = client_socket.recvfrom(1024) 
print(recv_message, server_address) 
```


## Analisis dengan Wireshark
- Setelah menjalankan kode UDP di atas, gunakan Wireshark untuk menangkap dan melihat data yang dikirim melalui jaringan. Berikut contoh tampilan paket UDP yang tercapture:

![image](https://github.com/user-attachments/assets/66513a03-2f7e-4108-8e05-0b6bc8eb173d)

- Terlihat bahwa client menggunakan port acak `63277` dan mengirim paket ke server di port `5000`, begitu juga sebaliknya saat server merespons.

- Ini adalah ciri khas dari UDP: **langsung mengirimkan data tanpa perlu membuat koneksi terlebih dahulu.**

### Perbandingan dengan TCP

Berbeda dengan UDP, protokol TCP membutuhkan proses koneksi terlebih dahulu (handshake) sebelum bisa mengirim data. Berikut contoh kode TCP untuk server dan client:

**Contoh Server TCP:** 
```python
import socket

# define server address, create socket, bind, and listen
server_address = ("localhost", 5000)
server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server_socket.bind(server_address)
server_socket.listen(1)

# accept client, receive its message and print
client_socket, client_address = server_socket.accept()
data = client_socket.recv(1024)
print("data:", data, "client address", client_address)

client_socket.sendall(data)

# close socket client and socket client
client_socket.close()
server_socket.close()
```

**Contoh Client TCP:** 
```python
import socket

server_address = ("127.0.0.1", 5000)
client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client_socket.connect(server_address)

message = b"Hi ..."
client_socket.send(message)

recv_message = client_socket.recv(1024)
print(recv_message, server_address)

client_socket.close()
```

Dan berikut contoh tampilan paket TCP di Wireshark:

![image](https://github.com/user-attachments/assets/0f4597ae-1fbb-4a74-a322-754871a2b674)

- Di sini kita bisa lihat bahwa TCP melakukan proses koneksi lebih dulu, ditandai dengan adanya paket **SYN, SYN-ACK, ACK** sebelum data benar-benar dikirim.

- Ini berbeda dengan UDP yang langsung mengirim data tanpa perlu "berkenalan" lebih dulu.

---

### Code Examples:
[GitHub Repository](https://github.com/studiawan/network-programming/tree/master/bab07)

