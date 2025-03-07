# Telnet & SSH


---

## Outline
- **SSH vs. Telnet**
- **SSH in Python**
- **SFTP (Secure File Transfer Protocol)**
- **Remote System Administration & Automation Tools**
- **Challenges in Remote Connections**
- **SSH Channels & Authentication**
- **Python Script Examples**

---

## Secure Shell (SSH): A Ubiquitous Tool
- Widely used in **web hosting, virtual servers, and cloud services**
- Secure **network communication and remote system administration**
- **SSH keys** enable **secure, password-less login**

---

## Remote System Administration & Automation Tools
- **Fabric** → [Python SSH Library](https://www.fabfile.org/)
- **Ansible** → [Automated Configuration Management](https://docs.ansible.com/)
- **SaltStack** → [Event-Driven Automation Tool](https://github.com/saltstack/salt)

---

## Telnet vs. SSH

### **Telnet**
- **Older protocol for remote shell access**
- **Insecure** (transmits credentials in plain text)
- **Deprecated in modern system administration**
- **Python Library** → [`telnetlib`](https://docs.python.org/3/library/telnetlib.html) (deprecated in Python 3.11, removed in 3.13)
- **RFC 854 (1989)**
- **Default Port:** `23`

### **SSH**
- **Secure, encrypted protocol** for remote shell, file transfer, and port forwarding
- **Advantages over Telnet:** Security & reliability
- **Python Library** → [`paramiko`](https://www.paramiko.org/)
- **RFC 4250-4256 (2006)**
- **Default Port:** `22`

---

## Python Example: Telnet
```python
import telnetlib

host = "your.telnet.server"
user = "your_username"
password = "your_password"

with telnetlib.Telnet(host) as tn:
    tn.read_until(b"login:")
    tn.write(user.encode('ascii') + b"\n")
    tn.read_until(b"Password:")
    tn.write(password.encode('ascii') + b"\n")
    tn.write(b"ls\n")
    print(tn.read_all().decode('ascii'))
```

---

## Python Example: SSH with Paramiko
```python
import paramiko

host = "your.ssh.server"
username = "your_username"
password = "your_password"

client = paramiko.SSHClient()
client.set_missing_host_key_policy(paramiko.AutoAddPolicy())
client.connect(host, username=username, password=password)

stdin, stdout, stderr = client.exec_command("ls -l")
print(stdout.read().decode())

client.close()
```

---

## SSH Authentication
- **Username & Password Authentication**
- **Public-Key Challenge-Response**
- **SSH Host Keys** → Verify server identity

### SSH Host Key Distribution
1. **Administrator distributes public keys to all systems**
2. **Each SSH client memorizes the server key upon first connection**

---

## SSH Channels in Paramiko
- SSH supports **multiplexing** (multiple sessions over a single SSH connection)
- Types of SSH channels:
  - **Interactive Shell Session**
  - **Single Command Execution (`exec_command()`)**
  - **File Transfer Session**
  - **Port Forwarding**

### Running an Interactive Shell via SSH
```python
import paramiko

host = "your.ssh.server"
username = "your_username"
password = "your_password"

client = paramiko.SSHClient()
client.set_missing_host_key_policy(paramiko.AutoAddPolicy())
client.connect(host, username=username, password=password)

shell = client.invoke_shell()
shell.send("echo Hello from SSH\n")
print(shell.recv(1024).decode())

client.close()
```

---

## SSH and SFTP
- **SSH (Secure Shell)** → Secure remote login & network services
- **SFTP (SSH File Transfer Protocol)** → Secure file transfer over SSH (Version 2+)
- **SCP (Secure Copy Protocol)** → Alternative to SFTP for direct file copying

### Benefits of SFTP
✅ Navigate remote directories
✅ Create & delete files/directories
✅ Secure file transfer using passwords or public keys
✅ Stateful protocol (allows `getcwd()`, `chdir()` commands)

---

## Python Example: SFTP File Transfer
```python
import paramiko

host = "your.sftp.server"
username = "your_username"
password = "your_password"

transport = paramiko.Transport((host, 22))
transport.connect(username=username, password=password)

sftp = paramiko.SFTPClient.from_transport(transport)

# Upload file
sftp.put("local_file.txt", "/remote/path/remote_file.txt")

# Download file
sftp.get("/remote/path/remote_file.txt", "local_file.txt")

sftp.close()
transport.close()
```

🔗 **SFTP Example Code:** [GitHub Repository](https://github.com/brandon-rhodes/fopnp/blob/m/py3/chapter16/sftp_get.py)

---

## References & More Examples
- **Python Telnet Library (`telnetlib`)** → [Documentation](https://docs.python.org/3/library/telnetlib.html)
- **Python SSH Library (`paramiko`)** → [Paramiko Official Website](https://www.paramiko.org/)
- **Fabric (SSH Automation Tool)** → [Fabric Documentation](https://www.fabfile.org/)
- **Example Python SSH/Telnet Scripts** → [GitHub Repository](https://github.com/brandon-rhodes/fopnp/tree/m/py3/chapter16)

---

## Terima Kasih 🎯
