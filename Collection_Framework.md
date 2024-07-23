# Collection Freamwork Question Answer

## What is the difference between collection and concurrent collections

Collections 
```
1. Not thread-safe: Multiple threads can't access them simultaneously without causing issues like data corruption or exceptions.

2. No atomic operations: Operations like add, remove, or update are not atomic, meaning they can be interrupted by other threads.

3. No concurrency control: No built-in mechanism to control concurrent access.

4. Faster performance: Since they don't handle concurrency, they are generally faster.
5. Examples: ArrayList, HashSet, HashMap

```

Concurrent Collections:
```
1. Thread-safe: Designed for multithreading, allowing multiple threads to access them simultaneously without issues.

2. Atomic operations: Operations like add, remove, or update are atomic, ensuring data integrity.

3. Concurrency control: Built-in mechanisms like locks, semaphores, or atomic variables control concurrent access.

4. Slower performance: Handling concurrency comes with a performance cost, making them generally slower.

5. Examples: ConcurrentHashMap, CopyOnWriteArrayList, BlockingQueue, ConcurrentLinkedQueue
```

## Here's an example that demonstrates the difference between a Collection (ArrayList) and a Concurrent Collection (CopyOnWriteArrayList):

### Example:
Suppose we have a shared list that multiple threads are accessing concurrently. We want to add elements to the list while iterating over it.

### Collection (ArrayList):
```

import java.util.ArrayList;
import java.util.List;

public class ArrayListExample {
    public static void main(String[] args) {
        List<String> sharedList = new ArrayList<>();

        // Thread 1: Add elements to the list
        Thread thread1 = new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                sharedList.add("Thread 1: " + i);
            }
        });

        // Thread 2: Iterate over the list and print elements
        Thread thread2 = new Thread(() -> {
            for (String element : sharedList) {
                System.out.println(element);
            }
        });

        thread1.start();
        thread2.start();
    }
}


Output:

This will throw a ConcurrentModificationException because the ArrayList is not thread-safe, and thread2 is iterating over the list while thread1 is modifying it.
```


### Concurrent Collection (CopyOnWriteArrayList):
```
import java.util.concurrent.CopyOnWriteArrayList;

public class CopyOnWriteArrayListExample {
    public static void main(String[] args) {
        List<String> sharedList = new CopyOnWriteArrayList<>();

        // Thread 1: Add elements to the list
        Thread thread1 = new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                sharedList.add("Thread 1: " + i);
            }
        });

        // Thread 2: Iterate over the list and print elements
        Thread thread2 = new Thread(() -> {
            for (String element : sharedList) {
                System.out.println(element);
            }
        });

        thread1.start();
        thread2.start();
    }
}


Output:
Thread 1: 0
Thread 1: 1
Thread 1: 2
Thread 1: 3
Thread 1: 4
Thread 1: 5
Thread 1: 6
Thread 1: 7
Thread 1: 8
Thread 1: 9

This will print the elements added by thread1 without throwing any exceptions, because CopyOnWriteArrayList is a thread-safe concurrent collection that allows concurrent modifications and iterations.
```
