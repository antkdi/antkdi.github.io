---
title: "try-catch-resources 와 예외처리"
comment: true
date: 2019-08-13
categories: [Programing, Java]
tags: [Exception, Try-Catch-Resources, Try-Catch]

---

<style>
table.myTable tr th {
    background-color: #FFDAB9;
}
table.myTable tr td {
    background-color: #FFEFD5;
}
</style>


안녕하세요.
오늘은 Java의 `try-catch-resources`와 `예외처리`에 대한 포스팅을 하겠습니다.

얼마 전 아는 지인이 이직을 하려다 코딩테스트에서 탈락 하는 것을 지켜보았습니다.

전문 리뷰어의 리뷰를 보니 바로 `try-catch-resources` 에 대한 내용이 나오더군요.

자 한번 알아 볼까요 ?



# Java의 예외처리 알아보기

여기서 설명하는 예외처리는 입출력 상황에서의 예외처리를 가지고 설명하겠습니다.

예외처리를 알려면, `Throwable` 클래스를 알아야 하는데요. 오라클의 API문서에서 자세하게 설명 하고 있습니다.

[Throwable Class](https://docs.oracle.com/javase/8/docs/api/?java/lang/Throwable.html)


>The Throwable class is the superclass of all errors and exceptions in the Java language. Only objects that are instances of this class (or one of its subclasses) are thrown by the Java Virtual Machine or can be thrown by the Java throw statement. Similarly, only this class or one of its subclasses can be the argument type in a catch clause. For the purposes of compile-time checking of exceptions, Throwable and any subclass of Throwable that is not also a subclass of either RuntimeException or Error are regarded as checked exceptions.
.... 주저리 주저리.. 너무 많아 요약하겠습니다.

1. Throwable 클래스는 Java 언어의 모든 오류 및 예외의 슈퍼 클래스이다.
2. 클래스의 인스턴스인 객체만 JVM에 의해 발생되거나 Java Throw 문에 의해 발생 할 수 있다.
3. 클래스 또는 서브 클래스중 하나만 catch 절에서 인수 유형이 될 수 있다.
4. Throwable 클래스 또는 서브 클래스는 Checked 예외로 간주 된다.

저희가 주목해야 하는 것은 위와 같은 개념인것 같습니다.
모든 예외의 슈퍼클래스이고, try-catch(여기) 에서 인수로 취할 수 있고, 컴파일 단계에서 Checked Exception으로 간주된다.

여기서 Error 와 Exception에 대한 개념을 잡고 넘어 가겠습니다.
- 오류(Error)와 예외(Exception)의 개념

	- 오류(Error)는 시스템에 비정상적인 상황이 생겼을 때 발생. 시스템 레벨에서 발생하기 때문에 심각한 수준의 오류이다. 미리 예측할 수 없기 때문에 애 코드레벨에서 오류에 대한 처리를 하지 않아도 된다.
	- 예외(Exception)는 개발자가 구현한 로직에서 발생. 예외는 발생할 상황을 미리 예측하여 처리할 수 있다. 개발자가 처리할 수 있기 때문에 예외를 구분하고 그에 따른 처리 방법을 정확히 알고 구현해야 한다.

#### 관계도
- Object
	- Throwable 	
        - Error
            - ..
        - Exception
        	- Checked Exception
        	- RuntimeException
        		- Unchecked Exception   	

상하위를 따지자면, 이런 관계가 될 수 있다.

### Checked Exception Vs. UnChecked Exception
<table class="myTable">
<thead>
<tr>
  <th></th>
  <th>Checked Exception</th>
  <th>Unchecked Exception</th>
</tr>
</thead>
<tbody>
<tr>
  <th>처리여부</th>
  <td>반드시 예외를 처리해야 함</td>
  <td>명시적인 처리를 강제하지 않음</td>
</tr>
<tr>
  <th>확인시점</th>
  <td> 컴파일 단계 </td>
  <td>실행단계</td>
</tr>
<tr>
  <th>트랜잭션 처리 여부</th>
  <td> Roll-Back 하지 않음 </td>
  <td> Roll-Back 함</td>
</tr>
<tr>
  <th>대표적인 예외</th>
  <td>Exception의 상속을 받는 하위 클래스 중 <br> RuntimeException 을 제외한 모든 예외 <br> - IOException <br> - SQLException</td>
  <td>RuntimeExcpetion 하위 예외 <br> - NullPointException <br> - IllegalArgrumentException <br> - IndexOutOfBoundException 등</td>
</tr>
</tbody>
</table>

설명이 무지하게 길어지는듯 하네요. 결국 우리가 알아 볼 것은 `Checked Exception` 이라는 것입니다.!

# Java 예외처리

자바의 예외처리 코드를 보겠습니다.

### Java의 예외처리 기본
``` java

try{
    // 예외가 발생 할 수 있는 코드
    throw new Exception(); //여기서는 임시로 예외 발생 
}catch (Exception e){
    //예외 발생 시 수행할 내용
     System.out.println(e.getMessage()); //메세지 출력
}finally{
    //무조건 수행
} 

```

위와 같은 코드를 작성 할 수 있습니다. 
여기에서 예외가 발생할 것 같은 코드를 try구문으로 감싸 주고 예외가 발생하면 처리할 내용을 catch로 잡아 줍니다.
위 코드 처럼 catch( `Exception e` ) 어떤 예외를 잡을 것인지 인수로 작성 할 수 있습니다.
위의 코드에서는 Exception Calss 를 잡아서 처리 하였습니다.
finally 구절은 예외가 발생 하던, 발상하지 않던 항상 처리되는 내용을 위한 구절입니다.
항상 DB Connection을 close() 한다던가 I/O 의 Stream을 close() 한다던가 하는 내용이 보통 들어가죠.


### 다중 예외처리 방법 

```java
try{
      //ArrayIndexOUtOfBoundsException 발생
      //NumberFormatException 발생
      //다른 Exception 발생
}catch(ArrayIndexOutOfBoundsException e){ 
}catch(NumberFormatException e){   
}catch(Exception e){    
}finally{
       
}

```

주의 할 점은 catch 구절이 여러개라도, 하나의 catch 구절만 수행되므로 하위 예외 클래스를 먼저 작성 해야 합니다.
그러면 다른 익셉션이 발생해도 같은 내용을 처리하고 싶을 땐 어떻게 할까요 ?

### 멀티 예외처리 - JDK 7 
```java

try{    
    // ArrayIndexOUtOfBoundsException 발생
    // NumberFormatException 발생
    // 다른 Exception 발생
}catch(ArrayIndexOutOfBoundsException | NumberFormatException e){ 
    // ArrayIndexoutOfBoundException 이나, NumberFormatExcpetion 이 발생 한 경우 한가지의 내용으로 처리
}catch(Exception e){    
}finally{
}

```


# 자동 리소스 닫기 예외처리

자 이제 오늘의 주제에 다 왔습니다.
저희가 알고 싶은 것은 try-catch-resources 였죠.
여지껏 예외처리 코드를 작성하면서 매번 작성했던 finally 구절의 불편함.. 이해 가십니까 ?
이놈을 없앨 수 있도록 JDK 7 부터 지원하는 방법 입니다.

코드를 보시죠


```java

try(
	FileInputStream fis = new FileInputStream("test1.txt")
        FileOutputStream fos = new FileOutputStream("test2.txt")){        

	// 수행 내용
}catch(IOException e){
        // 예외처리 내용
} // finally 어쩌구저쩌구 close() 가 없다!


```

정말 편리한 것 같습니다. 
Stream 또는 Socket, Connection 등 자원을 닫아야 하는 코드를 작성할 때는 이 방법이 좋아 보입니다.

여기서 알아야 될 것이 하나 더 있는데요.
try-catch-resources 형식의 예외처리를 하기 위해선 리소스 객체가 AutoCloseable 인터페이스의 구현체여야 합니다.

AutoClosable... 무엇인지 한번 보시죠.

# AutoCloseable 알기

[AutoCloseable Interface](https://docs.oracle.com/javase/8/docs/api/java/lang/AutoCloseable.html)

>an object that may hold resources (such as file or socket handles) until it is closed. The close() method of an AutoCloseable object is called automatically when exiting a try-with-resources block for which the object has been declared in the resource specification header. This construction ensures prompt release, avoiding resource exhaustion exceptions and errors that may otherwise occur.

번역 - 리소스가 닫힐 때까지 파일 또는 소켓 핸들과 같은 리소스를 보유 할 수있는 개체 자원 스펙 헤더에 오브젝트가 선언 된 자원으로 시도 블록을 종료하면 AutoCloseable 오브젝트의 close () 메소드가 자동으로 호출됩니다. 이 구성은 자원 소진 예외 및 다른 방법으로 발생할 수있는 오류를 피하면서 즉각적인 릴리스를 보장합니다.


```java

public class Test implements AutoCloseable {

    @Override
    public void close() throws Exception {
    }
}

```
위처럼 close 메서드를 오버라이딩 해야 하는거죠. 만약 자원을 사용하는 클래스를 직접 만들었다면,
해당 인터페이스를 구현해서 try-catch-resources 구문을 활용해 봅시다.

이상으로 Java의 예외처리와 , `try-catch-resource` 에 대해서 알아 보았습니다.
