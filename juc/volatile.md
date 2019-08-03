# volatile关键字与内存可见性

## volatile

> 在多线程环境下，JVM会为每一个线程分配一个独立的缓存空间用于提高效率，当线程要对多个线程共享数据进行操作时,会首先把数据从主内存中读取到自己的缓存空间中。对数据修改后，再写回到主内存中。如果在写回之前，该数据被另一个线程读取到，则会出现数据不一致的情况。这被称为**内存可见性问题**。即多个线程对于彼此对共享数据的操作是不可见的。该问题可以通过synchronized解决，但是效率会变低，于是就出现了==volatile==关键字。

### volatile的作用：当多个线程进行操作共享数据时，可以保证主内存中的数据是可见的。volatile还会禁止指令重排，造成性能的略微下降。

## volatile与synchronized的关系

1. 相较于synchronized，volatile是一种轻量级的同步策略
2. volatile不具备互斥性，即多个线程可以同时访问一个共享数据
3. volatile不能保证变量的原子性

## 代码示例（可以去掉flag前面的volatile来查看区别）

~~~java
public class TestVolatile {
    public static void main(String[] args) {
        ThreadDemo threadDemo = new ThreadDemo();
        new Thread(threadDemo).start();
        while (true){
            if (threadDemo.isFlag()){
                System.out.println("----------------");
                break;
            }
        }
    }
}

class ThreadDemo implements Runnable{

    private volatile boolean flag = false;

    public boolean isFlag() {
        return flag;
    }

    public void setFlag(boolean flag) {
        this.flag = flag;
    }

    @Override
    public void run() {
        try {
            Thread.sleep(200);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        flag = true;
        System.out.println("flag is " + isFlag());
    }
}
~~~
