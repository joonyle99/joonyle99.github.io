---
layout: single
title:  "Data Structure / Algorithm - 깊이 우선 탐색 (DFS)"
categories:
  - DataStructure_Algorithm
---

---

깊이 우선 탐색(Depth First Search)은 탐색하면서 깊은 것을 우선으로 탐색하는 알고리즘으로, <u> 맹목적으로 각 노드를 탐색할 때 사용된다. </u>  
또한 너비 우선 탐색(Breadth First Search)에서는 큐(Queue)가 사용되었다면, 깊이 우선 탐색에서는 <u> 스택(Stack)과 재귀함수(Recursive) </u>가 사용된다.

깊이 우선 탐색 알고리즘은 맨 처음에 시작 노드를 스택에 삽입하면서 시작한다. 또한 동시에 시작 노드를 방문했다고 알리는 <u> '방문 처리` </u>를 표시를 해준다.

이제 다음과 같은 간단한 알고리즘을 따라 동작한다.

1. 스택의 최상단 노드를 확인한다.
2. 최상단 노드에 방문하지 않은 인접 노드가 있으면, 그 노드를 스택에 넣고 방문 처리한다. 방문하지 않은 인접 노드가 없으면 스택에서 최상단 노드를 뺀다.

이 알고리즘을 계속해서 반복 수행하면서 모든 노드를 탐색하게 된다.

![](/assets/gif/algorithm_dfs.gif){: .align-center}
*깊이 우선 탐색*

이러한 DFS는 그 자체로는 큰 의미가 없고 DFS를 활용해서 문제를 해결하는 것에 주안점이 있기 때문에, 작동 원리에 대해서 확실하게 이해만 하면 된다.

또한 이 알고리즘은 Brute Force 알고리즘과 마찬가지로 탐색의 순서만 다를 뿐 모든 노드를 탐색한다는 점에서 <u> 완전 탐색 알고리즘 </u>이다.

[BFS / DFS 알고리즘 문제 리스트](https://www.acmicpc.net/workbook/view/17403)