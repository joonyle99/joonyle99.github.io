---
layout: single
title:  "C++ - 입출력 라이브러리"
categories:
  - cpp
---

---

### 목표
---

istream, ostream 클래스와 >>, << operator에 대해 알아보고 C++에서의 입출력에 대해 알아본다.

### 입출력 라이브러리
---

![](https://modoocode.com/img/2361DC4954A0CB38040ED8.webp){: .align-center}{: width="50%" height="50%"}

C++의 모든 입출력 클래스는 `ios_base` 를 기반 클래스로 한다. `ios` 클래스는 <u>스트림 버퍼를 초기화 하고 현재 입출력 작업의 상태를 처리</u>한다.

### istream과 operator >>
---


```c++
std::cin >> a;
```

`istream` 은 실제로 입력을 수행하는 클래스이다. `operator >>` 가 해당 클래스에 정의되어 있으며, `cin` 은 `istream` 클래스의 <u>객체</u>이다.

```c++
istream& operator>>(bool& val);

istream& operator>>(short& val);

istream& operator>>(unsigned short& val);

istream& operator>>(int& val);

istream& operator>>(unsigned int& val);

istream& operator>>(long& val);

istream& operator>>(unsigned long& val);

istream& operator>>(long long& val);

istream& operator>>(unsigned long long& val);

istream& operator>>(float& val);

istream& operator>>(double& val);

istream& operator>>(long double& val);

istream& operator>>(void*& val);
```

이 처럼 operator >> 는 다양한 자료형에 대해 오버로딩 되어있어, 타입 상관없이 입력을 받을 수 있는 것이다.

그러면 여기 있는 자료형 말고는 `operator >>` 를 사용할 수 없을까? 답은 x이다.  

```c++
std::string s;
std::cin >> s;
```

`std::string` 은 cin으로 입력받을 수 있는데, 이유는 멤버 함수를 두는 것 말고도, <u>외부 함수로 연산자 오버로딩을 할 수 있기 때문이다.</u>

```c++
// istream 클래스가 아닌 외부에서 오버로딩한다.
istream& operator >> (istream& in, std::string& s)
{
  // 구현한다
}
```

<!--

'operator >>'의 또다른 특징으로 모든 공백문자를 무시해버린다는 특징이 있다.

```c++
// 주의할 점
#include <iostream>
using namespace std;
int main() {
  int t;
  while (true) {
    std::cin >> t;
    std::cout << "입력 :: " << t << std::endl;
    if (t == 0) break;
  }
}
```

![image](https://modoocode.com/img/26385A3654A0F64C0EEDDB.webp){: .align-center}

위와 같이 코드를 짜게 되면, 'c' 입력시 무한루프에 빠지게 된다. 왜 그러한 현상이 생기는지 공백문자를 무시한다는 특징과 함께 살펴보자.  

ios 클래스에서 스트림의 상태를 관리한다고 하였는데, 이때 스트림의 상태를 관리하는 플래그 4개가 정의되어있다.

* goodbit : 스트림에 입출력 작업이 가능할 때
* badbit : 스트림에 복구 불가능한 오류 발생시
* failbit : 스트림에 복구 가능한 오류 발생시
* eofbit : 입력 작업시에 EOF 도달시

위와 같은 상황일때 어떤 비트가 켜질까? 답은 'failbit'이다. int 자료형에 char 자료형을 넣었기 때문이다. 이 경우 입력값을 받지 않고 return 해버리는데, 버퍼에 남아있는 "c\n" 이 문자열은 손대지 않기 때문에 쓰레기값이 무한루프로 출력되는 것이다.

```c++
// 해결 방안
#include <iostream>
#include <string>

int main() {
  int t;
  while (std::cin >> t) {
    std::cout << "입력 :: " << t << std::endl;
    if (t == 0) break;
  }
}
```

이와같이 조건문으로 'std::cin >> t'를 넣어주면 해결되는데, 자세히 살펴보면.

```c++
operator void*() const;
```

이 함수는 ios 객체를 void*로 변환하는데, failbit와 badbit가 모두 off라면 nullptr가 아닌 값을 리턴한다. 즉 스트림에 정상적으로 입출력 작업을 수행 할 수 있을 때만 nullptr이 아닌 값을 리턴한다는 것이다. 따라서 while()문이 정상적인 입출력인 경우에만 실행되는 코드가 된다.

-->