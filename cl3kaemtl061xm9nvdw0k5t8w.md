## Create a Server using Go net package - Part II

We have created a basic server in [Create a Server using Go net package - Part I](https://mehrnoosh.hashnode.dev/create-a-server-using-go-net-package)
Now we are going to improve it a bit. 
Time Out
Although go routines are very cheap, they still use resources. If a malicious user created several connections to the server and did nothing with them, the server would run out of resources.

