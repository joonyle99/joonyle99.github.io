---
layout: single
title:  "DirectX 3D 11 Tutorial 01 - Init Device"
---

---

목표 : DirectX3D 11의 필수 객체와 초기화 하는 방법에 대해 알아본다.

1. Device
2. Device Context
3. Swap Chain
4. Render Target View

---

### Device & Device Context
---

Device : 리소스 생성 및 관리를 담당. 버퍼, 텍스처, 셰이더 등을 생성할 때 이 객체를 사용  
Device Context : 실제 렌더링 명령을 하드웨어에 전달하는 역할

> DirectX3D 10에서는 Device가 렌더링과 리소스 생성 둘 다 수행했는데, DirectX3D 11에서는 역할을 나눠 Device Context를 통해 백버퍼에 렌더링을 수행하고, Device는리소스를 생성하는 함수를 실행한다.

### Swap Chain
---

![image](https://techpubs.jurassic.nl/manuals/nt/developer/Perf_GetStarted/sgi_html/figures/double.buffering.gif){: .align-center}

Swap Chain : Device가 렌더링된 백버퍼를 가져와 실제 모니터 화면에 콘텐츠를 표시하는 역할을 담당

스왑 체인에는 주로 앞면과 뒷면에 두 개 이상의 버퍼가 포함되며, 모니터에 표시하기 위해 렌더링하는 텍스처이다. Front Buffer는 실제 모니터에 표시되는 화면이고 Back Buffer는 장치가 그릴 렌더링 대상이다. 그리기 작업이 완료되면 스왑 체인은 두 버퍼를 스왑하여 Back Buffer를 Front Buffer에 표시한다.

```c++

    DXGI_SWAP_CHAIN_DESC swapDesc;
	ZeroMemory(&swapDesc, sizeof(swapDesc));

	swapDesc.BufferCount = 1;
	swapDesc.SwapEffect = DXGI_SWAP_EFFECT_DISCARD;
	swapDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
	swapDesc.BufferDesc.Format = DXGI_FORMAT_R8G8B8A8_UNORM;
	swapDesc.OutputWindow = g_hWnd;
	swapDesc.Windowed = TRUE;
	swapDesc.BufferDesc.Width = g_ClientWidth;
	swapDesc.BufferDesc.Height = g_ClientHeight;
	swapDesc.BufferDesc.RefreshRate.Numerator = 60;
	swapDesc.BufferDesc.RefreshRate.Denominator = 1;
	swapDesc.SampleDesc.Count = 1;
	swapDesc.SampleDesc.Quality = 0;

```

Swap Chain을 설명하는 DXGI_SWAPCHAIN_DESC 구조체​를 작성한다. 예를들어, BackBufferUsage는 백 버퍼의 사용 방법을 알려주는 플래그이다. 이 경우 Back Buffer를 렌더링하고 싶으므로 BackBufferUsage를 DXGI_USAGE_RENDER_TARGET_OUTPUT으로 설정한다. OutputWindow 필드는 Swap Chain이 화면에 이미지를 표시하는 데 사용할 윈도우를 나타낸다.

```c++

hr = D3D11CreateDeviceAndSwapChain(nullptr, g_driverType, nullptr, createDeviceFlags, featureLevels, numFeatureLevels,
			D3D11_SDK_VERSION, &swapDesc, &g_pSwapChain, &g_pDevice, &g_featureLevel, &g_pDeviceContext);

```

이처럼, Swap Chain에 대한 서술이 끝나면, Device와 Device Context, 그리고 Swap Chain을 함께 생성한다.

```c++

    /// 4. Create a render target view
	ID3D11Texture2D* pBackBuffer = nullptr;
	hr = g_pSwapChain->GetBuffer(0, __uuidof(ID3D11Texture2D), (void**)&pBackBuffer);

	if (FAILED(hr))
		return FALSE;

	hr = g_pDevice->CreateRenderTargetView(pBackBuffer, nullptr, &g_pRenderTargetView);

	// COM(구성 요소 개체 모델)에서는 메모리 관리를 위해 참조 카운트(reference count)를 사용합니다.
	// 백 버퍼를 생성할 때 참조 카운트가 증가하므로, 이를 사용한 후 반드시 참조 카운트를 감소시켜야 합니다.
	pBackBuffer->Release();

	if (FAILED(hr))
		return FALSE;

```

### Render Target View
---

다음으로 해야 할 일은 Render Target View를 만드는 것이다.Render Target View는 Direct3D 11의 리소스 뷰 유형인데, 리소스 뷰를 사용하면 특정 단계에서 리소스를 렌더링 파이프라인에 바인딩할 수 있다.

Swap Chain의 Back Buffer를 Render Target으로 바인딩하고 싶기 때문에 Render Target View를 만들어야 한다. 먼저 GetBuffer()를 호출하여 Back Buffer를 가져온 후, 선택적으로 생성할 Render Target View를 설명하는 D3D11_RENDERTARGETVIEW_DESC 구조를 채울 수 있다. 이 구조체에 대한 내용은CreateRenderTargetView의 두 번째 파라미터이인데, 튜토리얼에서는 기본 렌더 타깃 뷰로 충분하다.

```c++

    // Binding Render Target to Pipeline
	g_pDeviceContext->OMSetRenderTargets(1, &g_pRenderTargetView, nullptr);

```

Render Target View를 생성한 후에는 Device Context에서 OMSetRenderTargets()를 호출하여 파이프라인에 바인딩할 수 있다. 이렇게 하면 파이프라인이 렌더링하는 Output이 백 버퍼에 Merge된다.

```c++
    /// 5. Setup the viewport
	D3D11_VIEWPORT viewport;
	viewport.TopLeftX = 0;
	viewport.TopLeftY = 0;
	viewport.MinDepth = 0.0f;
	viewport.MaxDepth = 1.0f;
	viewport.Width = static_cast<FLOAT>(g_ClientWidth);
	viewport.Height = static_cast<FLOAT>(g_ClientHeight);

	// Binding Viewport setting to pipeline
	g_pDeviceContext->RSSetViewports(1, &viewport);
```

렌더링하기 전에 마지막으로 설정해야 할 것은 Viewport를 초기화하는 것이다. Viewport는 픽셀 공간이라고도 하는 대상 공간을 렌더링하기 위해 X와 Y는 -1에서 1까지, Z는 0에서 1까지 범위인 클립 공간 좌표를 매핑한다. Direct3D 11에서는 기본적으로 Viewport가 설정되지 않기 때문에, 먼저 뷰포트를 설정해야 한다.

### Render
---

```c++

    //--------------------------------------------------------------------------------------
    // Render the frame
    //--------------------------------------------------------------------------------------
    void Render()
    {
        constexpr FLOAT ClearColor[4] = { 0.2f, 0.2f, 0.2f, 0.5f };

        g_pDeviceContext->ClearRenderTargetView(g_pRenderTargetView, ClearColor);
        g_pSwapChain->Present(0, 0);
    }

```

Direct3D 11에서 제공하는 인스턴스 컨텍스트 함수 ClearRenderTargetView() 메서드를 사용해 Render Target (Back Buffer)를 단일 색상으로 채운다. 그러기 위해 화면을 채울 색을 설명하는 4개의 실수 배열을 정의하고. 그런 다음 이 배열을 ClearRenderTargetView()에 전달한다. Back Buffer를 채우고 나면 Swap Chain의 Present() 메서드를 호출하여 렌더링을 완료한다. Present()는 Swap Chain의 Back Buffer 내용을 사용자가 볼 수 있도록 화면에 표시하는 역할을 한다.