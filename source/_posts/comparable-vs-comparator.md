---
title: Comparable vs Comparator
categories:
  - Programming
  - Java
tags:
  - comparable
  - comparator
date: 2020-03-04 22:45:23
thumbnail:
---
### 1.개요
자바 객체를 비교하기 위해서 <code>Comparable</code> 혹은 <code>Comparator</code>라는 인터페이스를 사용하게 된다. 두 인터페이스의 차이점을 예제로 살펴보도록 하자.

### 2.예제로 살펴보기

Player 클래스 만들기
```java
public class Player {
    private int ranking;
    private String name;
    private int age;

    // 생성자 및 getter, setter
}
```
메인 메소드에서 footballTeam을 만들고 정렬을 시도하자.
```java
public static void main(String[] args) {
    List<Player> footballTeam = new ArrayList<>();
    Player player1 = new Player(59, "John", 20);
    Player player2 = new Player(67, "Roger", 22);
    Player player3 = new Player(45, "Steven", 24);

    footballTeam.add(player1);
    footballTeam.add(player2);
    footballTeam.add(player3);

    System.out.println("Before Sorting : " + footballTeam);
    Collections.sort(footballTeam);
    System.out.println("After Sorting : " + footballTeam);
}
```
아래와 같은 에러가 발생한다. 개인적인 생각으로는 footballTeam을 정렬하고 싶지만, 정렬해야하는 조건을 별도로 지정해주지 않았기 때문에 ranking, name, age 중 어떤 조건으로 정렬해야하는지 알지 못하기 때문에 발생하는 에러이지 않을까하는 생각이 든다.
```java
Error:(17, 20) java: no suitable method found for sort(java.util.List<Player>)
    method java.util.Collections.<T>sort(java.util.List<T>) is not applicable
      (inference variable T has incompatible bounds
        equality constraints: Player
        upper bounds: java.lang.Comparable<? super T>)
    method java.util.Collections.<T>sort(java.util.List<T>,java.util.Comparator<? super T>) is not applicable
      (cannot infer type-variable(s) T
        (actual and formal argument lists differ in length))
```

### 3.Comparable
<code>Comparable</code>는 객체 간의 일반적인 정렬이 필요할 때, <code>Comparable</code> 인터페이스를 확장해서 정렬의 기준을 정의하는 <code>compareTo()</code> 메서드를 오버라이딩하여 구현한다. (객체 클래스에 확장하여 사용)
아래 코드에서는 ranking 기준으로 정렬하는 코드이다.
```java
public class Player implements Comparable<Player>{
    private int ranking;
    private String name;
    private int age;

    // 생성자 및 getter, setter, toString

    @Override
    public int compareTo(Player otherPlayer) {
        return (this.getRanking() - otherPlayer.getRanking());
    }
}
```
<code>compareTo()</code> 메소드는 구현된 객체와 매개변수로 넘어온 객체를 비교하며, 비교에 따른 결과 값을 return 하게 된다. 객체와 매개변수 객체를 비교 하였을 때 작으면 음수, 같으면 0, 크면 양수를 반환한다.

main 함수를 다시 실행하면 아래와 같이 ranking을 기준으로 정렬된 결과가 출력된다.
```Java
Before Sorting : [Player{ranking=59, name='John', age=20}, Player{ranking=67, name='Roger', age=22}, Player{ranking=45, name='Steven', age=24}]
After Sorting : [Player{ranking=45, name='Steven', age=24}, Player{ranking=59, name='John', age=20}, Player{ranking=67, name='Roger', age=22}]
```
### 4.Comparator
<code>Comparator</code>는 객체 간의 특정한 정렬이 필요할 때, <code>Comparator</code> 인터페이스를 확장해서 특정 기준을 정의하는 <code>compare()</code> 메소드를 오버라이딩하여 구현한다.

아래는 ranking과 age로 정렬하는 별도의 구현 클래스이다.
```java
public class PlayerRankingComparator implements Comparator<Player> {
    @Override
    public int compare(Player firstPlayer, Player secondPlayer) {
       return (firstPlayer.getRanking() - secondPlayer.getRanking());
    }
}
```
```java
public class PlayerAgeComparator implements Comparator<Player> {
    @Override
    public int compare(Player firstPlayer, Player secondPlayer) {
       return (firstPlayer.getAge() - secondPlayer.getAge());
    }
}
```

아래와 같이 정렬 기준 클래스를 호출하여 정렬에 이용할 수 있다.
```java
PlayerRankingComparator playerComparator = new PlayerRankingComparator();
Collections.sort(footballTeam, playerComparator);

PlayerAgeComparator playerComparator = new PlayerAgeComparator();
Collections.sort(footballTeam, playerComparator);
```

### 5.다중 조건 정렬
위에서는 한가지 정렬 조건만을 이용해서 정렬을 했었다. 여러 개의 정렬 조건을 이용하고 싶다면, 자바8 이상에서는 lambda expression을 사용하면 쉽게 다중 조건 정렬을 할 수 있다.
아래 코드는 ranking 오름차순 정렬 후 age로 오름차순 정렬하는 코드이다.
```java
footballTeam.sort(Comparator.comparing(Player::getRanking).thenComparing(Player::getAge));
```

### 6.정리
Primitive Type나 Reference Type은 기본적으로 Java에서 제공해주는 Object의 경우는 기본적으로 <code>Comparable</code> 인터페이스를 구현하고 있어서 <code>Arrays.sort</code> 같은 함수를 사용하면 자동으로 자연적인 정렬(오름차순)이 가능하다. 하지만 별도로 만든 객체를 sort 메소드를 통해서 정렬하거나, 혹은 특정한 조건을 기반으로 정렬하는 경우는 반드시 <code>Comparable</code> 인터페이스를 구현하여야 직접 정렬 기준을 구현하여야 한다.
<code>Comparator</code>은 Reference Type을 내가 원하는대로 정렬하고 싶을때나 기존에 구현된 <code>Comparable</code>의 정렬과는 다른 결과를 얻고 싶을 때 사용한다.


### 참고 사이트
https://www.baeldung.com/java-comparator-comparable
https://dev-daddy.tistory.com/23
https://jeong-pro.tistory.com/173
https://javaplant.tistory.com/15
