# Network Programming


---

## Outline
- **XML Formats**
- **Understanding RPC**
- **XML-RPC**
- **Example Implementation of XML-RPC**
- **JSON-RPC**

---

## XML Formats
- **eXtensible Markup Language (XML)**
- **Used for structuring data** with specific tags
- **Designed for data exchange**, not for display

### Example of XML Format
```xml
<student>
    <name>John Doe</name>
    <age>21</age>
    <major>Computer Science</major>
</student>
```

---

## Remote Procedure Call (RPC)
- **Allows calling a function on another computer**
- **Concept is similar to calling a local function**
- **Used for distributed systems and microservices**

### Why Use RPC?
✅ **Reduces the load on a single computer**
✅ **Accesses data from another system**
✅ **Enables scalable architecture**

---

## XML-RPC (eXtensible Markup Language – Remote Procedure Call)
- **Calls procedures or functions on another computer**
- **Uses XML for data transmission via HTTP**

### **Supported Data Types**
- **Integers (`int`)**
- **Floats (`float`)**
- **Strings (`str`)**
- **Lists (`list`)**
- **Dictionaries (`dict`)**
- **DateTime (`datetime`)**

### Case Study
- **File system calls**
- **Microservice architecture in e-commerce**
- **Multiplayer gaming platforms**

🔗 **More Info:** [gRPC Showcase](https://grpc.io/showcase/)

---

## JSON-RPC
- **JSON (JavaScript Object Notation) based RPC protocol**
- **Lightweight, stateless communication**
- **No restrictions on data structure**

### **Why JSON-RPC?**
- **Transport-agnostic**: Works over HTTP, WebSockets, TCP, etc.
- **Simple and efficient for networked applications**

### **Python JSON-RPC Libraries**
🔗 [jsonrpc](https://json-rpc.readthedocs.io/en/latest/quickstart.html)
🔗 [tinyrpc](https://tinyrpc.readthedocs.io/en/latest/)

---

## Example: XML-RPC in Python

### **Using `xmlrpc.client` (Client-Side)**
```python
import xmlrpc.client

server = xmlrpc.client.ServerProxy("http://localhost:8000")
print("Sum: ", server.add(10, 20))
```

### **Using `xmlrpc.server` (Server-Side)**
```python
from xmlrpc.server import SimpleXMLRPCServer

def add(x, y):
    return x + y

server = SimpleXMLRPCServer(("localhost", 8000))
server.register_function(add, "add")
print("Server is running...")
server.serve_forever()
```

🔗 **Python XML-RPC Documentation**: [Python XML-RPC](https://docs.python.org/3/library/xmlrpc.html)

---

## Example: JSON-RPC in Python

### **Using `jsonrpcserver` (Server-Side)**
```python
from jsonrpcserver import method, serve

@method
def add(x, y):
    return x + y

if __name__ == "__main__":
    serve()
```

### **Using `jsonrpcclient` (Client-Side)**
```python
from jsonrpcclient import request

response = request("http://localhost:5000", "add", x=10, y=20)
print(response)
```

🔗 **Python JSON-RPC Documentation**: [Python JSON-RPC](https://json-rpc.readthedocs.io/en/latest/quickstart.html)

---

## Additional Resources
- **[Python XML-RPC Documentation](https://docs.python.org/3/library/xmlrpc.html)**
- **[gRPC Python Quickstart](https://grpc.io/docs/languages/python/quickstart/)**
- **[Example Code Repository](https://github.com/studiawan/network-programming/tree/master/bab09)**
- **[More Examples](https://github.com/brandon-rhodes/fopnp/tree/m/py3/chapter18)**

---

