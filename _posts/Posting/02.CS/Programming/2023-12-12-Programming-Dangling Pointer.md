---
layout: single
title:  "Programming - 댕글링 포인터 (Dangling Pointer)"
categories:
  - Programming
---

---

### 이미 해제된 메모리 주소에 접근하는 현상
---

이미 해제된 메모리에 접근하는 현상은 "댕글링 포인터"라고 한다.

유효한 포인터는 해당 포인터를 가지고 멤버 변수나 멤버 함수에 접근할 수 있다.  
하지만 이미 메모리가 해제되어 유효하지 않은 포인터는 멤버에 접근하려고 하면 오류나 예기치 못한 상황이 발생한다.

```c++
#include <iostream>

using namespace std;

class Team
{
private:
	string teamName;

private:
	string topName;
	string jugName;
	string midName;
	string adName;
	string supName;

public:
	Team()
		: teamName("T1"),
			topName("zeus"), jugName("oner"), midName("faker"),
			adName("gumayusi"), supName("keria")
	{}
	~Team() = default;

public:
	void PrintTeamName() { cout << teamName << endl; }
	void PrintAllPlayerName()
	{
		cout << "Top : " << topName << endl;
		cout << "Jug : " << jugName << endl;
		cout << "Mid : " << midName << endl;
		cout << "Ad : " << adName << endl;
		cout << "Sup : " << supName << endl;
	}
};

int main()
{
	// 포인터에 값을 동적 할당
	Team* pTeam = new Team;
	cout << "Team : " << pTeam << endl;
	pTeam->PrintTeamName();
	pTeam->PrintAllPlayerName();

	// 메모리를 해제 (쓰레기 값)
	delete pTeam;
	cout << "Team : " << pTeam << endl;

  // 이미 해제된 메모리에 접근하는 "댕글링 포인터" 발생
	pTeam->PrintTeamName();

	// 해제된 메모리를 nullptr로
	pTeam = nullptr;
	cout << "Team : " << pTeam << endl;
	pTeam->PrintTeamName();

	return 0;
}
```

위 예제는 댕글링 포인터가 발생하는 상황을 나타낸 것이다.

개발자가 힙 메모리에 직접 할당하고 직접 해제하는 동적할당은 실수가 많이 나오는 부분인데, 이러한 댕글링 포인터를 피하기 위한 몇 가지 주의사항이 있다.

1) 포인터를 다시 사용하기 위해 유효성을 검사한다.
```c++
	// 유효성 검사 (nullptr인지 확인)
	if (pTeam != nullptr)
		pTeam->PrintTeamName();
```

접근하려는 포인터가 nullptr인지 확인하고, 유효한 경우에만 멤버에 접근한다. 하지만 메모리 해제 시 nullptr이 아닌 쓰레깃값이 들어갈 수 있기 때문에 이 방법만으로는 부족하다.

2) 포인터를 해제한 후 nullptr로 만들어 주기
```c++
	// 메모리를 해제
	delete pTeam;
  // 해제된 메모리를 nullptr로
	pTeam = nullptr;
```

이렇게 두 단계를 거치면 댕글링 포인터를 예방할 수 있다. 이 부분을 습관화해두지 않으면 밤새 댕글링 포인터만 찾는 경험을 하게 될 수도 있다. 조심하자
