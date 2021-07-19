+++
title = "First look at WSGI and PEP 3333"
date = "2021-06-22"
draft = false
tags = ['python']
+++

WSGI - Web Server Gateway Interface. This is Python standard interface between web server software and web applications.
The specification was created in 2003 and described in PEP 333, further updated in 2010 in PEP 3333, with backward compatibility to prepare for Python 3 changes.

<!--more-->

#### Background
Initially dynamic request from the client to the server was passed as a path to script in the cgi-bin directory, and the webserver was forking the script into a new process. In 1997 CGI (Common Gateway Interface) standard was created that set environment variable names and their purposes, for example:

* QUERY_STRING - the part of URL after *?* character
* REQUEST_METHOD - the name of the HTTP method
* SCRIPT_PATH - relative path to the program, like /cgi-bin/script.py

While CGI programming was heavily used with languages like Perl or PHP. Starting a new script (forking) is slow, therefore it is better to have a separate web server and separate web application. 
In late 1990s [mod_python][mod-python] was created. As an interface to Apache server internals, it allowed having applications written in Python be used with the Apache server. It could be used as a tool for running a web development framework in production, though as the original author says, it is best for a scenario with high volume servers without a user interface (therefore no need for WSGI framework). When development slowed down, in 2003 Python community came up with the WSGI standard ([PEP3333][pep3333]). The goal was to promote portability as it was better to have fewer web frameworks but better supported, that could operate with different servers, because of a universal interface between them.

The road from Request to Python now could look like:

*HTTP* -> Nginx -> *HTTP* -> Gunicorn -> *WSGI* -> Flask -> *ROUTING* -> Python

WSGI interface has two sides: server and application. The server invokes callable object provided by the application side. It is possible to also create middleware components that must implement both the server and application sides.

#### Application side

The application object is callable (has ```__call__``` method) that accepts two arguments: environ and start_response (name of arguments can be different). 

```python
def simple_app(environ, start_response):
    status = "200 OK"
    response_headers = [("Content-type", "text/plain")]
    start_response(status, response_headers)
    return [b"Hello World!"]
```

* *environ* parameter is builtin Python dictionary object containing CGI-style environment variables (why CGI not regular HTTP, because servers have mature CGI functionality)
```python
{ # environ
    'REQUEST_METHOD': 'GET',
    'PATH_INFO': '/url/',
    'SERVER_PROTOCOL': 'HTTP/1.1',
    ...
}
```
* *start_response* is callable accepting HTTP status string and list of tuples describing HTTP response header
  
A different example of WSGI compatible application with *view* function implemented.

```python
def simple_app(environ, start_response):
    response = view(environ)
    status = "200 OK"
    response_headers = [("Content-type", "text/plain")]
    start_response(status, response_headers)
    return [response]

def view(environ):
    path = environ['PATH_INFO']
    return f'Hello from {path}'

```

#### Server side

Let's try to implement a simple server that prints requests. Basic request and response could look like:

**Request**
```
GET / HTTP/1.1
Host: localhost:8000
User-Agent: curl/7.54.0
Accept: */*
```

**Response**
```
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 11

Hello world
```

**Simple server**
```python
import socket

with socket.socket() as s:
    s.bind(('localhost', 8000))
    s.listen(1)
    conn, addr = s.accept()

    while True:
        with conn:
            request = conn.recv(1024).decode('utf-8')
            print(request)
            conn.sendall('Hello world'.encode('utf-8'))
```

And now update to have the server compatible with WSGI. Here we see the familiar function *application*.
When called by the server the application must return an iterable yielding zero or more bytestrings.
Function

```python
with conn:
    http_request = conn.recv(1024).decode('utf-8')
    request = parse_http(request)
    environ = to_environ(*request)
    response = application(start_response, environ)
    for data in response:
      conn.sendall(data.encode('utf-8'))
```

It is good to mention that the Python library contains a reference implementation of WSGI specification in module *wsgiref*. For example, there is a demo HTTP server for WSGI applications. 

```python
from wsgiref.simple_server import make_server

# as an application we can use our simple_app
with make_server('', 8000, simple_app) as httpd:
    print("Serving on port 8000...")

    # Serve until process is killed
    httpd.serve_forever()
```

#### Middleware

Middleware could play both roles, depending on the situation. Sometimes a middleware may be wrapped in another component, and that will create a "middleware stack". For the most part, middleware must conform to both server and application sides of WSGI.

And that's it. 

---

Resources:

* [Mod_python: The Long Story - Gregory Trubetskoy blog][mod-python]
* [PEP 3333 -- Python Web Server Gateway Interface v1.0.1][pep3333]
* [WSGI for Web Developers (Ryan Wilson-Perkin) - PyCon Canada - Talk][pycon]
* [wsgiref — WSGI Utilities and Reference Implementation][wsgiref]

[mod-python]: https://grisha.org/blog/2013/10/25/mod-python-the-long-story/
[pep3333]: https://www.python.org/dev/peps/pep-3333/
[pycon]: https://www.youtube.com/watch?v=WqrCnVAkLIo
[wsgiref]: https://docs.python.org/3.8/library/wsgiref.html
