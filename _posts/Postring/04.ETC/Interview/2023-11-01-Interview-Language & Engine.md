---
layout: single
title:  "Tech Interview - 언어 & 엔진"
categories:
  - interview
---

---

### 이미 해제된 메모리 주소에 접근하는 현상
---

이미 해제된 메모리에 접근하는 현상은 "댕글링 포인터"라고 한다.

유효한 포인터는 해당 포인터를 가지고 멤버 변수나 멤버 함수에 접근할 수 있다.  
하지만 이미 메모리가 해제되어 유효하지 않은 포인터는 멤버에 접근하려고 하면 오류나 예기치 못한 상황이 발생한다.

```c++
#include <iostream>

using namespace std;

class Team
{
private:
	string teamName;

private:
	string topName;
	string jugName;
	string midName;
	string adName;
	string supName;

public:
	Team()
		: teamName("T1"),
			topName("zeus"), jugName("oner"), midName("faker"),
			adName("gumayusi"), supName("keria")
	{}
	~Team() = default;

public:
	void PrintTeamName() { cout << teamName << endl; }
	void PrintAllPlayerName()
	{
		cout << "Top : " << topName << endl;
		cout << "Jug : " << jugName << endl;
		cout << "Mid : " << midName << endl;
		cout << "Ad : " << adName << endl;
		cout << "Sup : " << supName << endl;
	}
};

int main()
{
	// 포인터에 값을 동적 할당
	Team* pTeam = new Team;
	cout << "Team : " << pTeam << endl;
	pTeam->PrintTeamName();
	pTeam->PrintAllPlayerName();

	// 메모리를 해제 (쓰레기 값)
	delete pTeam;
	cout << "Team : " << pTeam << endl;

  // 이미 해제된 메모리에 접근하는 "댕글링 포인터" 발생
	pTeam->PrintTeamName();

	// 해제된 메모리를 nullptr로
	pTeam = nullptr;
	cout << "Team : " << pTeam << endl;
	pTeam->PrintTeamName();

	return 0;
}
```

위 예제는 댕글링 포인터가 발생하는 상황을 나타낸 것이다.

개발자가 힙 메모리에 직접 할당하고 직접 해제하는 동적할당은 실수가 많이 나오는 부분인데, 이러한 댕글링 포인터를 피하기 위한 몇 가지 주의사항이 있다.

1) 포인터를 다시 사용하기 위해 유효성을 검사한다.
```c++
	// 유효성 검사 (nullptr인지 확인)
	if (pTeam != nullptr)
		pTeam->PrintTeamName();
```

접근하려는 포인터가 nullptr인지 확인하고, 유효한 경우에만 멤버에 접근한다. 하지만 메모리 해제 시 nullptr이 아닌 쓰레깃값이 들어갈 수 있기 때문에 이 방법만으로는 부족하다.

2) 포인터를 해제한 후 nullptr로 만들어 주기
```c++
	// 메모리를 해제
	delete pTeam;
  // 해제된 메모리를 nullptr로
	pTeam = nullptr;
```

이렇게 두 단계를 거치면 댕글링 포인터를 예방할 수 있다. 이 부분을 습관화해두지 않으면 밤새 댕글링 포인터만 찾는 경험을 하게 될 수도 있다. 조심하자

### Const와 Readonly의 차이점과 Readonly 사용 이유
---

### Dispose 사용 이유
---

### NGUI Depth 최적화에 대해
---

### "I Love You!"를 "You! Love I"로 변환하는 방법 (추가 버퍼는 존재하지 않음)
---

### Rand(6)만 사용하여 Rand(18)을 만들기
---

### C# string의 가운데만 교체하는 기술이 가능한가?
---

### stringBuffer를 사용하는 이유
---

### Unity에서 if(player)가 되는 이유 (bool값이 아님에도)
---

### 객체지향 설계 5가지 원칙에 대해
---

### 유니티에서 적용하기 힘든 이유
---

### Unity의 GC(Garbage Collection)에 대해
---

### Unity의 GC 최적화 방법
---

### IEnumerator의 내부 구조에 대해
---

### Garbage가 발생하는 이유
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


### 코루틴의 호출 시점
---

### C#의 CLR(Common Language Runtime)에 대해
---

### C++에서의 Struct와 Class
---

C++에서 Struct와 Class는 거의 비슷한 기능을 한다.  
유일한 차이점은 Struct의 기본 접근 지정자(Access Modifier)가 `Public`이고 Class의 기본 접근 지정자는 `Private`이다.  
그럼에도 코딩 컨벤션에 따라, 이 둘의 용도 차이가 있을 수 있다. Struct는 간단한 데이터들의 모음인 POD(Plain Old Data)의 경우 사용하고, Class는 복잡한 데이터나 객체 지향 프로그래밍 요구 사항을 충족하기 위해 사용한다.  

```c++
struct mystruct
{
    // 데이터의 모음
    int data;
    int x;
    int y;
    /* 별 다른 로직을 구현하지 않는다! */
}
```

### C#에서의 Struct와 Class의 차이
---

C#에서 Struct와 Class는 유의미한 차이가 있다.  
마찬가지로 멤버 변수와 멤버 함수를 가질 수 있지만, Sturct는 `값 타입`이고 Class는 `참조 타입`이다.

| Struct | Class |
|:------:|:-----:|
|값 타입|참조 타입|
|스택에 할당|힙에 할당|
|상속 X|상속 O|

`값 타입`은 매개변수로 전달할 때 원본의 <u> 복사본 </u>을 보내어, 함수 내에서 원본에 영향을 주지 못한다. 또한 Struct 객체는 스택에 할당되기 때문에, 크기가 크면 스택 사용량 늘어나 비효율적이다.

반면 `참조 타입`은 매개변수로 전달할 때 원본을 참조할 수 있는 <u> 주소 </u>를 전달한다. 따라서 함수 내에서 원본의 수정도 가능하고, 힙 영역에 할당되기 때문에 Class의 크기가 크더라도 스택 영역의 사용량은 메모리 주소만큼만 증가한다. (64bit O/S - 16바이트)

C#에서의 Struct와 Class의 사용을 Unity로 살펴보자.

![](/assets/images/unity_struct.png) ![](../assets/images/unity_struct2.png)

`Vector2`와 `Quaternion`은 Struct로 만들어졌다. 비교적 간단한 데이터들의 모음이기 때문이다.

```c++
    Vector2 tempVec = Vector2(1, 1);
    Vector2 tempVec2 = new Vector2(1, 1); // 스택에 올라가는 Struct 객체도 new 키워드로 생성
```

하지만 C#에서는 이러한 구조체를 생성할 때 `new`라는 키워드를 사용하는데, new는 C++에서 힙 메모리에 동적 할당하는 키워드이지만 스택에 올라가는 구조체의 객체 생성에도 사용된다.

![](/assets/images/unity_class.png) ![](../assets/images/unity_class2.png)

`Rigidbody2D`나 `Collider2D`와 같이 복잡하고 큰 데이터들의 집합은 Class를 사용한다.

### 모든 객체를 동적으로 생성하면 안되는 이유
---

`new` 키워드로 객체를 동적으로 생성해 힙 메모리에 올려서 사용하면 생명주기를 제어할 수 있지만, 메모리 할당 및 해제에 필요한 작업이 스택보다 느리다. 스택 메모리는 크기가 작은 대신 빠른 엑세스 속도를 제공하고 데이터를 쌓고(Push) 제거(Pop)하는 것이 장점이다.

### Sort Order이란?
---

### 동기(Synchronous) 함수와 비동기(Asynchronous) 함수에 대해
---

### 접근 제한자(Public Private Protected)에 대해
---

### 상속을 사용해 본 경험에 대해
---

### new 할당이 실패하는 경우?
---

### FSM(Finite State Machine)에 대해
---

### Awake(), Start(), OnEnable()의 차이에 대해
---

### Update(), FixedUpdate(), LateUpdate()의 차이에 대해
---

### 객체지향의 특성 중 캡슐화에 대해 설명
---

### Unity Generic에 대해
---

### Unity Humanoid에 대해
---
