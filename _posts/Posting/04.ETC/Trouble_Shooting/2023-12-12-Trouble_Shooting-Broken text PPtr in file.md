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
그러다 어느 날 팀원이 작업한 내용을 받아 작업을 하려는데 갑작스러운 오류가 발생했다.

![](/assets/images/trouble_shooting_PPtr.png){: .align-center}
*에러 내용 (빨간색)*

Player에 붙어있는 Animator의 내용을 변경하고 저장할 때마다 저 오류가 떠서 신경 쓰였다.  
다행히도 실행에는 지장이 없는 빌드 전용 오류였기 때문에, 당장의 개발에는 영향을 주지는 않았다.

![](/assets/images/trouble_shooting_PPtr5.jpg){: .align-center}{: width="50%" height="50%"}
*Player Animator*

### "Broken text PPtr in file" 에러의 원인
---

[여기](https://discussions.unity.com/t/what-is-this-error-message-broken-text-pptr-in-file/198824)
에서 도움을 받았다.

> "The problem here is that some object in the file has a reference to another object, but that referenced object does not exist."

해당 파일에서 어떠한 객체를 참조하려고 하는데, 그 참조 객체가 존재하지 않아서 발생하는 문제라고 한다.

다시 에러 내용을 보았다.

![](/assets/images/trouble_shooting_PPtr2.png){: .align-center}
*Local file identifier doesn't exist!*

식별자가 존재하지 않는다는 오류 메시지를 출력하고 있다.

참조하려는 객체의 식별자(id)가 존재하지 않는 것으로 보아, 저 식별자에 해당하는 객체가 존재하지 않는 것 같다.
Animator 파일을 뜯어서 확인해 보기로 한다.

### Unity의 직렬화 언어 YAML
---

Unity에서는 YAML이라는 직렬화 언어를 이용한 모든 Asset을 수정할 수 있으며,
병합으로 충돌이 일어나거나 파일이 손상되는 경우, 파일을 직접 수정해야 하는 경우가 있다.

> YAML은 XML이나 Json과 같이 <u>재구성할 수 있는 포맷으로 변환</u>하는 직렬화 언어

[YAML 유니티 공식 문서](https://blog.unity.com/kr/engine-platform/understanding-unitys-serialization-language-yaml)

![](/assets/images//trouble_shooting_PPtr10.png){: .align-center}{: width="50%" height="50%"}
*Player_AnimatorController의 YAML 파일*

문제가 발생한 Animator 컴포넌트 또한 YAML 파일로 직접 수정을 할 수 있다.

### 해결 방법
---

Animator 파일을 Visual Studio Code 에디터를 이용해 열어서 내부를 직접 들여다보자  
Ctrl + F를 이용해 존재하지 않는다는 식별자(id)를 검색해 보면 다음과 같은 내용을 볼 수 있다.

![](/assets/images//trouble_shooting_PPtr8.png){: .align-center}{: width="50%" height="50%"}
*Player_AnimatorController의 YAML*

YAML 파일의 각 오브젝트에는 'File ID'라는 ID가 있다.  
이 ID는 파일 내 각 오브젝트에 대해 고유하며, 상호 간의 레퍼런스를 나타낸다.

> `3188422802415277760`가 해당 오브젝트의 File ID  
> `6534883310346915506`가 참조하려는 File ID

존재하지 않는다고 했던 식별자를 YAML 파일에서 찾을 수 있었고,  
Unity의 Animator에서는 해당 객체(3188422802415277760)와 참조하려는 객체(6534883310346915506)를 찾을 수 없었다.

팀원의 작업 내용이 Merge 되는 과정에서 문제가 발생해, 존재하지 않는 객체를 참조하면서 발생한 것이었다.  
이후, 존재하지 않는 객체를 제거해 문제를 해결했다.

### 후기
---

빌드 전용 오류라서 다행이었지, 바쁜 와중에 저러한 처음 보는 에러가 등장해 작업을 진행할 수 없다면 당황스러울 것 같다.  
이번에 에러를 해결하면서 다양한 오류가 존재하는구나 싶었고, 앞으로 어떠한 오류가 발생하더라도 해결할 수 있을 것 같다.