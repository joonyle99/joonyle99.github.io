---
layout: single
title:  "Unity - AddForce"
categories:
  - Unity
---

---

### AddForce()에 대해
---

![](/assets/images/unity_addforce.png)

> 설명 : ForceMode에 따라 벡터 방향으로 힘을 추가한다.

[Unity Documentation - AddForce()](https://docs.unity3d.com/ScriptReference/Rigidbody.AddForce.html)

### ForceMode
---

1. ForceMode.Force : 입력을 힘(N)으로 해석하고 <u> 힘 * DT / 질량 </u>의 값으로 속도를 변경한다.  
   -> DT와 질량에 영향을 받음
2. ForceMode.Acceleration : 입력을 가속도(m/s^2)로 해석하고 <u> 힘 * DT </u>의 값으로 속도를 변경한다.  
   -> DT에는 영향을 받지만, 질량에는 영향을 받지 않음
3. ForceMode.Impulse : 입력을 충격량(N * s or kg * m/s)으로 해석하고 <u> 힘 / 질량</u> 의 값으로 속도를 변경한다.  
   -> DT에는 영향을 받지 않지만, 질량에는 영향을 받음
4. ForceMode.VelocityChange : 입력을 직접적인 속도 변경(m/s)으로 해석하고 <u> 힘 </u>의 값에 따라 속도를 변경한다.  
   -> DT나 질량에 영향을 받지 않음

> 이미 내부에서 시간에 따른 DT 계산을 해주고 있기 때문에, FixedUpdate()에서 AddForce()를 실행할 때 Time.deltaFixedTime을 해줄 필요가 없다. 

<br>

| Force Mode | DT (Delta Time) | 질량 |
|:-----------------:|:----:|:----:|
| ForceMode.Force | O | O |
| ForceMode.Acceleration | O | X |
| ForceMode.Impulse | X | O |
| ForceMode.VelocityChange | X | X |

<br>

### 힘, 질량, 가속도
---

ForceMode.Force에서 입력값이 거치는 계산 과정에 대해 알아보자.

<center>

$ F = ma $

</center>

<center>

$ a = {\Delta v \over \Delta t} $

</center>

<center>

$ F = m \times {\Delta v \over \Delta t} $

</center>

<br>

힘(F)이 입력되었을 때, 힘의 방향벡터에 대해 변화하는 속도는 다음과 같다.

<center>

$ \Delta v = {F \over m} \times \Delta t $

</center>

<br>

### AddForce를 Update가 아닌 FixedUpdate에서 사용해야 하는 이유
---

DT에 상관이 없는 `ForceMode.Impulse` 와 같이 예외가 있을 수 있지만, DT가 상관있는 경우 물리학에 충돌 누락이 발생할 수 있으므로 Update()가 아닌 FixedUpdate에서 처리해야 한다.

또한 FixedUpdate -> OnTrigger & OnCollision -> Update 순서대로 이벤트가 발생하기 때문에 정확한 충돌을 위해 충돌 처리 전에 AddForce를 해야 한다.

### ASH 프로젝트에서 나무를 밀기 위해 사용하는 AddForce
---

![](/assets/gif/unity_addforceTree.gif){: .align-center}
*AddForce를 이용한 나무 쓰러뜨리기 기믹*

```c#
// 해당 코드는 FixedUpdate()에서 돌아간다
_rigid.AddForceAtPosition(Vector2.right * _dir * _power, forcePointTransform.position, ForceMode2D.Force);
```

AddForceAtPosition()은 힘을 주는 위치를 정해줄 수 있다는 점에서 AddForce()와 차이가 있지만, 나머지는 전부 같다.

ForceMode.Force를 사용하고 있으며 이는 <u> 물체의 질량, DT에 영향을 받는다 </u>.  
그리고 이 함수는 FixedUpdate()에서 고정된 DT를 가지고 물체의 속도를 변경한다.