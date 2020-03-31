---
title: '[Spring] @NoArgsConstructor, @RequiredArgsConstructor, @AllArgsConstructor'
categories:
  - Programming
  - Spring
tags:
  - NoArgsConstructor
  - RequiredArgsConstructor
  - AllArgsConstructor
date: 2020-03-31 22:12:15
thumbnail:
---
lombok 라이브러리를 사용하면 <code>getter</code>, <code>setter</code> 등을 어노테이션으로 편하게 사용할 수 있다. 그 중에 생성자를 자동으로 생성해주는 어노테이션에 소개하고자 한다.

```java
@NoArgsConstructor
@RequiredArgsConstructor
@AllArgsConstructor
public class Student {
    @NotNull
    private Long studentId;
    private String name;
    private int age;
}
```

|구분|설명|
|------|---|
|@NoArgsConstructor|파라미터가 없는 기본 생성자를 생성<br>```Student std = new Student();```|
|@RequiredArgsConstructor|<code>final</code>이나 <code>@NonNull</code>인 필드 값만 파라미터로 받는 생성자<br>```Student std = new Student(1L);```|
|@AllArgsConstructor|모든 필드 값을 파라미터로 받는 생성자 생성<br>```Student std = new Student(1L, "홍길동", 20);```|
