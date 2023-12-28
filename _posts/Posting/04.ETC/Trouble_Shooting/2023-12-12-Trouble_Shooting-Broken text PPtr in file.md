---
layout: single
title:  "Trouble Shooting - Broken text PPtr in file (feat.YAML / Unity)"
categories:
  - Trouble_Shooting
---

---

### 갑작스러운 오류 발생
---

어느 날 팀원이 작업한 내용을 받아 작업을 하려는데 갑작스러운 오류가 발생했다.

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

### Animator YAML 파일 살펴보기
---

![](/assets/images/unity_animator_YAML.png){: .align-center}{: width="50%" height="50%"}
*임의로 만든 Player Animator Controller*

임시로 간단하게 만든 Player Animator Controller의 YAML 파일이 어떻게 구성되어 있는지 살펴보자.  
Animator 파일을 Visual Studio Code 에디터를 이용해 열어서 내부를 직접 들여다보자.

```yaml
--- !u!91 &9100000
AnimatorController: # 애니메이터 컨트롤러
  m_ObjectHideFlags: 0
  m_CorrespondingSourceObject: {fileID: 0}
  m_PrefabInstance: {fileID: 0}
  m_PrefabAsset: {fileID: 0}
  m_Name: PlayerAnimatorController
  serializedVersion: 5
  m_AnimatorParameters:
  - m_Name: Jump
    m_Type: 9
    m_DefaultFloat: 0
    m_DefaultInt: 0
    m_DefaultBool: 0
    m_Controller: {fileID: 0}
  m_AnimatorLayers:
  - serializedVersion: 5
    m_Name: Base Layer
    m_StateMachine: {fileID: 4573993931456193}
    m_Mask: {fileID: 0}
    m_Motions: []
    m_Behaviours: []
    m_BlendingMode: 0
    m_SyncedLayerIndex: -1
    m_DefaultWeight: 0
    m_IKPass: 0
    m_SyncedLayerAffectsTiming: 0
    m_Controller: {fileID: 9100000}
--- !u!1107 &4573993931456193
AnimatorStateMachine: # 애니메이터 상태 머신
  serializedVersion: 6
  m_ObjectHideFlags: 1
  m_CorrespondingSourceObject: {fileID: 0}
  m_PrefabInstance: {fileID: 0}
  m_PrefabAsset: {fileID: 0}
  m_Name: Base Layer
  m_ChildStates:
  - serializedVersion: 1
    m_State: {fileID: -6806494905033057816}
    m_Position: {x: 360, y: 260, z: 0}
  - serializedVersion: 1
    m_State: {fileID: 4843047715885751131}
    m_Position: {x: 630, y: 270, z: 0}
  m_ChildStateMachines: []
  m_AnyStateTransitions:
  - {fileID: 5131914666601532872}
  m_EntryTransitions: []
  m_StateMachineTransitions: {}
  m_StateMachineBehaviours: []
  m_AnyStatePosition: {x: 650, y: 180, z: 0}
  m_EntryPosition: {x: 380, y: 190, z: 0}
  m_ExitPosition: {x: 950, y: 280, z: 0}
  m_ParentStateMachinePosition: {x: 800, y: 20, z: 0}
  m_DefaultState: {fileID: -6806494905033057816}
```

이 부분은 <u>애니메이터 상태 머신</u>과 <u>애니메이터 컨트롤러</u>에 해당하는 부분이다.  
Player Animator의 `Parameter`, `Animation State`, `Animator Layer` 등 애니메이터에 대한 다양한 정보를 포함한다.

```yaml
%YAML 1.1
%TAG !u! tag:unity3d.com,2011:
--- !u!1102 &-6806494905033057816
AnimatorState:  # Run Animation State
  serializedVersion: 6
  m_ObjectHideFlags: 1
  m_CorrespondingSourceObject: {fileID: 0}
  m_PrefabInstance: {fileID: 0}
  m_PrefabAsset: {fileID: 0}
  m_Name: Run
  m_Speed: 1
  m_CycleOffset: 0
  m_Transitions: []
  m_StateMachineBehaviours: []
  m_Position: {x: 50, y: 50, z: 0}
  m_IKOnFeet: 0
  m_WriteDefaultValues: 1
  m_Mirror: 0
  m_SpeedParameterActive: 0
  m_MirrorParameterActive: 0
  m_CycleOffsetParameterActive: 0
  m_TimeParameterActive: 0
  m_Motion: {fileID: 7400000, guid: b9d6d30bc43d6c243ae487a2228d2a64, type: 2}
  m_Tag: 
  m_SpeedParameter: 
  m_MirrorParameter: 
  m_CycleOffsetParameter: 
  m_TimeParameter: 
--- !u!1102 &4843047715885751131
AnimatorState:  # Jump Animation State
  serializedVersion: 6
  m_ObjectHideFlags: 1
  m_CorrespondingSourceObject: {fileID: 0}
  m_PrefabInstance: {fileID: 0}
  m_PrefabAsset: {fileID: 0}
  m_Name: Jump
  m_Speed: 1
  m_CycleOffset: 0
  m_Transitions:
  - {fileID: 8185673937708312599}
  m_StateMachineBehaviours: []
  m_Position: {x: 50, y: 50, z: 0}
  m_IKOnFeet: 0
  m_WriteDefaultValues: 1
  m_Mirror: 0
  m_SpeedParameterActive: 0
  m_MirrorParameterActive: 0
  m_CycleOffsetParameterActive: 0
  m_TimeParameterActive: 0
  m_Motion: {fileID: 7400000, guid: a5f90b7764561064888f856830aaeca6, type: 2}
  m_Tag: 
  m_SpeedParameter: 
  m_MirrorParameter: 
  m_CycleOffsetParameter: 
  m_TimeParameter: 
```

이 부분은 Animation State 부분이다. Run State와 Jump State를 나타낸다.

```yaml
--- !u!1101 &5131914666601532872
AnimatorStateTransition:  # AnyState -> Jump Transition
  m_ObjectHideFlags: 1
  m_CorrespondingSourceObject: {fileID: 0}
  m_PrefabInstance: {fileID: 0}
  m_PrefabAsset: {fileID: 0}
  m_Name: 
  m_Conditions:
  - m_ConditionMode: 1
    m_ConditionEvent: Jump
    m_EventTreshold: 0
  m_DstStateMachine: {fileID: 0}
  m_DstState: {fileID: 4843047715885751131}
  m_Solo: 0
  m_Mute: 0
  m_IsExit: 0
  serializedVersion: 3
  m_TransitionDuration: 0
  m_TransitionOffset: 0
  m_ExitTime: 0.75
  m_HasExitTime: 0
  m_HasFixedDuration: 1
  m_InterruptionSource: 0
  m_OrderedInterruption: 1
  m_CanTransitionToSelf: 1
--- !u!1101 &8185673937708312599
AnimatorStateTransition:  # Jump -> Exit Transition
  m_ObjectHideFlags: 1
  m_CorrespondingSourceObject: {fileID: 0}
  m_PrefabInstance: {fileID: 0}
  m_PrefabAsset: {fileID: 0}
  m_Name: 
  m_Conditions: []
  m_DstStateMachine: {fileID: 0}
  m_DstState: {fileID: 0}
  m_Solo: 0
  m_Mute: 0
  m_IsExit: 1
  serializedVersion: 3
  m_TransitionDuration: 0
  m_TransitionOffset: 0
  m_ExitTime: 0.75
  m_HasExitTime: 1
  m_HasFixedDuration: 1
  m_InterruptionSource: 0
  m_OrderedInterruption: 1
  m_CanTransitionToSelf: 1
```

이 부분은 Animation State Transition에 해당하는 부분이다. 'AnyState -> Jump'과 'Jump -> Exit'를 나타낸다.

### 해결 방법
---

Ctrl + F를 이용해 존재하지 않는다는 식별자(id)를 검색해 보면 다음과 같은 내용을 볼 수 있다.

![](/assets/images//trouble_shooting_PPtr8.png){: .align-center}{: width="50%" height="50%"}
*실제 Player_AnimatorController의 YAML*

YAML 파일의 각 오브젝트에는 'File ID'라는 ID가 있다.  
이 ID는 파일 내 각 오브젝트에 대해 고유하며, 상호 간의 레퍼런스를 나타낸다.

> `3188422802415277760`가 'Animation Transition'에 대한 식별자 File ID  
> `6534883310346915506`가 참조하려는 Transition의 '목적지 State'의 식별자 File ID

해석하면, "해당 Transition에서 참조하는 목적지 State가 존재하지 않는다"이다.

팀원의 작업 내용이 Merge 되는 과정에서 모종의 이유로 인해,  
Player Animator에 할당된 Animation을 삭제했는데, Player Animator YAML파일에는 삭제한 Animation을 참조하는 부분이 남아있고, Animation 파일만 삭제되어 발생한 문제인 것 같다.

이후, 존재하지 않는 객체를 제거해 문제를 해결했다.

### 후기
---

빌드 전용 오류라서 다행이었지, 바쁜 와중에 저러한 처음 보는 에러가 등장해 작업을 진행할 수 없다면 당황스러울 것 같다.  
이번에 에러를 해결하면서 다양한 오류가 존재하는구나 싶었고, 앞으로 어떠한 오류가 발생하더라도 해결할 수 있을 것 같다.