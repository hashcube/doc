# Sockets

## Overview

The DevKit provides a cross-platform socket. Use it like this:

~~~
var HOST = 'some.domain.com';
var PORT = 9999;

import gc.native.socketTransport as socketTransport;
this.sock = new socketTransport.Socket(HOST, PORT);

this.sock.onError = function(err) {
    logger.log('error: ' + err);
};
this.sock.onClose = function() {
    logger.log('closed');
};
this.sock.onConnect = function() {
    logger.log('connected to ' + HOST + ':' + PORT);
};
this.sock.onRead = function(data) {
    logger.log('received: ' + data);
};
~~~

## Connecting

The socket tries to connect to the server at `HOST:PORT` on initialization, and calls `onConnect` when the connection succeeds. The `onClose` callback fires when the connection is lost.

## Reading

The socket comes with a reader with 3 modes: `stream`, `json`, and `delimiter`. It defaults to `stream` mode, which just passes all data along to the `onRead` handler. Change the read mode like this:

~~~
this.sock.reader.setMode('json');
~~~

This line tells the reader to pass along JSON objects as they arrive. To use delimiter mode, do something like this:

~~~
this.sock.reader.setMode('delimiter', '\r\n');
~~~

That tells the reader to send data to the `onRead` handler line by line.

## Writing

To send data up to the server, simply call `socket.send`, like so:

~~~
this.sock.send('hello, server!');
~~~

## Error Handling

The socket passes all error messages to `socket.onError` (see example above).