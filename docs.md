# Documentation
# [client.py](pythreadserver/client.py)

## class Client(**kwargs)
### *Parameters*
```py
console: bool = True  # If true will output log messages to console
log_path: str = ""  # Path to save log text file. Leave empty for no saving.
port: int = constants.PORT # Port override (Default is 30000) 
```
### *Functions*
#### def connect(ip: str)
- Attempts connection with server at IP passed in function
> returns ``0`` if connection is successful
> 
> returns ``1`` if connection is refused
#### def send(data: bytes)
 - Adds ``data`` to the queue
> ``data`` should be bytes types or it will not be sent
> 
> returns `None`
#### def close()
- Closes connection with server and closes connection
- Log is outputted to `log_path`
> returns `None`

#### @on_receive
- Decorator to signal function to be event handler for when client receive data from the server
```py
@client.on_receive
def handle(data: bytes):
	# Handler code here
```
#### @on_connect
- Decorator to handle a valid connection to the server
```py
@client.on_connect
def handle():
	# Handler code here
```
#### @on_disconnect
- Decorator to handle disonncetion event from server
```py
@client.on_disconnect
def handle():
	# Handler code here
````
# [server.py](pythreadserver/server.py)
## class Server(**kwargs)
### *Parameters*
```py
console: bool = True  # If true will output log messages to console
log_path: str = ""  # Path to save log text file. Leave empty for no saving.
port: int = constants.PORT # Port override (Default is 30000) 
```
### *Attributes*
#### clients
- `clients` is an array of `ServerClient` objects that are currently connected
### *Functions*
#### def run()
- Starts listening for connections and handles incoming connections
- This function will not be left until the server closes.
> Note: the server can be stopped manually by entering 'stop' or a keyboard interupt (CTRL + C)
>
> returns `None`
#### def sendall(data: bytes)
- Queues `data` to be sent to all clients
> If `data` is not a bytes object it will not be sent
> 
> returns `None`
#### def get_clients()
- Returns a copy of the `clients` array
> returns `None`
#### def close()
- Disconnects all clients and closes the server
- Log is outputted to `log_path`
> returns `None`
#### @on_receive
- Decorator to signal function to be event handler for when the server receives data from a client
```py
@server.on_receive
def handle(client: ServerClient, data: bytes):
	# Handler code here
```
#### @on_connection
- Decorator to handle event of connection from a new client
```py
@server.on_connection
def handle(client: ServerClient):
	# Handler code here
```
#### @on_disconnect
- Decorator to handle event of connection closing with a client
```py
@server.on_disconnect
def handle(client: ServerClient):
	# Handler code here
```
> Note that the `ServerClient` object here is 
> no longer connected to the server and can not be sent data
## class ServerClient(parent: Server, sock_from, sock_to, addr)
- ServerClient objects should not be created but will be given as a 
    parameter in `@on_receive`, `@on_connection`, `@on_disconnect`
#### def send(data: bytes)
- Queues `data` to be sent to client as a packet
> If `data` is not a bytes object it will not be sent
> 
> returns `None`
#### def close()
- Will disconnect the client from the server and remove all references of the client
> returns `None`
# [textlog.py](pythreadserver/textlog.py)
- Utility file for logging console to file
## class Log(write_to_console: bool, filename: str)
#### def log(text: str)
- Stores text to log
- Prints text if write_to_console is `True`
> returns `None`
#### def close()
- Opens the file with `filename` and writes log to it
> This clears the log but each call writes over contents of the file
>
> returns `None`
# [constants.py](pythreadserver/constants.py)
- This file only holds constants that are used within the package classes

|name|description  |
|--|--|
|BUFFER_SIZE | Max number of bytes received each tick  |
| PORT | Default port for sockets |
