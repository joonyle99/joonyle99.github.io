---
layout: single
title:  "Book - 유니티 C# 스크립팅 마스터하기 - Chapter 2 : 디버깅"
categories:
  - Book
---

> "프로그램 테스트를 통해 버그의 존재를 증명할 수는 있지만,  
> 버그가 없음을 증명할 수는 없다" - 에츠허르 데이크스트라-

모든 조건과 모든 하드웨어와의 조합은 실로 무한하기 때문에 나온 말이다.

### 컴파일 오류와 콘솔

---

디버깅은 보통 **런타임 중에 오류를 잡아내는 기법**을 말한다.  
즉, 프로그램 로직이 만들어내는 결과에 따른 실행 중의 오류를 찾아낸다는 것이다.

따라서 문법이 아닌 **'로직'**에 초점이 있다.

### Debug.Log를 이용한 디버깅 : 개발자 지정 메시지

---

유니티에서 가장 고전적이고 잘 알려진 디버깅 기법은  
콘솔에 프로그램의 흐름과 오브젝트 속성을 보여주기 위해  
진단 메시지를 출력하는 Debug.Log() 함수를 이용하는 방법일 것이다.

Vector 및 Color를 포함한 모든 유니티 오브젝트에는 ToString() 함수가 있어,  
Debug.Log()로 사람이 읽을 수 있는 형태의 문자열로 출력할 수 있다.

> 게임을 빌드해 배포할 시기가 되었을 때, 간결함을 유지하기 위해  
> Debug.Log 절을 모두 지우거나 주석처리 하자

### ToString() 메소드 재정의 (Overriding)

---

Debug.Log() + ToString()은 자주 사용하는 조합이다.

C#의 모든 클래스는 기본적으로 ToString()을 상속하는데,  
사용자가 만든 클래스에 재정의해 사용할 수 있다.

![](../../../../assets/images/2024-03-15-Book-Unity%20Scripting%20Master%202/2024-03-15-03-00-38.png){: .align-center}

실제로 Object 클래스에 ToString() 함수가 정의되어있는 것을 볼 수 있었고,  
모든 클래스는 Object 클래스를 상속받기 때문에,  
C#의 모든 클래스는 ToString()을 상속받아, 재정의 할 수 있다.

```cs

private string monsterName;
private int monsterHp;
private float monsterSpeed;

public override string ToString()
{
    return string.Format($"몬스터 이름 : {monsterName} | 몬스터 체력 : {monsterHp} | 몬스터 속도 : {monsterSpeed}");
}

```

### 전처리기 전역 선언

---

위의 방법들로 결국 Debug.Log()를 이용해 콘솔에 문자열을 출력했다.  
하지만 이러한 코드들은 디버그 할때나 유용하던 것이고,  
배포할 때 되어서는 필요가 없을 수 있다.  
그렇다고 전부 지우자니, 유지보수 하면서 다시 사용하게 될 수도 있고...

이럴때는 특정 **Flag**를 활성화하면, 디버그 전용 코드를 실행할 수 있도록  
디버그와 릴리스 코드 단락을 분리할 수 있다.

![](../../../../assets/images/2024-03-15-Book-Unity%20Scripting%20Master%202/2024-03-15-03-15-18.png){: .align-center}
*조건 컴파일을 가능케 하는 전역 선언을 유니티에 추가한다*

```cs
    void Start()
    {
        ToString_Overriding obj = this;

#if SHOW_DEBUG_MESSAGE
        // 활성화
        Debug.Log(obj.ToString() + "Debug");
#elif SHOW_RELEASE_MESSAGE
        // 비활성화
        Debug.Log(obj.ToString() + "Release");
#endif
    }
```

### 시각적 디버깅

---

> 때때론, 1000개의 글자보다 그림 하나가 낫다

> Game 탭에서는 기본적으로 기즈모가 보이지 않는다.  
> Gizmo 툴바 버튼을 이용해 보이도록 해보자.

### 오류 기록

---

테스터가 한 사무실에 함께 모여 있든, 전 세계에 흩어져 있든 간에  
게임을 테스터에게 배포하기 위해 컴파일 및 빌드를 하는 시점에는  
**게임 플레이 중 오류와 예외가 일어났는지, 그리고 언제 일어났는지를 기록할 방법이 필요**하다.

한 가지 방법은 Log(로그) 파일을 이용하는 것이다.

Log 파일은 게임의 실행 시점에  
사람이 읽을 수 있는 형태로 생성하는 텍스트 파일로서,  
발생하는 모든 오류에 대한 자세한 내용을 기록한다.

유니티에 내장된 Application 클래스를 이용함으로써 델리게이트를 통해 예외 통지를 받는 방법이 있다.

... (ㄱㅖ속)