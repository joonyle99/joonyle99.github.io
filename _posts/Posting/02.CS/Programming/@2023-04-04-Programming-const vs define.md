---
layout: single
title:  "Programming - const vs define"
categories:
  - Programming
---

---

![](../../../../assets/images/2023-10-31-Programming-const%20vs%20define/2024-04-28-18-38-22.png){: .align-center}

const의 변수 값은 런타임에 결정되며읽기 전용 변수를 생성하는 것이다.  
define은 컴파일 타임에 값이 결정되는, 변수를 생성하지 않는 그저 "텍스트 대체"이다.  
값의 이름을 만드는 것이다

![](../../../../assets/images/2023-10-31-Programming-const%20vs%20define/2024-04-28-18-38-46.png){: .align-center}

define은 상수를 변수에 담는 것이 아닌 텍스트 변환이기 때문에, 메모리 할당이 되지 않는다.