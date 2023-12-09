---
layout: single
title:  "Unity - Scriptable Object"
categories:
  - Unity
toc: true
---

---

### 스크립터블 오브젝트(Scriptable Object)
---

유니티에서 제공하는 데이터 컨테이너로, 데이터(값)의 사본이 생성되는 것을 방지해 프로젝트의 메모리를 줄일 수 있다.  
즉, 메모리에 데이터 사본을 하나만 저장한다.

예를 들어 어떠한 몬스터가 HP, MP 데이터를 가지고 있을 때 몬스터가 1000마리가 생성된다면, 이 HP, MP라는 데이터를 독립적으로 가지고 있어 1000개의 데이터의 사본이 생기게 된다.

이를 방지하고자 사용하는 게 스크립터블 오브젝트이다.  
프리팹의 데이터를 정적 변수가 아닌 일반 변수로 선언할 경우, 인스턴스가 생성될 때마다 데이터에 대한 자체 사본이 생성되는데, 스크립터블 오브젝트를 사용하면 메모리에 스크립터블 오브젝트의 데이터를 저장하고 각각의 인스턴스에서는 이를 참조하는 방식으로 사용한다.

```c++
[CreateAssetMenu(fileName = "ColorData", menuName = "Scriptable Object/ColorData", order = 1)]
public class ColorData : ScriptableObject
{
    [SerializeField] private Color color = Color.magenta;

    public Color Color
    {
        get { return color; }
        set { color = value; }
    }
}
```

![](/assets/images/unity_scriptableObject2.png){: .align-center}{: width="70%" height="70%"}

스크립터블 오브젝트는 "변하지 않는 데이터"를 저장해 놓는 공간이다.  
에디터 모드에서는 스크립터블 오브젝트에 데이터 입력을 해 저장시킬 수 있다. 이는 스크립터블 오브젝트가 "에디터 네임스페이스"와 "에디터 스크립팅"을 사용하기 때문.  
하지만 빌드 버전에서는 스크립터블 오브젝트에 데이터 입력을 할 수 없어서 "세션 간의 데이터 지속성"을 보장할 수 없다.

### 데이터의 인스턴스화
---

화면에 500개의 Circle을 0.005초마다 생성하는 프로그램이 있다.  
이 경우 Circle 오브젝트가 인스턴스화될 때마다 메모리에 데이터의 사본이 생성되는데, 스크립터블 오브젝트를 사용해 메모리에 데이터의 사본 하나만 저장한 후 참조해 사용한다.

```c++
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class ColoredCircle : MonoBehaviour
{
    // 1. 스크립터블 오브젝트를 사용해 하나의 데이터 메모리에 참조하는 방식
    [SerializeField] private ColorData _colorData;

    // 2. 인스턴스 생성 시 데이터의 사본이 생김
    [SerializeField] private Color _color;

    void Start()
    {
        // 데이터의 사본이 생기지 않음
        this.GetComponent<SpriteRenderer>().color = _colorData.Color;
    }
}
```

![](/assets/images/unity_circleCreator.gif){: .align-center}