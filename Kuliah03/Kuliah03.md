# Pemrograman Jaringan

---

## Outline
- Server menangani banyak klien
- Select
- Server
- Klien
- Poll
- Tutorial Python
- Read/Write File
- Debug dengan Wireshark
- SocketServer

---

## Modul Select

### Modul Select

### Modul Poll

---

## Server dengan Select

1. **Create socket**
2. **Bind**
3. **Listen**
4. **Make list for sockets**
5. **While running**
   - Select
   - Accept and append to list
   - Recv and send

---

## Klien

1. **Create socket**
2. **Connect**
3. **While running**
   - Read input
   - Send and recv
   - Break if keyboard interrupt

---

## Konsep Klien-Server: Socket

- **Server**
- **Klien 1**
- **Klien 2**
- **Klien 3**

### Socket Server
- Socket klien di sisi server → satu socket per koneksi
- Disimpan dalam sebuah list
- **Socket klien 1**
- **Socket klien 2**
- **Socket klien 3**

Keterangan:
- **Socket klien di server**
- **Socket klien di klien**

---

## Contoh Server-Select (1)

```python
import socket

server_address = ('127.0.0.1', 5000)
server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
server_socket.bind(server_address)
server_socket.listen(5)

input_socket = [server_socket]
```

- Membuat socket, bind, listen
- Memaksa address dan port untuk bisa digunakan
- List untuk menampung socket

---

## Contoh Server-Select (2)

```python
import select

while True:
    read_ready, _, _ = select.select(input_socket, [], [])
    for sock in read_ready:
        if sock == server_socket:
            client_socket, client_address = server_socket.accept()
            input_socket.append(client_socket)
        else:
            data = sock.recv(1024)
            print(sock.getpeername(), data)
            if data:
                sock.send(data)
            else:
                sock.close()
                input_socket.remove(sock)
```

- Pengecekan apakah ada socket input yang siap diproses
- Jika socket yang siap adalah **server**, maka `accept()` klien dan tambahkan ke list
- Jika socket yang siap adalah **klien**, maka terima data dan kirimkan kembali ke klien
- Jika tidak ada data yang diterima, tutup socket dan hapus dari list

### Catatan:
- `Select` akan bergantian memproses socket yang siap
- **Windows**: List input hanya bisa berisi socket
- **Linux**: List bisa berisi objek lain

---

## Debug dengan Wireshark

### Contoh program klien-server sederhana:
- **Server/Klien**: [GitHub Repository](https://github.com/studiawan/network-programming)
- Jalankan **Wireshark** dengan hak akses root dan monitor loopback interface
- Jalankan **program server**
- Jalankan **program klien**
- **Amati perubahan** yang tercatat pada Wireshark

---

## Tampilan Wireshark

- **Ketika Klien Mengirim Pesan dan Menutup Koneksi**
- **Tampilan pada Data yang Dikirim**
- **Inisialisasi Program Select**
- **Klien Mengirim Data**
- **Klien Menutup Koneksi**

---

## SocketServer

### Socket Shutdown

---

## Framing

1. **Framing 1**: Socket Close
2. **Framing 2**: Streaming in Both Directions
3. **Framing 3**: Use Fix-Length Message
4. **Framing 4**: Use Delimiter
5. **Framing 5**: Use Prefix or Header

---

## Referensi Source Code Progjar

- [https://github.com/studiawan/network-programming](https://github.com/studiawan/network-programming)
- [https://github.com/brandon-rhodes/fopnp/tree/m/py3](https://github.com/brandon-rhodes/fopnp/tree/m/py3)
- [https://github.com/PacktPublishing/Python-Network-Programming-Cookbook-Second-Edition](https://github.com/PacktPublishing/Python-Network-Programming-Cookbook-Second-Edition)
- [https://github.com/PacktPublishing/Python-Network-Programming](https://github.com/PacktPublishing/Python-Network-Programming)
- [https://github.com/PacktPublishing/Learning-Python-Networking-Second-Edition](https://github.com/PacktPublishing/Learning-Python-Networking-Second-Edition)
- [https://github.com/PacktPublishing/Mastering-Python-Networking-Third-Edition](https://github.com/PacktPublishing/Mastering-Python-Networking-Third-Edition)
- [https://github.com/PacktPublishing/Python-Networking-Cookbook](https://github.com/PacktPublishing/Python-Networking-Cookbook)
- [https://github.com/PacktPublishing/Mastering-Python-for-Networking-and-Security-Second-Edition](https://github.com/PacktPublishing/Mastering-Python-for-Networking-and-Security-Second-Edition)

---

