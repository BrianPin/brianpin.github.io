---
layout: post
comments: true
title: Implementing a concurrent msg queue in Java
mathjax: true
excerpt: "Learning Java 8, concurrent utilities"
---
Learning Java is always my interests. There is great IDE support. Also centralized document on the web. Plus recently the trend looks like going in favor of Java in the area of cloud computing. Hadoop, Spark, NoSQL all related to Java. That is why I am starting from the basics.
First, of all the Java frameworks, concurrent is always the one that is the hardest and most complex one to learn. Other than the deep learning curve, (Oops, did I say `deep learning`?) Java 8 has a complete new set of interface and classes that hide the Thread behind the scene. For example, `ExecutorService` and `Executors`. **Executors** provides various methods to provide various thread pools. **ExecutorService** provides job submission interfaces whether you want to execute single job or multiple jobs, or scheduled sometime later for execution. These are all fun to learn.

During the first day of memorial long weekend, I coded a concurrent fixed length job queue using Java 8 features. Here is the class called `BoundedConcurrentQueue`:

```java
package queue;

import java.util.concurrent.locks.*;

public class BoundedConcurrentQueue {
    final Lock lock = new ReentrantLock();
    final Condition notFull = lock.newCondition();
    final Condition notEmpty = lock.newCondition();
    int readIndex = 0;
    int writeIndex = 0;
    String[] msg;
    public BoundedConcurrentQueue(int n) {
        msg = new String[n];
    }
    /// No explicit constructor because not necessary
    public void addMsg(String newMsg) throws InterruptedException {
        lock.lock();
        try {
            while ((writeIndex+1)%msg.length == readIndex)
                notFull.await();
            msg[writeIndex++] = newMsg;
            notEmpty.signal();
            System.out.println(newMsg+" added to index: " +
                (writeIndex-1)%msg.length + " read index: " + readIndex);
            writeIndex = writeIndex % msg.length;
        } finally {
            lock.unlock();
        }
    }
    public String getMsg() throws InterruptedException {
        lock.lock();
        String thisMsg;
        try {
            while (readIndex == writeIndex)
                notEmpty.await();

            thisMsg = msg[readIndex++];
            readIndex = readIndex % msg.length;
            notFull.signal();
            return thisMsg;
        } finally {
            lock.unlock();
        }
    }
}
```
And here is the driver code to run it:
```java
package queue;

import java.util.Arrays;
import java.util.List;
import java.util.concurrent.*;
import queue.BoundedConcurrentQueue;

public class AppMain {

    public static void main(String[] args) {
        BoundedConcurrentQueue boundedQ = new BoundedConcurrentQueue(2);
        List<Callable<Void>> writerTasks = Arrays.asList(
                (Callable<Void>)(() -> {boundedQ.addMsg("msga"); return null;}),
                (Callable<Void>)(() -> {boundedQ.addMsg("msgb"); return null;}),
                (Callable<Void>)(() -> {boundedQ.addMsg("msgc"); return null;}),
                (Callable<Void>)(() -> {boundedQ.addMsg("msgc"); return null;}),
                (Callable<Void>)(() -> {boundedQ.addMsg("msgd"); return null;}),
                (Callable<Void>)(() -> {boundedQ.addMsg("msge"); return null;}),
                (Callable<Void>)(() -> {boundedQ.addMsg("msgf"); return null;}),
                (Callable<Void>)(() -> {boundedQ.addMsg("msgg"); return null;}),
                (Callable<Void>)(() -> {boundedQ.addMsg("msgh"); return null;}),
                (Callable<Void>)(() -> {boundedQ.addMsg("msgi"); return null;}),
                (Callable<Void>)(() -> {boundedQ.addMsg("msgj"); return null;}),
                (Callable<Void>)(() -> {boundedQ.addMsg("msgk"); return null;}),
                (Callable<Void>)(() -> {boundedQ.addMsg("msgl"); return null;}));

        Callable<Void> consumerTask = ()-> {
            while (true) {
                System.out.println("got msg " + boundedQ.getMsg());
            }
        };

        ExecutorService w = Executors.newSingleThreadExecutor();
        ExecutorService r = Executors.newSingleThreadExecutor();
        // Read multiple messages from the queue using executors
        r.submit(()->{
            ExecutorService readerExecutor = Executors.newFixedThreadPool(1);
            readerExecutor.submit(consumerTask);			
        });
        // Write multiple messages to the queue using executors
        w.submit(()->{
            ExecutorService writeExecutor =
                Executors.newFixedThreadPool(writerTasks.size());
            try {
                writeExecutor.invokeAll(writerTasks);
            } catch (InterruptedException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        });
    }
}
```

Here is the quick take away:

1. `Condition` is an interface and already associated with the `lock` classes. This is why I appreciate **Java** for the frameworks that already done good amount of software architectures for us.

2. `Condition`s allow you to temporarily release the lock so that you leave the hole to let the other party to do something to the shared resurce, which here is the `queue`. When you found there is nothing you can do, you call **await** to temporarily release the lock and put the current thread into sleep. When you are done with something, you then **signal** the thread which are waiting for this **condition** to happen. Then the **await-ing** thread can wake up and do its work.

3. I made quite a few bugs in the **readIndex** and **writeIndex** manipulation. It is very easy to make some bug when manipulating these two little guys.

4. I still don't know why when I made the `Callable<Void>` list, I have to `return null;`. In Java 8, is it because Callable<Void>?

5. Most of examples I can find on the web are implementing some consumer and produce thread. I ignore all that and using a Java 8 style. I believe this is something new. Also most of the examples you can find are not using **read pointer/index** and **write pointer/index**. This feature is also unique on the web.

6. Actually there is a hierarchy of Java built-in classes and interfaces in the util.concurrent module.
___
interfaces `BlockingQueue<E>`, `ConcurrentMap<K, V>`

classes `ArrayBlockingQueue`, `DelayQueue`, `ConcurrentHashMap<K, V>`, and `ConcurrentSkipList<K, V>`

---
