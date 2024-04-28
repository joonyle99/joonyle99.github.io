---
layout: single
title:  "DirectX 11 - DirectX 2D의 V-SYNC"
categories:
  - DirectX11
---

---

![](../../../../assets/images/2023-08-02-DirectX%2011-Collider%20copy/2024-04-28-18-56-17.png){: .align-center}

D2D에서 렌더 타겟을 만들 때, 프로퍼티 옵션을 설정해 줄 수가 있다.

![](../../../../assets/images/2023-08-02-DirectX%2011-Collider%20copy/2024-04-28-18-56-31.png){: .align-center}

그중 세 번째 인자인 D2D1_PRESENT_OPTIONS 부분이다.

![](../../../../assets/images/2023-08-02-DirectX%2011-Collider%20copy/2024-04-28-18-56-40.png){: .align-center}

옵션들은 이렇게 4가지가 존재한다.

![](../../../../assets/images/2023-08-02-DirectX%2011-Collider%20copy/2024-04-28-18-56-52.png){: .align-center}

그리고 각각의 옵션은 다음과 같은 속성을 가진다.

**D2D1_PRESENT_OPTIONS_NONE**은 기본 옵션으로 모니터의 주사율과 그래픽카드의 속도를 맞춰주는 기능이다.  
즉 DirextX 2D에서의 V-SYNC 기능인 것이다.

디버그를 위해 모니터의 주사율과 그래픽카드의 속도를 맞추지 않을 수도 있는데,  
그 옵션이 바로 **D2D1_PRESENT_OPTIONS_IMMEDIATELY** 속성을 이용하는 것이다.  
이 속성은 그래픽카드가 모니터 주사율을 기다려주지 않고 계속해서 업데이트되는데,  
이러면 생길 수 있는 문제점이 찢어짐 (Tearing) 현상이다.

모니터가 갱신되기 전에 그래픽카드가 다음 프레임으로 넘어간다.  
모니터에 한 프레임이 전부 그려지지 않았는데 그래픽카드는 새로운 프레임을 그리기 때문에,  
모니터가 다음 프레임을 그리려고 할 때면 훨씬 이후의 프레임이 모니터에 그려지다가 화면이 찢어지는 것처럼 보이는 것이다.

결론  
: 디버그 모드에서는 V-SYNC 모드를 해제하고 프레임을 체크하며 작업하고,  
나중에 릴리스 빌드 할 때는 제한을 걸어 모니터 주사율만큼만 갱신되도록 하자..! 가 되겠다.