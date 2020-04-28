---
title: "Spring AOP 와 Custom Annotation"
comment: true
date: 2019-07-15
categories: [Programing, Java]
tags: [SpringBoot, Annotaion, AOP]

---

## Spring AOP로 Custom Annotation을 통해 로깅하기 

### 1.AOP 개념

>Aspect-Oriented Programming (AOP) complements Object-Oriented Programming (OOP) by providing another way of thinking about program structure. The key unit of modularity in OOP is the class, whereas in AOP the unit of modularity is the aspect. Aspects enable the modularization of concerns such as transaction management that cut across multiple types and objects. (Such concerns are often termed crosscutting concerns in AOP literature.)

>One of the key components of Spring is the AOP framework. While the Spring IoC container does not depend on AOP, meaning you do not need to use AOP if you don't want to, AOP complements Spring IoC to provide a very capable middleware solution.

Aspect-Oriented Programming (AOP)은 프로그램 구조에 대한 또 다른 사고 방식을 제공함으로써 객체 지향 프로그래밍 (OOP)을 보완합니다. OOP의 모듈성의 핵심 단위는 클래스이지만 AOP에서는 모듈화 단위가 측면입니다. 여러 측면에서는 여러 유형 및 객체를 가로 지르는 트랜잭션 관리와 같은 문제를 모듈화 할 수 있습니다. (이러한 우려는 종종 AOP 문헌에서 교차 절단 문제로 불린다.)

Spring의 핵심 컴포넌트 중 하나는 AOP 프레임 워크이다. Spring IoC 컨테이너는 AOP에 의존하지 않지만 원하지 않는 경우 AOP를 사용할 필요가 없다는 것을 의미하며, AOP는 Spring IoC를 보완하여 매우 뛰어난 미들웨어 솔루션을 제공한다.

Spring 2.x 의 AOP 도입부를 번역해 보았습니다.

이해가 잘 안가니 코드작성

``` java
public class CalculationService {

	private int a = 0;
    private int b = 0;
    
    public int cal_add(int x, int y){
        a += x;
        b -= y;
        return a+b;
    }
}
```

위 메서드는 정수 두개를 입력받아 누적 더하기와, 누적 빼기를 저장하고 그 값들을 더한 결과를 반환하는 메서드이다.

이 포스트 에서는 결과를 연산 하기 전의 x, y 값의 출력을  `Spring AOP` 를 통해 해보도록 하자.

단순하게 아래 코드 처럼 출력도 가능하다.

``` java
public class CalculationService {

	private int a = 0;
    private int b = 0;
    
    public int cal_add(int x, int y){
		System.out.println("arg0: " + x);
		System.out.println("arg1: " + y);
        a += x;
        b -= y;
        return a+b;
    }
}
```

하지만 `cal_add` 같은 메서드가 하나가 아니라면 불필요한 중복 코드가 늘어나게 될것이고,
이런 관점에서 프로그래밍 해보자는 것이 바로 AOP의 개념이다.

>Aspect-Oriented Programming (AOP)은 프로그램 구조에 대한 또 다른 사고 방식을 제공함으로써 객체 지향 프로그래밍 (OOP)을 보완합니다. OOP의 모듈성의 핵심 단위는 클래스이지만 AOP에서는 모듈화 단위가 측면입니다. 여러 측면에서는 여러 유형 및 객체를 가로 지르는 트랜잭션 관리와 같은 문제를 모듈화 할 수 있습니다. (이러한 우려는 종종 AOP 문헌에서 교차 절단 문제로 불린다.)



### MyLogger Annotation 작성
``` java 

@Retention(RetentionPolicy.RUNTIME)
public @interface MyLogger {
}
```



@MyLogger 어노테이션 로깅 로직 작성
``` java

@Component
public class MyLoggerHandler {


    @Around("@annotation(kr.co.antkdi.app.aop_logging.MyLogger)")
    public Object loggingAdvice(ProceedingJoinPoint joinPoint) throws Throwable {

        Object[] parameterValues = joinPoint.getArgs();
        MethodSignature signature = (MethodSignature) joinPoint.getSignature();
        Method method = signature.getMethod();

        for (int i = 0; i < method.getParameters().length; i++) {
            System.out.println(method.getParameters()[i].getName() + ":" + parameterValues[i]);
        }
        Object returnObj = joinPoint.proceed();
        return returnObj;
    }
}

```

로깅할 메서드에 어노테이션 적용
``` java
@Service("calculationService")
public class CalculationService {

    private int a = 0;
    private int b = 999;

    @MyLogger
    public int cal(int x, int y){
        a += x;
        b -= y;
        return a+b;
    }
}
    
```

적용된 로깅 실행
``` java

@Component
public class ApplicationRunner  implements CommandLineRunner {

    @Autowired
    CalculationService calculationService;

    @Override
    public void run(String... args) {
        calculationService.cal(10, 6);
    }
}
```

결과
``` shell
arg0:10
arg1:6
```