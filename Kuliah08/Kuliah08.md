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

---

## Klien-Server: Mengirim Objek

- **Data yang dikirim adalah objek**
- **Objek harus dikonversi ke bytes terlebih dahulu**
- **Format objek yang dapat digunakan**: `pickle`, `JSON`, `XML`, dll.

---

## Object Serialization

### Keuntungan:
✅ Memudahkan pengiriman data
✅ Tidak perlu parsing manual (concatenation + split)
✅ Lebih praktis

### Kekurangan:
❌ **Keamanan data** (perlu enkripsi untuk mengamankan data serialized)

---

## Object Serialization dalam Klien-Server

1. **Klien**: Data objek dikonversi menjadi **bytes**
2. **Server**: Menerima, membuka objek, dan memprosesnya
3. **Server**: Membungkus kembali objek untuk dikirim ke klien
4. **Klien**: Menerima dan membuka kembali objek dari server

---

## Implementasi Object Serialization di Python

### Contoh: Pickle Standalone

```python
import pickle

# Declare a list
mylist = []
mylist.append('This is string')  # String
mylist.append(5)                 # Integer
mylist.append(('localhost', 5000))  # Tuple

# Print list
print(mylist)

# Serializing (pickling) object
p = pickle.dumps(mylist)
print(p)

# Deserializing (unpickling) object
u = pickle.loads(p)
print(u)
```

---

## JSON dan XML
- **JSON** → Data yang lebih ringan dan banyak digunakan di aplikasi web
- **XML** → Struktur data yang lebih kompleks dan bisa digunakan untuk interoperabilitas sistem

### Contoh JSON di Python
[GitHub Repository](https://github.com/studiawan/network-programming/tree/master/bab08)

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

