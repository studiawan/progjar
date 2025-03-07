# Pemrograman Socket

## Outline
- Review Terminologi
- Port Number
- Socket
- Konsep Klien-Server
- Dasar Pemrograman Socket
- Contoh TCP Socket

---

## Review Terminologi

### Port Number
Port pada Klien dan Server:
- **Klien**
- **Server**

### Socket
Tipe Socket di Python:
- **UDP** → `SOCK_DGRAM`
- **TCP** → `SOCK_STREAM`

### Tipe Alamat

---

## Konsep Klien-Server

- **Server**
- **Klien 1**
- **Klien 2**
- **Klien 3**

### Konsep Klien-Server: Host
- **Server**
- **Klien 1**
- **Klien 2**
- **Klien 3**

### Konsep Klien-Server: PORT
- **Port server:** `5000`
- **Port klien 1:** `45576`
- **Port klien 2:** `55676`
- **Port klien 3:** `36575`

Keterangan:
- **Port server**
- **Port klien**

### Konsep Klien-Server: Socket
- **Server**
- **Socket klien di sisi server** → satu socket per koneksi
- **Socket klien 1**
- **Socket klien 2**
- **Socket klien 3**

---

## Pemrograman Socket: Server

```python
import socket

socket_server = socket.socket()
socket_server.bind(('127.0.0.1', 5000))
socket_server.listen(1)
socket_client, client_address = socket_server.accept()
```

## Pemrograman Socket: Klien

```python
import socket

socket_client = socket.socket()
socket_client.connect(('127.0.0.1', 5000))
```

```python
socket_client.send(data)
recv_data = socket_client.recv(buffer)
```

---

## Contoh Server TCP: Menerima Data Klien dan Mengirimkannya Kembali

```python
import socket, sys

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind(('127.0.0.1', 5000))
s.listen(1)

try:
    while True:
        client_sock, client_addr = s.accept()
        data = client_sock.recv(65535)
        client_sock.send(data)
        client_sock.close()
except KeyboardInterrupt:
    s.close()
    sys.exit(0)
```

- Membuat socket server bertipe TCP, dengan alamat IPv4
- Bisa berjalan pada host yang sama atau berbeda

---

## Contoh Klien TCP

```python
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect(('127.0.0.1', 5000))
s.send(b"Hello tcp ...")
data = s.recv(65535)
s.close()
```

- Koneksi ke server dengan alamat IP dan port yang sudah disepakati
- `send`: mengirim data bertipe byte (tanda `b` di awal string)
- `recv`: menerima data sebesar buffer yang ditentukan (65535)
- `close`: menutup socket

---

## Kesalahan yang Sering Terjadi

Referensi contoh program Python dan socket:
- [https://github.com/studiawan/network-programming](https://github.com/studiawan/network-programming)
- [https://github.com/PacktPublishing/Python-Network-Programming](https://github.com/PacktPublishing/Python-Network-Programming)
- [https://github.com/PacktPublishing/Python-Network-Programming-Cookbook-Second-Edition](https://github.com/PacktPublishing/Python-Network-Programming-Cookbook-Second-Edition)
- [https://github.com/brandon-rhodes/fopnp](https://github.com/brandon-rhodes/fopnp)

---

