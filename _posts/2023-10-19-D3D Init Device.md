---
layout: single
title:  "DirectX 3D 11 Tutorial 01 - Init Device"
---

목표 : DirectX3D 11의 필수 객체와 초기화 하는 방법에 대해 알아본다.

필요 객체
1. Device
2. Device Context
3. Swap Chain
4. Render Target View

DirectX3D 10에서는 Device가 렌더링과 리소스 생성 둘 다 수행했는데, DirectX3D 11에서는 역할을 나눠 Device Context를 통해 백버퍼에 렌더링을 수행하고, Device는 리소스를 생성하는 함수를 실행한다.
​
Device : 리소스 생성 및 관리를 담당. 버퍼, 텍스처, 셰이더 등을 생성할 때 이 객체를 사용

Device Context : 실제 렌더링 명령을 하드웨어에 전달하는 역할

https://techpubs.jurassic.nl/manuals/nt/developer/Perf_GetStarted/sgi_html/figures/double.buffering.gif

Swap Chain : Device가 렌더링된 백버퍼를 가져와 실제 모니터 화면에 콘텐츠를 표시하는 역할을 담당

​

스왑 체인에는 주로 앞면과 뒷면에 두 개 이상의 버퍼가 포함되며, 모니터에 표시하기 위해 렌더링하는 텍스처임. Front Buffer는 실제 모니터에 표시되는 화면이고 ReadOnly다. Back Buffer는 장치가 그릴 렌더링 대상이다. 그리기 작업이 완료되면 스왑 체인은 두 버퍼를 스왑하여  Back Buffer를 Front Buffer에 표시한다.

https://postfiles.pstatic.net/MjAyMzEwMTJfMTI1/MDAxNjk3MDg5Mzg0NjQx.voIZOG70eWqRLmJyRXzfIeuSE1rfCkiuwZGP09z0X6Ig.-1wHcrddNa_fUDUBm0796cMP325Kxlnu_TSDxZosDQYg.PNG.gongjunyeol/image.png?type=w773

Swap Chain을 설명하는 DXGI_SWAPCHAIN_DESC 구조체​를 작성한다. 예를들어, BackBufferUsage는 백 버퍼의 사용 방법을 알려주는 플래그이다. 이 경우 Back Buffer를 렌더링하고 싶으므로 BackBufferUsage를 DXGI_USAGE_RENDER_TARGET_OUTPUT으로 설정한다. OutputWindow 필드는 Swap Chain이 화면에 이미지를 표시하는 데 사용할 윈도우를 나타낸다.

https://postfiles.pstatic.net/MjAyMzEwMTJfMTkx/MDAxNjk3MDkwNjM4NTIz.frxngpI0lGKSyV05rvnE7Y3SVmPwXAvwN1DS285pL10g.Wn8W27M3C2cq68EMM1or_cXM_kcVPXzC_e0Se6uamE4g.PNG.gongjunyeol/image.png?type=w773

이처럼, Swap Chain에 대한 서술이 끝나면, Device와 Device Context, 그리고 Swap Chain을 함께 생성한다.

https://postfiles.pstatic.net/MjAyMzEwMTJfOTEg/MDAxNjk3MDkwNDIxNzcy.4X2XFHB-lACQM-YvxcJLxTXvwoYogKMuKEKbTLfFKtIg.LFigtl5ITgYrnEQGsar320KRxn7DEpd7few9CfaxbNIg.PNG.gongjunyeol/image.png?type=w773

다음으로 해야 할 일은 Render Target View를 만드는 것이다.Render Target View는 Direct3D 11의 리소스 뷰 유형인데, 리소스 뷰를 사용하면 특정 단계에서 리소스를 그래픽스 파이프라인에 Binding할 수 있다.

​

Swap Chain의 Back Buffer를 Render Target으로 Binding하고 싶기 때문에 Render Target View를 만들어야 한다. 먼저 GetBuffer()를 호출하여 Back Buffer를 가져온 후, 선택적으로 생성할 Render Target View를 설명하는 D3D11_RENDERTARGETVIEW_DESC 구조를 채울 수 있다. 이 구조체에 대한 내용은CreateRenderTargetView의 두 번째 파라미터이인데, 튜토리얼에서는 기본 렌더 타깃 뷰로 충분하다.

https://postfiles.pstatic.net/MjAyMzEwMTJfMTcx/MDAxNjk3MDkwNTY5OTU4.R4YYThnyGMV3E1R-fZXWXoF711wjOwZA0lGMHpb6xpog.K-NyQwEThVz1O5DuN1RtD9ZwS2kf7u9Qw3AyOrZ19fUg.PNG.gongjunyeol/image.png?type=w773

Render Target View를 생성한 후에는 Device Context에서 OMSetRenderTargets()를 호출하여 파이프라인에 바인딩할 수 있다. 이렇게 하면 파이프라인이 렌더링하는 Output이 백 버퍼에 Merge된다.

https://postfiles.pstatic.net/MjAyMzEwMTJfMTAy/MDAxNjk3MDkwODY1MDQ1.wIhhKPtXAsqMhMVbmJUx_kBd2SlenFtZXZ1iiLjCHusg.NAtPVOqo4cpKU5Q-tBfW5Js8u7O3bd5SSKygp1KrqmQg.PNG.gongjunyeol/image.png?type=w773

https://postfiles.pstatic.net/MjAyMzEwMTJfMjQx/MDAxNjk3MDkwODgxMDIx.nMr3eiHY1u7S1vOsDXdtTmPsA3-UMIleag4wY-2guAAg.CbuA0AvJbY--COHy7fyjXAe-vBlscoBslagpWjojUi4g.PNG.gongjunyeol/image.png?type=w773

렌더링하기 전에 마지막으로 설정해야 할 것은 Viewport를 초기화하는 것이다. Viewport는 픽셀 공간이라고도 하는 대상 공간을 렌더링하기 위해 X와 Y는 -1에서 1까지, Z는 0에서 1까지 범위인 클립 공간 좌표를 매핑한다. Direct3D 11에서는 기본적으로 Viewport가 설정되지 않기 때문에, 먼저 뷰포트를 설정해야 한다.

https://postfiles.pstatic.net/MjAyMzEwMTJfMTIz/MDAxNjk3MDkxMjA3MjU1.6yYVMm3kOkTrF6Lg2wUw2sUdyy88XddsIzxD0kuJvAog.Cd6sMFrufg1Ho1Easz1yNS9tRZKtqWqJpKcqoIlT8fYg.PNG.gongjunyeol/image.png?type=w773

Direct3D 11에서 제공하는 인스턴스 컨텍스트 함수 ClearRenderTargetView() 메서드를 사용해 Render Target (Back Buffer)를 단일 색상으로 채운다. 그러기 위해 화면을 채울 색을 설명하는 4개의 실수 배열을 정의하고. 그런 다음 이 배열을 ClearRenderTargetView()에 전달한다. Back Buffer를 채우고 나면 Swap Chain의 Present() 메서드를 호출하여 렌더링을 완료한다. Present()는 Swap Chain의 Back Buffer 내용을 사용자가 볼 수 있도록 화면에 표시하는 역할을 한다.

https://postfiles.pstatic.net/MjAyMzEwMTJfOTcg/MDAxNjk3MDkyMTY4OTgw.vMLpAootgsZHLZU2KZFHPUlhykkk-3aCvVXSx2uLnjYg.prk-0OM17BYkNYWuzpHHWlRhRParM8ehrRKW5LBeBG0g.PNG.gongjunyeol/image.png?type=w773