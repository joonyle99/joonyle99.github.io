---
layout: single
title:  "Data Structure / Algorithm - 스택의 활용"
categories:
  - DataStructure_Algorithm
---

---

3가지 문제를 통해 스택의 활용에 대해 알아볼 것이다.

[탑 - 백준 2493번 문제](https://www.acmicpc.net/problem/2493)  
[옥상 정원 꾸미기 - 백준 6198번 문제](https://www.acmicpc.net/problem/6198)  
[오큰수 - 백준 17298번 문제](https://www.acmicpc.net/problem/17298)  

### 탑 - 백준 2493번 문제

---

```c++
#include <bits/stdc++.h>
using namespace std;

int N, h;
stack<pair<int, int>> myStack;

int main()
{
	// Break the ios for C and C++
	std::ios::sync_with_stdio(false);

	// Untie the streams that bind cin and cout (Output cout before cin's buffer is empty)
	std::cin.tie(nullptr);

	// Title : 탑

	// 1. 높이가 서로 다른 탑을 수평 직선의 왼쪽부터 오른쪽 방향으로 차례로 세운다
	// 2. 모든 탑의 레이저 송신기는 레이저 신호를 지표면과 평행하게 수평 직선의 왼쪽 방향으로 발사한다
	// 3. 하나의 탑에서 발사된 레이저 신호는 가장 먼저 만나는 단 하나의 탑에서만 수신이 가능하다

	// 각 탑이 레이저를 발사할 때, 자신보다 왼쪽에 있는 탑들 중에서 자신보다 높이가 높은 가장 가까운 탑을 찾아야 하는데,
	// 현재 탑의 높이보다 낮은 탑들은 더 이상 레이저 신호를 수신할 가능성이 없으므로, 스택에서 제거할 수 있다
	// 이는 스택의 LIFO 특성을 활용하여, 현재 탑보다 높이가 높은 탑들만을 스택에 남기면서 문제를 해결할 수 있음을 의미한다

	// 예제: 6 9 5 7 4
	//
	// 1. 6을 입력받는다. 현재 스택은 비어있으므로 0을 출력하고, <탑의 번호, 탑의 높이>를 pair로 묶어 스택에 넣는다
	// 2. 9를 입력받는다. 현재 스택에는 6이 들어있는데, 6은 9보다 낮고 9에 의해 가려져 레이저를 수신할 가능성이 없다. 따라서 pop한다. 그러고는 스택을 체크하여 비어있다면 0을 출력한다
	// 3. 5를 입력받는다. 현재 스택에는 9가 들어있는데, 9는 5보다 높으므로 5의 레이저를 수신한다. 따라서 9의 높이를 가진 탑의 번호를 출력한다.
	// 4. 7을 입력받는다. 현재 스택에는 9, 5가 들어있는데, 5는 7보다 낮으므로 pop한다. 9에서 레이저를 수신하기 때문에 9의 높이를 가진 탑의 번호를 출력한다
	// 5. 4를 입력받는다. 현재 스택에는 9, 7이 들어있는데, 7은 4보다 높으므로 4의 레이저를 수신한다. 따라서 7의 높이를 가진 탑의 번호를 출력한다.

	cin >> N;
	for (int i = 0; i < N; ++i)
	{
		cin >> h;
		// 현재 높이보다 낮다면 반복해서 제거한다
		while (!myStack.empty() && myStack.top().second < h) myStack.pop();
		// 현재 높이보다 높다면 바로 출력한다
		if (!myStack.empty() && myStack.top().second >= h) cout << myStack.top().first << ' ';
		// 만약 스택이 비어있다면 0을 출력한다
		if (myStack.empty()) cout << 0 << ' ';
        // 스택에 현재 탑의 정보를 넣는다
		myStack.emplace(i + 1, h);
	}

	return 0;
}
```