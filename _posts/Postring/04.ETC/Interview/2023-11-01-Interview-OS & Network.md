---
layout: single
title:  "Tech Interview - 운영체제 & 네트워크"
categories:
  - interview
---

---

### VM(Virtual Machine)에 대해
---

### 라운드 로빈 스케줄링(Round-Robin)에 대해
---

### 기아 상태란?
---

### 가상 메모리(Virtual Memory)의 Page, Fault, Trashing ...에 대해
---

### TCP 3 way handshake란?
---

### 메모리 레이아웃에 대해 (Code, Data, Heap, Stack)
---

### 빅 엔디안(Big Endian)과 리틀 엔디안(Little Endian)
---

##### 빅 엔디안

이 방식은 일반적으로 우리가 알고있는 숫자 사용 방식으로, 낮은 주소에 높은 자릿수부터 저장된다.  
메모리에 저장되어있는 순서 그대로 읽을 수 있으며 평소 사용하던 방식이므로 이해하기가 쉽다.

`0x12345678`이라는 4 x 8 = 32비트 크기의 정수가 있다고 가정했을때, 아래와 같이 메모리에 저장된다.

![](/assets/images/curious_bigEndian.png)

> 이 방식은 네트워크를 통해 데이터를 전송할 때 주로 사용된다.

##### 리틀 엔디안

이 방식은 빅 엔디안과는 반대인 방식으로, 낮은 주소에 낮은 자릿수부터 저장된다.

똑같이 `0x12345678`이라는 4 x 8 = 32비트 크기의 정수가 있다고 가정했을때, 아래와 같이 메모리에 저장된다.

![](/assets/images/curious_littleEndian.png)

> 이 방식은 인텔의 x86 아키텍처 환경에서 주로 사용된다.
> 따라서 인텔 기반의 시스템에서 소켓 통신을 할 때는 바이트 순서를 신경써서 데이터를 전달해야 한다.

##### Big vs Little ?

빅 엔디안과 리틀 엔디안 중 어느 방식이 더 우수하다고 말할 수는 없다. 단지 데이터를 어떤 순서로 저장하는지에 대한 차이만 있을 뿐이다.

물리적으로 데이터를 조작하거나 산술 연산을 수행할 때는 리틀 엔디안 방식이 더 효율적이고, 데이터의 각 바이트를 배열처럼 취급할 때는 빅 엔디안 방식이 더 적합하다.