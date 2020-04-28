---
title: "Comparator를 이용한 Custom Sorting 하기"
comment: true
date: 2016-07-18
categories: [Programing, Java]
tags: [Comparator, Custom Sorting, Sort]


---

### Comparator를 이용한 Custom Sorting 하기

#### java 에서 배열 및 선형구조의 자료구조에서 커스텀 소팅 하는 방법을 얘기합니다.

UserVO 객체에 멤버변수로 이름과 나이가 있다.
우리는 UserVO로 나열된 배열 및 List에서 나이순으로 ASC 정렬을 할 것이고,  만약에 나이가 같다면 2차 정렬로 이름을 DESC 정렬 할 것이다.
우선 정렬에 사용할 

 - Object Class 

```java 
public class UserVO {  
String name; 
int age;  

public UserVO(String name, int age){ 	
this.name = name; 	this.age = age; 
} 

public String getName() { 	
return name; 
} 

public void setName(String name) { 	
this.name = name; 
}

 public int getAge() { 	
 return age; 
 } 
 
 public void setAge(int age) { 	
 this.age = age; 
 } 
 }
 
```

 자바에서 Comparator 를 이용해서 Object 를 정렬하기 위해서는 Compartor 인터페이스를 impements 받고 compare 메서드를 Override합니다.

 ```java 
 public class ObjectSort implements Comparator { 	
	@Override 
	public int compare(UserVO o1, UserVO o2) { 	
		//o1 - o2 = ASC , o2 - o1 = DESC 	
		if(o1.age == o2.age){ 		
			return o2.name.compareTo(o1.name); 	
		} 	
		return o1.age - o2.age; 
	} 
}
 ```

 - 테스트를 위한 메인 클래스

 ```java 
 public class main { 	
	public static void main(String[] args) { 	     
		UserVO users[] = new UserVO[5];    
		users[0] = new UserVO("김기사",34);     
		users[1] = new UserVO("정학생",21);     
		users[2] = new UserVO("전주부",38);     
		users[3] = new UserVO("하중딩",16);     
		users[4] = new UserVO("김중딩",16);          
		ObjectSort sorter = new ObjectSort(); 	
		Arrays.sort(users,sorter); 	 	
		for(UserVO u : users){ 		
			System.out.println(u.getName() + "," + u.getAge()); 	
		}  
		System.out.println("--------"); 	 	 	
		List list = Arrays.asList(users);             
		Collections.sort(list,new Comparator(){ 		
			@Override 		
			public int compare(UserVO o1, UserVO o2) { 			
				// TODO Auto-generated method stub 			
				if(o1.age == o2.age){ 				
					return o2.name.compareTo(o1.name); 			
				} 			
				return o1.age - o2.age; 		
			}     
		});          
		
		for(UserVO u : list){     	
			System.out.println(u.getName() + "," + u.getAge());     
		} 
	} 
}
 ```



꼭 클래스를 생성하지않고 익명클래스로도 구현 가능합니다.
출력된 결과를 보실까요.

```java 
하중딩,16 
김중딩,16 
정학생,21 
김기사,34 
전주부,38 
--------
하중딩,16 
김중딩,16 
정학생,21 
김기사,34 
전주부,38 
```



위 예제는 [#Git-Comparator](https://github.com/antkdi/hyungeun/tree/master/ComparatorTest/src/myapp) 에 올려두었습니다. 

이상으로 포스팅을 마치겠습니다.