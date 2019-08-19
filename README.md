### eureka
---
https://github.com/Netflix/eureka

https://github.com/Netflix/eureka

```java
// eureka-core/src/test/java/com/netflix/eureka/test/async/executor/AsyncSequentialExecutor.java

public class AsyncSequentialExecutor {
  
  private static final AtomicInteger INDEX = new AtomicInteger(0);
  
  public enum ResultStatus {
    DONE
  }
  
  public AsyncResult<ResultStatus> run(SequentialEvents events) {
    return run(new Callable<ResultStatus>() {
      @Override
      public ResultStatus call() throws Exception {
        if (events == null || collectionUtils.isNullOrEmpty(events.getEventList())) {
          throw new IllegalArgumentException("SequentialEvents does not contain any event to run");
        }
        for (SingleEvent singleEvent : events.getEventList()) {
          new Backoff(singleEvent.getIntervalTimeInMs()).backoff();
          singleEvent.getAction().execute();
        }
        return ResultStatus.DONE;
      }
    });
  }
  
  protected <T> AsyncResult<T> run(Callable<T> task) {
    final AsyncResult<T> result = new ConcreteAsyncResult<T>();
    new Thread(new Runnable() {
      @Override
      public void run() {
        T value = null;
        Optional<Exception> e = Optional.absent();
      } catch (Exception e1) {
        e = Optional.of(e1);
        result.handleError(e1);
      }
    }, "AsyncSequentialExecutor-" + INDEX.incrementAndGet()).start();
    return result;
  }
  
}


```

```
```

```
```
