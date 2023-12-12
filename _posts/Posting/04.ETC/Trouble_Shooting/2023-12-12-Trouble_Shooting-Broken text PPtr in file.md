---
layout: single
title:  "Trouble Shooting - Broken text PPtr in file (feat.YAML / Unity)"
categories:
  - Trouble_Shooting
---

---

### 갑작스러운 오류 발생
---

현재 Unity를 이용한 프로젝트를 진행 중이며, VCS(Version Control System)로 Git을 사용하고 있다.  
그러다 어느 날 팀원이 작업한 내용을 받아 작업을 하려는데 갑작스러운 에러가 떴다.

![](/assets/images/trouble_shooting_PPtr.png){: .align-center}
*에러 내용 (빨간색)*

처음 보는 에러였다. Player에 붙어있는 Animator의 내용을 변경하고 저장할 때마다 저 에러가 떠서 신경 쓰였다.  
급하게 발표를 준비하던 때라서 바쁜 상황이었지만, 다행히도 실행에는 지장이 없는 빌드 전용 오류였다.

![](/assets/images/trouble_shooting_PPtr5.jpg){: .align-center}{: width="50%" height="50%"}
*Player Animator*

### "Broken text PPtr in file" 에러의 원인
---

[여기](https://discussions.unity.com/t/what-is-this-error-message-broken-text-pptr-in-file/198824)
에서 힌트를 얻었다. "Bunny83"님 말에 따르면

> "The problem here is that some object in the file has a reference to another object, but that referenced object does not exist."

해당 파일에서 어떠한 객체를 참조하려고 하는데, 그 참조 객체가 존재하지 않아서 발생하는 문제라고 한다.

다시 에러 내용을 보았다.

![](/assets/images/trouble_shooting_PPtr2.png){: .align-center}
*Local file identifier doesn't exist!*

식별자가 존재하지 않는다는 에러 메시지를 출력하고 있다.

참조하려는 객체의 식별자(id)가 존재하지 않는 것으로 보아 저 숫자가 의심스러워 보인다, 저 식별자에 해당하는 객체가 존재하지 않는 것 같다.
Animator 파일을 뜯어서 확인해 보기로 한다.

### 직렬화 언어 YAML
---

Unity에서는 YAML이라는 직렬화 언어를 이용한 모든 Asset을 수정할 수 있다.
YAML(Yet Another Markup Language)은 XML이나 Json과 같이 나중에 재구성할 수 있는 포맷으로 변환한다.

[YAML 유니티 공식 문서](https://blog.unity.com/kr/engine-platform/understanding-unitys-serialization-language-yaml)

### 해결 방법
---

Animator 파일을 Visual Studio Code 에디터를 이용해 열어서 내부를 직접 들여다보자

![](/assets/images/trouble_shooting_PPtr6.png){: .align-center}{: width="50%" height="50%"}
*Player_AnimatorController.controller*

YAML 언어로 작성된 파일이라는 것, Animator State Transition에 대한 정보라는 것, 식별자가 있다는 것을 알 수 있었다.

의심이 간다고 했던 숫자를 Ctrl + F를 이용해 검색해 보면

![](/assets/images/trouble_shooting_PPtr3.png){: .align-center}
*이미 에러를 해결한 후라서 Git History로 대체*

존재하지 않는다고 했던 식별자를 파일에서 찾을 수 있었다.  
그리고 이 식별자에 해당하는 State Transition을 Animator에서 찾아보았는데 찾을 수 없었다. (다른 값들은 Unity Animator에서 찾을 수 있었다.)

팀원의 작업 내용이 Merge 되는 과정에서 문제가 발생해 존재하지 않는 객체를 참조하면서 발생한 것 같다.  
이후, 해당 식별자가 존재하는 객체 전부를 지원 문제를 해결했다.

### 후기
---

빌드 전용 오류라서 다행이었지, 바쁜 와중에 저러한 처음 보는 에러가 등장해 작업을 진행할 수 없다면 당황스러울 것 같다.
이번에 에러를 해결하면서 다양한 에러가 존재하는구나 싶었고, 개발자는 버그 잡는 게 익숙해져야겠다고 생각하는 계기가 되었다.