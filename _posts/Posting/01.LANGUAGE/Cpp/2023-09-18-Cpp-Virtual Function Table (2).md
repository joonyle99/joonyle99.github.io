---
layout: single
title:  "C++ - 가상 함수 테이블 (2)"
categories:
  - Cpp
---

---

### 가상 함수 테이블

---

![](../../../../assets/images/2023-09-18-Cpp-Virtual%20Function%20Table%20(2)/2024-04-28-19-05-54.png){: .align-center}

p1, p2,  p3 모두 같은 가상 함수 테이블(__vfptr == Virtual Function Table Pointer)을 참조하고 있다.  
-> 0x00007ff7cebdcfd0

그리고 가상 함수 테이블에 등록된 [0] 인덱스의 가상 함수 Func()  
-> 0x00007ff693531505

가상 함수 테이블은 Class 종속이며, 객체 종속이 아니다.  
따라서 p1, p2, p3의 __vfptr은 모두 같다.

__vfptr은 해당 클래스에 가상 함수가 선언되면 생기는데 컴파일러가 클래스에 자동으로 추가해주는 멤버 변수이며 "컴파일 타임"에 생성된다.  
또한 이 가상 테이블을 참조하는 코드인 p1->Func();와 같이 가상 테이블을 참조해야 하는 경우 "런타임"이다.

![](../../../../assets/images/2023-09-18-Cpp-Virtual%20Function%20Table%20(2)/2024-04-28-19-06-01.png){: .align-center}

__vfptr은 객체마다 존재하는 "포인터 멤버변수"이며 서로 다른 메모리 주소를 가지지만,  
결국에 같은 클래스라면 __vfptr은 같은 메모리 주소를 가리킨다.

__vfptr이 가리키는 메모리 주소는 해당 클래스의 vtable 주소이다.

### 추가 궁금증

---

가상 함수가 선언되어 있지 않는 경우  
-> 1. 상속을 받아도 가상 함수 테이블이 생성되지 않는가?

![](../../../../assets/images/2023-09-18-Cpp-Virtual%20Function%20Table%20(2)/2024-04-28-19-06-18.png){: .align-center}

=> Yes, vfptr이 생성되지 않는다.

-> 2. 멤버 변수도 상속받지 못하나?

![](../../../../assets/images/2023-09-18-Cpp-Virtual%20Function%20Table%20(2)/2024-04-28-19-06-24.png){: .align-center}

=> 그건 아니다. student 객체에서 name과 age 멤버 변수를 상속받을 수 있다.