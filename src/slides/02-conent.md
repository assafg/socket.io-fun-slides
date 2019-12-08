> We need real time updates!


--- 

#### Option1

# `Polling`

---

## Things to consider

1. Intervals
2. Data quantity
3. Cleanup
4. Scale

---

#### Option 2

# `Long polling`

---

## Things to consider

1. Scale
2. Infrastructure and support
3. Architecture -> Data flow

---

#### Option 3

# Websockets

----

## Things to consider

1. Client support
2. Scale
3. App integration
4. Fallbacks

----

# Web Sockets

1. Supported by all modern browsers (IE 10+)
2. Dedicated client-server TCP connection
3. Browsers support hundreds of concurrent connections

---

## Considerations when designing a socket communication layer

1. What data is transfered over TCP?
   1. Everything?
   2. Updates (data)?
   3. Update Notifications only
2. Channel splitting
3. Data integrity
4. Connection "Welcome" strategy

---

## Getting started with Socket.io

`yarn add socket.io`

---

## Server

```javascript
io.on('connect', (socket) => {
    socket.emmit('hello');

    socket.on('disconnect', function(){
        // cleanup
    });

    socket.on('some-message', (data) => {
        // Do something with the data
    })
});
```

---

## Client

```javascript
const socket = io('http://server-url');
socket.on('message', (msg) => {
    //do something with msg

    //...
    socket.emit('some-message', clientMessage)
});

//...
socket.emit('another-message', anotherClientMessage);
```

---

## Namespaces

```javascript
// Server
var io = require('socket.io')(80);
var chat = io
  .of('/chat')
  .on('connection', function (socket) {
    socket.emit('a message', {
        that: 'only'
      , '/chat': 'will get'
    });
    chat.emit('a message', {
        everyone: 'in'
      , '/chat': 'will get'
    });
  });

var news = io
  .of('/news')
  .on('connection', function (socket) {
    socket.emit('item', { news: 'item' });
  });
```

---

## Namespaces - cont.

```javascript
// Client
const chat = io.connect('http://localhost/chat')
    , news = io.connect('http://localhost/news');
  
chat.on('connect', function () {
    chat.emit('hi!');
});
  
news.on('news', function () {
    news.emit('woot');
});
```
---

## Rooms - withing a Namespace

```javascript
io.on('connection', function(socket){

  // ...

  socket.join('some room');
});

// ...

io.to('some room').emit('some event');

```

---

Socket.io cheatsheet

```javascript
io.on('connect', onConnect);

function onConnect(socket){

  // sending to the client
  socket.emit('hello', 'can you hear me?', 1, 2, 'abc');

  // sending to all clients except sender
  socket.broadcast.emit('broadcast', 'hello friends!');

  // sending to all clients in 'game' room except sender
  socket.to('game').emit('nice game', "let's play a game");

  // sending to all clients in 'game1' and/or in 'game2' room, except sender
  socket.to('game1').to('game2').emit('nice game', "let's play a game (too)");

  // sending to all clients in 'game' room, including sender
  io.in('game').emit('big-announcement', 'the game will start soon');

  // sending to all clients in namespace 'myNamespace', including sender
  io.of('myNamespace').emit('bigger-announcement', 'the tournament will start soon');

  // sending to a specific room in a specific namespace, including sender
  io.of('myNamespace').to('room').emit('event', 'message');

  // sending with acknowledgement
  socket.emit('question', 'do you think so?', function (answer) {});

  // sending without compression
  socket.compress(false).emit('uncompressed', "that's rough");

  // sending a message that might be dropped if the client is not ready to receive messages
  socket.volatile.emit('maybe', 'do you really need it?');

  // specifying whether the data to send has binary data
  socket.binary(false).emit('what', 'I have no binaries!');

  // sending to all clients on this node (when using multiple nodes)
  io.local.emit('hi', 'my lovely babies');

  // sending to all connected clients
  io.emit('an event sent to all connected clients');

};
```

----

# Demo

[react-redux-socket.io](https://github.com/assafg/react-redux-socket.io)

----

# Lab - Online tic-tac-toe game

----
