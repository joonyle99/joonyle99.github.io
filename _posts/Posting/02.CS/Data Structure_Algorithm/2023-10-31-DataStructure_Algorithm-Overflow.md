---
layout: single
title:  "Data Structure / Algorithm - 자료형과 오버플로우"
categories:
  - DataStructure_Algorithm
---

---

### C++ 자료형의 크기
---

백준 문제를 풀다가 로직도 전부 맞고 예외도 없는 것 같은데 계속 실패가 떠서 골머리를 앓고 있었다.  
그러다 다른 사람이 푼 코드를 보았는데 자료형이 나랑 달랐다. 그 순간 깨달았다.

[C++ 데이터 형식 범위 (MSDN)](https://learn.microsoft.com/ko-kr/cpp/cpp/data-type-ranges?view=msvc-170)

C언어를 배울 때 초반에 나오는 내용 중에 자료형에 대한 내용이 있다. 자료형에 대한 내용 중 '자료형의 범위'에 대한 내용을 망각하고 있었다.

### 내가 겪었던 문제
---

[백준 2012번 - 등수 매기기](https://www.acmicpc.net/problem/2012)

이 문제는 <u> 불만도의 합 </u>을 구하는 프로그램을 짜는 문제인데, 불만도의 자료형 선언을 'int'로 선언했다. 하지만 입력 횟수 N이 500,000인 조건 속에서 불만도에 들어갈 수 있는 값이 최대 `50만 x 50만 = 2500억`이었다.

int 자료형의 경우, `+- 21억`까지의 정수만 담을 수 있기 때문에 <u> 오버플로우(Overflow) </u>가 발생한다.

문제를 인지하고 'long long' 자료형을 사용해 해결했다. long long은 +- `920경`의 정수까지 담을 수 있어서 불만도의 합 2500억의 수는 충분히 담을 수 있었다.