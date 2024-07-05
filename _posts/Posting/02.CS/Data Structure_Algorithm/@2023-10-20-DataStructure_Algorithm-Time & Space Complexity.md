---
layout: single
title:  "Data Structure / Algorithm - 시간 & 공간 복잡도"
categories:
  - DataStructure_Algorithm
---

---

### 코딩 테스트의 시간제한
---
코딩 테스트 문제의 시간제한은 <u> 1초 ~ 3초 </u> 정도.  
일반적으로 1초에 실행할 수 있는 최대 연산 횟수는 약 <u> 1억 번 </u>.  

-> '입력 횟수 N'과 주어진 '시간제한'을 통해 적절한 시간 복잡도의 알고리즘을 선택한다.

> <u> 입력횟수 N의 최댓값 10만, 시간제한 1초 </u>인 경우 O(N^2)인 알고리즘 연산은 '10만 * 10만 = 100억'이므로 이 알고리즘은 사용할 수 없다.

### 1초당 최대 연산 횟수
---
* O(N) : 약 1억 번
* O(N^2) : 약 1만 번
* O(N^3) : 약 500번
* O(2^N) : 약 20번
* O(N!) : 11번

```c++
#include <iostream>

using namespace std;

int main()
{
	int a, b;
	cin >> a >> b;
	cout << a + b;

	return 0;
}
```

> 이 예제 코드에서는 '>>', '<<', '+'의 총 4번 연산이 발생하는데, 1초에 약 1억 번의 연산이 가능하다고 했으니, 위 코드의 연산은 0.00000001초도 안 걸릴 것이다.

### 제한 시간이 1초일 경우, N의 범위에 따른 시간 복잡도 선택
---

![](https://blog.kakaocdn.net/dn/nWbDr/btqYkaZqOuE/xFOyFSEYKbp2Wlz0xQ7lSk/img.png){: .align-center}
*N의 크기에 따른 허용 시간복잡도*

* N의 범위가 500: 시간 복잡도가 O(N^3) 이하인 알고리즘을 설계
* N의 범위가 2,000: 시간 복잡도가 O(N^2) 이하인 알고리즘을 설계
* N의 범위가 100,000: 시간 복잡도가 O(NlogN) 이하인 알고리즘을 설계
* N의 범위가 10,000,000: 시간 복잡도가 O(N) 이하인 알고리즘을 설계
* N의 범위가 10,000,000,000: 시간 복잡도가 O(logN) 이하인 알고리즘을 설계

### 공간 복잡도(Space Complexity)
---

일반적으로 메모리 사용량 기준은 'MB' 단위로 표기한다.  
int를 기준으로 배열 크기에 따른 메모리 사용량은 다음과 같다.

* int a[1000] : 4byte * 1000 = 4,000 byte = 4KB
* int a[1000000] : 4byte * 1000000 = 4,000,000 byte = 4MB
* int a[2000][2000] : 4byte * 2000 * 2000 = 16,000,000 byte = 16MB

> 자료형 int의 크기는 4byte

![](/assets/images/algorithm_stackSize.png){: .align-center}
*Visual Studio 2022에서 Stack의 용량은 1MB이 기본 설정*

프로젝트에서 int형 배열 하나만 사용한다고 가정한 경우, 요소의 최대 개수 (약 26만개)

> int[262144] => 4 * 262144 = 1048576 Byte (약 100만 byte == 1 MB)

* 1 MB - 1048576 Byte
* 2 MB - 2097152 Byte
* 4 MB - 4194304 Byte

데이터 영역의 크기가 스택 영역의 크기보다 크기 때문에, 큰 배열을 스택 메모리에 선언하는 경우 메모리 초과가 발생할 수 있다.