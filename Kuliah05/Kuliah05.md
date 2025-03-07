# Pemrograman Jaringan

---

## Outline
- HTTP 1.1 (RFC 2616)
- **HTTP 2**
- **HTTP 3**
- **URI, URL, URN**
- **HTTP Request & Response**
- **HTTP Headers**
- **HTTP Methods & Status Codes**
- **HTTP di Python**
- **HTML Parsing**
- **Python `requests` Module**

---

## URI, URL, dan URN
- **URI (Uniform Resource Identifier)**
- **URL (Uniform Resource Locator)**
- **URN (Uniform Resource Name)**

### Contoh
```
URI: http://www.progjar.org/wiki/tugas.txt
URL: http://www.progjar.org
URN: /wiki/tugas.txt
```

---

## Ilustrasi HTTP

- **HTTP Klien** (ex: Chrome)
- **HTTP Server** (ex: Nginx)
- **HTTP Request** → **HTTP Response**

### Struktur HTTP Request
1. **Request Line**
2. **Request Header**
3. **Request Body** (POST)

### Struktur HTTP Response
1. **Status Line**
2. **Response Header**
3. **Response Content** (HTML, CSS, JS)

---

## HTTP Headers

### HTTP Request Header Contoh:
```http
GET / HTTP/1.1
Host: www.its.ac.id
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:109.0) Gecko/20100101 Firefox/110.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Cookie: _ga=GA1.3.1553611766.1631318143; pll_language=en;
Upgrade-Insecure-Requests: 1
```

### HTTP Response Header Contoh:
```http
HTTP/1.1 200 OK
Server: nginx/1.10.3
Date: Sun, 12 Mar 2023 05:30:09 GMT
Content-Type: text/html; charset=UTF-8
Content-Length: 18085
Connection: keep-alive
Link: <https://www.its.ac.id/wp-json/>; rel="https://api.w.org/"
Content-Encoding: gzip
```

---

## HTTP Request Methods
- **GET** → Mengambil data dari server
- **POST** → Mengirimkan data ke server
- **PUT** → Memperbarui sumber daya di server
- **DELETE** → Menghapus sumber daya di server
- **HEAD** → Mengambil header tanpa konten

---

## HTTP Status Codes

### Contoh Status Code
- **200 OK** → Permintaan berhasil
- **301 Moved Permanently** → Redirect permanen
- **400 Bad Request** → Permintaan tidak valid
- **401 Unauthorized** → Akses tidak diizinkan
- **403 Forbidden** → Dilarang mengakses
- **404 Not Found** → Sumber daya tidak ditemukan
- **500 Internal Server Error** → Kesalahan di server

---

## HTTP di Python

### Parsing HTTP Data
```python
data = sock.recv(4096)
print('data:', data)
if data:
    data = data.decode('utf-8')
    request_header = data.split('\r\n')
    print('request header:', request_header)
    request_file = request_header[0].split()[1]
    response_header = b''
    response_data = b''

    if request_file in ['index.html', '/', '/index.html']:
        with open('index.html', 'r') as f:
            response_data = f.read()

        content_length = len(response_data)
        response_header = 'HTTP/1.1 200 OK\r\nContent-Type: text/html; charset=UTF-8\r\nContent-Length: ' + str(content_length) + '\r\n\r\n'
        sock.sendall(response_header.encode('utf-8') + response_data.encode('utf-8'))
    else:
        sock.sendall(b'HTTP/1.1 404 Not Found\r\n\r\n')
```
- **Memproses Header**
- **Memproses Konten yang Akan Dikirim**

---

## Menggunakan Modul `requests` di Python
```python
import requests

r = requests.get('https://api.github.com/user', auth=('user', 'pass'))
print(r.status_code)  # 200
print(r.headers['content-type'])  # 'application/json; charset=utf8'
print(r.encoding)  # 'utf-8'
print(r.text)  # JSON Response
print(r.json())  # Dictionary Response
```

- **Melakukan request ke server**
- **Mengambil respon dengan mudah**

---

## Referensi
- [Python Requests Documentation](https://requests.readthedocs.io/en/latest/)
- [GitHub: Python Requests](https://github.com/psf/requests)
- [Belajar HTTP di httpbin.org](http://httpbin.org)
- [Contoh Program](https://github.com/studiawan/network-programming/tree/master/bab05)

---
