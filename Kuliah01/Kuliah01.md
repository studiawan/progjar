# Pemrograman Jaringan  

## Outline  
- Pemrograman Jaringan  
- Referensi  
- Terminologi  
- TCP/IP Layer Model  
- Client-Server  
- Pengenalan Python  

## Pemrograman Jaringan  

### Referensi  
- **A. Ratan, E. Chou, P. Kathiravelu, F. Sarker**  
  *Python Network Programming*, Packt Publishing, 2019.  
  [GitHub Repository](https://github.com/PacktPublishing/Python-Network-Programming)  

- **P. Kathiravelu, M. F. Sarker**  
  *Python Network Programming Cookbook 2nd Edition*, Packt Publishing, 2017.  
  [GitHub Repository](https://github.com/PacktPublishing/Python-Network-Programming-Cookbook-Second-Edition)  

- **B. Rhodes, J. Goerzen**  
  *Foundations of Python Network Programming 3rd Edition*, Apress, 2014.  
  [GitHub Repository](https://github.com/brandon-rhodes/fopnp)  

---

## Terminologi  
- **Host**  
- **IP Address**  
- **Port**  
- **Protocol**  
- **TCP/IP Layer**  


## Host  



## IP Address  


## Port  
*"Pintu" untuk komunikasi data*  
- Memungkinkan satu node memberikan banyak layanan  
- **Contoh:**  


## Protokol  


## TCP/IP Layer  


## Model Klien-Server  
- **Server:** Aplikasi penyedia layanan yang bisa digunakan aplikasi lain  
- **Client:** Aplikasi yang mengirim request ke server dan menunggu response  

```
Klien  ----->  Server  
         (Request)  
         
Klien  <-----  Server  
         (Response)  
```

### Contoh Klien-Server  
- **Google Chrome**  
- **Web server** [ITS Website](https://its.ac.id)  

```
            Request
Client      ----->  Web Server  
            <-----    
            Response
```

---

## Pengenalan Python  

---

## Instalasi Python  

---

## Editor atau IDE Python  
**Editor untuk Pemrograman Jaringan:**  
- **Untuk Tugas:** VS Code *(direkomendasikan)*  
- **Untuk Kuis/UTS/UAS:** LiClipse *(diwajibkan)*  

 [Download LiClipse](https://www.liclipse.com/download.html)  

---

## Referensi Python  
- **Tutorial Python Bahasa Indonesia:**  
  - [Belajar Python](https://belajarpython.com/)  
  - [Petani Kode - Tutorial Python](https://www.petanikode.com/tutorial/python/)  
  - **Dan lainnya...**  

---

## Memulai Python  
- **Linux atau macOS:** Ketik `python` di Terminal  
- **Windows:**  
  - Ketik `python` di Command Prompt *(harus diset environment variables terlebih dulu)*  
  - Atau gunakan Anaconda Prompt  

### Dua Mode Python:  
1. **Interactive Mode**  
2. **Script Mode**  


---

## Virtual Environment pada Python  
- Setiap proyek aplikasi Python sering membutuhkan **library dan versi berbeda**  
- **Virtual Environment:** Setiap proyek memiliki environment masing-masing  

### Contoh:  
Membuat virtual environment dengan **Anaconda**  
```sh
conda create --name simple-client-server python=3.9
```
Selain Anaconda, ada banyak cara lain untuk membuat virtual environment  

---

## Contoh Program Python  
🔗 [GitHub Repository](https://github.com/PacktPublishing/Python-Network-Programming)  

---

## Contoh-contoh