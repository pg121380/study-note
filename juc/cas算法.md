## 模拟CAS算法

~~~java
package pub.liyf.study.juc;

/**
 * 模拟CAS算法
 */
public class TestCAS {
    public static void main(String[] args) {
        final CompareAndSwap cas = new CompareAndSwap();
        for(int i = 0;i < 100;i++){
            new Thread(()->{
                int expectedValue = cas.get();
                boolean compareAndSet = cas.compareAndSet(expectedValue, (int) (Math.random()) * 101);
                System.out.println(compareAndSet);
            }).start();
        }
    }
}

class CompareAndSwap{

    private int value;

    public synchronized int get(){
        return value;
    }

    public synchronized int compareAndSwap(int expected, int newValue){
        int oldValue = value;

        if(oldValue == expected){
            this.value = newValue;
        }
        return oldValue;
    }

    public synchronized boolean compareAndSet(int expected,int newValue){
        return expected == compareAndSwap(expected, newValue);
    }
}

~~~
