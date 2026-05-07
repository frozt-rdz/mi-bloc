#  Problem
Suppose we have a class:
```
public class Foo {
  public void first() { print("first"); }
  public void second() { print("second"); }
  public void third() { print("third"); }
}
```
The same instance of Foo will be passed to three different threads. Thread A will call first(), thread B will call second(), and thread C will call third(). Design a mechanism and modify the program to ensure that second() is executed after first(), and third() is executed after second().

**Note:**

We do not know how the threads will be scheduled in the operating system, even though the numbers in the input seem to imply the ordering. The input format you see is mainly to ensure our tests' comprehensiveness.
##  Solution
```
import java.util.concurrent.atomic.AtomicInteger;
class Foo {
    private AtomicInteger primeroFinalizado = new AtomicInteger();
    private AtomicInteger segundoFinalizado = new AtomicInteger();
    public Foo() {
        
    }

    public void first(Runnable printFirst) throws InterruptedException {
        
        // printFirst.run() outputs "first". Do not change or remove this line.
        printFirst.run();
        primeroFinalizado.incrementAndGet();
    }

    public void second(Runnable printSecond) throws InterruptedException {
        
        // printSecond.run() outputs "second". Do not change or remove this line.
        while(primeroFinalizado.get() != 1);
        printSecond.run();
        segundoFinalizado.incrementAndGet();
    }

    public void third(Runnable printThird) throws InterruptedException {
        
        // printThird.run() outputs "third". Do not change or remove this line.
        while(segundoFinalizado.get() != 1);
        printThird.run();
    }
}
```
## Warning
While this solution functions correctly, it is not optimal.

While the program is running, the second and third Threads are evaluating the condition in the while loop at every moment. 

In other words, both Threads are awaiting but actively (busy-waiting); this condition consumes the CPU's resources needlessly.

One way to fix that is to use [CountDownLatch](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CountDownLatch.html). This class allows us to "sleep" Threads until tasks in other threads are done. It's important to consider the following: This is a one-shot phenomenon; the count cannot be reset.

##  Alternative solution:
```
import java.util.concurrent.atomic.CoutnDownLatch;
class Foo {
    private CountDownLatch primeroFinalizado = new CountDownLatch(1);
    private CountDownLatch segundoFinalizado = new CountDownLatch(1);
    public Foo() {
        
    }

    public void first(Runnable printFirst) throws InterruptedException {
        
        // printFirst.run() outputs "first". Do not change or remove this line.
        printFirst.run();
        primeroFinalizado.countDown();
    }

    public void second(Runnable printSecond) throws InterruptedException {
        
        // printSecond.run() outputs "second". Do not change or remove this line.
        primeroFinalizado.await();
        printSecond.run();
        segundoFinalizado.countDown();
    }

    public void third(Runnable printThird) throws InterruptedException {
        
        // printThird.run() outputs "third". Do not change or remove this line.
        segundoFinalizado.await().
        printThird.run();
    }
}
```
