---
layout: single
title:  "Programming - 랜덤 엑세스 (Random Access)"
categories:
  - Programming
---

---

랜덤 엑세스란 데이터에 무작위로 접근할 수 있다는 뜻으로,  
필요한 데이터에 직접적으로 접근해 읽거나 쓸 수 있다는 의미이다.

1. 주기억장치 (RAM)

![](../../../../assets/images/2023-06-10-Programming-Random%20Access/2024-04-28-18-40-47.png){: .align-center}

이름에서부터 알 수 있듯 Random Access Memory는 메모리 주소를 가지고,  
그 주소에 직접적으로 데이터를 저장한다.

2. 배열 (Array)

![](../../../../assets/images/2023-06-10-Programming-Random%20Access/2024-04-28-18-41-02.png){: .align-center}

RAM과 마찬가지로 이미 정해진 공간에 데이터를 저장한다.  
그리고 그 데이터를 읽거나 쓰기 위해서 직접적으로 접근한다.

List는 배열과 달리 포인터로 이루어진 자료구조이며,  
직접 접근이 아닌 "순회"의 방식으로 접근해야 한다.

따라서 배열은 O(1)의 시간복잡도로 탐색할 수 있고,  
리스트는 O(n)의 시간복잡도로 탐색할 수 있다.