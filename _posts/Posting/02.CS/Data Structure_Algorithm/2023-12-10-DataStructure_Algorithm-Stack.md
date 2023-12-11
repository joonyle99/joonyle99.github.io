---
layout: single
title:  "Data Structure / Algorithm - 스택 (Stack)"
categories:
  - DataStructure_Algorithm
---

---

### 스택 자료구조
---

![](https://blog.kakaocdn.net/dn/CSWsW/btq2t9Wc0um/qTQgiVXqjxA0l9weuUKTH1/img.png){: .align-center}
*[스택 자료구조](https://buildgoodhabit.tistory.com/141)*

스택은 LIFO(Last In First Out) 구조로 <u>나중에 들어간 것이 먼저 나오는 자료구조</u>이다.

### 스택이 활용되는 곳
---
1. 스택 메모리 (함수의 복귀 주소를 호출 스택에 저장)
2. 실행취소 (Ctrl + Z)
3. 웹 브라우저의 앞 / 뒤 페이지 이동
4. 문자열 역순화
5. 재귀적 알고리즘

### 스택을 왜 사용할까?
---
리스트나 배열을 사용해도 스택과 같이 동작할 수 있는데 굳이 스택(Stack)을 사용하는 이유는 뭘까?  
그 이유는 바로 <u>자료구조의 추상화</u> 때문이다. 인터페이스로 실제 코드를 숨기면서 에러 발생률을 낮추고 필요한 작업만 실행할 수 있다.  
실제로 스택은 리스트 / 배열과 달리 가장 처음 들어간 데이터에 바로 접근할 수 없고 데이터의 삽입과 삭제가 한쪽면에서만 이루어진다는 특징이 있다.

![](https://blog.kakaocdn.net/dn/eregHj/btrAfDRtW67/ufHF4LV7bbpRv1TIKLkQDk/img.png){: .align-center}{: width="50%" height="50%"}
*[프링글스의 스택 자료구조](https://roi-data.com/entry/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-4-%EC%8A%A4%ED%83%9DStack%EC%9D%B4%EB%9E%80-%EC%97%B0%EC%82%B0-%EA%B5%AC%ED%98%84%EB%B0%A9%EB%B2%95)*

가장 안쪽의 과자를 꺼내먹기 위해서는 위에서부터 하나하나 Pop하면서 먹어야 한다.

### 스택의 활용 - 웹 브라우저
---

![](https://img1.daumcdn.net/thumb/R800x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F2719483C5885B0B72C){: .align-center}

우리는 평소에 웹 브라우저를 사용하면서 앞 / 뒤 페이지 이동 버튼을 자주 사용한다.  
<u>"스택의 가장 마지막에 넣은 데이터를 먼저 가져온다"</u> 라는 특성을 활용해 기능을 구현한다.

<!--
![](/assets/images/algorithm_web.png){: .align-center}

![](/assets/images/algorithm_web2.png){: .align-center}
-->

[백준 23300번 - 웹 브라우저](https://www.acmicpc.net/problem/23300)
> 이 문제에서 압축 기능에 대해서는 설명하지 않는다.

#### 초기 세팅

```c++
	std::stack<int> backSpace; // 뒤로 이동할 페이지를 쌓아두는 곳
	std::stack<int> frontSpace; // 앞으로 이동할 페이지를 쌓아두는 곳

	int accessCount = 0; // 페이지 이동 횟수 (뒤 / 앞 페이지 이동 제외)
	int curPageNumber = 0; // 현재 페이지
```

2개의 스택을 사용한다. backSpace와 frontSpace.  
앞으로 이 공간에 뒤로 이동할 페이지와 앞으로 이동할 페이지를 쌓는다.

#### 페이지 이동

```c++
if (task == 'A')    // A : 페이지 이동
{
	// 이동할 페이지 입력
	int pageNumber;
	cin >> pageNumber;

	// 처음 접속하는 경우 뒤로 가기 공간에 추가하지 않음
    // 뒤로 이동할 페이지를 backSpace에 쌓는다
	if (accessCount != 0)
		backSpace.push(curPageNumber);

	// 페이지 이동 시 앞으로 가기 공간에 저장된 페이지가 모두 삭제된다.
	while (!frontSpace.empty())
		frontSpace.pop();

	// 입력된 페이지로 이동한다.
	curPageNumber = pageNumber;

	// 페이지 이동 카운트 증가
	accessCount++;
}
```

![](/assets/images/algorithm_web3.png){: .align-center}
*왼쪽이 backSpace, 오른쪽이 frontSpace, 가운데가 curPageNumber*

페이지 이동은 사용자가 이동할 페이지를 입력하면 curPageNumber에 그 값을 저장한다.  
이후 accessCount가 0이 아니라면 backSpace에 curPageNumber를 Push한다.

#### 페이지 뒤로 가기

```c++
else if (task == 'B')   // B : 페이지 뒤로 가기
{
    // 뒤로 가기 스택이 비어있지 않다면
	if (!backSpace.empty())
	{
		// 뒤로가면 현재 페이지를 앞으로가기 공간에 추가한다.
		frontSpace.push(curPageNumber);

		// 뒤로가기 공간의 가장 최근에 방문한 페이지로 이동
		curPageNumber = backSpace.top();

		// 뒤로가기 공간을 pop
		backSpace.pop();
	}
}
```

![](/assets/images/algorithm_web5.png){: .align-center}
*왼쪽이 backSpace, 오른쪽이 frontSpace, 가운데가 curPageNumber*

frontSpace에 curPageNumber를 넣고, backSpace의 가장 최근 페이지를 curPageNumber로 만든다.  
이후 backSpace의 최상위 페이지를 Pop한다.

#### 페이지 앞으로 가기

```c++
else if (task == 'F')   // F : 페이지 앞으로 가기
{
    // 앞으로 가기 스택이 비어있지 않다면
	if (!frontSpace.empty())
	{
		// 앞으로 가면 현재 페이지를 뒤로가기 공간에 추가한다.
		backSpace.push(curPageNumber);

		// 앞으로가기 공간의 가장 최근에 방문한 페이지로 이동
		curPageNumber = frontSpace.top();

		// 앞으로가기 공간을 pop
		frontSpace.pop();
	}
}
```

('페이지 뒤로 가기' 작업과 동일)

backSpace의 curPageNumber를 넣고, frontSpace의 가장 최근 페이지를 curPageNumber로 만든다.  
이후 frontSpace의 최상위 페이지를 Pop한다.