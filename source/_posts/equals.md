---
title: '== vs equals()'
categories:
  - Programming
  - Java
tags:
  - equals()
  - ==
  - == vs equals()
date: 2020-03-06 21:13:40
thumbnail:
---
Java에서 <code>equals()</code>와 <code>==</code>는 객체 혹은 내용을 비교할 때 사용하곤 한다.
이 둘은 어떤 차이점이 있을까?

### == : 동일성 비교
+ Primitive Type(int, float, double, long 등)일 경우에는 값이 같은 지 확인
+ 그 외에 객체나 Reference Type일 경우에는 주소가 같은지 확인

```java
//리터럴 생성
String str1 = "hi";
String str2 = "hi";
System.out.println(str1 == str2);   //true
//new를 이용한 생성
String str3 = new String("hi");
String str4 = new String("hi");
System.out.println(str3 == str4);   //false
System.out.println(str1 == str3);   //false
```
String의 경우는 조금 특별한데, 리터럴 생성을 하면 String Constant Pool 이라는 영역에 할당되고, new 를 이용한 생성을 하면 Heap에 할당된다.
리터럴로 생성한 문자가 같을 경우 메모리 상에서 같은 주소를 바라보게 되고,
new로 생성할 경우 생성 할 때마다 새로운 인스턴스를 생성하여 각각 다른 주소 공간에 위치하게 된다.
그렇기 때문에
+ <code>str1 == str2</code> : 리터럴로 생성되어 같은 주소를 바라보고 있기 때문에 <strong>true</strong>
+ <code>str3 == str4</code> : new로 생성되어 각각 다른 주소에 생성되기 때문에 <strong>false</strong>
+ <code>str1 == str3</code> : 리터럴과 new로 생성되어 서로 다른 주소에 위치하기 때문에 <strong>false</strong>
결론적으로 <code>==</code> 두 객체가 같은 주소를 가르킬 때 true를 반환한다.

### equals() : 동등성 비교
+ Primitive Type(int, float, double, long 등)일 경우에는 값이 같은 지 확인
+ 그 외 객체나 Reference Type일 경우에는 주소가 같은지 확인
+ <code>==</code>과 유사하지만 다른 점은 완전히 같은 객체(같은 주소)를 가리키지 않아도 <code>equals()</code> 메서드 오버라이딩을 통해서 true로 만들 수 있다.

```java
String str1 = "hi";
String str2 = "hi";
System.out.println(str1.equals(str2));   //true
String str3 = new String("hi");
String str4 = new String("hi");
System.out.println(str3.equals(str4));   //true
System.out.println(str1.equals(str3));   //true
```
String에는 이미 <code>equals()</code> 메소드가 오버라이딩 되어 있는 상태이기 때문에 주소값이 다르더라도 내용이 같다면 true를 반환하고 있다.

```java
Student s1 = new Student("1");
Student s2 = new Student("1");
System.out.println(s1.equals(s2));  //false
System.out.println(s1.hashCode());  //1634198
System.out.println(s2.hashCode());  //110456297
```
Student 객체를 만든 뒤 동일한 id 값을 부여한 후 비교하면 주소 값이 다르기 때문에 false를 반환한다. <strong>(hash 값도 다르다.)</strong> true로 만들기 위해서는 <code>equals()</code>와 <code>hashcode()</code> 메소드를 오버라이딩 해준다. IDE에서 자동으로 생성해주나, 조건에 따라 수정하여 사용하도록 한다.

```java
public class Student {
    private String id;

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof Student)) return false;
        Student student = (Student) o;
        return Objects.equals(id, student.id);
    }

    @Override
    public int hashCode() {
        return Objects.hash(id);
    }

    // 생성자, getter, setter
}
```
이후 코드를 재실행하면 결과 값은 true가 나오고 두 객체 모두 동일한 hash 값을 가지게 된다.
```java
Student s1 = new Student("1");
Student s2 = new Student("1");
System.out.println(s1.equals(s2));  //true
System.out.println(s1.hashCode());  //80
System.out.println(s2.hashCode());  //80
```
<code>hashcode()</code> 왜 오버라이딩하는지 궁금해 할 것이다. 우리도 모르는 사이에 hash값을 쓰는 곳이 많다. 예를 들면 <code>hashSet</code>이나 <code>hashmap</code> 같은 컬렉션 들이다. <code>equals()</code>만 오버라이딩하고 <code>hashcode()</code>는 하지 않았을 경우엔 같은 객체라 할지라도 <code>hashcode()</code> 값이 다르게 나올 수 있는데, 이때는 다른 객체로 인식하기 때문이다.
```java
Set<Student> stu = new HashSet<>();
stu.add(s1);
stu.add(s2);
System.out.println(stu.size());
// hashcode() 오버라이딩 한 경우 : 1
// hashcode() 오버라이딩 하지 않은 경우 : 2
```
<code>hashcode()</code>를 오버라이딩 한 경우 hashcode 값이 같으므로 같은 객체로 인식하여 1이 나온다. (Set은 중복 허용 X) 반면 오버라이딩을 하지 않았을 경우 hashcode 값이 다르므로 다른 객체로 인식하기 때문에 size 값은 2가 된다.
그러므로 <code>equals()</code>를 오버라이딩 할 경우 반드시 <code>hashcode()</code>도 오버라이딩 하여 혹시나 모를 불상사를 예방하는 것이 좋다.

### 정리
+ <code>==</code>는 객체 간의 <strong>동일성</strong>을 판단하기 위해 사용한다. <strong>동일성</strong>은 두 개의 객체가 완전히 같을 경우를 의미 한다.
+ <code>equals()</code>는 객체 간의 <strong>동등성</strong>을 판단하기 위해 사용한다. <strong>동등성</strong>은 두 개의 객체가 같은 내용을 가질 경우를 의미 한다.
+ <code>==</code> 연산자는 주소 값의 비교, <code>equals()</code>는 내용을 비교 한다.
+ <code>equals()</code>를 오버라이딩 할 경우 반드시 <code>hashcode()</code>도 오버라이딩 하여야 한다.
+ <code>equals()</code>로 true가 나온 경우 <code>hashcode()</code>의 값이 동일 해야 한다. 그러나 <code>hashcode()</code>가 동일하다고 해서 반드시 같은 객체는 아니다.

### 참고 사이트
https://jeong-pro.tistory.com/172
https://joont.tistory.com/143
https://madplay.github.io/post/java-string-literal-vs-string-object
