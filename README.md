# http-server
Here we will make a simple local HTTP (HyperText Transfer Protocol) web server for Mac OS X using Python 3.

# What is an HTTP Server?
The primary functions of an HTTP server, also known as a web server, are to listen for incoming HTTP requests on a specific TCP socket address (more on this later) and to then handle the request and send a response back to the client, which is your browser.

In a less abstract example, consider typing https://www.amazon.com/ into your browser's address bar.

<img width="866" alt="amazon" src="https://user-images.githubusercontent.com/50773875/58148391-cf932900-7c1b-11e9-9920-e7be721e12af.png">

Obviously, the Amazon homepage will be rendered on your browser window. However, what essentially happens when you type in **www.amazon.com** into your adress bar is that browser will form a network message called an HTTP request. This request then travels to an Amazon computer with a web server running, which intercepts the request and responds with the html of the Amazon homepage. Your browser renders the html received from the Amazon server, which is what you see on your screen.

Every new interaction that you have with the Amazon homepage will initiate new requests and responses via HTTP.

<img width="620" alt="Screen Shot 2019-05-21 at 11 10 19 PM" src="https://user-images.githubusercontent.com/50773875/58148868-ad9aa600-7c1d-11e9-9557-93af9694f595.png">

# TCP Socket Address
Any HTTP request or response needs directions to reach its destination. This is achieved by each HTTP message carrying a destination TCP socket address. Each socket address is comprised of both an IP adress and a port number.

You may be wondering where the socket address is when you simply typed **www.amazon.com** into the address bar. The domain name is converted to an IP address through the Domain Name System (DNS), which can be thought of as the internet's phone book: it contains a directory of domain names and translates them to their IP addresses. The port number for your Amazon search is automatically set to 80 for http and 443 for https, so even though you did not exactly specify the port number, it's still there.

With this background knowledge of web servers in mind, we can begin construction of our server.

# First, Make a Simple HTML File:

```html
<html>
  <head>
    <title>Python is awesome!</title>
  </head>
    <body>
        <h1>Firstname Lastname</h1>
        <p>Congratulations! Your simple HTTP Server is working!</p>
    </body>
</html>
```
Now save this file as `index.html`

# Building Our Server
To create our simple web server in Python 3, we will need to import the modules `http.server` and `socketserver`

This is the full code that is used for the server:
```python
import http.server
import socketserver

PORT = 8080
Handler = http.server.SimpleHTTPRequestHandler

with socketserver.TCPServer(("", PORT), Handler) as httpd:
    print("serving as port", PORT)
    httpd.serve_forever()
```
Let's examine exactly how this works.

As mentioned before, the web server is listening for requests on a specific TCP socket address. In addition to this, the web server must also be told how to handle an incoming request. The most simple handlers will serve static files.

The `http.server.SimpleHTTPRequestHandler` is a simple handler that serves static files from the current directory and its subdirectories.

For the **socketserver.TCPServer** class, **TCPServer** designates that the server will use TCP in order to send or receive messages.

For our TCP Server to work, we need two things:
1. The TCP socket address (IP address and port number)
2. The handler

In `socketserver.TCP(("", PORT), Handler)`, we can see that the TCP socket address is shown through `(IP address, port number)`

Leaving the IP address as an empty string signifies that the server will be listening on all available IP addresses.

As our variable `PORT` stores the value 8080, the server will be listening to incoming requests from the port number of 8080.

We specify the handler through using the simple handler from `http.server.SimpleHTTPRequestHandler`

The `serve_forever()` command is a way to initiate the server through the TCPServer instance so that it is listening to all incoming requests.

Save this as `server.py`, and in the same directory as `index.html`, as the **SimpleHTTPRequestHandler** will by default search for a file named `index.html` in the current working directory.

# Running the Server
Open your terminal, and enter the directory in which `index.html` is stored by using the `cd` command. We will discuss an alternate Pythonic approach to this later.

Once in the appropriate directory, using your terminal enter `python server.py` to open and run the server.

<img width="417" alt="Screen Shot 2019-05-22 at 12 49 37 PM" src="https://user-images.githubusercontent.com/50773875/58200482-1884c500-7c90-11e9-9087-8fda78c11498.png">

Open your browser, then type localhost:8080 in the address bar.

<img width="545" alt="Screen Shot 2019-05-22 at 12 47 48 PM" src="https://user-images.githubusercontent.com/50773875/58200394-effccb00-7c8f-11e9-8816-6ca09a8dea32.png">

# Changing Directories in Python
You can change directory directly in your Python code by adding this to the beginning of our code in `server.py`:

```python
import os
os.chdir('/Users/evanpfeifer/Desktop')
```

# A Simpler Method...
As a closing note, there is a (much) simpler way to create a web server in Python, one that actually doesn't require you to write any scripts.

Simply go to your terminal at write in `python -m http.server 8080`

Of course, this may be a whole lot simpler, but where's the fun in that!







