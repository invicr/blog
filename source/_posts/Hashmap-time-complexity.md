---
title: HashMap time complexity
categories:
  - Programming
  - Java
tags:
  - hashmap
date: 2020-03-03 19:16:42
thumbnail:
---
### HashMap이란?
Key, Value를 하나의 데이터(Entry)로 저장하는 Map의 구현제 중 하나이다.

HashMap의 특징은 아래와 같다.
+ null key와 null value를 모두 허용한다.
+ 내부적으로 데이터에 접근할 때 동기화를 보장하지 않는다.
+ 데이터의 순서를 보장하지 않는다.
+ 중복된 key값을 허용하진 않지만, 중복된 값은 갖을 수 있다.

### HashMap 구조
HashMap은 key를 hash 함수에 넣어 나온 value를 bucket의 특정 인덱스에 저장한다. H(k) = k mod M 의 계산식을 통해서 버킷의 크기보다 작은 인덱스에 저장을 하도록 한다. (1/M확률로 충돌이 발생하게 된다.)

![](https://codenuclear.com/wp-content/uploads/2017/11/bucket_entries.jpg)
<center>이미지 출처 : https://codenuclear.com/difference-between-hashmap-hashtable/</center>

### Time complexity
key는 고유하며 해시 함수의 결과로 나온 해시에 매칭되는 value를 찾으면 되기 때문에 시간 복잡도는 O(1)이다.
최악의 경우는 해시 충돌로 인해 모든 bucket의 value들을 찾아 봐야 하는 경우도 있기 때문에 시간 복잡도는 O(n)이다.
