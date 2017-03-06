---
layout: post
comments: true
title: Some notes about Java concurrent structure and Future<T> objects
mathjax: true
excerpt: "I am afraid a lot of newbies feel hard to understand what the Future<T> means. So here we are"
---

When I first started learning [Netty](https://www.wikiwand.com/en/Netty_(software)), I feel confused about so many seems to know but actually I don't know classes and objects. For example: `ServerBootstrap`, `interface Channel`, `ChannelFuture`. I started looking for documents and tutorials. In the end I always end up with some official examples or code that still doesn't tell me in plain English, what does `Future<T>` means? Luckily few weeks later, since I was mostly doing my day job and didn't have too much time reading all these things, I found there is a better way to learn Netty and Java. 

*Follow hello-world server examples* and *Type in code by hand* and some help from the web: [DZone](https://dzone.com/articles/javautilconcurrentfuture)

For reference, my day job is mostly filled with C++ code and I am very familiar with *reactor pattern* programming since this style of network programming can create the best performance and throughput as much as your hardware can achieve.

The reactor pattern is that you never blocked by system functions that is caused by waiting for remote connections or reading data or writing to the peer. But you need to install a callback function and in the callback function you will be ready to receive the connection result or something that is incoming from the peer. But in C++, this type of code is usally not organized. I mean, it is not as verbose as Java. So here is where Java organize the asynchronous and event driven code: Using a class called Future<T> to abstract the concept of a event that will happen in the future. 

Have you ever went to a resturant that you finished your order, and the waiter gave you and electronic device for notification? That electronic device is the Java's Future<T> object in real world.

Look at this example from an article from DZone: [java.util.concurrent.Future Basics](https://dzone.com/articles/javautilconcurrentfuture)

```Java
private final ExecutorService pool = Executors.newFixedThreadPool(10);
public Future<String> startDownloading(final URL url) throws IOException {
    return pool.submit(new Callable<String>() {
        @Override
        public String call() throws Exception {
            try (InputStream input = url.openStream()) {
                return IOUtils.toString(input, StandardCharsets.UTF_8);
            }
        }
    });
}
```

*startDownloading* may actually not started yet when it returns. Instead, it gives you a ticket of the *Future*. That in my opinion, holds the reference to the callback function that is going to be used when the event has happened.


