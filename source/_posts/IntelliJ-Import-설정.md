---
title: '[IntelliJ] Import  설정'
categories:
  - Programming
  - IntelliJ
tags:
  - IntelliJ
  - import
  - import.*없애기
date: 2020-03-03 19:37:02
thumbnail:
---
IntelliJ에서는 import 구문이 5개 이상 추가 되면 *로 표시가 된다.
```Java
import java.util.*;
```
*로 표시되면, 어떤 패키지 경로를 참조하고 있는지 알기 어렵기 때문에, 현재 사용중인 모든 패키지 경로를 표시해주는 것이 좋다.

설정을 변경하기 위해서는 아래 메뉴로 들어가서 설정을 변경한다.
> Preferences > Editor > Code Style > Java  

![](import.png)

빨간박스 안에 들어 있는 숫자를 <code>999</code>로 변경하고 적용하면 아래와 같이 import 구문이 변경된다. (IntelliJ에 Auto Import가 설정되어 있지 않으면, import 작업을 별도로 해주어야 한다.)

```java
import java.util.Collection;
import java.util.HashMap;
import java.util.Set;
```
