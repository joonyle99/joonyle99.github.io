---
layout: single
title:  "Programming - 스택 메모리 가드"
categories:
  - Programming
---

---

### 디버그 모드와 릴리스 모드에서의 스택 메모리 가드

![](../../../../assets/images/2023-07-31-Programming-Stack%20Memory%20Guard/2024-07-13-17-53-46.png){: .align-center}
*Debug 모드에서 실행한 결과*

디버그 모드에서는 스택 메모리에 "메모리 가드"가 존재한다.  
즉, 두 지역 변수의 간격이 존재한다.

int의 자료형은 4바이트이기 때문에 연속된 지역변수를 사용하는 a와 b의 메모리 주소 차이는 4바이트가 되어야 정상이다.  
하지만 32바이트 차이가 나는 것을 볼 수 있다.

이는 Debug 모드에서 스택 메모리에 스택 메모리 가드를 두는데,  
이는 인접한 메모리 주소 간 침범을 방지하기 위함이다.

![](../../../../assets/images/2023-07-31-Programming-Stack%20Memory%20Guard/2024-07-13-17-54-36.png){: .align-center}
*Release 모드에서 실행한 결과*

릴리스 모드에서는 효율성을 위해 이 메모리 가드가 없다.  
따라서 위 사진과 같이 두 지역변수의 메모리 주소 차이는 4바이트가 나는 것을 볼 수 있다.

### 코드

```c++
#include <bits/stdc++.h>
using namespace std;

int main()
{
	// Break the ios for C and C++
	std::ios::sync_with_stdio(false);

	// Untie the streams that bind cin and cout (Output cout before cin's buffer is empty)
	std::cin.tie(nullptr);

#ifdef _DEBUG
	cout << "<디버그 모드>" << "\n\n";
#else
	cout << "<릴리즈 모드>" << "\n\n";
#endif

	int a = 10;
	int b = 20;

	std::cout << "변수 a의 주소: " << &a << '\n';
	std::cout << "변수 b의 주소: " << &b << "\n\n";

	// 주소 차이 계산
	ptrdiff_t diff = reinterpret_cast<char*>(&b) - reinterpret_cast<char*>(&a);

	std::cout << "두 변수 사이의 메모리 주소 차이: " << diff << " 바이트" << '\n';

	return 0;
}
```