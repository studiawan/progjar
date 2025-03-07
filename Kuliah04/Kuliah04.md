# Pemrograman Jaringan

---

## Outline
- **Thread di Python**
- **Penerapan Thread di Socket**
- **Satu Server**
- **Banyak Server Thread**
- **Async Server**

---

## Thread di Python

### Contoh Thread 1: Class Inheritance
```python
import threading
import datetime

class ThreadClass(threading.Thread):
    def run(self):
        now = datetime.datetime.now()
        print("%s says Hello World at time: %s" % (self.getName(), now))

for i in range(5):
    t = ThreadClass()
    t.start()
```
- **Method `start()`** memanggil method `run()` pada thread
- **Class inheritance** dan **method overriding**

### Contoh Thread 2: Target Function
```python
import threading

def worker():
    print('Worker')

threads = []
for i in range(5):
    t = threading.Thread(target=worker)
    threads.append(t)
    t.start()
```

### Contoh Thread 3: Target Function with Args
```python
import threading

def worker(num):
    print('Worker: %s' % num)

threads = []
for i in range(5):
    t = threading.Thread(target=worker, args=(i,))
    threads.append(t)
    t.start()
```

### Contoh Thread 4: Thread with Logging
```python
import threading
import logging

logging.basicConfig(level=logging.DEBUG, format='(%(threadName)-10s) %(message)s')

class MyThread(threading.Thread):
    def run(self):
        logging.debug('running')

for i in range(5):
    t = MyThread()
    t.start()
```

### Contoh Thread 5: Class Inheritance + Args
```python
logging.basicConfig(level=logging.DEBUG, format='(%(threadName)-10s) %(message)s')

class MyThread(threading.Thread):
    def __init__(self, num):
        threading.Thread.__init__(self)
        self.num = num
    
    def run(self):
        logging.debug(str(self.num) + ' running')

for i in range(5):
    t = MyThread(i)
    t.start()
```

---

## Thread dan Socket

### Arsitektur Thread dan Socket
- **Satu proses, satu server, banyak client thread**
- **Satu proses, banyak server thread**
- **Satu proses, banyak server thread, banyak client thread**

### Contoh Thread dan Socket: Satu Server
```python
import select
import socket
import sys
import threading

class Server:
    def __init__(self):
        self.host = 'localhost'
        self.port = 5000
        self.backlog = 5
        self.size = 1024
        self.server = None
        self.threads = []

    def open_socket(self):
        self.server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        self.server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
        self.server.bind((self.host, self.port))
        self.server.listen(5)

    def run(self):
        self.open_socket()
        input_list = [self.server, sys.stdin]
        running = 1
        while running:
            input_ready, _, _ = select.select(input_list, [], [])
            for s in input_ready:
                if s == self.server:
                    client_socket, client_address = self.server.accept()
                    c = Client(client_socket, client_address)
                    c.start()
                    self.threads.append(c)
                elif s == sys.stdin:
                    _ = sys.stdin.readline()
                    running = 0

        self.server.close()
        for c in self.threads:
            c.join()

class Client(threading.Thread):
    def __init__(self, client, address):
        threading.Thread.__init__(self)
        self.client = client
        self.address = address
        self.size = 1024

    def run(self):
        running = 1
        while running:
            data = self.client.recv(self.size)
            print('received:', self.address, data)
            if data:
                self.client.send(data)
            else:
                self.client.close()
                running = 0

if __name__ == "__main__":
    server = Server()
    server.run()
```

---

## Asynchronous Server

### Poll System Call
```python
import select, zen_utils

def all_events_forever(poll_object):
    while True:
        for fd, event in poll_object.poll():
            yield fd, event
```
- `yield` mengembalikan generator (fungsi yang menghasilkan iterator dengan urutan nilai)

### Poll untuk Async Server
```python
def serve(listener):
    sockets = {listener.fileno(): listener}
    addresses = {}
    bytes_received = {}
    bytes_to_send = {}
    poll_object = select.poll()
    poll_object.register(listener, select.POLLIN)
```
- Menggunakan **poll** sebagai pengganti **select**
- **Listener** adalah socket
- **select.POLLIN**: konstanta yang menandakan tidak ada data untuk dibaca

### Accept Connection
```python
elif sock is listener:
    sock, address = sock.accept()
    print('Accepted connection from {}'.format(address))
    sock.setblocking(False)
    sockets[sock.fileno()] = sock
    addresses[sock] = address
    poll_object.register(sock, select.POLLIN)
```
- **Setblocking** ke `False`
- **Menambahkan socket ke dictionary**
- **Mendaftarkan socket ke `poll_object`**

### Main Function
```python
if __name__ == '__main__':
    address = zen_utils.parse_command_line('low-level async server')
    listener = zen_utils.create_srv_socket(address)
    serve(listener)
```

---

## Referensi
- [https://github.com/brandon-rhodes/fopnp/blob/m/py3/chapter07](https://github.com/brandon-rhodes/fopnp/blob/m/py3/chapter07)
- [https://github.com/PacktPublishing/Python-Network-Programming](https://github.com/PacktPublishing/Python-Network-Programming)

---


