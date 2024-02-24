---
title: "Spring Async Annotation과 Thread Pool 관리하기"
comment: true
date: 2019-07-11
categories: [Programing, Java]
tags: [Spring Boot, Annotation, Async, Thread, ThreadPoolTaskExecutor]

---


### Spring @Async와 Thread Pool 관리하기

#### Java에서의 Anonymous Class 를 이용한 비동기 Thread 사용법


- 메세지를 출력 하는 메소드 `print1`
``` java
public class PrintService {

	public void print1(final String message){
    	System.out.println(message)
    }
}
```

- 비동기로 메소드를 출력 하는 메소드 `print2`

``` java
public class PrintService {

    public void print2(final String message) throws Exception {
        new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println(message);
            }            
        }).start();
    }

}
```

<U>* **동기식 메소드와 비동기식 메소드는 메소드 자체의 형태가 틀려지게 된다.**</U>*

#### Spring @Async 를 이용한 비동기 메소드 작성법
``` java
public class PrintService {

	@Async
	public void print1(final String message){
    	System.out.println(message)
    }
}
```

위와 같이 `@Async` 어노테이션을 활용하면 작성자는 메소드에만 집중 할 수 있다.

다만, 위의 경우에도 Thread 자체가 관리가 되지 않아, 위험한 코드가 될 수 있다.

만약 `print1` 메소드가 단순 출력이 아니라 복잡하고 시간이 오래 걸리는 작업을 하는 경우라면

요청 한만큼 비동기 Thread를 만들고 수행하려 할 것이다.

그리고 또 다른 제약사항도 있는데 다음과 같다.
 - 같은 객체의 메소드를 호출하게 되면 비동기 처리를 하지 못한다.
	- 스프링의 Bean으로 등록되어야만 스프링에서 호출 가능 하기 때문
 - 호출하려는 메소드는 접근제어자가 public 이어야 한다.
	- 내부적으로 AOP로 메소드를 가로채서 수행하기 때문에 public이 아닌 경우 접근이 안되기 때문


#### Spring 에서 Thread 관리 하기
`@Async` 는 설정 하지 않으면 `SimpleAsyncTaskExecutor`를 사용하게 됩니다.

- 나에게 맞는 ThreadPool 세팅하기


``` java


@Configuration
@EnableAsync
public class AsyncConfigure {

	private final int CORE_POOL_SIZE = 3;
    private final int MAX_POOL_SIZE = 20;
    private final int QUEUE_CAPACITY = 15;
    private final String CUSTOM_THREAD_NAME_PREFIX = "CUSTOM_THREAD-";

    @Bean(name = "threadPoolTaskExecutor")
    public Executor threadPoolTaskExecutor() {
        ThreadPoolTaskExecutor taskExecutor = new ThreadPoolTaskExecutor();
        taskExecutor.setCorePoolSize(CORE_POOL_SIZE);
        taskExecutor.setMaxPoolSize(MAX_POOL_SIZE);
        taskExecutor.setQueueCapacity(QUEUE_CAPACITY);
        taskExecutor.setThreadNamePrefix(CUSTOM_THREAD_NAME_PREFIX);
        taskExecutor.initialize();
        return taskExecutor;
    }

}

```


이제 SimpleAsyncTaskExecutor가 아닌 설정한 ThreadPoolTaskExecutor thread를 관리하게된다.

그럼 각자 상황에 맞게 비동기 멀티 쓰레딩 코딩을 할 수 있게 되었습니다.

