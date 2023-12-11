---
layout: single
title:  "C++ - push_back vs emplace_back (feat.vector)"
categories:
  - Cpp
---

---

<!--
### push_back()
---

[push_back - MSDN](https://learn.microsoft.com/ko-kr/cpp/standard-library/vector-class?view=msvc-170#push_back)

[emplace_back - MSDN](https://learn.microsoft.com/ko-kr/cpp/standard-library/vector-class?view=msvc-170#emplace_back)

std::vector<>의 push_back()와 emplace_back()은 둘 다 벡터 끝에 요소를 추가하는 함수이다.

비슷한 동작을 하지만, 여기에는 중요한 차이점이 있다.

```c++
#include <iostream>
#include <vector>

using namespace std;

class Student
{
public:
	string name;
	int age;

public:
	Student()
	{
		std::cout << "인자 없는 기본 생성자" << std::endl;
	}
	Student(string n, int a)
	{
		std::cout << "인자 있는 기본 생성자" << std::endl;
	}
	Student(const Student& s)
	{
		std::cout << "복사 생성자" << std::endl;
	}
	/*
	Student(const Student&& s)
	{
		std::cout << "이동 생성자" << std::endl;
	}
	*/
	~Student()
	{
		std::cout << "소멸자" << std::endl;
	}
};

int main()
{
	std::vector<Student> myVector;
	myVector.reserve(2);

	myVector.push_back(Student("페이커", 27));

	cout << "======================================================" << endl;

	myVector.emplace_back("데프트", 27);

	cout << "======================================================" << endl;

	return 0;
}
```

![](/assets/images/cpp_push_back.png)

push_back()을 한 경우

1. 임시객체 생성자가 호출되며, 스택 영역에 할당된다.
2. 복사 생성자 or 이동 생성자를 톻애 만들어져 있는 임시객체를 본뜬 또 하나의 임시 객체가 만들어진다.
3. 생성된 요소를 벡터 컨테이너 끝에 추가한다.
4. 1번의 임시 객체가 소멸된다.

-->