---
layout: single
title:  "C++ - 가상 함수 테이블 (1)"
categories:
  - Cpp
---

---

![](../../../../assets/images/2023-06-12-Cpp-Type%20&%20Name%20copy/2024-04-28-18-59-47.png){: .align-center}

클래스 A와 A를 상속받은 클래스 B가 있다.  
그리고 A에 Print()라는 함수가 있는 상황이다.  
A의 객체와 B의 객체를 만들어보자.

![](../../../../assets/images/2023-06-12-Cpp-Type%20&%20Name%20copy/2024-04-28-19-00-08.png){: .align-center}

객체 b는 A의 상속을 받을 것을 확인할 수 있는데,  
만약 여기서 클래스 A에 가상함수를 추가해보겠다.

![](../../../../assets/images/2023-06-12-Cpp-Type%20&%20Name%20copy/2024-04-28-19-00-20.png){: .align-center}

기존의 Print() 함수에 virtual 키워드를 추가하여 가상함수로 만들어주면

![](../../../../assets/images/2023-06-12-Cpp-Type%20&%20Name%20copy/2024-04-28-19-00-30.png){: .align-center}

a객체와 b객체에 __vfptr(virtual function pointer)가 생성된 것을 볼 수 있다.

__vfptr는 a와 b 각각에 생긴 서로 다른 "가상 함수 테이블"을 뜻하고,  
따라서 주소도 ae30, ae40으로 다른 것을 확인할 수 있다.

여기서 b객체에 Print() 함수를 오버라이딩(재정의) 하지 않았기 때문에,  
a와 b의 함수포인터가 같은 것도 확인할 수 있다. (0x00007ff791a7149c)

![](../../../../assets/images/2023-06-12-Cpp-Type%20&%20Name%20copy/2024-04-28-19-01-01.png){: .align-center}

만약 클래스 B에 Print() 함수를 오버라이드 한다면?

![](../../../../assets/images/2023-06-12-Cpp-Type%20&%20Name%20copy/2024-04-28-19-01-16.png){: .align-center}

위와 같이 void* 자료형의 함수포인터가 각각 다른 주소를 가리키고 있다. (b149c와 b14ab)

여기서 궁금할 수 있는 부분이 가상함수테이블 __vfptr이 같은 클래스의 객체라도 전부 다르게 생성될까? 라는 의문이 들 수 있다.  
한 번 테스트 해보자.

​![](../../../../assets/images/2023-06-12-Cpp-Type%20&%20Name%20copy/2024-04-28-19-01-47.png){: .align-center}

클래스 A의 객체 aa와 aaa를 추가로 생성했다.  
각각의 __vfptr을 확인해보면 모두 같은것을 확인할 수 있다.

즉 같은 클래스의 객체는 같은 가상 함수 테이블을 가진다고 할 수 있겠다.

자식 클래스에서는 가상 함수 테이블을 이용하여 RTTI (Run Time Type Information) 작업을 한다.  
런타임 중 객체의 타입이 확정되지 않은 경우 (ex. A a = new B())  
가상 테이블과 함수 포인터를 이용해 현재 타입에 맞는 함수를 실행한다.

객체지향 C++의 꽃 다형성 (Polymorphism)의 아름다움을 엿볼 수 있었다.
