---
layout: single
title:  "Programming - 모듈러 연산의 분배법칙 (feat. Hashing)"
categories:
  - Programming
---

---

![](../../../../assets/images/2023-09-24-Programming-Moduler%20Operation/2024-04-28-19-09-44.png){: .align-center}

![](../../../../assets/images/2023-09-24-Programming-Moduler%20Operation/2024-04-28-19-09-51.png){: .align-center}

처음에는 왼쪽항 처럼 한번에 모듈러 연산을 실행해 주었는데,  
모듈러 연산의 분배법칙을 이용해서 다시 코딩해주니까 효율성(?) 측면에서 해결이 된 모습이다.

모듈러 연산의 분배법칙을 이용하면 큰 수 연산을 더 작은 수의 연산으로 줄일 수 있어서 여러 면에서 효율성이 증가한다.

```c++
#include <iostream>
#include <string>

using namespace std;

int main()
{
	ios::sync_with_stdio(false);
	cin.tie(nullptr);

	int L; cin >> L;
	string str; cin >> str;

	// Hashing Function
	// Sigma(ai * r^i) mod M

	long long result = 0;
	long long r = 1;

	// 문자열의 각 요소를 char로 읽어온다
	for (int i = 0; i < L; ++i)
	{
		constexpr int power = 31;

		/// Moduler 연산의 분배법칙을 사용해야 한다.
		
		// char는 정수형으로 표현이 되는데 -96을 해준다.
		int a = (static_cast<int>(str[i]) - 96) % 1234567891;

		if (i != 0)
			r *= power;

		r %= 1234567891;

		// Hashing 함수를 적용해 result에 저장한다.
		result += (a * r);
	}

	cout << result % 1234567891 << '\n';

	return 0;
}
```