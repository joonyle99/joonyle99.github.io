---
layout: single
title:  "Algorithm - 그리디 알고리즘"
categories:
  - algorithm
---

---

그리디(Greedy) 알고리즘은 이름에서 알 수 있듯 탐욕적인 알고리즘이다.

이 알고리즘은 항상 최선의 결과를 도출하지는 않지만, 어느 정도 <u> 최선에 근사한 값을 빠르게 구할 수 있다는 장점 </u> 이 있다. 또한 <u> 특정한 상황 </u>에서는 그리디 알고리즘이 최적의 해를 보장할 수 있기도 하다.

그리디 알고리즘의 대표적인 예제는 거스름돈 문제이다.

거스름돈은 가장 적은 화폐로 줘야 하는데 "무조건 더 큰 화폐 단위부터 거슬러 준다"라는 알고리즘만 지키면 최적의 해를 보장할 수 있다. 문제의 조건 중 <u> 무시할 수 있는 부분 </u>이 생기며, <u> 복잡한 조건을 간단하게 </u> 만들 수 있는 알고리즘이다.

> 처음에는 문제를 어떻게 풀지 몰라 고민하다가, 이렇게 풀면 이러한 조건은 무시되는구나! 하고 깨달음이 오는 문제가 그리디 알고리즘 문제이다.

코딩테스트 문제에서 위의 예시와 같이 <u> 극단적으로 접근 </u> 하면 그리디 알고리즘을 고려해 볼 수 있다. 이러한 성질로 그리디 알고리즘은 <u> 정렬(Sort) </u>과 함께 사용되는 경우가 많다.

그리디 알고리즘은 최적의 해를 보장하는 경우도 있으나, 그렇지 못한 경우가 훨씬 더 많다. 그러한 경우에는 <u> BFS, DFS, DP </u> 기법과 같은 최적의 해를 찾기위한 알고리즘을 사용해야 한다.

---

[그리디 알고리즘 예제 - ATM](https://www.acmicpc.net/problem/11399)

```c++

#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main()
{
	ios::sync_with_stdio(false);
	cin.tie(nullptr);

	int N; cin >> N;
	int partialSum = 0;
	int res = 0;

	std::vector<int> timeVec(N + 1);

	for (int i = 1; i <= N; ++i)
		cin >> timeVec[i];

	// atm 1대에 n명이 줄을 섬
	// n명은 1번부터 n번까지 번호가 매겨짐
	// ** 줄을 서는 순서에 따라 인출 시간의 합이 달라짐 **
	// 3 1 4 3 2인 경우 = 39분
	// 1 2 3 3 4인 경우 = 32분
	// 돈을 인출하는 데 필요한 시간의 합의 최솟값 구하기

	// 인출하는 시간이 적은 사람부터 돈을 인출해야 한다.
	std::sort(timeVec.begin(), timeVec.end());

	for (int i = 1; i <= N; ++i)
	{
                partialSum += timeVec[i];
                res += partialSum;
	}

	cout << res << endl;

	return 0;
}

```

#### 위 문제를 그리디 알고리즘으로 풀어야 하는 이유

> "무조건 인출 시간이 적게 걸리는 사람이 먼저 돈을 인출"이라는 조건이 만족하여야 최적의 해를 구할 수 있는데, 이처럼 그리디 알고리즘은 '<u> 문제를 단순하게 </u>' 만드는 과정이 가장 핵심이다.

[그리디 알고리즘 문제 리스트](https://www.acmicpc.net/workbook/view/17254)