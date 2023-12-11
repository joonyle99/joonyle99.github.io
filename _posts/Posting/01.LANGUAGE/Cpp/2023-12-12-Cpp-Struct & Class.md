---
layout: single
title:  "C++ - Struct vs Class"
categories:
  - Cpp
---

---

### C++에서의 Struct와 Class
---

C++에서 Struct와 Class는 거의 비슷한 기능을 한다.  
유일한 차이점은 Struct의 기본 접근 지정자(Access Modifier)가 `Public`이고 Class의 기본 접근 지정자는 `Private`이다.  
그럼에도 코딩 컨벤션에 따라, 이 둘의 용도 차이가 있을 수 있다. Struct는 간단한 데이터들의 모음인 POD(Plain Old Data)의 경우 사용하고, Class는 복잡한 데이터나 객체 지향 프로그래밍 요구 사항을 충족하기 위해 사용한다.  

```c++
struct mystruct
{
    // 데이터의 모음
    int data;
    int x;
    int y;
    /* 별 다른 로직을 구현하지 않는다! */
}
```

### C#에서의 Struct와 Class의 차이
---

C#에서 Struct와 Class는 유의미한 차이가 있다.  
마찬가지로 멤버 변수와 멤버 함수를 가질 수 있지만, Sturct는 `값 타입`이고 Class는 `참조 타입`이다.

| Struct | Class |
|:------:|:-----:|
|값 타입|참조 타입|
|스택에 할당|힙에 할당|
|상속 X|상속 O|

`값 타입`은 매개변수로 전달할 때 원본의 <u> 복사본 </u>을 보내어, 함수 내에서 원본에 영향을 주지 못한다. 또한 Struct 객체는 스택에 할당되기 때문에, 크기가 크면 스택 사용량 늘어나 비효율적이다.

반면 `참조 타입`은 매개변수로 전달할 때 원본을 참조할 수 있는 <u> 주소 </u>를 전달한다. 따라서 함수 내에서 원본의 수정도 가능하고, 힙 영역에 할당되기 때문에 Class의 크기가 크더라도 스택 영역의 사용량은 메모리 주소만큼만 증가한다. (64bit O/S - 16바이트)

C#에서의 Struct와 Class의 사용을 Unity로 살펴보자.

![](/assets/images/unity_struct.png) ![](../assets/images/unity_struct2.png)

`Vector2`와 `Quaternion`은 Struct로 만들어졌다. 비교적 간단한 데이터들의 모음이기 때문이다.

```c++
    Vector2 tempVec = Vector2(1, 1);
    Vector2 tempVec2 = new Vector2(1, 1); // 스택에 올라가는 Struct 객체도 new 키워드로 생성
```

하지만 C#에서는 이러한 구조체를 생성할 때 `new`라는 키워드를 사용하는데, new는 C++에서 힙 메모리에 동적 할당하는 키워드이지만 스택에 올라가는 구조체의 객체 생성에도 사용된다.

![](/assets/images/unity_class.png) ![](../assets/images/unity_class2.png)

`Rigidbody2D`나 `Collider2D`와 같이 복잡하고 큰 데이터들의 집합은 Class를 사용한다.
