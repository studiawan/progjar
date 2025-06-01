# Telnet & SSH (Secure Shell)


---

## Daftar Isi
- **Remote System Administration & Automation Tools**
- **Telnet vs. SSH**
- **Implementasi SSH dalam Python**
- **Autentikasi SSH**
- **SSH Channels in Paramiko**
- **SSH dan SFTP**
- **Tantangan dalam Remote Connections**
- **Referensi & Contoh Lebih Lanjut**

---

## Secure Shell (SSH): Alat yang Umum Digunakan
SSH merupakan protokol yang sangat penting dalam dunia teknologi informasi modern:
- Digunakan secara luas dalam **web hosting, virtual servers, dan cloud services**
- Menyediakan **komunikasi jaringan yang aman dan remote system administration**
- **Kunci SSH** memungkinkan **login yang aman tanpa password**
- Menjadi standar industri untuk akses remote yang aman

---

## Remote System Administration & Automation Tools
Berikut adalah beberapa tools populer untuk Remote System Administration:
- **Fabric** → [Library Python SSH](https://www.fabfile.org/)  
  Untuk deployment dan administrasi sistem
- **Ansible** → [Manajemen Konfigurasi Otomatis](https://docs.ansible.com/)  
  Platform otomatisasi IT
- **SaltStack** → [Alat Otomatisasi Berbasis Event](https://github.com/saltstack/salt)  
  Manajemen konfigurasi dan Otomasi  

---

## Telnet vs. SSH

### **Telnet**
- **Protokol lama untuk akses shell remote**
- **Tidak aman** (mengirimkan kredensial dalam teks biasa)
- **Sudah tidak direkomendasikan dalam sistem administrasi modern**
- **Library Python** → [`telnetlib`](https://docs.python.org/3/library/telnetlib.html) (deprecated di Python 3.11, dihapus di 3.13)
- **RFC 854 (1989)**
- **Port Default:** `23`

### **SSH**
- **Protokol terenkripsi yang aman** untuk shell remote, transfer file, dan port forwarding
- **Keunggulan dibanding Telnet:** Keamanan & keandalan
- **Library Python** → [`paramiko`](https://www.paramiko.org/)
- **RFC 4250-4256 (2006)**
- **Port Default:** `22`

---

### Contoh Python: Telnet 
```python
import telnetlib

# PERINGATAN: Contoh ini hanya untuk pembelajaran
# Jangan gunakan Telnet dalam lingkungan produksi!

host = "your.telnet.server"
user = "your_username"
password = "your_password"

try:
    with telnetlib.Telnet(host, timeout=10) as tn:
        tn.read_until(b"login:")
        tn.write(user.encode('ascii') + b"\n")
        tn.read_until(b"Password:")
        tn.write(password.encode('ascii') + b"\n")
        tn.write(b"ls\n")
        result = tn.read_all().decode('ascii')
        print(result)
except Exception as e:
    print(f"Error koneksi Telnet: {e}")
```

---

### Contoh Python: SSH dengan Paramiko
```python
import paramiko
import sys

host = "your.ssh.server"
username = "your_username"
password = "your_password"

# Membuat client SSH
client = paramiko.SSHClient()

# Mengatur kebijakan untuk host key yang tidak dikenal
client.set_missing_host_key_policy(paramiko.AutoAddPolicy())

try:
    # Membuat koneksi
    client.connect(host, username=username, password=password, timeout=10)
    
    # Menjalankan perintah
    stdin, stdout, stderr = client.exec_command("ls -la")
    
    # Membaca output
    output = stdout.read().decode('utf-8')
    error = stderr.read().decode('utf-8')
    
    if output:
        print("Output:")
        print(output)
    if error:
        print("Error:")
        print(error)
        
except paramiko.AuthenticationException:
    print("Autentikasi gagal!")
except paramiko.SSHException as ssh_exception:
    print(f"Error SSH: {ssh_exception}")
except Exception as e:
    print(f"Error: {e}")
finally:
    client.close()
```

---

## Autentikasi SSH

SSH mendukung beberapa metode autentikasi:

### **1. Autentikasi Username & Password**
- Metode paling umum dan sederhana
- Memerlukan username dan password yang valid

### **2. Autentikasi Public Key**
- Lebih aman dibanding password
- Menggunakan pasangan kunci publik-privat
- Tidak memerlukan input password secara manual

### **3. Autentikasi Host Key**
- Memverifikasi identitas server
- Mencegah serangan man-in-the-middle

### Distribusi SSH Host Key
1. **Administrator mendistribusikan kunci publik ke semua sistem**
2. **Setiap client SSH mengingat kunci server pada koneksi pertama**
3. **Verifikasi otomatis pada koneksi berikutnya**

---

### Contoh Autentikasi dengan Kunci SSH

```python
import paramiko
import os

host = "your.ssh.server"
username = "your_username"
key_path = os.path.expanduser("~/.ssh/id_rsa")  # Path ke private key

client = paramiko.SSHClient()
client.set_missing_host_key_policy(paramiko.AutoAddPolicy())

try:
    # Autentikasi menggunakan private key
    private_key = paramiko.RSAKey.from_private_key_file(key_path)
    client.connect(host, username=username, pkey=private_key)
    
    # Menjalankan perintah
    stdin, stdout, stderr = client.exec_command("whoami")
    print(f"Logged in as: {stdout.read().decode().strip()}")
    
except FileNotFoundError:
    print(f"File kunci privat tidak ditemukan: {key_path}")
except paramiko.AuthenticationException:
    print("Autentikasi dengan kunci privat gagal!")
except Exception as e:
    print(f"Error: {e}")
finally:
    client.close()
```

---

## SSH Channels in Paramiko
SSH mendukung **multiplexing** (beberapa sesi melalui satu koneksi SSH). Jenis-jenis channel SSH:

- **Sesi Shell Interaktif** - Terminal interaktif penuh
- **Eksekusi Single Command** (`exec_command()`) - Menjalankan satu perintah
- **Sesi Transfer File** - SFTP/SCP
- **Port Forwarding** - Tunneling koneksi

### Menjalankan Shell Interaktif melalui SSH
```python
import paramiko
import time

host = "your.ssh.server"
username = "your_username"
password = "your_password"

client = paramiko.SSHClient()
client.set_missing_host_key_policy(paramiko.AutoAddPolicy())

try:
    client.connect(host, username=username, password=password)
    
    # Membuat shell interaktif
    shell = client.invoke_shell()
    
    # Mengirim perintah
    shell.send("echo 'Halo dari SSH'\n")
    time.sleep(1)  # Menunggu response
    
    # Membaca output
    output = shell.recv(1024).decode('utf-8')
    print(output)
    
    # Mengirim perintah lain
    shell.send("pwd\n")
    time.sleep(1)
    output = shell.recv(1024).decode('utf-8')
    print(output)
    
except Exception as e:
    print(f"Error: {e}")
finally:
    client.close()
```

---

## SSH dan SFTP
### **SSH (Secure Shell)**
- Protokol untuk **login remote yang aman dan layanan jaringan**
- Menyediakan enkripsi end-to-end
- Mendukung berbagai jenis autentikasi

### **SFTP (SSH File Transfer Protocol)**
- **Transfer file yang aman melalui SSH** (Versi 2+)
- Protokol yang stateful (mendukung navigasi direktori)
- Lebih aman dibanding FTP tradisional

### **SCP (Secure Copy Protocol)**
- **Alternatif untuk SFTP** untuk penyalinan file langsung
- Lebih sederhana namun kurang fleksibel dibanding SFTP

### Keunggulan SFTP
✅ Navigasi direktori remote  
✅ Membuat & menghapus file/direktori  
✅ Transfer file aman menggunakan password atau kunci publik  
✅ Protokol stateful (memungkinkan perintah `getcwd()`, `chdir()`)  
✅ Resume transfer file yang terputus  
✅ Kontrol akses dan permission file  

---

### Contoh Python: Transfer File dengan SFTP
```python
import paramiko
import os

host = "your.sftp.server"
username = "your_username"
password = "your_password"

# Membuat koneksi transport
transport = paramiko.Transport((host, 22))

try:
    transport.connect(username=username, password=password)
    
    # Membuat client SFTP
    sftp = paramiko.SFTPClient.from_transport(transport)
    
    # Upload file
    local_file = "file_lokal.txt"
    remote_file = "/path/remote/file_remote.txt"
    
    if os.path.exists(local_file):
        sftp.put(local_file, remote_file)
        print(f"File {local_file} berhasil diupload ke {remote_file}")
    
    # Download file
    sftp.get(remote_file, "file_download.txt")
    print(f"File {remote_file} berhasil didownload")
    
    # Membuat direktori remote
    try:
        sftp.mkdir("/path/remote/direktori_baru")
        print("Direktori baru berhasil dibuat")
    except IOError:
        print("Direktori sudah ada atau gagal dibuat")
    
    # Menampilkan daftar file
    file_list = sftp.listdir("/path/remote/")
    print("Daftar file di direktori remote:")
    for file in file_list:
        print(f"- {file}")
    
except paramiko.AuthenticationException:
    print("Autentikasi SFTP gagal!")
except Exception as e:
    print(f"Error SFTP: {e}")
finally:
    if 'sftp' in locals():
        sftp.close()
    transport.close()
```

🔗 **Contoh Lain dari Kode SFTP:** [GitHub Repository](https://github.com/brandon-rhodes/fopnp/blob/m/py3/chapter16/sftp_get.py)

---

## Tantangan dalam Koneksi Remote

### **Masalah Keamanan**
- **Man-in-the-middle attacks** - Serangan penyadapan
- **Brute force attacks** - Serangan percobaan password
- **Key management** - Pengelolaan kunci yang kompleks

### **Masalah Jaringan**
- **Latency tinggi** - Koneksi lambat
- **Packet loss** - Kehilangan data
- **Firewall restrictions** - Pembatasan firewall

### **Solusi Best Practices**
- Gunakan autentikasi kunci publik
- Implementasikan rate limiting
- Gunakan port non-standard
- Aktifkan logging dan monitoring
- Implementasikan fail2ban atau tools serupa

---

## Referensi & Contoh Lebih Lanjut

### **Dokumentasi Resmi**
- **Library Telnet Python (`telnetlib`)** → [Dokumentasi](https://docs.python.org/3/library/telnetlib.html)
- **Library SSH Python (`paramiko`)** → [Website Resmi Paramiko](https://www.paramiko.org/)
- **Fabric (Tool Otomatisasi SSH)** → [Dokumentasi Fabric](https://www.fabfile.org/)

### **Repository Contoh**
- **Contoh Script Python SSH/Telnet** → [Repository GitHub](https://github.com/brandon-rhodes/fopnp/tree/m/py3/chapter16)
- **Paramiko Examples** → [Repository Resmi Paramiko](https://github.com/paramiko/paramiko/tree/main/demos)

---

## Terima Kasih 🎯
