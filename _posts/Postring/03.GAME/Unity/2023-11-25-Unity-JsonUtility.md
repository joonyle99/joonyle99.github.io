---
layout: single
title:  "Unity - JsonUtility"
categories:
  - unity
---

---

### 세션 간 데이터 지속성 구현
---

기본적으로 유니티는 게임을 종료하고 다시 시작하면, 이전 데이터들이 초기화된 상태로 시작된다. (개발자가 미리 설계해 둔 설정)  
하지만 게임을 다시 켰을 때, 이전 플레이 기록이 남아있을 필요가 있는 경우가 존재한다. (게임의 저장 시스템, 랭킹 시스템 등)

온라인 게임의 경우, 게임을 진행하면서 생긴 데이터들은 서버에 저장되지만, 로컬 게임의 경우 데이터가 플레이어의 로컬 환경에 저장된다.
어떠한 경우에서든 데이터의 지속성을 보장하기 위해서는 변경된 데이터를 저장해야 하는데, 이를 위해 JSON 방식을 사용한다.

### JSON(JavaScript Object Notation)
---

JSON(JavaScript Object Notation)은 데이터를 저장한 후 플랫폼 간에 주고받는 데 사용되는 문자열 텍스트 형식으로,
JavaScript를 사용하지만, 언어 독립적이라 다른 프로그래밍 언어에서도 사용할 수 있다.

### JsonUtility를 이용한 데이터 지속성 구현
---

```c++
public class ColorManager : MonoBehaviour
{
    public static ColorManager Instance;

    [ColorUsage(true, true)]
    public Color color = Color.white;

    ...
}
```

이와 같이 Color 변수를 가진 ColorManager 클래스가 있다.
게임을 플레이하며 이 변수에 플레이어가 선택한 색상 정보가 저장되는데 이 색상 데이터가 세션이 끝난 후, 다음 세션에서도 사용하기 위해 다음과 같이 동작한다.

![](/assets/images/unity_jsonUtility.jpg){: .align-center}{: width="80%" height="80%"}

1. 데이터를 저장하기 위한 "직렬화"
2. 데이터를 로드하기 위한 "역 직렬화"

이처럼 데이터를 하나의 "Object"로 관리하고 "JSON"으로 변환해 데이터를 저장한다. 이후 데이터 로드를 위해 "JSON"을 "Object"로 변환해 저장된 데이터를 이용한다.

JsonUtility는 "직렬화 가능 클래스"를 사용해 데이터를 JSON 변환할 수 있도록 도와주는 유니티 제공 헬퍼 클래스이다.
이를 사용해 **SaveData** 오브젝트 클래스, **SaveColor()**, **LoadColor()** 메서드를 구현한다.

```c++
// 직렬화 가능 클래스
[System.Serializable]
class SaveData
{
    public Color color;
}

public void SaveColor()
{
    // SaveData 클래스를 생성
    SaveData data = new SaveData();

    // SaveData에 ColorManager의 color를 저장
    data.color = color;

    // SaveData 객체를 Json 파일로 변환
    string json = JsonUtility.ToJson(data);

    // Json 문자열 파일을 AppData에 쓰기
    File.WriteAllText(Application.persistentDataPath + "/savefile.json", json);

    // Debug.Log(Application.persistentDataPath);
}

public void LoadColor()
{
    // AppData의 Json파일의 경로를 가져옴
    string path = Application.persistentDataPath + "/savefile.json";

    // 파일이 있는지 확인
    if (File.Exists(path))
    {
        // Json 파일을 모두 읽어온다
        string json = File.ReadAllText(path);

        // 읽어온 Json 파일을 SaveData의 형태로 (클래스의 형태로) 변환해 데이터를 로드한다
        SaveData data = JsonUtility.FromJson<SaveData>(json);

        color = data.color;
    }
}
```

![](/assets/images/unity_appData.png){: .align-center}

유니티에서 제공하는 "파일 입출력" 시스템을 이용해 플레이어의 로컬 환경에 데이터가 JSON 파일로 저장된 모습을 볼 수 있다.