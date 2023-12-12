---
layout: single
title:  "Data Structure / Algorithm - 너비 우선 탐색 (BFS)"
categories:
  - DataStructure_Algorithm
---

### BFS
---

너비 우선 탐색(Breadth First Search)은 탐색을 할 때, 너비를 우선으로 탐색하는 탐색 알고리즘이다. '맹목적인 탐색'을 하고자 할 때 사용할 수 있는 탐색 기법이다.

너비 우선 탐색에서 <u> 사용되는 자료구조는 큐(Queue) </u>로, <u> 최단 경로 </u>를 찾아준다는 점에서 최단 길이를 보장해야 할 때 많이 사용한다. (아래 그림 참고)

너비 우선 탐색 알고리즘은 맨 처음에 시작 노드를 큐에 삽입하면서 시작한다. 또한 시작 노드를 방문했다는 <u> '방문 처리' </u>를 해준다.

이제 너비 우선 탐색은 다음과 같은 간단한 알고리즘에 따라서 작동한다.

1. 큐에서 하나의 노드를 꺼낸다.
2. 해당 노드에 연결된 노드 중 방문하지 않은 노드를 방문하고, 차례대로 큐에 삽입한다.

이 알고리즘을 계속해서 반복 수행하면서 모든 노드를 탐색하게 된다.

![](/assets/gif/algorithm_bfs.gif){: .align-center}
*너비 우선 탐색*

이러한 BFS는 너비를 우선으로 탐색한다는 '특성'이 중요한 것이고, 이를 활용해 다른 알고리즘에 적용한다는 것이 핵심이다. 자체로는 큰 의미가 없다.

또한 이 알고리즘은 Brute Force 알고리즘과 마찬가지로 탐색의 순서만 다를 뿐 모든 노드를 탐색한다는 점에서 <u> 완전 탐색 알고리즘 </u>이다.

[BFS / DFS 알고리즘 문제 리스트](https://www.acmicpc.net/workbook/view/17403)

### BFS 코드 스니펫
---

#### BFS에서 사용되는 변수 선언
```c++
// M : 가로 길이
// N : 세로 길이
// K : 입력 횟수
int M, N, K;

// 지도
int map[MAX_Y][MAX_X] = {};

// 방문 정보
bool visited[MAX_Y][MAX_X] = {};

// 상 하 좌 우
int dx[4] = { 0,0,-1,1 };
int dy[4] = { -1,1,0,0 };
```

#### BFS 기본 세팅
```c++
cin >> M >> N >> K;

int outterCount = 0;

// 지도 세팅
while (K-- > 0)
{
    int X, Y;
    cin >> X >> Y;

    map[Y][X] = 1;
}

// 모든 지도를 돌면서 BFS 탐색
for (int i = 0; i < N; ++i)
{
    for (int j = 0; j < M; ++j)
    {
        // 방문 정보와 지도 정보를 가지고 탐색 결정
        if (!visited[i][j] && map[i][j] == 1)
        {
            BFS(i, j);
            outterCount++;
        }
    }
}

cout << outterCount << '\n';
```

#### BFS 메인 로직
```c++
int BFS(int startY, int startX)
{
	std::queue<std::pair<int, int>> myQueue;
	myQueue.emplace(startX, startY);
	visited[startY][startX] = true;
	int innerCount = 1;

	// 큐에 아무것도 없을 때까지
	while (!myQueue.empty())
	{
		// ** 큐에서 하나의 노드를 꺼낸다 **
		auto front = myQueue.front();
		myQueue.pop();

		// [상 하 좌 우] 탐색
		for (int i = 0; i < 4; ++i)
		{
			// 다음 위치
			std::pair<int, int> nextPos(front.first + dx[i], front.second + dy[i]);

			// 지도를 넘어가면 다시 탐색
			if (nextPos.first < 0 || nextPos.second < 0 || nextPos.first > M - 1 || nextPos.second > N - 1)
				continue;

      // 방문 정보와 지도 정보를 가지고 탐색 결정
			if (!visited[nextPos.second][nextPos.first] && map[nextPos.second][nextPos.first] == 1)
			{
				// ** 연결된 노드 중 방문하지 않은 노드를 방문하고, 차례대로 큐에 삽입한다. **
				myQueue.push(nextPos);
				visited[nextPos.second][nextPos.first] = true;
				innerCount++;
			}
		}
	}

	return innerCount;
}
```