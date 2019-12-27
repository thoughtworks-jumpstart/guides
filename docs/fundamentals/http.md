# HTTP

HTTP stands for HyperText Transfer Protocol. An HTTP client sends a request message to an HTTP server. The server, in turn, returns a response message.

See [this website](https://www.ntu.edu.sg/home/ehchua/programming/webprogramming/HTTP_Basics.html) for more diagrams.

## Request methods

HTTP protocol defines a set of request methods, these are the common ones that we regularly use:

GET: A client can use the GET request to get a web resource from the server.
POST: Used to post data up to the web server.
PUT: Ask the server to store the data.
DELETE: Ask the server to delete the data.

## Browser

Whenever you type a URL in your favourite browser to get a web resource using HTTP, for example http://www.example.com, the browser turns the URL into a GET request message and sends it to the HTTP server. The HTTP server interprets the request message, and returns you an appropriate response message, which is either the resource you requested or an error message.

The browser can also send POST request messages if your website has a form.

## Uniform Resource Locator (URL)

URL (Uniform Resource Locator) is used to uniquely identify a resource over the web.

A URL might look like this:

```
protocol://hostname:port/path-and-file-name
```

| protocol | hostname   | port  | path   | querystring |
| -------- | ---------- | ----- | ------ | ----------- |
| http://  | localhost  | :3000 | /books | ?id=1       |
| https:// | google.com |       | /      |             |

### Protocol

Protocol is how the request will be transmitted. We usually deal with _http_ or _https_. Other common protocols include _file_ and _ftp_ that might even need a username and password in the URL.

### Host

Host is the name of the server. Your own computer will be called localhost. Another computer could be a numeric IP address and on the internet, the host name will look like example.com. There could be a _subdomain_ prefix to the hostname. _www_ is a very common subdomain.

### Port

Every server has numbered ports whereby communication can take place. Some ports are reserved, like 80 and 443. If you omit the port, 80 is assumed for HTTP and 443 is for HTTPS. For your own app API, you should generally use a port number [greater than 1023](https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers). 3000, 8080 and 8088 are commonly used port numbers as they are easy to remember.

### Path

Path is used to uniquely identify pages or other resources like images.

### Querystring

Querystring is an optional part of the URL that consists of name/value (key/value) pairs.
It will start with a question mark (?) and name/value pairs are separated by ampersands (&).

The format is: ?key1=value1&key2=value2

```
?id=1
?type=VEGETABLE
?color=red
?type=VEGETABLE&color=red
```

## HTTP requests and responses

Example of a GET request made when visiting www.example.com using a browser:

```
GET /index.html HTTP/1.1
Host: www.example.com
Accept: image/gif, image/jpeg, */*
Accept-Language: en-us
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1)
(blank line)
```

`Host`, `User-Agent` are request headers that exist in the HTTP request.

Example of the response:

```
HTTP/1.1 200 OK
Date: Sun, 18 Oct 2009 08:56:53 GMT
Server: Apache/2.2.14 (Win32)
Last-Modified: Sat, 20 Nov 2004 07:16:26 GMT
ETag: "10000000565a5-2c-3e94b66c2e680"
Accept-Ranges: bytes
Content-Length: 44
Connection: close
Content-Type: text/html

<html><body><h1>this is an example</h1></body></html>
```

The first line is called the _status line_. It consists of the HTTP version `1.1`, the status code `200` and the reason phrase `OK`. We will look into more status codes later.

`Content-Length`, `Content-Type` are response headers in the HTTP response.

The browser will display the content of the response message according to the media type of the response (as in the `Content-Type` response header). Possible content types include "text/plain", "text/html", "image/gif", "image/jpeg", "audio/mpeg", "video/mpeg", "application/msword", and "application/pdf".

### Status codes

### Tracking HTTP requests and responses

Using developer tools on a browser, we can track and view the HTTP requests made on a website.

For example, when we visit http://localhost:3000/books, we could use the Network developer tool to view the GET request made.

<img src="../_media/get-request.png" alt="get request on /books uri" width="600"/>

## HTTP Headers
