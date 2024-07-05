---
layout: single
title:  "Book - 유니티 C# 스크립팅 마스터하기 - Chapter 1 : Unity & C# 기초 복습"
categories:
  - Book
---

### 1. 유니티 엔진을 활용한 게임 개발에 C#을 선택하는 이유

---

유니티 스크립팅을 처음 접할 때는 C#을 선택하는 이유에 대해 의문을 가질 수 있다.

유니티는 다양한 프로그래밍 언어를 지원하지만, 일반적으로 사용되는 언어는 다음과 같다.

1. C#
2. UnityScript (JavaScript for Unity)
3. Boo

> UnityScript는 Unity 2017 버전부터 공식적으로 지원하지 않으며, C# 사용이 권장된다.

우선, C#는 유니티 엔진의 기본 언어이면서 가장 널리 사용되는 언어이다.  
그렇기 때문에 가장 지원받는 언어이고, 튜토리얼 및 자료들을 찾는 데 이점이 있다.

이외에도 C#은 게임 개발 분야에 큰 영향력을 지닌 C++과 가장 닮아있기도 하다.

### 2. C# 클래스의 이름

---

유니티는 스크립트 파일 안에 만들 C# 클래스의 이름을 정할 때,  
파일의 이름을 참조하기 때문에,  
클래스의 이름과 파일의 이름을 맞춰줘야 한다.

### 3. ToString()

---

유니티의 여러 클래스와 오브젝트는 ToString() 함수를 가지고 있다.  
디버깅 과정에서 오브젝트와 데이터를 콘솔에 출력할 때, 무척 유용하다.

* **Debug.Log() + ToString()**

### 4. 이벤트

---

이벤트는 본질적으로 함수이다.  
유니티의 MonoBehavior이 제공하는 **이벤트 콜백 함수들(Start, Update, ...)**은  
유니티 고유의 이벤트라고 설명하는 것이 더 정확할 수 있겠다.

Start()는 레벨이 시작되었을 때, Update()는 새 프레임이 시작되었을 때,  
와 같이 오브젝트에게 뭔가 중요한 것이 일어났다는 사실을 알릴 때 불리는 **'콜백 함수'**이다.

### 5. FPS는 무조건 30 이상을 유지해라

---

Game 씬의 Stats를 참고해 FPS를 확인해 보자.  
FPS가 30보다 낮다면 .. 여기까지 하겠다.

### 6. MonoBehavior

---

유니티에서 새로운 스크립트가 생성되면,  
해당 클래스는 MonoBehavior의 상속을 받은 클래스가 자동으로 만들어진다.

MonoBehavior는 크게 두 가지 역할을 한다.

1. C# 스크립트를 게임 오브젝트에 컴포넌트로 부착할 수 있도록 한다
2. 유니티에서 제공하는 이벤트 콜백 함수 (Awake, Start, Update, ...)를 제공한다

MonoBehavior의 상속을 받아, 새롭게 만들어진 클래스는  
MonoBehavior의 기능을 이어받아, 추가적인 기능을 구현하게 된다.

> 상속을 남용하면 클래스가 비대하고 무거워져 혼란스러워진다  
> 상속은 다른 클래스와 공통적인 기능성을 공유하는 클래스를 만들 때 만들 필요가 있다.

### 7. C# 프로퍼티

---

1. 변수의 값이 항상 올바르도록 강제해, 할당된 값을 확실히 한다.  
2. 변수의 값이 변경되었을 때를 '감지'해 이 값에 영향을 받는 다른 함수나 동작을 실행할 수 있다.

```cs
public int Property
{
    get
    {
        if (_property > 5 && _property < 10) return -1;
        return _property;
    }

    set
    {
        _property = value;

        EventFunction();
    }
}
```

<br>

추가로 나는 프로퍼티의 갱신 타이밍이 궁금했다.  
유니티의 생명 주기 중 어디에서 값이 업데이트되는지.

유니티의 수많은 이벤트 콜백 함수들에서 계속해서 프로퍼티의 값이 변경되면,  
그때마다 함수가 호출될까?

실험해보자

![](../../../../assets/images/2024-03-15-Book-Unity%20Scripting%20Master%201/2024-03-15-02-35-15.png){: .align-center}

```cs

public class PropertyWhenCall : MonoBehaviour
{
    int _property = 5;

    int Property
    {
        get
        {
            DebugConsole_InGetter();
            return _property;
        }
        set
        {
            DebugConsole_InSetter();
            _property = value;
        }
    }

    public void DebugConsole_InGetter()
    {
        Debug.Log("DebugConsole_InGetter");
    }
    public void DebugConsole_InSetter()
    {
        Debug.Log("DebugConsole_InSetter");
    }

    public void DebugConsole_InUpdate()
    {
        Debug.Log("DebugConsole_InUpdate");
    }

    void Awake()
    {
        Property = (int)Random.Range(0f, 100f);
    }

    // Start is called before the first frame update
    void Start()
    {
        Property = (int)Random.Range(0f, 100f);
    }

    // Update is called once per frame
    void Update()
    {
        Property = (int)Random.Range(0f, 100f);
        DebugConsole_InUpdate();
    }

    void LateUpdate()
    {
        Property = (int)Random.Range(0f, 100f);
    }
}

```

프로퍼티 값을 한 프레임 내에서 여러 번 수정한다고,  
그만큼 함수가 호출되지 않았다.

프로퍼티는 프레임에 맞춰 Update()와 동시에 실행되는 것이었다.

### 8. 주석 남기기

---

주석을 남기는 습관이 배어있지 않다면,  
습관화할 것을 권한다.

다른 개발자들뿐만 아니라, 스스로에게도 득이 된다.

시간이 지나 코드를 보게 되었을 때,  
주석이 있는 것과 없는 것은 천지 차이다.

특히 XML 주석(///)을 자주 이용해라  
왜냐하면, 함수 요약 설명과 파라미터 주석이 코드 내용에 반응해 표시된다.

### 9. SendMessage와 BroadcastMessage

---

유니티 API의 MonoBehavior 클래스는 SendMessage()와 BroadcastMessage() 함수를 제공한다.  
오브젝트에 붙은 모든 컴포넌트가 가지고 있는 함수를,  
함수의 이름으로 실행할 수 있다.

BroadcastMessage()는 SendMessage()에서 좀 더 확장된 기능으로,  
씬 내의 모든 자식 오브젝트를 포함해 재귀적으로 동작한다.

> SendMessageOptions.DontRequireReceiver