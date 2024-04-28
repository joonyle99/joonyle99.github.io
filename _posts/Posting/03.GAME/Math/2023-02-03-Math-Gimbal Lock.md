---
layout: single
title:  "Math - 짐벌락 현상(gimbal lock)"
categories:
  - Math
---

---

출처: [https://youtu.be/yxMQKsab5TQ](https://youtu.be/yxMQKsab5TQ)

![](../../../../assets/images/2023-02-03-Math-Gimbal%20Lock/2024-04-29-00-02-20.png)

위의의 사진을 보게 되면 X(빨강), Y(초록), Z(파랑) 축을 볼 수 있다.

![](../../../../assets/images/2023-02-03-Math-Gimbal%20Lock/2024-04-29-00-03-10.png)

![](../../../../assets/images/2023-02-03-Math-Gimbal%20Lock/2024-04-29-00-03-14.png)

짐벌락은 같은 방향으로 오브젝트의 두 회전 축이 겹치는 현상을 말한다.  
겹쳐진 두 축으로 인해, 두 축 중 한 축의 계산을 할 수 없게 된다.  
(왼쪽은 X축으로 회전, 오른쪽은 Z축으로 회전하는 사진)

즉 X축과 Z축이 겹쳐버려 둘 중 하나의 축을 회전시키면 어떤 축을 회전시킨 것인지 알 수 없다.  
이것이 짐벌락 현상이다.

하지만 이 문제는 Quaternion(쿼터니언)으로 해결할 수가 있다.