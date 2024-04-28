---
layout: single
title:  "Programming - 배열의 Index가 0부터 시작하는 이유"
categories:
  - Programming
---

---

![](../../../../assets/images/2023-07-25-Programming-Why%20Array%20Index%20Start%200/2024-04-28-18-50-46.png){: .align-center}
*Ram*

![](../../../../assets/images/2023-07-25-Programming-Why%20Array%20Index%20Start%200/2024-04-28-18-51-11.png){: .align-center}
*Random Access Memory*

![](../../../../assets/images/2023-07-25-Programming-Why%20Array%20Index%20Start%200/2024-04-28-18-51-33.png){: .align-center}
*배열은 시작 주소와 길이로 이루어짐*

**배열이란 RAM에 접근해서 데이터 공간을 확보하는 것이다.  
확보하는 방식은 우선 배열의 시작 주소를 정하고 해당 배열의 길이를 설정한다.  
여기서 주목해야 할 점은 배열의 시작 주소를 정한다는 것이다.  
사람의 관점이 아닌 컴퓨터의 관점에서 인덱스란 배열의 시작 주소를 기준으로 0만큼 떨어진 요소 1만큼 떨어진 요소.  
즉 offset을 단위로 인덱스를 측정한다.