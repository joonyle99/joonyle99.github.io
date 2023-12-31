---
layout: single
title:  "Programming - 줄 바꿈 방식 (CR / LF / CRLF)"
categories:
  - Programming
---

---

### Warning
---

Github Blog를 업로드하기 위해 `git add .`를 하다가, 다음과 같은 경고가 떴다.

![](/assets/images/curious_warning.png){: width="70%" height="70%"}

사실 전에도 몇 번 본 적이 있었는데, 별일 없어서 넘어갔다가 이번에는 신경 쓰여서 알아보기로 했다.

이 용어들은 타자기에서 유래되었다고 하는데

* `CR (Carrige Return)` : 커서를 현재 라인의 처음으로 이동시켜 텍스트를 한 줄 아래로 내림 (\r)
* `LF (Line Feed)` : 커서를 다음 라인으로 이동 (\n)
* `CRLF (CR + LF)` : CR과 LF를 연속적으로 사용해 줄 바꿈을 나타냄 (\r\n)

현재 Unix 계열의 OS인 Linux, MacOS는 `LF`를 사용하지만, Windows는 `CRLF`방식을 사용한다. 이렇게 서로 다른 줄 바꿈 방식을 사용하는 것은 텍스트 파일을 교환할 때 문제를 일으킬 수 있기 때문에 에디터가 자동으로 처리해 준다.

이 정보를 토대로 다시 경고를 해석해 보면 `LF`는 `CRLF`으로 대체될 것이라는 뜻이다. 실제로 해당 파일에 들어가 보니 `LF`를 사용 중이었다.

![](/assets/images/curious_LF.png)

따라서 현재 실행되고 있는 운영체제 Windows의 줄 바꿈 방식에 맞게 `CRLF`으로 자동으로 바꾼다는 경고였던것이다.