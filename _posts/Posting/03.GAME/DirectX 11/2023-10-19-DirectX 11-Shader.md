---
layout: single
title:  "DirectX 11 - 셰이더"
categories:
  - DirectX 11
---

---

### 목표
---

Vertex Shader, Pixel Shader, Rasterization에 대해 알아본다.

### Past Remind
---

Shader란 GPU에서 짧게 실행되는 프로그램으로, Input Data를 처리해 Output Data로 만든다.

```c++

	// Set the input layout
	g_pDeviceContext->IASetInputLayout(g_pVertexLayout);

	// Set vertex buffer
	g_pDeviceContext->IASetVertexBuffers(0, 1, &g_pVertexBuffer, &stride, &offset);

```

지난 튜토리얼의 Device 초기화 단계에서 Vertex Buffer, Vertex Layout을 Vertex Shader에 연결했다.

```c++

	// Render a triangle
	g_pDeviceContext->VSSetShader(g_pVertexShader, nullptr, 0);
	g_pDeviceContext->PSSetShader(g_pPixelShader, nullptr, 0);
	g_pDeviceContext->Draw(3, 0);

```

그리고 Render 단계에서 Vertex Shader와 Pixel Shader를 렌더링 파이프라인에 바인딩했다.

### Shader & Vertex Shader
---

Shader는 Vertex Shader, Geometry Shader, Pixel Shader가 존재하는데, Geometry Shader는 선택적이므로 기본적으로 2개만 사용한다. 그 중 **Vertex Shader**는 Input Data(정점의 위치, 노말 벡터, 색상 정보 등)를 Shader 코드에 따라 처리하고 반환한다.

```c++

float4 VS( float4 Pos : POSITION ) : SV_POSITION
    {
        return Pos;
    }

```

위의 코드는 HLSL로 작성된 Vertex Shader 코드이며, 입력받은 정점 데이터를 변환없이 그대로 출력하는 내용의 코드이다.

### Pixel Shader & Rasterization
---

이번에는 **Pixel Shader**에 대해 알아보자.  

최신 컴퓨터 모니터는 Raster Display로, 화면에 Pixel로 이루어져 있다.  
각 Pixel들은 독립적인 색상을 가지고 있으며, 삼각형 하나라도 수많은 Pixel로 이루어진다.  

![image](https://gasbebe.github.io/images/fragmentAnim.gif){: .align-center}

세 개의 꼭짓점으로 이루어진 삼각형을 픽셀의 집합으로 변환하는 과정을 **Rasterization** 이라고 하며, 다음 과정을 거친다.

1. 세 개의 꼭짓점 내부의 픽셀을 결정한다.
2. 각 픽셀에 대해 Pixel Shader를 호출한다. (각 픽셀이 어떠한 색상을 가져야 하는지 계산하는 단계)
3. 색상이 결정된 픽셀을 다음 파이프라인으로 Output한다.

```c++

float4 PS( float4 Pos : SV_POSITION ) : SV_Target
    {
        return float4( 1.0f, 1.0f, 0.0f, 1.0f );    // Yellow, with Alpha = 1
    }

```

위의 Pixel Shader 코드는 삼각형을 구성하는 모든 픽셀을 단일 색상으로 출력하는 코드이다.
