---
layout: single
title:  "C++ - 포인터의 값복사 vs 참조복사"
categories:
  - Cpp
---

---

```c++
#include <iostream>

using namespace std;

class A
{

};

class B
{

};

void CallByValue(A* _pA, B* _pB)
{
	cout << "_pA의 주소값 : " << &_pA << endl;
	cout << "_pA의 값 : " << _pA << endl;

	cout << "_pB의 주소값 : " << &_pB << endl;
	cout << "_pB의 값 : " << _pB << endl;
}

void CallByReference(A*& _pA, B*& _pB)
{
	cout << "_pA의 주소값 : " << &_pA << endl;
	cout << "_pA의 값 : " << _pA << endl;

	cout << "_pB의 주소값 : " << &_pB << endl;
	cout << "_pB의 값 : " << _pB << endl;
}

int main()
{
	A* pA = new A;
	B* pB = new B;

	cout << "pA의 주소값 : " << &pA << endl;
	cout << "pA의 값 : " << pA << endl;

	cout << "pB의 주소값 : " << &pB << endl;
	cout << "pB의 값 : " << pB << endl;

	cout << "=======================================================" << endl;
	cout << "CallByValue() 함수 실행" << endl;
	cout << "=======================================================" << endl;

	CallByValue(pA, pB);

	cout << "=======================================================" << endl;
	cout << "CallByReference() 함수 실행" << endl;
	cout << "=======================================================" << endl;

	CallByReference(pA, pB);



	return 0;
}
```

![](../../../../assets/images/2023-07-06-Cpp-Call%20by%20Value%20&%20Reference/2024-04-28-18-48-45.png){: .align-center}

아무리 '포인터'라도 변수일 뿐이다.  
포인터 값을 값복사 하면 포인터에 접근해도 원본에 반영이 되지 않는다.

반면 포인터 변수를 Call By Reference하면 그 포인터를 그대로 가져온다.