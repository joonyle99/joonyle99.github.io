---
layout: single
title:  "Data Structure / Algorithm - 너비 우선 탐색 (BFS)"
categories:
  - DataStructure_Algorithm
---

---

너비 우선 탐색(Breadth First Search)은 탐색을 할 때, 너비를 우선으로 탐색하는 탐색 알고리즘이다. '맹목적인 탐색'을 하고자 할 때 사용할 수 있는 탐색 기법이다.

너비 우선 탐색에서 <u> 사용되는 자료구조는 큐(Queue) </u>로, <u> 최단 경로 </u>를 찾아준다는 점에서 최단 길이를 보장해야 할 때 많이 사용한다.

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