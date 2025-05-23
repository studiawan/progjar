# Pemrograman Jaringan

---

## Outline
- Klien-Server: Mengirim Bytes
- **Klien-Server: Mengirim Objek**
- **Object Serialization**
- **JSON dan XML**
- **Compression**
- **TLS/SSL**
- **Implementasi di Python**

---

## Klien-Server: Mengirim Bytes

1. **Klien mengirim request dalam bentuk bytes**
2. **Server menerima request dan mengirim response dalam bentuk bytes**
### Contoh Implementasi Bytes Transfer

```python
# Client
import socket

client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client_socket.connect(('localhost', 8080))

# Mengirim data bytes
message = "Hello Server".encode('utf-8') # => mengubah string menjadi bytes
client_socket.send(message)

# Menerima response
response = client_socket.recv(1024)      # => menerima respons server
print(response.decode('utf-8'))          # => decode bytes ke string
client_socket.close()
```

```python
# Server
import socket

server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server_socket.bind(('localhost', 8080))
server_socket.listen(1)

while True:
    client_socket, address = server_socket.accept()
    
    # Menerima data bytes
    data = client_socket.recv(1024)             # => menerima request
    print(f"Received: {data.decode('utf-8')}")  # => decode bytes ke string
    
    # Mengirim response bytes
    response = "Hello Client".encode('utf-8')   # => mengirim response bytes
    client_socket.send(response)
    client_socket.close()
```
---

## Klien-Server: Mengirim Objek (Object Serialization)

Objek, pada konteks ini berupa struktur data yang kompleks, tidak bisa secara langsung dikirimkan ke jaringan soket. Tipe data yang bisa dikirimkan melalui jaringan soket hanya `bytes`, sedangkan sebuah objek **tidak bisa** dikonversikan menjadi bytes dengan cara `student_data.encode('utf-8')`. Untuk itu diperlukan pendekatan `serialization` atau serialisasi untuk membungkus objek menjadi `bytes`. Serialisasi dilakukan dengan mengubah objek menjadi format `pickle`, `JSON`, `XML`, dan lainnya. 
### Contoh Data Objek

```python
# Contoh objek yang akan dikirim
student_data = {
    'id': 12345,
    'name': 'John Doe',
    'grades': [85, 90, 78, 92],
    'info': {
        'major': 'Computer Science',
        'semester': 6
    }
}
```

### Object Serialization dalam Klien-Server

1. **Klien**: Data objek dikonversi menjadi **bytes**
2. **Server**: Menerima, membuka objek, dan memprosesnya
3. **Server**: Membungkus kembali objek untuk dikirim ke klien
4. **Klien**: Menerima dan membuka kembali objek dari server

**Alur Komunikasi Objek** 

```
[Klien] Objek → Serialize → Bytes → Network → Bytes → Deserialize → Objek [Server]
[Server] Objek → Serialize → Bytes → Network → Bytes → Deserialize → Objek [Klien]
```

### Keuntungan:
✅ Memudahkan pengiriman data  
✅ Tidak perlu parsing manual (concatenation + split)  
✅ Lebih praktis  
✅ Mendukung struktur data kompleks  
✅ Otomatis handling tipe data  

### Kekurangan:
❌ Keamanan data (perlu enkripsi untuk mengamankan data serialized)  
❌ Overhead proses serialization/deserialization  
❌ Ketergantungan pada format tertentu  
❌ Masalah kompatibilitas antar versi  


### Contoh Implementasi Object Serialization di Python

#### 1. Pickle Standalone

```python
#!/usr/bin/env python

import pickle

# declare a list
mylist = []

# assigning value to list
mylist.append('This is string')     # string
mylist.append(5)                    # integer
mylist.append(('localhost', 5000))  # tuple

# print list
print("----- This is original list: -----\n") 
print(mylist, "\n\n")

# pickle object (in this example, the object is a list)
p = pickle.dumps(mylist)

# print pickled object
print("----- This is pickled list: -----\n") 
print(p, "\n\n")

# unpickle object
u = pickle.loads(p)

# print unpickled object
print("----- This is unpickled list: -----\n") 
print(u, "\n\n")
```
- Membuat list yang berisi berbagai tipe data (string, integer, tuple)
- Menggunakan pickle.dumps() untuk mengonversi objek Python menjadi format binary
- Menggunakan pickle.loads() untuk mengembalikan objek binary ke bentuk aslinya
- Menampilkan perbandingan antara objek asli, hasil pickle, dan hasil unpickle

#### 2. Pickle/JSON Client-Server
**Client:**
```python
#!/usr/bin/env python

import socket
import time
import sys
import pickle
import json

def now():
    return time.asctime(time.localtime(time.time()))

# building socket
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect(('localhost', 5001))

while 1: 
    # infinite send pickled message
    try:
        # arranging message
        msg = []
        msg.append("Hi Server")
        msg.append(now())
        msg_dict = {
            'message': 'Hi Server',
            'timestamp': now()
        }

        # pickling message
        msg = pickle.dumps(msg) # pickle
        msg_dict = bytes(json.dumps(msg_dict), 'utf-8') #JSON

        # s.send(msg)     #=> send pickle
        s.send(msg_dict)  #=> send JSON
        time.sleep(1)
    # exception        
    except(KeyboardInterrupt, SystemExit):
        s.close()
        sys.exit(0)
```
- Membuat koneksi socket ke server di `localhost:5001`
- Mengirim pesan berulang setiap 1 detik dalam infinite loop
- Menyiapkan data dalam dua format: list (untuk pickle) dan dictionary (untuk JSON)
- Menggunakan `pickle.dumps()` atau `json.dumps()` untuk serialisasi
- Menangani interrupt (Ctrl+C) untuk menutup koneksi dengan aman

**Server:**
```python
#!/usr/bin/env python

import socket
import pickle
import sys
import json

# some definitions
SIZE = 1024

# building socket
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind(('localhost', 5001))
s.listen(5)

while 1:
    try:
        # receive connected client
        client, client_address = s.accept()

        # receive client message
        while 1:
            try:
                message = client.recv(SIZE)

                if not message:
                    break

                # deserialize pickle message and print it
                # message = pickle.loads(message)

                # deserialize JSON message and print it
                message = message.decode('utf-8')
                message = json.loads(message)
                print(client_address, message)

            except(KeyboardInterrupt, SystemExit):
                sys.exit(0)
    except(KeyboardInterrupt, SystemExit):
        sys.exit(0)
```
- Membuat socket server yang listen di `localhost:5001`
- Menerima koneksi dari multiple client (maksimal 5 pending connections)
- Menerima pesan dari client dalam buffer 1024 bytes
- Mendukung deserialisasi dari format Pickle `(pickle.loads())` atau JSON `(json.loads())`
- Menampilkan alamat client dan isi pesan yang diterima

---

## JSON dan XML
### JSON (JavaScript Object Notation)
- **JSON** → Data yang lebih ringan dan banyak digunakan di aplikasi web
- **Human-readable format**
- **Cross-platform compatibility**
- **Web API standard**
### Contoh JSON di Python
**JSON Server:**
```python
import socket
import json
import threading

class JSONServer:
    def __init__(self, host='localhost', port=8889):
        self.host = host
        self.port = port
        self.socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    
    def start(self):
        self.socket.bind((self.host, self.port))
        self.socket.listen(5)
        print(f"JSON Server listening on {self.host}:{self.port}")
        
        while True:
            client_socket, address = self.socket.accept()
            thread = threading.Thread(target=self.handle_client, 
                                    args=(client_socket, address))
            thread.start()
    
    def handle_client(self, client_socket, address):
        try:
            # Receive JSON data
            data = client_socket.recv(4096).decode('utf-8')
            request_obj = json.loads(data)
            print(f"Received JSON from {address}: {request_obj}")
            
            # Process the JSON object
            if request_obj.get('action') == 'calculate_average':
                grades = request_obj.get('grades', [])
                average = sum(grades) / len(grades) if grades else 0
                
                response_obj = {
                    'status': 'success',
                    'action': 'calculate_average',
                    'result': {
                        'average': round(average, 2),
                        'total_grades': len(grades),
                        'highest': max(grades) if grades else 0,
                        'lowest': min(grades) if grades else 0
                    }
                }
            else:
                response_obj = {
                    'status': 'success',
                    'message': 'Request received',
                    'echo': request_obj
                }
            
            # Send JSON response
            response_json = json.dumps(response_obj, indent=2)
            client_socket.send(response_json.encode('utf-8'))
            
        except json.JSONDecodeError as e:
            error_obj = {'status': 'error', 'message': f'Invalid JSON: {str(e)}'}
            client_socket.send(json.dumps(error_obj).encode('utf-8'))
        except Exception as e:
            error_obj = {'status': 'error', 'message': str(e)}
            client_socket.send(json.dumps(error_obj).encode('utf-8'))
        finally:
            client_socket.close()

if __name__ == "__main__":
    server = JSONServer()
    server.start()
```
- Multi-threading: Setiap client connection ditangani dalam thread terpisah
- JSON Processing: Otomatis deserialize request dan serialize response
- Business Logic: Mendukung operasi calculate_average untuk menghitung statistik nilai
- Error Handling: Comprehensive exception handling untuk JSON parsing errors
- Flexible Response: Echo mode untuk request yang tidak dikenali

**JSON Client:**
```python
import socket
import json

class JSONClient:
    def __init__(self, host='localhost', port=8889):
        self.host = host
        self.port = port
    
    def send_json(self, obj):
        try:
            # Create socket connection
            client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            client_socket.connect((self.host, self.port))
            
            # Serialize to JSON and send
            json_data = json.dumps(obj)
            client_socket.send(json_data.encode('utf-8'))
            
            # Receive and parse JSON response
            response_data = client_socket.recv(4096).decode('utf-8')
            response_obj = json.loads(response_data)
            
            return response_obj
            
        except Exception as e:
            return {'status': 'error', 'message': str(e)}
        finally:
            client_socket.close()

# Usage example
if __name__ == "__main__":
    client = JSONClient()
    
    # Test JSON object
    request_data = {
        'action': 'calculate_average',
        'student_name': 'Bob Smith',
        'grades': [85, 92, 78, 88, 95],
        'metadata': {
            'course': 'Network Programming',
            'semester': 'Fall 2024'
        }
    }
    
    # Send JSON to server
    result = client.send_json(request_data)
    print("Server JSON response:")
    print(json.dumps(result, indent=2))
```
- Simple Interface: Method send_json() untuk komunikasi one-shot
- Auto Connection Management: Otomatis membuka dan menutup socket connection
- JSON Serialization: Menghandle konversi Python object ↔ JSON string
- Error Resilient: Mengembalikan error response jika terjadi masalah komunikasi

### XML (eXtensible Markup Language)
- **XML** → Struktur data yang lebih kompleks dan bisa digunakan untuk interoperabilitas sistem
- **Self-documenting structure**
- **Schema validation support**
- **Enterprise integration standard**
### Contoh XML di Python
```python
import xml.etree.ElementTree as ET

# Membuat XML structure
def create_student_xml(student_data):
    root = ET.Element("student")
    
    # Basic info
    ET.SubElement(root, "id").text = str(student_data['id'])
    ET.SubElement(root, "name").text = student_data['name']
    
    # Grades
    grades_elem = ET.SubElement(root, "grades")
    for grade in student_data['grades']:
        ET.SubElement(grades_elem, "grade").text = str(grade)
    
    # Metadata
    metadata_elem = ET.SubElement(root, "metadata")
    for key, value in student_data['metadata'].items():
        ET.SubElement(metadata_elem, key).text = str(value)
    
    return ET.tostring(root, encoding='unicode')

# Parse XML
def parse_student_xml(xml_string):
    root = ET.fromstring(xml_string)
    
    student_data = {
        'id': int(root.find('id').text),
        'name': root.find('name').text,
        'grades': [int(grade.text) for grade in root.find('grades')],
        'metadata': {elem.tag: elem.text for elem in root.find('metadata')}
    }
    
    return student_data

# Example usage
student_info = {
    'id': 12345,
    'name': 'Charlie Brown',
    'grades': [88, 92, 85],
    'metadata': {
        'major': 'Computer Science',
        'semester': '6'
    }
}

xml_data = create_student_xml(student_info)
print("XML Data:")
print(xml_data)

parsed_data = parse_student_xml(xml_data)
print("\nParsed Data:")
print(parsed_data)
```
**Fungsi create_student_xml()**
- Membuat root element <student>
- Menambahkan child elements untuk id dan name
- Membuat nested structure untuk grades dengan multiple <grade> elements
- Membuat section metadata dengan dynamic key-value pairs
- Mengembalikan XML string dengan encoding unicode

**Fungsi parse_student_xml()**
- Menggunakan ET.fromstring() untuk parsing XML
- Mengekstrak data dengan find() method untuk single elements
- Menggunakan list comprehension untuk mengambil multiple grades
- Menggunakan dictionary comprehension untuk metadata yang dinamis


### Perbandingan Format Serialization

| Format | Ukuran | Kecepatan | Keamanan | Cross-Platform | Human Readable |
|--------|---------|-----------|----------|----------------|----------------|
| **Pickle** | Kecil | Cepat | ⚠️ Risiko | ❌ Python Only | ❌ Binary |
| **JSON** | Sedang | Sedang | ✅ Aman | ✅ Universal | ✅ Ya |
| **XML** | Besar | Lambat | ✅ Aman | ✅ Universal | ✅ Ya |
---

## Compression
- **Teknik kompresi** dapat digunakan untuk mengurangi ukuran data sebelum dikirim melalui jaringan
- Format yang dapat digunakan: `gzip`, `zlib`, dll.

---

## TLS/SSL (Transport Layer Security / Secure Sockets Layer)

- **SSL (1995)** → Versi awal protokol keamanan untuk komunikasi jaringan
- **TLS (1999)** → Pengganti SSL yang lebih aman
- **Fungsi utama TLS:**
  - **Memverifikasi identitas server**
  - **Melindungi data dalam perjalanan**

📌 **Sumber:** [How SSL Works](https://certs.securetrust.com/support/support-how-ssl-works.php)

---

## Implementasi TLS/SSL di Python

### Modul `ssl`

- Modul bawaan Python untuk menangani **Transport Layer Security (TLS)**
- Menggunakan **OpenSSL library**

🔗 **Dokumentasi Python SSL:** [Python SSL Docs](https://docs.python.org/3/library/ssl.html)

### Contoh Implementasi SSL
[GitHub Repository](https://github.com/brandon-rhodes/fopnp/tree/m/py3/chapter06)

---

