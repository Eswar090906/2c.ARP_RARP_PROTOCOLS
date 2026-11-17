# 2c.SIMULATING ARP /RARP PROTOCOLS
## AIM
To write a python program for simulating ARP protocols using TCP.
## ALGORITHM:
## Client:
1. Start the program
2. Using socket connection is established between client and server.
3. Get the IP address to be converted into MAC address.
4. Send this IP address to server.
5. Server returns the MAC address to client.
## Server:
1. Start the program
2. Accept the socket which is created by the client.
3. Server maintains the table in which IP and corresponding MAC addresses are
stored.
4. Read the IP address which is send by the client.
5. Map the IP address with its MAC address and return the MAC address to client.
P
## PROGRAM - ARP
## Client
```
import socket

s = socket.socket()

s.connect(('localhost', 9000))

print("Connected to ARP Server")

reader = s.makefile("r")

while True:

    ip = input("Enter logical Address: ")

    if ip.lower() == "exit":
        s.sendall(b"exit\n")
        break

    s.sendall((ip + "\n").encode())

    mac = reader.readline().strip()

    print("MAC Address:", mac)

s.close()
```
## Server
```
import socket

s = socket.socket()

s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)

s.bind(('localhost', 9000))
s.listen(5)

print("ARP Server is waiting for client...")

c, addr = s.accept()

print("Client connected:", addr)

address = {
    "165.165.80.80": "6A:08:AA:C2",
    "165.165.79.1": "8A:BC:E3:FA"
}

reader = c.makefile("r")

while True:

    ip = reader.readline().strip()

    if not ip or ip.lower() == "exit":
        print("Connection closed")
        break

    print("Received IP Address:", ip)

    try:
        mac = address[ip]
    except KeyError:
        mac = "Not Found"

    c.sendall((mac + "\n").encode())

    print("MAC Address sent:", mac)

c.close()
s.close()
```
## OUPUT - ARP
<img width="2172" height="724" alt="image" src="https://github.com/user-attachments/assets/07d17c61-56fa-49b7-b549-799060d99ba1" />


## PROGRAM - RARP
## Client
```
import socket

s = socket.socket()

s.connect(('localhost', 6000))

print("Connected to RARP Server")

reader = s.makefile("r")

while True:

    mac = input("Enter MAC Address: ")

    if mac.lower() == "exit":
        s.sendall(b"exit\n")
        break

    s.sendall((mac + "\n").encode())

    ip = reader.readline().strip()

    print("Logical Address:", ip)

s.close()
```
## Server
```
import socket

s = socket.socket()

s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)

s.bind(('localhost', 6000))
s.listen(5)

print("RARP Server is waiting for client...")

c, addr = s.accept()

print("Client connected:", addr)

address = {
    "6A:08:AA:C2": "192.168.1.100",
    "8A:BC:E3:FA": "192.168.1.99"
}

reader = c.makefile("r")

while True:

    mac = reader.readline().strip()

    if not mac or mac.lower() == "exit":
        print("Connection closed")
        break

    print("Received MAC Address:", mac)

    mac = mac.upper()

    try:
        ip = address[mac]
    except KeyError:
        ip = "Not Found"

    c.sendall((ip + "\n").encode())

    print("Logical Address sent:", ip)

c.close()
s.close()
```
## OUPUT -RARP
<img width="2169" height="725" alt="image" src="https://github.com/user-attachments/assets/17c02eb0-6be3-4ef7-80a7-f68a5baac7cf" />


## RESULT
Thus, the python program for simulating ARP protocols using TCP was successfully 
executed.
