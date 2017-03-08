# Express.js
A Node Library for Request Handling
March 6, 2017

There is a [Codeschool course on Express.js](https://www.codeschool.com/courses/building-blocks-of-express-js).

1. The Internet
2. Building with Express

_______

## 1. The Internet !

### Clients and Servers
* Client requests a resource
* Server responds with resource
* Client initiates, server responds, not other direction!
* These are *roles* – not technical specs or computer types.

> Clients/servers are conceptual roles.

Clients always initiate. Think of a banker. The client approaches the banker to request money. The banker doesn't go around asking people if they want money!

Your laptop is a server. It's connected to the internet and can send responses to a client. It can also be a client, depending on what you're doing!

### Web servers
Processes, not physical machines.

### HTTP
Protocol: "Rules of engagement or interaction".

An application-level protocol – used at the app level.

A protocol for communicating on the internet. It's huge. Specification, not implementation. Think of a dance or manners like Emily Post's.

Specifies allowable metadata and content, but not details of metadata and content.

Stateless: state is cleared once communication is done, is not persistent. Doesn't remember previous communication!

> Check slides for details about who made HTTP and how it was made.

### HTTP over TCP/IP
      Client  ===> 'the internet' : TCP/IP ====> server
      request ===>     routing...          ====> response
      response <====================================|

An HTTP Request is just a message with a certain format.

* HTTP structures the communication.
* TCP/IP is the address/routing system that routes the message.

> Every request gets exactly one (total) response ... even if TCP slices up the message in transmission, you will always get one response.

**REQUEST:**
* Route => URI/L
* Verb => GET/POST/PUT/DELETE
* Headers => Meta-data about the request like 'Host', Accept, Accept-Encoding
* Payload => Body, data sent by the request

**RESPONSE:**
* Status => Code and Standard message (i.e. 200 OK)
* Headers => Meta-data associated with the response
* Body => payload of response

> CRUD - Create, Read, Update, Delete

**Common Status Codes:**
* 200 – OK
* 201 – Created
* 304 – Cached
* 400 – Bad Request
* 401 – Unauthorized
* 404 – Not Found
* 500 – Server Error

## Creating a Server
You can specify scripts in your NPM package.json, under the scripts key!

      var http = require('http');
      var server = http.createServer(); // <== Nice!

      // When a request comes in...
      server.on('request', function (request, response) {

        // you can check out a request object here
        // you can check out a response object here

        response.end('This is what you get back as a response!');

      });
      // This .on() is similar to an event listener

      // Start listening on port 1337. Once you've started, do callback!
      server.listen(1337, function() {

        console.log('Im awake! Listening on port 1337.');

      });

> Port: a communication channel on your computer. Only one program can use a port at a time. Port numbers are identifiers to communicate between programs. Your default port is 80 (localhost). 0-1023 are reserved ports

To connect to a server, you need an IP Address and a port.

>Don't forget that your JS may be running on the server-side, not in your browser (client)!

__________

## 2. Building with Express.js

Express is a pipeline. We tell it what to do along the pipeline as requests come in.

Requests are objects, like an event handlers.

It matches on verb and route and allows chaining with router.

> Request / Response cycle:
- A client initiates a request
- the server listens and receives the request
- server processes the request
- server sends a SINGLE RESPONSE back to the client


### Middleware
In Express middleware === the route handling function. The thing that takes request and response.

> If you see 'Cant set headers after they're sent, you are probably trying to send more than one response. A request handler should only ever send one response.

Express takes in the request and looks for a matching request. Is this a GET? Is it a POST? What URL is it? The app processes through the possibilities until it finds one that matches.

      var express = require('express');
      var bodyParser = require('bodyparser');
      var app = express() // <== returns an express app, similar to a server
      // A server listens on port, but won't handle requests unless you tell it to.

      app.listen(1337, function() {
          // do stuff
      });

      // You must send back a response. If you don't send a response, the client will just wait!
      app.get('/example', function(request, response) {
          response.send();
      });

      // Sending back HTML
      app.get('/example', function(request, response) {
          // You can send ONE AND ONLY ONE response per request. Not zero! Not more than one!
          response.send('<h3 style="background-color:blue"></h3>');
      });

      // Sending back a file
      app.get('/example', function(request, response) {
          response.sendFile(__dirname + '/example.html');
      });

>NOTE: ROUTES ARE NOT FILE PATHS despite how this looks!

      //Sending back JSON
      app.get('/example', function(request, response) {
          response.send({
              name: 'Jason',
              favoriteColor: 'javascript'
          });
      });

      // Looks for any file at the request.url
      app.get('/**', function(request, response) {
          response.sendFile(__dirname + '/' + request.url);
      });


### `Get` and `Post` are both verbs

      app.get('/example', function(request, response) {
          console.log('coming back to you from a GET request');
      });
      app.post('/example', function(request, response) {
          console.log('coming back to you from a POST request');
      });

The use of Next()

      app.get('/**', function(request, response, next) {
          console.log('firstly!');
          next();
          response.sendFile(__dirname + '/' + request.url);
      });
      app.get('/**', function(request, response) {
          response.send('finally!');
      });

      // This will always match because I have no url parameter!
      app.use(function(request, response, next) {
          console.log('I run always');
          next();
      });

### Static file serving example

      app.use(function(request, response, next) {
          fs.readFile(__dirName + '/' + request.url, function(err, contents) {
              if (err) {} else {
                  response.send(contents);
              }
          });
      });

### Send an error 404 to the user

      app.use(function(request, response) {
          response.status(404).end();
      });

### Query String: `request.query`
Query string will appear as a question mark after the uri.

      app.get('/times-two', function (request, response) {
        response.send(request.query.number * 2);
      });
      // ^ Will show us any number times two
      // For example: /times-two?number=100 shows 200

### Parameters: `request.params`
request.params is an object where the keys are parts of the url specified by `:` something. Variable portions of our urls!

      // After `:` are our parameters. /times-two/123 will show us 246
      app.get('/times-two:num', function (request, response) {
        response.send(request.params.num * 2);
      });


### Payload: `request.body`

Get requests can't have a body.

Anytime a request comes in parse the body with JSON, then runs next to continue processing requests

      app.use(bodyParser.json());
      //
      app.post('/times-two:num', function (request, response) {
        console.log(request.body);

        // Express does not parse the body by itself!
        response.send(request.num * 2);
      });

### Routers:
A layer of route handlers (middlewares). More modular approach to route handling.
____
