---
layout: single
title:  "Data Structure / Algorithm - 스택의 활용"
categories:
  - DataStructure_Algorithm
---

---

### 스택을 활용하는 문제의 유형

---

[스택에 관한 이전 포스팅](https://joonyle99.github.io/datastructure_algorithm/DataStructure_Algorithm-Stack/)

1. 현재 내 값으로 인해 이전 값이 필요 없어질 때 (ex.높이 7에 의해 높이 3 건물은 가려지기 때문에 필요 없어짐)
2. 조건이 한 방향으로만 진행된다고 보장될 때 (Restricted Structure인 스택의 특성을 이용)

다음 3가지 문제를 통해 스택의 활용에 대해 알아볼 것이다.

* 탑 - [백준 2493번 문제](https://www.acmicpc.net/problem/2493)  
* 옥상 정원 꾸미기 - [백준 6198번 문제](https://www.acmicpc.net/problem/6198)  
* 오큰수 - [백준 17298번 문제](https://www.acmicpc.net/problem/17298)  

### 탑 - [백준 2493번 문제](https://www.acmicpc.net/problem/2493)

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

### 옥상 정원 꾸미기 - [백준 6198번 문제](https://www.acmicpc.net/problem/6198)

---

```c++
#include <bits/stdc++.h>
using namespace std;

int main()
{
	// Break the ios for C and C++
	std::ios::sync_with_stdio(false);

	// Untie the streams that bind cin and cout (Output cout before cin's buffer is empty)
	std::cin.tie(nullptr);

	// Title : 옥상 정원 꾸미기

	/// O(N^2)의 시간복잡도를 가진 방식
	// 입력 데이터 N이 최대 80,000이므로
	// 1초에 연산 6,400,000,000번이다 이는 64억번으로 한참 모자르다
	// 시간 초과 발생

	/*
	int heightArr[100001];

	int N; cin >> N;
	for (int i = 0; i < N; ++i)
	{
		cin >> heightArr[i];
	}

	// 64억까지 들어갈 수 있으므로 int가 아닌 long long을 사용한다
	long long count = 0;

	for (int origin = 0; origin < N; ++origin)
	{
		for (int target = origin + 1; target < N; ++target)
		{
			if (heightArr[origin] > heightArr[target])
				count++;
			else
				break;
		}
	}

	cout << count << '\n';
	*/

	/// monotone stack이란?
	// -> 스택 내부를 오름차순 또는 내림차순으로 유지 해주는 알고리즘
	// https ://justicehui.github.io/medium-algorithm/2019/01/01/monotoneStack/
	// -> 해당 알고리즘으로 O(N)의 시간 복잡도 정도로 줄여주는 강력한 테크닉
	// 한 건물 한건물을 추가할때마다 stack 에 자기보다 큰놈들만 냅두기

	stack<int> myStack;

	// 자료형 조심하자 최대로 64억까지 나올 수가 있는데 int가 되겠냐
	long long count = 0;

	int N; cin >> N;
	for (int i = 0; i < N; ++i)
	{
		int h; cin >> h;
		/// 핵심 로직 montone stack 사용
		// 높이가 자신보다 낮거나 같은 빌딩들을 빼낸다
		// cf) myStack.empty()를 먼저 썼기 때문에 empty()면 top()함수까지 가지 않고 while은 종료된다
		// 왼쪽에 자기보다 높이가 큰 빌딩밖에 남지 않는데,
		// 그러한 빌딩들은 자신을 내려다 볼 수 있다는 뜻이다
		// 여기서 포인트는 자신보다 낮은 높이의 빌딩을 제외하면서 자연스럽게 높이가 큰 것만 남기게 되면
		// ** 결국에 새롭게 들어오는 빌딩은 남은 모든 빌딩들에게 벤치마킹 될 수 있다는 것이다. **
		//
		// 스택에 남아있는 숫자(높이) 개수가 현재 나를 보고 있는 건물 수이다
		while (!myStack.empty() && myStack.top() <= h)
		{
			myStack.pop();
		}
		count += myStack.size();
		myStack.push(h);
	}

	cout << count << '\n';

	/// 스택을 활용해서 문제를 해결하는 이유
	
	// 뭔가 논리적인 비약이 크다. 근데 곱씹어 생각해 봤을 때, { 10, 3, 7, 4 } 구간에서 높이가 7인 건물은 3보다 같거나 크므로, 3은 스택에서 제거가 되며 7은 스택에 넣어진다. 그리고 스택에는 { 10, 7 } 만 남아있다.

	// ** 일반적으로 생각했을 때, 다음 높이가 4인 건물의 입장에서 보았을 때도, 높이가 3인 건물은 높이가 7인 건물에 의해 가려지므로, 3 건물은 4 건물에 당연히 보여지지 않는 건물이다. **

		// 그래서 요약하자면, 다음과 같은 상황에서 스택이 적절히 사용될 수 있다고 생각했다.

		// 현재 내 값으로 인해 이전 값이 필요 없어질 때(ex.높이 7에 의해 높이 3 건물은 필요 없어짐)
		// 조건이 한 방향으로만 진행된다고 보장될 때(스택의 특성 LIFO)

	return 0;
}
```

### 오큰수 - [백준 17298번 문제](https://www.acmicpc.net/problem/17298)

---

```c++
#include <bits/stdc++.h>
using namespace std;

int main()
{
	// Break the ios for C and C++
	std::ios::sync_with_stdio(false);

	// Untie the streams that bind cin and cout (Output cout before cin's buffer is empty)
	std::cin.tie(nullptr);

	// Title : 오큰수

	// 오큰수란?
	// 자기 오른쪽의 원소 중에서 자신보다 큰 수 중 가장 왼쪽에 있는 수

	stack<int> myStack;

	int N; cin >> N;
	vector<int> myVector(N);
	vector<int> result(1000001, -1);
	for (int i = 0; i < N; ++i)
	{
		// 3 5 2 7
		int v; cin >> v;
		myVector[i] = v;
		while (!myStack.empty() && myVector[myStack.top()] < v)
		{
			result[myStack.top()] = v;
			myStack.pop();
		}
		myStack.push(i);
	}
	for (int i = 0; i < N; ++i)
	{
		cout << result[i] << ' ';
	}

	return 0;
}
```