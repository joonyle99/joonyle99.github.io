---
layout: single
title:  "Data Structure / Algorithm - HashMap (수정)"
categories:
  - DataStructure_Algorithm
---

---

### HashMap 중복 시 해결 방법 (체이닝 이외의 방법 제시)
---

![](../assets/images/data_hashFunc.png){: width="55%" height="55%"}

HashMap(해시 테이블)은, `Key`값을 해시 함수에 대입하여 나온 `Bucket Number`에 그 값을 저장하는 자료구조이다.

![](/assets/images/data_hashComplict.png){: width="55%" height="55%"}

이런 HashMap에는 문제점이 하나 있는데, 고유한 `Key`에 여러 `Value`가 저장될 수 있다는 점이다. 즉, 다른 `Key`를 가진 `Value`가 같은 `Buckkey Number`에 저장된다는 것이다.

이러한 문제를 해결하기 위한 방법인 Chaining과 Open Address에 대해 알아보자

1. Chaining



2. OpenAddress