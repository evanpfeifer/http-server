# http-server
Here we will make a simple local HTTP web server for Mac OS X using Python 3.

# What is an HTTP Server?
The primary functions of an HTTP server, also known as a web server, are to listen for incoming HTTP (HyperText Transfer Protocol) requests on a specific TCP socket address and to then handle the request and send a response back to the client. 

# First, make a simple HTML file:

```html
<html>
  <head>
    <title>Simple Server</title>
  </head>
    <body>
        <h1>Firstname Lastname</h1>
        <p>Your simple HTTP Server is working!</p>
    </body>
</html>
```
Now save this file as `index.html`

# Building our server
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
