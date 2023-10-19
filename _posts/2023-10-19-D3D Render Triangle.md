---
layout: single
title:  "DirectX 3D 11 Tutorial 02 - Render Triangle"
---

---

DirectX3D 11에서는 Vertex 정보가 Buffer Resource에 저장된다. Vertex 정보를 저장하는 데 사용되는 Buffer를 Vertex Buffer라고 한다.  Vertex Buffer를 세 개의 Vertex가 들어갈 수 있을 만큼 충분히 큰 Vertex Buffer를 생성하고 Vertex들로 채워야 한다.

​

DirectX3D 11에서는 Buffer Resource를 생성할 때 Buffer 크기를 Byte 단위로 지정해야 한다. Buffer가 Vertex 세 개가 들어갈만큼 충분히 커야 한다는 것은 알고 있지만 각 Vertex에는 몇 Byte가 필요할까? 이 질문에 답하려면 Vertex Layout에 대한 이해가 필요하다.

Vertex에는 Position 좌표가 있거나, Normal Vector, Color, Texture 좌표(Texture Maping) 등과 같은 다른 속성도 가지고 있는 경우가 많다. Vertex Layout은 각 속성이 사용하는 데이터 유형, 각 속성의 크기, 메모리 내 속성의 순서 등 이러한 속성이 메모리에 위치하는 방식을 정의한다. 버텍스의 크기는 구조체의 크기에서 편리하게 구할 수 있습니다.

이 튜토리얼에서는 정점의 위치로만 작업한다. 따라서 Vertex를 XMFLOAT3 유형의 단일 필드로 Struct 구조체에 정의한다.

​

이제 버텍스를 나타내는 구조체가 생겼다. 이는 시스템 메모리에 Vertex 정보를 저장하는 역할을 한다. 하지만 Vertex 정보만 들어있는 Vertex Buffer를 GPU에 전달하는 것은 메모리 덩어리를 전달하는 것에 불과할 것이다. Buffer에서 Vertex의 올바른 속성을 추출하려면 GPU가 Vertex Layout 대해서도 알고 있어야 한다.

​

-> Vertex 정점 정보가 가진 속성을 적절히 GPU에 전달하기 위해서는 Input Layout을 사용한다.

DirectX3D 11에서 Input Layout은 GPU가 이해할 수 있는 방식으로 Vertex의 구조를 설명한다. 각 Vertex의 속성은 D3D11_INPUT_ELEMENT_DESC 구조체로 설명할 수 있다. D3D11_INPUT_ELEMENT_DESC를 하나 이상의 배열로 정의한 다음, 해당 배열을 사용하여 Vertex 전체를 설명하는 Input Layout 객체를 생성한다.

이제 Shader에 대해 알아보자. Vertex Shader는 Vertex Layout과 긴밀하게 결합되어 있는데, 그 이유는 Vertex Layout 오브젝트를 생성하려면 Vertex Shader의 Input Signature 필요하기 때문이다. 위 그림에서 POSITION이 Input Signature이다.

D3DCompileFromFile에서 도출된 ID3DBlob 객체를 사용하여 Vertex Shader의 Input Signature를 나타내는 Blob을 확보한다. 

이후 ID3D11Device::CreateInputLayout()을 호출하여 Vertex Layout 객체를 생성하고,

ID3D11DeviceContext::IASetInputLayout()을 호출하여 Vertex Layout을 파이프라인에 설정할 수 있다.

이제 다음으로 해야할 작업은 Vertex Data를 저장하는 Vertex Buffer를 생성하는 것이다. DirectX3D 11에서 Vertex Buffer를 생성하려면 두 개의 구조체, D3D11_BUFFER_DESC와 D3D11_SUBRESOURCE_DATA를 채운 다음 ID3D11Device::CreateBuffer()를 호출해, Vertex Buffer를 생성할 수 있다.

​

D3D11_BUFFER_DESC : Vertex Buffer 객체에 대한 서술 구조체

D3D11_SUBRESOURCE_DATA : Vertex Buffer에 "복사"할 실제 데이터(정점 데이터)에 대한 서술 구조체

​

Vertex Buffer에 복사되는 Vertex Data는 vertices, 즉 SimpleVertex 구조체의 배열이다.

버텍스 버퍼가 생성되면 ID3D11DeviceContext::IASetVertexBuffers()를 호출하여 Device에 Binding 한다.

정리하자면 정점 데이터를 SimpleVertex 구조체를 통해 정의하고, Vertex Layout를 생성해 Vertex의 속성 구조를 설명한다. 이후 정점 데이터를 Vertex Buffer에 복사한다.

Primitive Topology는 GPU가 삼각형을 렌더링하는 데 필요한 세 개의 정점을 얻는 방법을 말한다. 모든 렌더링되는 오브젝트는 삼각형의 집합으로 이루어지며 세 개의 정점이 모여 삼각형을 이룬다.

하나의 삼각형을 렌더링하기 위해서는 3개의 정점을 GPU로 전송해야 한다. 따라서 Vertex Buffer에는 3개의 Vertex가 있다. 두 개의 삼각형을 렌더링하려면 어떻게 해야 할까? 한 가지 방법은 GPU에 6개의 정점을 보내는 것이다. 처음 세 개의 정점은 첫 번째 삼각형을 정의하고 두 번째 세 개의 정점은 두 번째 삼각형을 정의한다. 이 Topology를 Triangle List라고 한다. Triangle List는 이해하기 쉽다는 장점이 있지만, 매우 비효율적이다. 연속적으로 렌더링된 삼각형이 정점을 공유할 때 이러한 경우가 발생한다. 예를 들어 그림 3a는 두 개의 삼각형으로 구성된 정사각형을 보여준다 (일반적으로 삼각형은 정점을 시계 방향으로 나열하여 정의). Triangle List를 사용하여 이 두 삼각형을 GPU에 전송하면 vertex Buffer는 다음과 같은 상태가 된다.

A B C C B D 

B와 C 정점은 Vertex Buffer에 두 번 들어가게 된다. 왜냐하면 두 삼각형은 맞붙어있고, 두 정점을 공유하고 있기 때문이다.

​

이 문제를 개선하기 위해, 두 번째 삼각형을 렌더링할 때 Vertex Buffer에서 세 개의 정점을 모두 가져오는 대신 이전 삼각형의 정점 중 2개를 사용하고 Vertex Buffer에서 새로운 정점 1개만 가져온다고 GPU에 알려주면 Vertex Buffer를 더 작게 만들 수 있다. 이 기능은 DirectX3D에서 지원되며 이 Topology를 Triangle Strip이라고 한다.

​

Triangle Strip을 렌더링할 때 첫 번째 삼각형은 Vertex Buffer의 처음 세 개의 버텍스로 정의되고, 다음 삼각형은 이전 삼각형의 마지막 두 버텍스와 버텍스 버퍼의 다음 버텍스로 정의된다. 그림 3a의 사각형을 예로 들어 Triangle Strip을 사용하면 Vertex Buffer는 다음과 같다.

​

A B C D

​

이렇게 Triangle Strip Topology를 사용하면 Vertex Buffer 크기가 6개의 버텍스에서 4개의 버텍스로 줄어든다.

마찬가지로 그림 3b를 Triangle Strip을 사용해서 VB에 넣어보자.

​

Triangle List : A B C C B D D E C

​

Triangle Strip : A B C D E

​

Triangle Strip을 사용할 때 Vertex Buffer의 정점이 삼각형을 그릴때 순서가 시계 방향 순서를 형성하지 않는 경우가 있다. 이 경우 자연스러운 현상이며, 이를 극복하기 위해 GPU는 이전 삼각형에서 오는 두 정점의 순서를 자동으로 바꾼다. 이 작업은 두 번째 삼각형, 네 번째 삼각형, 여섯 번째 삼각형, 여덟 번째 삼각형 .. 등의 경우에만 수행된다. 이렇게 하면 모든 삼각형의 꼭짓점 감기 순서가 시계 방향이 된다.

위 그림은 Triangle List를 사용해 중복되는 꼭짓점이 VB에 들어있는 상태로 작업을 할 것이라는 설정이다.

삼각형의 실제 렌더링을 수행하는 코드이며, 렌더링을 위해 Vertex Shader와 Pixel Shader를 만들었다. Vertex Shader는 삼각형 각각의 정점을 올바른 위치로 변환하는 역할을 하고, Pixel Shader는 삼각형의 각 픽셀에 대한 최종 출력 색상을 계산한다 (사실 완전한 최종은 아니다 Output Merger Stage가 있기 때문).

​

ID3D11DeviceContext::VSSetShader() 와 ID3D11DeviceContext::PSSetShader()를 호출 해 셰이더를 사용한다. 마지막으로 ID3D11DeviceContext::Draw()를 호출하여 현재 Vertex Buffer, Vertex Layout 및 Primitive Topology를 사용하여 삼각형을 렌더링하도록 GPU에 명령한다. Draw()의 첫 번째 파라미터는 GPU에 전송할 버텍스 수이고, 두 번째 파라미터는 전송을 시작할 첫 번째 버텍스의 인덱스이다. 하나의 삼각형을 렌더링하고 Vertex Buffer의 시작부터 렌더링하기 때문에 두 매개변수에 각각 3과 0을 사용한다.