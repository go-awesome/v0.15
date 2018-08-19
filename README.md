# v0.15 of µWebSockets

µWS ("[micro](https://en.wikipedia.org/wiki/Micro-)WS") is fast and lightweight message passing for the modern web.

#### Pub/sub
One of the most popular usages of WebSockets is to deliver a message sent under a topic to all subscribers of said topic. This functionality is easy to implement and even easier to completely mess up, resulting in a very inefficient solution.

With v0.15 we now have built-in protocol independent pub/sub support delivering top notch broadcast performance.

```c++
ws->sub("some/topic/*");
ws->pub("another/topic/here", message);
```

#### Build optimized WebSocket & HTTP servers & clients in no time.
```c++
uWS::App a; // or uWS::SSLApp(options) for SSL

a.onGet("/", [](auto *s, auto *req, auto *args) {

    s->writeStatus(200)->writeHeader("Hello", "World")->end(buffer, 512);

}).onGet("/largefile", [](auto *s, auto *req, auto *args) {

    s->write([](int offset) {
        // stream chunks of the large file
    }, fileSize);

}).onPost("/upload/:filename", [](auto *s, auto *req, auto *args) {

    s->read([std::string filename = args[0]](int offset, auto chunk) {
        // stream chunks of data in to database with filename
    });

}).onWebSocket("/wsApi", [](auto *protocol) {

    protocol->onConnection([](auto *ws) {

        // store this websocket somewhere

    })->onMessage([](auto *ws, auto message, auto opCode) {

        ws->send(message, opCode);

    })->onDisconnection([]() {

        // remove this websocket

    });

}).listen("localhost", 3000, 0);
```
