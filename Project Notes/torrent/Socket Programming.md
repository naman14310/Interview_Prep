# Socket Programming 

Socket programming is a way of connecting two nodes on a network to communicate with each other. One socket(node) listens on a particular port at an IP, while other socket reaches out to the other to form a connection. Server forms the listener socket while client reaches out to the server.

![img](https://media.geeksforgeeks.org/wp-content/uploads/Socket-Programming-in-C-C-.jpg)

#### Stages for server:

1. Socket creation

2. Setsockopt (This helps in manipulating options for the socket referred by the file descriptor sockfd - helps in reuse of address and port)

3. Bind (binds the socket to the address and port number specified in addr)

4. Listen (It puts the server socket in a passive mode, where it waits for the client to approach the server to make a connection)

5. Accept (It extracts the first connection request on the queue of pending connections for the listening socket, sockfd, creates a new connected socket, and returns a new file descriptor referring to that socket. At this point, connection is established between client and server, and they are ready to transfer data)


#### Stages for Client:

1. Socket connection

2. Connect (The connect() system call connects the socket referred to by the file descriptor sockfd to the address specified by addr. Server’s address and port is specified in addr)


-----


## Interview Questions

#### Q1. Differences between TCP and UDP 

![img](https://drive.google.com/file/d/1ZuN19KpypFDEs7jXAjYzDX81i-s-GWJ3/view?usp=sharing)
