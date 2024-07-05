---
layout: single
title:  "Programming - 객체지향의 SOLID 원칙"
categories:
  - Programming
---

---

### SOLID 5가지 원칙

---

[자세한 내용 - 커밋으로 보기](https://github.com/joonyle99/CPlusPlus_Study/commit/dc18a4d9e2d2a07a91d0cab1dbb49d64650279b6#diff-e3537b0b8347509c13f676f06bdea9ff4573f99e50ebb7b6f7249c16ab1f3e05)  
[자세한 내용 - 파일로 보기](https://github.com/joonyle99/CPlusPlus_Study/blob/dc18a4d9e2d2a07a91d0cab1dbb49d64650279b6/CPlusPlus_Review/CPlusPlus_Review/OOP%20SOLID%20principle.cpp)

 
1. **단일 책임 원칙 - SRP(Single Responsibility Principle)**
- 작성된 클래스는 <u>하나의 기능(책임)만 가진다</u>
- '응집력'과 '결합력'을 고려해 클래스에 '하나의 책임'만을 부여한다

<br>

2. **개방 폐쇄 원칙 - OCP(Open Close Principle)**
- <u>인터페이스의 내부 구현은 '개방' 인터페이스의 외부 구조는 '폐쇄'</u>
- 예지력을 동원해 적절한 크기로 '추상화(인터페이스)'하는 작업이 필요하다
- 인터페이스는 가능하면 변경되어서는 안된다. 변경이 되는 것과 안되는 것을 확실하게 하자

<br>

3. **리스코브 치환의 원칙 - LSP(Liskov Substitution Principle)**
- <u>'기반 클래스' 대신 '파생 클래스'의 인스턴스를 사용해도 문제가 없어야 한다</u>
- 그렇게 해도 '프로그램의 정확성에 영향을 미치지 않아야' 한다

<br>

4. **인터페이스 분리의 원칙 - ISP(Interface Segregation Principle)**
- <u>'하나의 큰 인터페이스'보다 필요한 메서드만을 제공하는 '여러 개의 구체적인 인터페이스'</u>를 만들자
- '유연성'이 증가하고 '의존성'이 줄어든다

<br>

5. **의존성 역전의 원칙 - DIP(Dependency Inversion Principle)**
- <u>'고수준 모듈'이 '저수준 모듈'에 의존하지 않도록</u> 한다
- 둘 다 '추상화에 의존'하도록 한다
- '고수준 모듈 DataManager'에서 '저수준 모듈 FileStorage 및 Database'의 세부 사항에 의존하지 않도록 한다