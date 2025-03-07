# Network Programming

---

## Outline
- **Electronic Mail (Email)**
- **Mail Servers**
- **SMTP (RFC 2821)**
- **SMTP Commands & Responses**
- **Python SMTP Example**
- **Gmail API**
- **IMAP, POP, and SMTP**

---

## SMTP (Simple Mail Transfer Protocol)
- Defined in **RFC 2821**
- Used for sending email between servers
- Can be used via **command-line interaction** or **Python libraries**

### E-mail Clients vs. Webmail Services
- **Email Clients** → Applications like Outlook, Thunderbird
- **Webmail Services** → Gmail, Yahoo Mail, Outlook Web Access

---

## Python SMTP Example

### Sending Email with `smtplib`
```python
import smtplib

def prompt(prompt):
    return input(prompt).strip()

fromaddr = prompt("From: ")
toaddrs = prompt("To: ").split()
print("Enter message, end with ^D (Unix) or ^Z (Windows):")

msg = ("From: %s\r\nTo: %s\r\n\r\n" % (fromaddr, ", ".join(toaddrs)))
while True:
    try:
        line = input()
    except EOFError:
        break
    if not line:
        break
    msg = msg + line

print("Message length is", len(msg))

server = smtplib.SMTP('smtp.office365.com', 587)
server.set_debuglevel(1)
server.starttls()
server.login('hudan@its.ac.id', 'xxx')
server.sendmail(fromaddr, toaddrs, msg)
server.quit()
```

- **Prepares the message**
- **Uses `smtplib` to send email via SMTP server**

---

## Sample SMTP Interaction
### Example Commands & Responses
```plaintext
send: 'EHLO 192.168.1.2\r\n'
reply: b'250-SI1PR02CA0017.outlook.office365.com Hello...'
reply: b'250-SIZE 157286400\r\n'
reply: b'250-STARTTLS\r\n'
reply: b'250-AUTH LOGIN XOAUTH2\r\n'
...
send: 'AUTH LOGIN xxx\r\n'
reply: b'334 xxx\r\n'
reply: b'235 2.7.0 Authentication successful\r\n'
...
send: 'MAIL FROM:<hudan@its.ac.id> SIZE=109\r\n'
reply: b'250 2.1.0 Sender OK\r\n'
...
send: 'RCPT TO:<studiawan@gmail.com>\r\n'
reply: b'250 2.1.5 Recipient OK\r\n'
...
send: 'DATA\r\n'
reply: b'354 Start mail input; end with <CRLF>.<CRLF>\r\n'
data: (354, b'Start mail input; end with <CRLF>.<CRLF>')
...
send: 'QUIT\r\n'
reply: b'221 2.0.0 Service closing transmission channel\r\n'
```

### Key SMTP Commands:
- `EHLO` → Identifies client to server
- `MAIL FROM` → Specifies sender
- `RCPT TO` → Specifies recipient
- `DATA` → Sends message content
- `QUIT` → Ends session

---

## Python Libraries for SMTP
- **`smtplib`** → Standard Python library for sending email
- **`email`** → Standard Python module for creating email messages

🔗 **Example Code:** [GitHub Repository](https://github.com/studiawan/network-programming/tree/master/bab10)

---

## Gmail API

- **RESTful API** for accessing Gmail mailboxes and sending mail
- Ideal for:
  - **Read-only mail extraction, indexing, and backup**
  - **Automated/programmatic email sending**
  - **Email account migration**
  - **Email filtering and organization**

🔗 **Gmail API Docs:** [Gmail API Guide](https://developers.google.com/gmail/api/guides)

### Using Gmail API with Python
- Provides secure, OAuth-based access to Gmail
- Supports sending, reading, and managing emails programmatically

---

## Email Protocols: IMAP, POP, SMTP
- **IMAP (Internet Message Access Protocol)** → Used for retrieving emails while keeping them on the server
- **POP3 (Post Office Protocol v3)** → Used for retrieving emails by downloading and deleting from the server
- **SMTP (Simple Mail Transfer Protocol)** → Used for sending emails

---

