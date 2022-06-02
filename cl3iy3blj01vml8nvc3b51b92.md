## Create a Server using Go net package

Create a server from scratch using the Go net package. Package net provides a portable interface for network I/O, including TCP/IP, UDP, domain name resolution, and Unix domain sockets.
The net package has Listener interface. A Listener is a generic network listener for stream oriented protocols. As an interface a Listener should implement three methods.

```go
type Listener interface {
	// Accept waits for and returns the next connection to the listener.
	Accept() (Conn, error)

	// Close closes the listener.
	// Any blocked Accept operations will be unblocked and return errors.
	Close() error

	// Addr returns the listener's network address.
	Addr() Addr
}
``` 
The Listen function is responsible for creating a Listener.

```go
func Listen(network, address string) (Listener, error)
``` 
The network must be "tcp", "tcp4", "tcp6", "unix" or "unixpacket".

```golang
func main() {
	// Listen on TCP port 2000 of the local host
	listener, err := net.Listen("tcp4","127.0.0.1:2000")
	if err != nil {
		log.Fatal(err)
	}
}
``` 
The Accept() function is responsible for accepting any incoming request, since it should be ready to accept any request that may come in any time from now on, the Accept() should be put in a for ever loop. This function returns a connection if it is successful and an error otherwise.

```golang
for {
		conn, err := listener.Accept()
		if err != nil {
			log.Fatal(err)
		}
		log.Println(conn.RemoteAddr())
	}
``` 
For simplicity, above code just returns the network address of client.  Build and run the file and in another terminal use the following code to connect to server:

```bash
curl localhost:2000
``` 
Know in the first terminal you can see something like this:

```bash
2022/05/23 20:08:40 127.0.0.1:37188
``` 
But the above code has a big problem. What if processing a connection is a blocking process? In this case  the server can not accept anymore connection until the process is over. To prevent this problem any connection processes should done inside go routines.
To fully understand what is happening, try running below code using / without go routines.

```golang
func main() {
	// Listen on TCP port 2000 of the local host
	listener, err := net.Listen("tcp4","127.0.0.1:2000")
	if err != nil {
		log.Fatal(err)
	}
	for {
		conn, err := listener.Accept()
		if err != nil {
			log.Fatal(err)
		}
		go func(c net.Conn) {
			io.Copy(c, c)
		}(conn)
	}
}
``` 
After building and running the above code test it with the following command:

```bash
nc localhost 2000
Hello
``` 
The nc command runs Netcat, a utility for sending raw data over a network connection.
So any thing you send to server will com back to you. Now open another terminal and do the same in it. If you didn't use go routines you would not recive any response until you shut down the first connection.

 
 





