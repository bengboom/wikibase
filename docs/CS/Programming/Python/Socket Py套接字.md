
[Youtube sentdex sockets totorial with python 3.](https://www.youtube.com/watch?v=Lbfe3-v7yE0)

## sending and receiving data

server.py
```python
import socket

S=socket.socket(socket.AF_INET, socket.sock_STREAM) 
	# AF:ipv4(address family)  
S.bind( (socket.gethostname(),port) )
S.listen(5)  # listen for incoming clients

while True:
	clientsocket, address = s.accept()
	print(f"Connection from {address} has been established")
	clientsocket.send( bytes("Welcome to the server!", "UTF-8" )  )
	clientsocket.close()
	
```

>socket. listen(backlog) Listen for connections made to the socket. The backlog argument specifies the maximum number of queued connections and should be at least 1; **the maximum value is system-dependent** (usually 5). Obviously the system value is more than 5 on your system.


client.py
```python
import socket

s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
s.connect((socket.gethostname(),1234))

full_msg = ''

while True:
	msg = s.recv(8)  # stream of data
	if len(msg) <= 0:
		break
	full_msg += msg.decode("utf-8")
	print(msg.decode("utf-8"))
print(full_msg)

```

## buffering and streaming data

server.py
```python
import socket
import time

HEADERSIZE = 10

S=socket.socket(socket.AF_INET, socket.sock_STREAM)  # ipv4 , TCP 
S.bind( (socket.gethostname(),port) )
S.listen(5)  # listen for incoming clients

while True:
	clientsocket, address = s.accept()
	print(f"Connection from {address} has been established")

	msg = "Welcome to the server"
	print(f'{len(msg):<{HEADERSIZE}}' + msg)
	
	clientsocket.send( bytes(msg, "UTF-8" )  )
	
```

client.py
```python
import sock111et

s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
s.connect((socket.gethostname(),1234))

HEADERSIZE = 10

full_msg = ''

while True:
	new_msg = True
	full_msg = ''
	while True:
		msg=s.recv(16)
		if new_msg:
			print(f'new message lengt: {msg[:HEADERSIZE]}')
			msglen = int(msg[:HEADERSIZE])
			new_msg = False
	full_msg += msg.decode("utf-8")

	if len(full_msg)-HEADERSIZE == msglen :
		print("full msg recvd")
		print(full_msg[HEADERSIZE:])
		new_msg = True

```

## sending and receiving Python Objects w/ Pickle

>“Pickling” is **the process whereby a Python object hierarchy is converted into a byte stream**, and “unpickling” is the inverse operation, whereby a byte stream (from a binary file or bytes-like object) is converted back into an object hierarchy.

server.py
```python
import socket 
import time
import pickle

d={1: "Hey", 2: "There"}
msg = pickle.dumps(d)
print(msg) # contain original info, in bytes form
# so no need to convert to byte later

```

client.py
```python
import pickle
d=pockle.load(full_msg[HEADERSIZE:])
```

## Creating chat application with sockets in Python Multithread

server.py
```python
import select

serversocket=socket.socket(socker.AF_INET, socket.SOCK_STREAM)
serversocket.setsockopt(socket.SOL_SOCKET, socket,SO_REUSEADDR, 1 )

serversocket.bind((IP,PORT))
serversocket.listen()

sockets_list = [server_socket]

clients = {}

def receive_message(client _socket):
	try:
		message_header = client_cocket.recv(HEADER_LENGTH)
		
		if not len(message_header):
			return False
			
		message_length = int(message_header.decode(utf-8).strip())
		return {'header':message_header, 'data': client_socket.recv(message_length)}

	except:
		return False

while True:
	read_sockets, _, exception_sockets = select.select(sockets_list, [], sockets_list)

	for notified_socket in read_socket:
		if notified_socket == server_socket:
			client_socket, client_address = server_socket.accept()

			user = receive_message(client_socket)
			if user is False:
				continue
			sockets_list.append(client_socket)
			clients[client_socket] = user
			print(f"Accepted new connection from {client_address[0]}"{client_address[1]} username:{user['data'.decode('utf-8')]}")

		else :
			message = receive_message(notified_socket)
			if message is False:
				print(f"Closed connection from {clients[notified_socket]['data'].decode('utf-8')})
				del clients[notified_socket]
				continue
			
			user = client[notified_socket]
					  
			print(f"Receivd message from {user['data'].decode('utf-8')}: {message['data'].decode('utf-8')} ")

			for client_socket in clients:
				if client_socket != notified_socket:
					client_socket.send(user['header'] + user['data'] +message['header'] + message['data'] )


	for notified_socket in exception_sockets:
		sockets.list.remove(notified_socket)
		del clients[notified_socket]
			

```

client.py
```python
import socket 
```


[Another video](https://www.youtube.com/watch?v=3QiPPX-KeSc)

### Server Code
```python
import socket 
import threading

HEADER = 64
PORT = 5050
SERVER = socket.gethostbyname(socket.gethostname())
ADDR = (SERVER, PORT)
FORMAT = 'utf-8'
DISCONNECT_MESSAGE = "!DISCONNECT"

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind(ADDR)

def handle_client(conn, addr):
    print(f"[NEW CONNECTION] {addr} connected.")

    connected = True
    while connected:
        msg_length = conn.recv(HEADER).decode(FORMAT)
        if msg_length:
            msg_length = int(msg_length)
            msg = conn.recv(msg_length).decode(FORMAT)
            if msg == DISCONNECT_MESSAGE:
                connected = False

            print(f"[{addr}] {msg}")
            conn.send("Msg received".encode(FORMAT))

    conn.close()
        

def start():
    server.listen()
    print(f"[LISTENING] Server is listening on {SERVER}")
    while True:
        conn, addr = server.accept()
        thread = threading.Thread(target=handle_client, args=(conn, addr))
        thread.start()
        print(f"[ACTIVE CONNECTIONS] {threading.activeCount() - 1}")

print("[STARTING] server is starting...")
start()
```


### Client Code
```python
import socket

HEADER = 64
PORT = 5050
FORMAT = 'utf-8'
DISCONNECT_MESSAGE = "!DISCONNECT"
SERVER = "192.168.1.26"
ADDR = (SERVER, PORT)

client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect(ADDR)

def send(msg):
    message = msg.encode(FORMAT)
    msg_length = len(message)
    send_length = str(msg_length).encode(FORMAT)
    send_length += b' ' * (HEADER - len(send_length))
    client.send(send_length)
    client.send(message)
    print(client.recv(2048).decode(FORMAT))

send("Hello World!")
input()
send("Hello Everyone!")
input()
send("Hello Tim!")

send(DISCONNECT_MESSAGE)
```