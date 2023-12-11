---
layout: single
title:  "C++ - cin과 getline()"
categories:
  - Cpp
---

---

### 데이터 읽는 방식
---

cin : cin은 <u>공백</u>이나 <u>개행 문자('\n')</u>를 기준으로 데이터를 구분하여 읽는다.  
getline : getline은 <u>개행 문자('\n')</u>를 기준으로 데이터를 읽어온다.

```c++
	int age; string name;

	cout << "Enter your age and name : ";
	cin >> age >> name;

	// 예시 입력: 25 John Doe
	// age에는 25가 들어가고, name에는 "John"만 들어가게 됨

	cout << "=====================================" << endl;

	string fullName;

	cout << "Enter your full name: ";
	getline(std::cin, fullName);

	// 예시 입력: John Doe
	// fullName에는 "John Doe"가 들어가게 됨
```

### 개행 문자 처리
---

cin : cin은 개행 문자를 읽고, 버퍼에 남겨둔다.  
getline : 개행 문자를 읽은 후, 제거하기 때문에 버퍼에 남지 않는다.

```c++
int num;
char ch;

cout << "Enter a number and a character: ";
cin >> num >> ch;

// 예시 입력: 42 A
// num에는 42가 들어가고, ch에는 'A'가 들어가지만 '\n'은 버퍼에 남게 됨

string sentence;

cout << "Enter a sentence: ";
getline(std::cin, sentence);

// 예시 입력: Hello World
// sentence에는 "Hello World"가 들어가게 됨
```

### cin과 getline을 함께 사용하면 생기는 문제
---

cin 다음에 getline을 사용할 때 발생하는 문제는 주로 개행 문자('\n') 때문이다.  
cin으로 데이터를 읽을 때 사용자가 엔터 키를 누르면 개행 문자가 입력 버퍼에 남아 있게 되는데, 이 개행 문자가 getline에 영향을 주게된다.

```c++
std::string number;
std::string name;

std::cout << "Enter a number: ";
std::cin >> number;               // 엔터로 입력 종료 후, '\n'가 입력 버퍼에 남아있음

std::cout << "Enter your name: "; // 남아있는 '\n' 때문에 변수에 값을 입력받기 전에 입력 종료
std::getline(std::cin, name);

std::cout << "Number: " << number << std::endl;
std::cout << "Name: " << name << std::endl;
```

![](/assets/images/cpp_cin_ignore1.png){: .align-center}

 이런 경우, cin.ignore() 함수를 사용하여 '\n'를 무시한다.

```c++
  // 남은 개행 문자를 무시
    std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
```

![](/assets/images/cpp_cin_ignore2.png){: .align-center}


<!--
### C++의 입출력 라이브러리
---

![](https://modoocode.com/img/2361DC4954A0CB38040ED8.webp){: .align-center}{: width="50%" height="50%"}

C++의 모든 입출력 클래스는 `ios_base` 를 기반 클래스로 한다.  
이를 상속받는 `ios` 클래스는 <u>스트림 버퍼를 초기화 하고 현재 입출력 작업의 상태를 처리</u>한다.  
이를 상속받는 `istream`은 <u>입력을 수행하는 입력 스트림</u>, `ostream`은 <u>입력된 내용을 출력하는 출력 스트림</u>이다.

`std::cin` 은 `istream` 클래스의 객체이며, `iostream` 클래스에 존재한다.  
`std::cout` 은 `ostream` 클래스의 객체이며, `iostream` 클래스에 존재한다.

![](/assets/images/cpp_cinNcout.png){: .align-center}
*iostream 클래스에 존재하는 istream의 객체 cin, ostream의 객체 cout*
-->

<!--
`operator >>` 가 `istream`에 내장 함수로 오버로딩되어 있다.

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

이 처럼 operator >> 는 내장 함수로 다양한 자료형에 대해 오버로딩 되어있어, 타입 상관없이 입력을 받을 수 있는 것이다.

그러면 여기 있는 자료형 말고는 `>>` 를 사용할 수 없을까? 물론 할 수 있다.

```c++
std::string s;
std::cin >> s;
```

`std::string` 은 cin으로 입력받을 수 있는데, 이유는 멤버 함수를 istream에 내장 함수로 두는 것 말고도, <u>외부 함수로 연산자 오버로딩을 할 수 있기 때문이다.</u>

```c++
// istream 클래스가 아닌 외부에서 오버로딩한다.
istream& operator >> (istream& in, std::string& s)
{
  // string 입력 코드
}
```
-->

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
