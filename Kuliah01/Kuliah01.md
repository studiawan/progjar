# Pemrograman Jaringan  

## Outline  
- Pemrograman Jaringan  
- Referensi  
- Terminologi  
- TCP/IP Layer Model  
- Client-Server  
- Pengenalan Python  

## Pemrograman Jaringan  
- Pemrograman yang melibatkan tidak hanya satu *host* saja 
- Sering disebut pemrograman *socket*
- Bisa juga disebut pemrograman klien-server

## Referensi  
- **A. Ratan, E. Chou, P. Kathiravelu, F. Sarker**  
  *Python Network Programming*, Packt Publishing, 2019.  
  [GitHub Repository](https://github.com/PacktPublishing/Python-Network-Programming)  

- **P. Kathiravelu, M. F. Sarker**  
  *Python Network Programming Cookbook 2nd Edition*, Packt Publishing, 2017.  
  [GitHub Repository](https://github.com/PacktPublishing/Python-Network-Programming-Cookbook-Second-Edition)  

- **B. Rhodes, J. Goerzen**  
  *Foundations of Python Network Programming 3rd Edition*, Apress, 2014.  
  [GitHub Repository](https://github.com/brandon-rhodes/fopnp)  

## Terminologi  
- Host
- IP Address
- Port  
- Protocol
- TCP/IP Layer

## Host  
- Sebuah komputer tunggal atau perangkat digital lain
- Sering juga disebut *workstation* or *node*
- Terhubung ke jaringan komputer

## IP Address  
- Alamat numerik yang digunakan untuk mengidentifikasi perangkat dalam jaringan komputer
- IP address memungkinkan perangkat seperti komputer atau smartphone untuk berkomunikasi satu sama lain dalam jaringan 
- Versi:
	- IPv4: Alamat IP yang terdiri dari 32-bit dan biasanya ditulis dalam format desimal bertitik, contoh: 192.168.1.1
	- IPv6: Alamat IP yang lebih baru dengan 128-bit, menggunakan format hexadecimal, contoh: 2001:db8::ff00:42:8329
- Sifat:
	- IP Publik: Digunakan di internet dan dapat diakses dari mana saja.
	- IP Privat: Digunakan dalam jaringan lokal dan tidak bisa diakses dari luar (contoh: 192.168.x.x, 10.x.x.x, 172.16.x.x).

## Port  
- "Pintu" untuk komunikasi data
- Memungkinkan satu node memberikan banyak layanan  
- Contoh:

  | Port | Layanan |
  |------|---------|
  | 21   | FTP     |
  | 22   | SSH     |
  | 23   | Telnet  |
  | 25   | SMTP    |
  | 53   | DNS     |
  | 80   | HTTP    |

## Protokol  
- Sekumpulan aturan dalam komunikasi pesan pada jaringan komputer
- Contoh: HTTP, FTP

## TCP/IP Layer  
- Application
  - HTTP, SMTP, FTP, dll
  - Di layer ini diimplementasikan pemrograman jaringan
- Transport
  - UDP, TCP
- Network
- Link
- Physical

## Model Klien-Server  
- Server: Aplikasi penyedia layanan yang bisa digunakan aplikasi lain  
- Client: Aplikasi yang mengirim request ke server dan menunggu response  

```
       (Request)
Klien ----------->  Server  
       (Response)  
```

## Contoh Model Klien-Server  

```
           Request
Client    ---------->  Web Server  
(Chrome)  <----------  (https://its.ac.id)  
           Response
```

## Pengenalan Python  
- Bahasa pemrograman tingkat tinggi yang mengutamakan filosofi *readability*
- Diproses oleh *intepreter*, bukan *compiler*
- Bersifat *portable*

## Mengapa Python?
- Karena Python itu mudah
  - Mudah dibaca dan mudah diprogram
  - Mudah referensi dan pustakanya
- Banyak digunakan di berbagai bidang
- Kelemahan: *slow runtime*
- Kelebihan: *fast development*

## Instalasi Python  
- Linux atau macOS: built-in 3.6+
- Windows: versi 3.6+
  - [Unduh Python](https://www.python.org/downloads/)
- Anaconda (ekosistem Python)
  - [Unduh Anaconda](https://docs.anaconda.com/anaconda/install/windows/)

## Editor atau IDE Python  
- IDLE (Windows)
- Vim (customized)
- Eclipse & PyDev
- Notepad++ & Plugin Python 
- Sublime
- PyCharm
- VS Code
- Dll 

## Editor untuk kuliah ini
Editor untuk Pemrograman Jaringan:
- Untuk Tugas: VS Code *(direkomendasikan)*  
- Untuk Kuis/UTS/UAS: LiClipse *(diwajibkan)*  
  [Download LiClipse](https://www.liclipse.com/download.html)  

## Referensi Python  
- Tutorial Python Bahasa Indonesia:  
  - [Belajar Python](https://belajarpython.com/)  
  - [Petani Kode - Tutorial Python](https://www.petanikode.com/tutorial/python/)  
  - Dan lainnya ...

## Memulai Python  
- Linux atau macOS: Ketik `python` di Terminal  
- Windows:
  - Ketik `python` di Command Prompt (harus diset *environment variables* terlebih dulu)  
  - Atau gunakan Anaconda Prompt  

## Dua Mode Python
- *Interactive mode:* menggunakan terminal
- *Script mode:* menggunakan editor

## Virtual Environment pada Python  
- Setiap proyek aplikasi Python sering membutuhkan *library* dan versi berbeda 
- *Virtual environment*: Setiap proyek memiliki *environment* masing-masing  
- Contoh:  
  Membuat virtual environment dengan **Anaconda**  
  ```sh
  conda create --name simple-client-server python=3.12
  ```
  Selain Anaconda, ada banyak cara lain untuk membuat *virtual environment*  

## Contoh Program Python dan Pemrograman Jaringan 
🔗 [GitHub Repository](https://github.com/PacktPublishing/Python-Network-Programming)  
