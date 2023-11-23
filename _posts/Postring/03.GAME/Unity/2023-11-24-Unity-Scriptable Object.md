---
layout: single
title:  "Unity - Scriptable Object"
categories:
  - unity
---

---

### 스크립터블 오브젝트(Scriptable Object)
---

유니티에서 제공하는 데이터 컨테이너로, 데이터의 사본이 생성되는 것을 방지할 수 있다.  
예를들어 어떠한 몬스터가 HP, MP 데이터를 가지고 있을 때 몬스터가 1000마리가 생성된다면, 이 HP, MP라는 데이터를 독립적으로 가지고 있어 1000개의 데이터의 사본이 생기게 된다.

이를 방지하고자 사용하는게 스크립터블 오브젝트이다.  
프리팹의 데이터를 일반 변수로 정적 변수가 아닌 일반 변수로 구현할 경우, 인스턴스가 생성될때마다 데이터에 대한 자체 사본이 생성되는데, 스크립터블 오브젝트를 사용하면 메모리에 스크립터블 오브젝트의 데이터를 저장하고 각각의 인스턴스에서는 이를 참조하는 방식으로 사용한다.

```c++
[CreateAssetMenu(fileName = "ColorData", menuName = "Scriptable Object/ColorData", order = int.MaxValue)]
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

![](/assets/images/unity_scriptableObject.png){: .align-center}{: width="50%" height="50%"}

스크립터블 오브젝트는 "변하지 않는 데이터"를 저장해놓는 공간이다.  
에디터 모드에서는 스크립터블 오브젝트에도 데이터 입력을 해 저장시킬 수 있지만, 빌드 버전에서는 안되기 때문에 "데이터 지속성"이 존재하지 않는다.

### 500개의 Circle 오브젝트 생성
---

화면에 500개의 Circle을 0.005초마다 생성하는 Creator 오브젝트가 있다.  
이 경우 Circle 오브젝트가 인스턴스화 될 때마다 데이터의 사본이 생성되는데, 스크립터블 오브젝트를 사용하면 이를 방지할 수 있다.

```c++
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class ColoredCircle : MonoBehaviour
{
    [SerializeField] private ColorData _colorData;

    // 인스턴스 생성 시 데이터의 사본이 생김
    [SerializeField] private Color _color;

    void Start()
    {
        // 데이터의 사본이 생기지 않음
        this.GetComponent<SpriteRenderer>().color = _colorData.Color;
    }
}
```

![](/assets/images/unity_circleCreator.gif){: .align-center}