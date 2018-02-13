## Servers and Express Lecture

Now that we're running JS on our machine, we need to set up a bay for people to communicate with our machine. 
First we need to set rules for that conversation.

### The Internet
A client **requests** a resource. The server **responds** with the resource. _The client always initiates!_ It is always unidirectional.

To be a server, all the server has to do is listen for requests - it doesn't have to be a special machine or anything (eg, Corey waiting for help tickets is a server).

A process runs on a computer (or whatever) and listens on a port for incoming requests.

#### Protocol
Protocol is the rules for how we interact or communicate. Protocol is the specification, not the implementation.

We use the http protocol - the client makes a request and server makes a response - 1:1 request:response.

"Knock Knock" is an application level protocol - it specifies the sequence and content of messages, but not how the messages are transmitted.

### HTTP
HTTP is an application-level communications protocol. The physical connection between the client and the server is not specified - all it is is a certain way that the request and the response must be formatted.  
HTTP is stateless - it's lifecycle is one request-response cycle. It doesn't remember previous requests and responses and doesn't pay attention to other request-response cycles.

HTTP often works over TCP/IP. The client initializes a request (via the URL in the browser), the request goes over the internet via a TCP/IP transmission-level protocol (like a wire), and then the request goes to the server. The server formulates a response and sends it back.  
Every request gets exactly one response.

**HTTP Request**  
There are several kinds of requests - GET ("read"), POST ("create"), PUT ("update"), DELETE ("delete") - by default, typing in a URL is a GET request.  
Parts of an HTTP Request:
- _Verb_ - GET, POST, etc (ie method)
- _URI_ - '/docs/1/related' - uniform resource identifier - this tells us where we're going
- _headers_ - things about where it's going - most of the headers are related to the TCP/IP
- _body_ - this is a string (which is the only thing that TCP will pass)

**HTTP Response**
- _status_ - this tells us how the request went - did we get what we're looking for.
  - 200: Okay (2xx is generally a success)
  - 201: Created
  - 304: Cached
  - 400: Bad Request
  - 401: Unauthorized
  - 404: Not Found
  - 418: I'm A Teapot
  - 500: Server Error
- _headers_
- _body_ - a response must always come with a body. The body is what tells it what to render.

### Express
__Express is a node library for request handling.__ It wraps around Node's HTTP library.  
Express gives us very easy route handling.

req.params - variable parameters in the url (eg, /pokemon/:id => /pokemon/1)
req.query - optional queries in the url (can be anything - eg, /pokemon => /pokemon?sorted=true)