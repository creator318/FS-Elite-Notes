#Java 

### 1. What will happen when the following code is executed?
```java
class MyCallable implements Callable<Integer> {
    public Integer call() {
        return 100;
    }
}

public class Test {
    public static void main(String[] args) {
        ExecutorService service = Executors.newSingleThreadExecutor();
        Future<Integer> future = service.submit(new MyCallable());
        service.shutdown();
    }
}
```  

- ❌ call() will never be executed because get() is missing.
- ❌ call() will throw an exception.
- ❌ Compile-time error.
- ✅ call() will still be executed even without future.get().

### 2. What happens for the following code?
```java
Callable<String> c = () -> {
    Thread.sleep(2000);
    return "Result";
};

ExecutorService executor = Executors.newFixedThreadPool(1);
Future<String> future = executor.submit(c);
System.out.print(future.get(1, TimeUnit.SECONDS));
```

- ❌ "Result" will be printed  
- ❌ ExecutionException
- ❌ IllegalStateException  
- ✅ wTimeoutException  

### 3. What will be the output of the following code?  
```java
class MyRunnable implements Runnable {
    public void run() {
        System.out.print("Runnable");
    }
}

public class Test {
    public static void main(String[] args) {
        Thread t = new Thread(new MyRunnable());
        t.start();
    }
}
```

- ✅ Runnable  
- ❌ Runtime exception  
- ❌ No output  
- ❌ Compile-time error  

### 4. What will be the output?
```java
class MyCallable implements Callable<String> {
    public String call() {
        return "Callable";
    }
}

public class Test {
    public static void main(String[] args) throws Exception {
        FutureTask<String> task = new FutureTask<>(new MyCallable());
        new Thread(task).start();
        System.out.print(task.get());
    }
}
```

- ✅ Callable  
- ❌ Compile-time error
- ❌ Runtime exception
- ❌ null

### 5. In the following code, how many threads are created?
```java
public class Test {
    public static void main(String[] args) {
        ExecutorService service = Executors.newFixedThreadPool(5);
        for (int i = 0; i < 10; i++) {
            service.submit(() -> System.out.print(Thread.currentThread().getName() + " "));
        }
        service.shutdown();
    }
}
```

- ✅ 5
- ❌ Depends on JVM
- ❌ 15
- ❌ 10

### 6. At the point of printing, what is most likely?
```java
ExecutorService service = Executors.newFixedThreadPool(2);
Future<Integer> future1 = service.submit(() -> 1);
Future<Integer> future2 = service.submit(() -> 2);
System.out.print(future1.isDone() + " " + future2.isDone());
service.shutdown();
```

- ❌ false false
- ❌ true false
- ❌ true true
- ✅ Any combination depending on timing  

### 7. What is wrong with this Runnable usage?  
```java
Runnable r = () -> {
    Thread.sleep(1000);
    System.out.println("Done");
};

new Thread(r).start();
```

- ❌ Multiple threads will be created  
- ❌ Compile-time error due to missing return
- ❌ No problem
- ✅ `Thread.sleep()` must be inside try-catch block  

### 8. In this code, what type does the Future hold?
```java
Future<?> future = executor.submit(() -> System.out.println("Task"));
```
- ❌ `Future<String>`  
- ❌ `Future<Integer>`  
- ❌ `Future<Void>`  
- ✅ `Future<Object>`  

### 9. Identify the problem in the following code:
```java
class MyTask implements Runnable {
    public String run() {
        return "Hello";
    }
}
```

- ❌ Valid code  
- ❌ Infinite loop  
- ✅ Compile-time error because Runnable.run() must return void
- ❌ Runtime exception  

### 10. What happens in this code?
```java
ExecutorService executor = Executors.newSingleThreadExecutor();

Future<String> future = executor.submit(() -> {
    throw new RuntimeException("Error occurred!");
});

future.get();
```

- ✅ ExecutionException is thrown when get() is called
- ❌ Thread terminates silently  
- ❌ Future returns null  
- ❌ No Exception is thrown