---
layout: single
title:  "Unity - Life Cycle (수정)"
categories:
  - Unity
---

---

### Unity의 Life Cycle에 대해
---

Unity 스크립트는 사용자가 따로 호출하지 않아도 호출되는 함수가 있다. 그러한 함수들의 호출 주기를 생명주기(Life Cycle)라고 한다. 이러한 함수들의 호출 시기는 Programmable 하지 않기 때문에, 미리 정해져 있는 이러한 함수들의 생명주기를 이해하는 것이 Unity를 다루는 데 필수적이다.

유니티 스크립트 라이프 사이클 문서 : 
[Unity Documentation - Life Cycle](https://docs.unity3d.com/Manual/ExecutionOrder.html#BeforeTheFirstFrameUpdate)

<br>

![](/assets/images/unity_simpleLifeCycle.png){: width="50%" height="50%"}{: .align-center}
*간소화된 유니티의 생명주기*

#### 에디터

* Reset : 인스펙터 창에서 리셋 버튼을 눌렀을 때 호출되며, 객체의 속성을 초기화한다. (Editor 전용 함수)

![](/assets/images/unity_lifecycle07.png){: width="50%" height="50%"}{: .align-center}
*Reset함수는 인스펙터 창에서 리셋 버튼을 눌렀을 때 호출*


#### 첫 장면 로드
* Awake : 오브젝트에 컴포넌트로 붙어있는 스크립트가 실행될 때 한 번만 호출된다. (활성화가 되어있지 않아도 호출)
* OnEnable : 오브젝트가 활성화될 때마다 호출된다.

![](/assets/images/unity_lifecycle05.png){: width="50%" height="50%"}{: .align-center}
*Awake 함수는 스크립트가 활성화되어 있지 않아도 호출*


#### 업데이트 전 첫 프레임
* Start : Update 함수가 호출되기 전에 한 번만 호출된다. 다른 스크립트의 모든 Awake 함수가 호출된 이후에 실행된다.

![](/assets/images/unity_lifecycle08.png){: width="70%" height="70%"}{: .align-center}
*Start 함수는 씬에 있는 모든 오브젝트의 Awake, OnEnable이 실행된 후 호출*

<!--
![](/assets/images/unity_lifecycle03.png){: width="50%" height="50%"}{: .align-center}
*오브젝트와 스크립트가 모두 활성화된 상태*

![](/assets/images/unity_lifecycle02.png){: width="60%" height="60%"}{: .align-center}
*Awake -> OnEnable -> Start 순으로 호출*
-->


#### 업데이트 단계

* FixedUpdate : Delta Time이 0.02초로 고정된 업데이트 함수. 일정 시간 간격 업데이트 함수로 OnTrigger & OnCollision 전에 실행된다. 정확한 물리 & 충돌판정을 하기 위해 사용된다.
* OnTrigger & OnCollision : FixedUpdate와 Update 사이에 실행되는 Physics 함수.
* Update : 프레임마다 호출되는 함수로 게임의 핵심 로직에 사용된다. Delta Time이 일정하지 않다. e.g) 1 / 144 = 0.007
* LateUpdate : Update함수가 실행되고 나서 호출된다. 주로 카메라 이동 로직에 사용한다.

![](/assets/images/unity_lifecycle10.png){: width="70%" height="70%"}{: .align-center}
*FixedUpdate -> OnCollision -> Update -> Late Update*

> 씬에 있는 모든 오브젝트의 Update()를 순회하면서 실행시킨다.

#### 마무리 단계

* OnDisable : 오브젝트 또는 스크립트가 비활성화되었을 때 호출된다.
* OnDestroy : 오브젝트의 마지막 프레임이 업데이트된 후 실행된다.
* OnApplicationQuit : App 종료 전에 모든 오브젝트에서 호출된다.
