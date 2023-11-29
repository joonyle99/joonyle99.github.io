---
layout: single
title:  "C++ - const vs constexpr"
categories:
  - cpp
---

---

### 공통점
---

const 또는 constexpr가 붙은 변수에 값이 할당된 이후, 값의 수정이 불가능

```c++
	// 상수 변수

	const int a = 100;
	a = 1000;	// 값 변경 불가능

	constexpr int b = 200;
	b = 2000;	// 값 변경 불가능
```

### 차이점
---

![](/assets/images/cpp_constVsconstexpr.png){: .align-center}

const : Run-Time에"도" 값을 결정  
constexpr : Compile-Time에 값을 결정

```c++
    int temp;
    std::cin >> temp;				// 런타임에 값 할당

    const int a = temp + 100;		// 가능
    constexpr int b = temp + 200;	// 불가능 -> temp는 std::cin에 의해 런타임에 정해지는 변수
                                    // constexpr 변수는 컴파일 타임에만 값을 결정
```

### 결론
---

constexpr을 사용할 수 있는 경우라면, const 대신 constexpr을 이용하자.  
런타임에 값이 변경되지 않음을 보장할 수 있다.