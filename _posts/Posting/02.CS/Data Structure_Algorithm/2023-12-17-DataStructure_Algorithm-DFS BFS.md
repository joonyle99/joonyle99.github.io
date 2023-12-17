---
layout: single
title:  "Data Structure / Algorithm - DFS와 BFS에 대해 더 알아보기"
categories:
  - DataStructure_Algorithm
---

---

### 백준 11725번 - 트리의 부모 찾기
---

[문제 링크](https://www.acmicpc.net/problem/11725)

문제는 다음과같이 간단하다.

![](/assets/images/dataAlgorithm_tree0.png){: .align-center}
*백준 11725번 - 트리의 부모 찾기*

1. 연결되어 있는 노드의 연결 정보를 준다.
2. 루트 노드는 1번 노드라고 정한다
3. 각 노드의 부모 노드를 출력한다

![](/assets/images/dataAlgorithm_tree1.png){: .align-center}
*노드의 연결 정보*

이 사진과 같이 노드의 연결 정보가 주어진다.  
노드의 연결 데이터를 다루기 위해 두 가지 방법을 고안할 수 있는데,

1. 인접 행렬
2. 인접 리스트

![](https://blog.kakaocdn.net/dn/An8Dn/btqIqmfKxBX/KwJNGT1yfPIYsdhAuAerP0/img.jpg)
*인접 행렬과 인접 리스트*

'인접 행렬'은 N x M의 2차원 배열로 서로의 연결 정보를 표현하는 방식이다.  
반면 '인접 리스트'는 인접한 노드에 대한 연결 정보만 표현하는 방식이다.

인접 행렬은 <u>인접하지 않은 노드에 대한 정보</u>도 가지고 있으므로 메모리 낭비가 크고,
인접리스트는 <u>인접한 노드에 대한 정보</u>에 대한 정보만 가지고 있으므로 불필요한 메모리 낭비를 방지한다.

이 문제 풀이에서 메모리 사용에 있어 더 효율적인 인접 리스트를 사용할 예정이다.  
정확히는 '인접 리스트 방식'을 사용할 것이다.

인접 리스트는 말 그대로 List<> 자료구조로 구현하지만,
나 같은 경우에는 c++의 vector<> STL을 사용해 동적 배열로 노드의 연결 정보를 표현할 것이다.

![](/assets/images/dataAlgorithm_tree2.png){: .align-center}
*그림으로 표현*

우선 문제 풀이에서 사용할 변수를 전역으로 선언한다. (스택 메모리는 데이터 영역보다 크기가 작아 메모리 초과가 발생할 수도 있기 때문에, 크기가 큰 배열은 전역으로 선언한다.)

```c++
// 노드의 개수
int N;

// 인접 리스트 방식으로 트리의 관계를 정의
std::vector<std::vector<int>> tree(100001);

// 노드의 정점에 대한 방문처리
bool visited[100001] = {};

// 부모 노드를 저장 (index : 자식 번호, value : 부모 번호)
std::vector<int> parentArr(100001);
```

이후, 인접 리스트 방식으로 연결 정보를 입력하고 완전 탐색을 시작한다.

```c++
	// 연결 정보가 주어진다.
	for (int i = 0; i < N - 1; ++i)
	{
		int from, to;
		cin >> from >> to;

		tree[from].push_back(to);
		tree[to].push_back(from);
	}

    /// 루트 노드에서부터 모든 노드를 탐색한다.

    // 1번 노드(루트 노드)부터 시작한다.
	// DFS(1);
	// BFS(1);
```

### DFS 방식
---

```c++
// DFS는 완전 탐색이므로 인접 리스트의 모든 노드 정보를 순회한다.
void DFS(int parentNode)
{
	// 노드는 "부모 노드"의 입장으로 진입한다.
	// 방문 처리
	visited[parentNode] = true;

	// 부모 노드의 모든 자식들을 순회하며, 자식들의 부모를 결정
	for (int i = 0; i < tree[parentNode].size(); ++i)
	{
		// 부모 노드에 연결된 "자식 노드"
		int childNode = tree[parentNode][i];

		// * 방문 정보를 검사하는 이유는 자식 노드만 가져오기 위함 *
		if (!visited[childNode])
		{
			// 해당 노드를 부모 노드로 하고 Recursive
			DFS(childNode);

			// 자식 노드에 부모 정보를 등록
			parentArr[childNode] = parentNode;
		}
	}
}
```

![](/assets/gif/algorithm_dfs.gif){: .align-center}
*DFS의 작동 방식*

### BFS 방식
---

```c++
// BFS는 완전 탐색이므로 인접 리스트의 모든 노드 정보를 순회한다.
void BFS(int rootNode)
{
	// 너비 우선 탐색을 위해 "Queue 자료구조"를 사용한다
	std::queue<int> myQueue;
	// 처음 진입 노드인 루트 노드를 방문 처리
	visited[rootNode] = true;
	// 루트 노드를 처음에 Queue에 담고 시작
	myQueue.push(rootNode);

	// Queue에 노드가 있으면 계속해서 반복
	while (!myQueue.empty())
	{
		// Queue의 가장 앞에 있는 요소, 즉 가장 먼저 들어온 노드를 꺼낸다. (부모 노드)
		int parentNode = myQueue.front();
		// 꺼낸 노드를 parentNode에 저장했기 때문에 Pop()한다.
		myQueue.pop();

		// 부모 노드의 모든 자식들을 순회하며, 자식들의 부모를 결정
		for (int i = 0; i < tree[parentNode].size(); ++i)
		{
			// 부모 노드에 연결된 "자식 노드"
			int childNode = tree[parentNode][i];

			// * 방문 정보를 검사하는 이유는 자식 노드만 가져오기 위함 *
			if (!visited[childNode])
			{
				// Queue에 자식 노드를 Push()해준다.
				myQueue.push(childNode);

				visited[childNode] = true;

				// 자식 노드에 부모 정보를 등록
				parentArr[childNode] = parentNode;
			}
		}
	}
}
```

![](/assets/gif/algorithm_bfs.gif){: .align-center}
*BFS의 작동 방식*

### 아쉬운 점
---

이 문제에서는 한 가지 아쉬운 점이 있다.

많은 경우에서 위와 같이 DFS와 BFS 중 어떤 것을 쓰더라도 크게 상관이 없는데,  
모든 노드를 방문해야만 하거나 탐색 방식이 중요하지 않은 경우가 있다.

위의 문제에서도 DFS와 BFS 모두 같은 성능을 보여준다는 것이다.

![](/assets/images/dataAlgorithm_dfs_bfs_diff.png){: .align-center}
*DFS와 BFS의 속도가 같은 경우*

### DFS와 BFS를 구분해야 하는 경우
---

하지만 분명 DFS가 압도적으로 편하거나 BFS가 편한 경우가 있다.
코딩 테스트처럼 '시간 제한', '메모리 제한'이 있는 경우, DFS와 BFS를 잘 골라서 풀어야 조건에 맞출 수 있다.

#### DFS

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FGlmWb%2FbtrnCPbhk8J%2Fin0bgtZGaCQyZI2l7RJYk0%2Fimg.png){: .align-center}{: width="70%" height="70%"}
*DFS의 동작 순서 2*

DFS는 한 루트를 우직하게 파고들며, <u>재귀적 특성</u>과 <u>백 트래킹</u>을 해야 하는 경우 많이 활용된다.  
또한 모든 경로를 하나하나 모두 탐색하는 '완전 탐색'의 상황을 선호한다.


#### BFS

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdaONN3%2FbtrnJBwlGfm%2FZSKtMi9cUjRRW7odtmI3Yk%2Fimg.png){: .align-center}{: width="70%" height="70%"}
*BFS의 동작 순서 2*

BFS는 '깊이'나 '거리'의 특징을 이용한 문제에서 활용되는데, 대표적으로 <u>최단 거리</u> 문제가 있다.