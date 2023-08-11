# synchronized

### 재진입 락 (reentrant lock) 예시

```java
public interface Counter {
    void increase();
    int getValue();
    String getCounterName();
}
```

```java
public class SyncIncrementer implements Counter {
    private static final String COUNTER_NAME = "Sync Counter";
    private int value = 0;

    @Override
    public synchronized void increase() {
        value++;
    }

    @Override
    public int getValue() {
        return value;
    }

    @Override
    public String getCounterName() {
        return COUNTER_NAME;
    }
}
```

```java
public class ReentrantSyncIncrementer implements Counter {
    private static final String COUNTER_NAME = "Reentrancy Sync Counter";
    private final Object lockObject = new Object();
    private int value = 0;

    @Override
    public void increase() {
        synchronized (lockObject) {
            System.out.println("First Sync Block");

            synchronized (lockObject) {
                System.out.println("Second Sync Block");

                synchronized (lockObject) {
                    System.out.println("Third Sync Block");
                    value++;
                }
            }
        }
    }

    @Override
    public int getValue() {
        return value;
    }

    @Override
    public String getCounterName() {
        return COUNTER_NAME;
    }
}
```

여기 increase 메서드에서 중첩된 synchronized 블록이 사용되면서 재진입 락을 허용해준다.   
각 블록은 동일한 lockObject를 락으로 사용하고 있다. 이로 인해 같은 스레드에서 이미 획득한 락을 다시 획득할 수 있게 되며, 이것이 재진입 락의 핵심 원리.   

```java
abstract class AbstractUsage {
    private static final int NUMBER_OF_THREADS = 3;
    private static final int UPPER_BOUND = 1_000;

    static void increaseCountAsUpperBoundBy(Counter incrementer) {
        final ExecutorService threadPool = Executors.newFixedThreadPool(NUMBER_OF_THREADS);

        IntStream.range(0, UPPER_BOUND)
            .forEach(count -> threadPool.submit(incrementer::increase)); // synchronized되지 않은 Counter의 increase method

        try {
            // 위 작업이 완료될 때까지 대기
            threadPool.awaitTermination(100, TimeUnit.MILLISECONDS);

            final int finalCountValue = incrementer.getValue();
            final String incrementerName = incrementer.getCounterName();

            System.out.println("The final value of `" + incrementerName + "` is " + finalCountValue);
            System.out.println("[ Expected Value (1_000) == " + incrementerName + " ] is " + (UPPER_BOUND == finalCountValue));

            threadPool.shutdown();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

```
출력 결과
The final value of `Reentrancy Sync Counter` is 702
[ Expected Value (1_000) == Reentrancy Sync Counter ] is false
```


```java
public class Usage02 extends AbstractUsage {
    /*
    # `synchronized` 영역 재진입 예시
    `synchronized` 메서드와 블록 내에 또 Synchronized 영역이 있는 경우 이때는 기존에 획득한 락을 유지
    동일한 락에 대해 여러 번 재진입이 가능하다는 것을 의미함
     */

    public static void main(String[] args) {
        // 락을 이미 획득한 경우 계속 획득할 수 있음
        increaseCountAsUpperBoundBy(new ReentrantSyncIncrementer());
    }
}
```
