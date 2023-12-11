---
layout: single
title:  "Data Structure / Algorithm - 시퀀스 컨테이너 vs 연관 컨테이너 (수정)"
categories:
  - DataStructure_Algorithm
---

---

### 시퀀스 컨테이너 vs 연관 컨테이너
---

시퀀스 컨테이너 : vector<>, list<>, deque<>, ... 과 같이 데이터를 순서 있게 보관하는 컨테이너 (Index가 있다).  
연관 컨테이너 : map<>, set<>, hash_map<>, hash_set<> ... 과 같이 `Key`와 `Value`로 데이터를 보관하는 컨테이너.

> hash_map<>과 hash_set<>은 unordered_map<>과 unordered_set<>으로 이름이 변경되었다.

<!--
> 연관 컨테이너는 `Key`값의 중복을 허용하지 않는데, 만약 중복되는 `Key`값을 사용하고 싶다면 multi-를 붙여 사용한다.
> e.g ) multi_map<>, multi_set<>
-->

<!--
> 연관 컨테이너 중 map<>과 set<>은 `Key`를 기준으로 정렬된 상태로 데이터를 보관하고, 그 순서대로 순회한다. 반면에 hash_map<>, hash_set<> 의 Hash STL은 정렬이 필요 없으며 Random Access 방식으로 빠른 데이터 검색이 가능하다 (시간 복잡도 O(1))
-->