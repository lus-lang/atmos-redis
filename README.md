# redis

`redis` is a client for Redis and Valkey servers. It speaks RESP3 with pipelines, transactions, publish and subscribe, server-side scripts, and a pool of connections. Every command of the server is a method of the client. The package also provides the stores that `rate_limit`, `session`, and `queue` accept.

The following example sets a key that expires, sends two commands in one write, and prints the messages of a channel.

```lus nocheck
local redis = require("https://atmosp.here/2.0/redis.lus")

local r = redis.connect("redis://localhost:6379")
r:set("visits", 0, { ex = 60 })
print(r:incr("visits"))                              --> 1
local replies = r:pipeline(function(p)
    p:get("visits")
    p:hgetall("user:1")
end)
print(replies[1])                                    --> 1
for channel, message in r:subscribe("news") do
    print(channel, message)
end
```

Every call waits on a socket. Inside a detached coroutine a call suspends that coroutine while the others run, so the handlers of a `web` application share one client.

The package requires Lus 2.0 with TLS sockets and declares the permission `network:tcp`, which it uses to connect to the server. It never calls `pledge`, so the program that loads it keeps control of its permissions.

The reference is in the doc comments of `redis.lus`, which `lus doc redis.lus` prints.
