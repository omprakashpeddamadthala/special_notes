
### - Coding: Best Time to Buy and Sell Stock 📈

**🤔 Question:** How do you find the best time to buy and sell stock to maximize profit?

**✅ Answer:** The goal is to find the maximum difference between two numbers in an array, where the smaller number (buy price) appears before the larger number (sell price).

**Real-time Use Case:** This is a classic algorithm problem that's great for understanding time and space complexity. It's a simplified model of stock trading analysis.

**Code Example:**
You can solve this efficiently in a single pass (O(n) time complexity) by keeping track of the minimum price found so far and the maximum profit.

```java
class Solution {
    public int maxProfit(int[] prices) {
        int minPrice = Integer.MAX_VALUE;
        int maxProfit = 0;
        for (int price : prices) {
            if (price < minPrice) {
                minPrice = price;
            } else if (price - minPrice > maxProfit) {
                maxProfit = price - minPrice;
            }
        }
        return maxProfit;
    }
}
```

### - Java Concepts: OOP: Encapsulation vs Abstraction 🎭

**🤔 Question:** What's the difference between Encapsulation and Abstraction?

**✅ Answer:**
*   **Abstraction** is about hiding complex implementation details and showing only the necessary features of an object. It answers "what" an object does.
*   **Encapsulation** is about bundling data (attributes) and methods that operate on the data into a single unit (a class). It answers "how" an object does something and controls access to the data.

**Real-time Use Case:**
*   **Abstraction:** Think of a car. You know how to drive it (steer, accelerate), but you don't need to know the internal mechanics of the engine. The car's interface (steering wheel, pedals) is the abstraction.
*   **Encapsulation:** A bank account is a good example. The account balance is private data. You can only interact with it through public methods like `deposit()` and `withdraw()`, which have built-in checks.

**Code Example:**

```java
// Abstraction (through an interface)
interface Vehicle {
    void start(); // Shows WHAT it does
    void stop();
}

// Encapsulation
class Car implements Vehicle {
    private boolean engineOn; // Data is hidden (private)

    @Override
    public void start() { // Exposes functionality
        this.engineOn = true;
        System.out.println("Engine started.");
    }

    @Override
    public void stop() {
        this.engineOn = false;
        System.out.println("Engine stopped.");
    }
}
```

### - Default methods in interfaces vs Abstract classes 🆚

**🤔 Question:** When should I use default methods in an interface versus an abstract class?

**✅ Answer:**
*   **Default methods in interfaces** (since Java 8) allow you to add new methods to an interface without breaking existing implementing classes. They are great for providing optional or common behavior. A class can implement multiple interfaces.
*   **Abstract classes** are used when you want to provide a common base with shared state (fields) and methods for a group of related classes. A class can only extend one abstract class.

**Real-time Use Case:**
*   **Interface with default method:** The `java.util.Collection` interface added the `stream()` default method in Java 8, allowing all collections to use streams without forcing every collection class to implement it from scratch.
*   **Abstract class:** You might have an `AbstractAnimal` class with a `name` field and a `sleep()` method that all specific animal classes (like `Dog`, `Cat`) can inherit.

**Code Example:**

```java
// Interface with default method
interface Animal {
    void makeSound();
    default void sleep() { // Optional behavior
        System.out.println("Zzz...");
    }
}

// Abstract class
abstract class Canine {
    protected String name; // Shared state

    public Canine(String name) {
        this.name = name;
    }

    abstract void bark(); // Must be implemented by subclasses
}
```

### - Collections: HashMap, TreeMap, HashSet 🗺️

**🤔 Question:** What are the differences between HashMap, TreeMap, and HashSet?

**✅ Answer:**
*   **HashMap:** Stores key-value pairs. It provides O(1) average time complexity for insertion, deletion, and retrieval. The order of elements is not guaranteed.
*   **TreeMap:** Also stores key-value pairs but maintains them in sorted order of the keys. It has O(log n) time complexity for most operations.
*   **HashSet:** Stores only unique elements (no duplicates). Like HashMap, it offers O(1) average time complexity for adding, removing, and checking for an element. It doesn't maintain any order.

**Real-time Use Case:**
*   **HashMap:** Use it when you need fast lookups and the order doesn't matter, like storing user data by user ID.
*   **TreeMap:** Use it when you need to iterate over keys in a sorted order, like a dictionary or a phonebook.
*   **HashSet:** Use it to keep track of unique items, like the set of unique visitors to a website.

**Code Example:**

```java
// HashMap: Unordered key-value pairs
Map<String, Integer> wordCounts = new HashMap<>();
wordCounts.put("hello", 1);
wordCounts.put("world", 2);

// TreeMap: Sorted key-value pairs
Map<String, Integer> sortedWordCounts = new TreeMap<>();
sortedWordCounts.put("world", 2);
sortedWordCounts.put("hello", 1); // "hello" will come before "world"

// HashSet: Unordered unique elements
Set<String> uniqueNames = new HashSet<>();
uniqueNames.add("Alice");
uniqueNames.add("Bob");
uniqueNames.add("Alice"); // This will be ignored
```

### - Fail-safe vs Fail-fast iterators 🏃‍♂️💨

**🤔 Question:** What's the difference between fail-safe and fail-fast iterators?

**✅ Answer:**
*   **Fail-fast iterators** throw a `ConcurrentModificationException` if the collection is modified while iterating over it (except through the iterator's own `remove()` method). They operate on the original collection.
*   **Fail-safe iterators** do not throw an exception if the collection is modified. They work on a clone or a snapshot of the collection, so they don't see the modifications made after the iteration started.

**Real-time Use Case:**
*   **Fail-fast:** The iterators for `ArrayList` and `HashMap` are fail-fast. This helps to detect bugs early in single-threaded environments.
*   **Fail-safe:** The iterators for concurrent collections like `CopyOnWriteArrayList` and `ConcurrentHashMap` are fail-safe, making them suitable for multi-threaded environments where collections might be modified by other threads during iteration.

**Code Example:**

```java
// Fail-fast example
List<String> list = new ArrayList<>();
list.add("A");
list.add("B");
for (String s : list) {
    // list.add("C"); // This would throw ConcurrentModificationException
}

// Fail-safe example
List<String> concurrentList = new CopyOnWriteArrayList<>();
concurrentList.add("X");
concurrentList.add("Y");
for (String s : concurrentList) {
    concurrentList.add("Z"); // This is safe, but the new element won't be seen in this loop
}
```

### - HashMap initial size and resizing 📏

**🤔 Question:** How do the initial size and resizing of a HashMap work?

**✅ Answer:** A `HashMap` has an initial capacity (default is 16) and a load factor (default is 0.75). When the number of elements exceeds `capacity * load_factor`, the HashMap is resized (usually doubled in size). This resizing operation is expensive as it involves creating a new internal array and rehashing all existing elements.

**Real-time Use Case:** If you know you're going to store a large number of elements (e.g., 1 million), it's more efficient to initialize the `HashMap` with a larger capacity to avoid multiple resizing operations.

**Code Example:**

```java
// Default initial capacity (16)
Map<String, Integer> map1 = new HashMap<>();

// Specifying an initial capacity to avoid resizing
Map<String, Integer> map2 = new HashMap<>(1000);
```

### - JVM Architecture: Purpose of the 3 types of ClassLoaders 🏛️

**🤔 Question:** What is the purpose of the three main ClassLoaders in the JVM?

**✅ Answer:** ClassLoaders are responsible for loading Java classes into the JVM at runtime. They follow a delegation hierarchy.
1.  **Bootstrap ClassLoader:** The parent of all class loaders. It loads the core Java libraries (like `java.lang.*`) from the `rt.jar` file. It's written in native code.
2.  **Extension ClassLoader:** The child of the Bootstrap ClassLoader. It loads classes from the extension directories (`jre/lib/ext`).
3.  **System/Application ClassLoader:** The child of the Extension ClassLoader. It loads the application's own classes from the classpath.

**Real-time Use Case:** This hierarchical structure ensures that core Java classes are always loaded from a trusted source (Bootstrap) and prevents application classes from spoofing them.

### - Object immutability 🧊

**🤔 Question:** What is an immutable object and how do you create one?

**✅ Answer:** An immutable object is an object whose state cannot be changed after it is created. The `String` class is a classic example of an immutable class in Java.

To create an immutable class:
1.  Declare the class as `final` so it can't be extended.
2.  Make all fields `private` and `final`.
3.  Don't provide any setter methods.
4.  Initialize all fields in the constructor.
5.  If the class has mutable fields (like a `List`), make defensive copies of them in the constructor and return copies from the getters.

**Real-time Use Case:** Immutable objects are inherently thread-safe, making them great for use in multi-threaded applications. They are also useful as keys in a `HashMap` because their hash code will not change.

**Code Example:**

```java
public final class ImmutablePerson { // final class
    private final String name; // private final fields
    private final List<String> hobbies;

    public ImmutablePerson(String name, List<String> hobbies) {
        this.name = name;
        this.hobbies = new ArrayList<>(hobbies); // Defensive copy
    }

    public String getName() {
        return name;
    }

    public List<String> getHobbies() {
        return new ArrayList<>(hobbies); // Return a copy
    }
}
```

### - Garbage Collection types and algorithms 🗑️

**🤔 Question:** What are the different types of Garbage Collection (GC) and their algorithms?

**✅ Answer:** Garbage Collection is the process of automatically freeing up memory by deleting objects that are no longer reachable by the program.
*   **Types of GC Events:**
    *   **Minor GC:** Cleans the Young Generation space.
    *   **Major/Full GC:** Cleans the entire heap (Young and Old Generations).
*   **Common GC Algorithms:**
    *   **Serial GC:** Uses a single thread for GC. Good for small applications with low memory needs.
    *   **Parallel GC:** The default GC in many JVMs. Uses multiple threads for GC to speed up the process.
    *   **G1 (Garbage-First) GC:** The default since Java 9. It divides the heap into regions and prioritizes collecting from regions with the most garbage, aiming for predictable pause times.
    *   **ZGC:** A low-latency GC designed for very large heaps, aiming for pause times under 10 milliseconds.

**Real-time Use Case:** The choice of GC algorithm depends on the application's needs. A web server might use G1 or ZGC for low-latency responses, while a batch processing application might use the Parallel GC for high throughput.

### - Thread Pools: Types and Use Cases 🏊‍♂️

**🤔 Question:** What are the different types of thread pools and their use cases?

**✅ Answer:** Thread pools manage a pool of worker threads to execute tasks concurrently, which is more efficient than creating a new thread for every task. The `Executors` class provides factory methods for creating common types of thread pools.

*   **`newFixedThreadPool(int nThreads)`:** Creates a thread pool with a fixed number of threads.
    *   **Use Case:** When you need to limit the number of concurrent tasks, for example, to avoid overwhelming a database with too many connections.
*   **`newCachedThreadPool()`:** Creates a thread pool that creates new threads as needed but will reuse previously constructed threads when they are available.
    *   **Use Case:** For executing many short-lived asynchronous tasks.
*   **`newSingleThreadExecutor()`:** Creates a thread pool with a single thread that will execute tasks sequentially.
    *   **Use Case:** When you need to ensure tasks are executed in a specific order.
*   **`newScheduledThreadPool(int corePoolSize)`:** Creates a thread pool that can schedule commands to run after a given delay, or to execute periodically.
    *   **Use Case:** For running background tasks at regular intervals, like a daily cleanup job.

**Code Example:**

```java
ExecutorService executor = Executors.newFixedThreadPool(10);
executor.submit(() -> {
    System.out.println("Task running in a thread pool!");
});
executor.shutdown();
```

### - String pool vs regular objects 🏊‍♀️

**🤔 Question:** What is the difference between a String in the string pool and a regular String object?

**✅ Answer:**
*   **String Pool:** The string pool (or string intern pool) is a special storage area in the Java heap. When you create a string literal (e.g., `String s1 = "hello";`), the JVM checks the pool first. If the string already exists, it returns a reference to the existing string; otherwise, it creates a new string in the pool.
*   **Regular String Object:** When you create a string using the `new` keyword (e.g., `String s2 = new String("hello");`), a new string object is always created in the heap, outside the pool.

**Real-time Use Case:** Using string literals saves memory because multiple references can point to the same string in the pool.

**Code Example:**

```java
String s1 = "hello"; // Goes into the string pool
String s2 = "hello"; // s2 points to the same object as s1 in the pool
String s3 = new String("hello"); // A new object is created in the heap

System.out.println(s1 == s2); // true
System.out.println(s1 == s3); // false
```

### - How to store sensitive data (char[] vs String) 🤫

**🤔 Question:** Why is it better to store sensitive data like passwords in a `char[]` instead of a `String`?

**✅ Answer:** It is more secure to use a `char[]` for sensitive data for two main reasons:
1.  **Immutability of Strings:** Strings are immutable. Once a `String` object is created, its value cannot be changed. This means the password remains in memory in plain text until the garbage collector cleans it up, which could be a while. An attacker with access to a memory dump could find it.
2.  **Mutability of char[]:** A `char[]` is mutable. You can explicitly overwrite the array with zeros or other characters after you are done with it, effectively erasing the sensitive data from memory immediately.

**Real-time Use Case:** Any application that handles passwords or other sensitive credentials should use a `char[]` to hold that data for the shortest possible time and then wipe it. The Swing `JPasswordField`'s `getPassword()` method returns a `char[]` for this reason.

**Code Example:**

```java
char[] password = {'p', 'a', 's', 's', 'w', 'o', 'r', 'd'};

// ... use the password ...

// After use, wipe the array
java.util.Arrays.fill(password, ' ');
```

### - Java Streams: Grouping by multiple keys and frequency counting 📊

**🤔 Question:** How can you group elements in a Java Stream by multiple keys and count their frequency?

**✅ Answer:** You can achieve this by using a nested `Collectors.groupingBy()` collector or by creating a custom key class.

**Real-time Use Case:** Imagine you have a list of sales records and you want to count the number of sales per product, per region.

**Code Example:**
Let's say we have a list of `Employee` objects and we want to count them by their department and gender.

```java
class Employee {
    String department;
    String gender;
    // constructor, getters
}

List<Employee> employees = // ... list of employees

// Group by department, then by gender, and count
Map<String, Map<String, Long>> countByDeptAndGender = employees.stream()
    .collect(Collectors.groupingBy(
        Employee::getDepartment,
        Collectors.groupingBy(
            Employee::getGender,
            Collectors.counting()
        )
    ));

System.out.println(countByDeptAndGender);
// Output: {Sales={Male=5, Female=8}, Engineering={Male=12, Female=4}}
```
