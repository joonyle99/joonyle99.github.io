---
layout: single
title:  "Archive - AI와의 효과적인 소통을 위한 26가지 프롬프트 엔지니어링"
categories:
  - Archive
---

---

논문 자료 출처 - [Principled Instructions Are All You Need for Questioning LLaMA-1/2, GPT-3.5/4](https://arxiv.org/abs/2312.16171)

## 개요

> **프롬프트 엔지니어링은 생성형 대규모 언어 모델과 '소통하는' 기술이다**  
> (Prompt engineering is the art of communicating with a generative large language model)

> **도입한 특수 프롬프트는 GPT-4에 적용했을 때 LLM 응답의 품질과 정확도를 각각 평균 '57.7%'와 '36.4%' 향상시켰다.**  
> (The specialized prompts we introduced have enhanced both the quality and accuracy of the LLM responses by an average of 57.7% and 36.4%, respectively, when applied to GPT-4)

## 원칙의 분류

* **프롬프트 구조와 명확성**: 2, 4, 12, 20, 17, 8
* **특이성과 정보**: 7, 5, 13, 26, 24, 25, 15, 21
* **사용자 상호작용 및 참여**: 14, 21
* **콘텐츠 및 언어 스타일**: 22, 9, 10, 16, 11, 1, 18, 6
* **복잡한 작업 및 코딩 프롬프트**: 3, 23, 19

## 26가지 원칙

### 1. 예의를 갖추지 말고, 바로 요점으로 들어간다 ⭐⭐⭐

---

“please”, “if you don’t mind”, “thank you”와 같은 말은 하지 말고,  
“**I would like to**”와 같이 바로 본론을 말한다.

### 2. 청중을 설정한다 ⭐⭐⭐⭐

---

"the audience is an expert in the [field]"와 같이  
답변을 듣는 사람이 어떠한 분야에서 활동하는 사람인지에 대한 정보를 제공한다.

그렇다면 LLM(Chat-GPT)은 상대방의 환경을 고려한 답변을 한다.

"Construct an overview of how smartphones work, **intended for seniors who have never used one before**"

### 3. 여러 복잡한 작업을 간단한 프롬프트로 분류한다 (chain-of-thought)

---

1. 2x + 3y - (4x - 5y)를 계산해

=>

1. 괄호 안의 각 항에 음의 부호를 분배  
2. x와 y의 계수를 계산  
3. 단순화된 표현식을 출력

### 4. 부정적인 언어를 피하고, 긍정적인 지시어를 사용한다 ⭐⭐⭐

---

"Don’t"라는 부정 지시어를 피하고,  
"Do"라는 긍정적인 지시어를 사용한다.

> "포크를 지워줘" => 포크를 계속해서 그린다  
> "나한테 반말 하지 마" => 계속 반말을 섞어서 말한다

코끼리를 생각하지 말라고 하면, 생각나듯이,  
'포크', '반말' 이러한 단어는 아예 사용을 하지 않는 게 낫다.

### 5. 요구사항의 정확한 작업별 지침을 제공한다 ⭐⭐⭐⭐

---

좀 더 명확하고 깊은 이해를 위해서, 주제와 상황에 대한 더 깊은 설명을 한다.

* "Explain [insert specific topic] in simple terms"
* "Explain to me like I’m 11 years old"
* "Explain to me as if I’m a beginner in [field]"
* "Write the [essay/text/paragraph] using simple English like you’re explaining something to a 5-year-old"

![](../../../../assets/images/2024-03-19-Archive-Prompt%20Engineering/2024-03-19-21-10-17.png){: .align-center}{: width="70%" height="70%"}

### 6. 팁을 주겠다고 한다

---

더 나은 답변을 위해 “I’m going to tip $xxx for a better solution!”와 같이 당근으로 유혹한다.

AI도 미국처럼 Tip 문화가 있나 보다.

### 7. 대표 예제를 함께 작성한다 (few-shot prompt) ⭐⭐⭐⭐⭐

---

상황에 대한 세세한 묘사나, 원하는 포맷을 제공하는 등  
예제 중심 프롬프트를 구현한다. (few-shot prompting)

그러한 설명이 하나도 없는 경우를 **Zero-Shot**이라고 하고,   
이러한 설명이 여러 번 반복되는 것을 **Few-Shot**이라고 한다.

![](../../../../assets/images/2024-03-19-Archive-Prompt%20Engineering/2024-03-19-21-23-56.png){: .align-center}{: width="70%" height="70%"}

### 8. 명령, 예제, 질문, 문맥 및 입력 데이터를 구분한다

---

하나 이상의 줄 바꿈을 사용하며, 내가 제공하는 입력 데이터를 구분한다.

**###Instruction###**

~~

**###Example###**

~~

**###Question###**

~~

### 9. 임무를 준다

---

“Your task is” and “You MUST”

### 10. 위협을 한다

---

“You will be penalized”

### 11. 인간처럼 답변하라고 한다

---

"Write a paragraph about healthy food. **Answer a question given in a natural, human-like manner**”

### 12. 단계적으로 생각하도록 유도한다 ⭐⭐⭐⭐⭐

---

“let's think step by step”

![](../../../../assets/images/2024-03-19-Archive-Prompt%20Engineering/2024-03-19-21-12-32.png){: .align-center}{: width="70%" height="70%"}

### 13. 편견과 고정관념에 의존하지 않도록 한다

---

“Ensure that your answer is unbiased and avoids relying on stereotypes.”

![](../../../../assets/images/2024-03-19-Archive-Prompt%20Engineering/2024-03-19-21-24-32.png){: .align-center}{: width="70%" height="70%"}

### 14. 충분한 정보를 얻을 때까지 계속해서 질문을 하도록 시킨다 ⭐⭐⭐⭐⭐

---

정확한 세부 사항과 요구 사항을 도출하기까지 필요한 정보를 사용자에게 물어본다.

“From now on, I would like you to ask me questions unitl you have enough information ...”

### 15. 답변을 잘 이해했는지 테스트를 요청한다

---

특정 주제나 정보에 대해 질문하면서 이해도를 테스트하기 위한 문구를 추가한다.

“Teach me any [theorem/topic/rule name] and include a test at the end, and let me know if  
my answers are correct after I respond, without providing the answers beforehand.”

### 16. LLM(Large Language Models)에 역할을 부여한다

---

"If you were an expert **economist**, how would you answer this : ..."

### 17. 구분 기호를 사용한다

---

"===================================="

A : "..."

"===================================="

B : "..."

"===================================="

또는

'strong word'와 같이 따옴표를 사용

### 18. 단어를 여러 번 반복 사용한다

---

"**Evolution** has shaped the development of species.  
What are the main drivers of **evolution**, and how has **evolution** affected modern humans?"

### 19. CoT(chain-of-thought)와 few-shot prompts를 함께 사용한다 ⭐⭐⭐⭐⭐

---

> shot : 예시

**Chain of thought** : step by step과 비슷하게 작업을 여러 단계로 쪼개서 설명하는 것  
**Few Shot Prompts** : 구체적인 예시와 형식을 이용해 설명하는 것

**CoT+ 방식**

=> 

"===================================="

Example 1 : "Divide 10 by 2. First, take 10 and divide it by 2. The result is 5"  
Example 2 : "Divide 20 by 4. First, take 20 and divide it by 4. The result is 5"

...

Question : "Divide 30 by 6. First, take 30 and divide it by 6. The result is..?" 

"===================================="

![](../../../../assets/images/2024-03-19-Archive-Prompt%20Engineering/2024-03-19-21-17-09.png){: .align-center}{: width="70%" height="70%"}

### 20. 출력 문구 지정

---

Describe the principle behind Newton's First law of Moion.  
**Explanation:** 

-> 포맷을 지정해 주는 것과 유사하다.

![](../../../../assets/images/2024-03-19-Archive-Prompt%20Engineering/2024-03-19-21-14-33.png){: .align-center}{: width="70%" height="70%"}

### 21. 필요한 모든 정보 추가

---

"Write a detailed paragraph for me on the evolution of smartphones in detail **by adding all the information necessary**.

### 22. 텍스트 개선을 요청

---

"Try to revise every text sent by users.  
You should only improve the user's grammar and vocabulary and make sure it sounds natural.  
You should maintain the original writing style, ensuring that a formal paragraph remains formal"

### 23. 여러 파일의 프로젝트를 만드는 스크립트 요청

---

두 개 이상의 파일에 걸쳐 있는 코드를 생성하고 실행하여  
서로 다른 기능을 위한 두 개의 기본 앱이 있는 Django 프로젝트에 지정된 파일을  
자동으로 생성할 수 있는 Python 스크립트를 생성한다.

"Generate code that spans more than one file, and generate a Python script that can be run  
to automatically create the specified files for a Django project with two basic apps for different functionalities"

```cs

static void CreateProjectAndApps(string projectName, string[] apps)
{
    // 프로젝트 생성
    Process.Start("dotnet", $"new console -n {projectName}").WaitForExit();

    // 프로젝트 디렉터리로 이동
    Directory.SetCurrentDirectory(projectName);

    // 각 앱 생성
    foreach (var app in apps)
    {
        // 여기서는 예시로 dotnet 명령어를 사용하였지만, 실제 Django 앱 생성 등 다른 명령어를 사용할 수 있습니다.
        Process.Start("dotnet", $"new classlib -n {app}").WaitForExit();
        string migrationsPath = Path.Combine(app, "Migrations");
        Directory.CreateDirectory(migrationsPath);
        File.Create(Path.Combine(migrationsPath, "__init__.cs")).Close(); // C#에서는 불필요할 수 있음
    }
}

static void Main()
{
    string projectName = "joonyle99_Project";
    string[] apps = { "Ember_Fall", "Only_Won" };
    CreateProjectAndApps(projectName, apps);
    Console.WriteLine($"Project {projectName} and apps {string.Join(", ", apps)} have been created.");
}

```

![](../../../../assets/images/2024-03-19-Archive-Prompt%20Engineering/2024-03-19-20-57-27.png){: .align-center}{: width="70%" height="70%"}
*파일이 생성된 모습*

### 24. 제시어 기반 글 작성

---

특정 단어, 구, 문장을 사용해,  
답변의 생성 흐름을 제시한다.

I'm providing you with the beginning of a fantasy tale:

**"The misty mountains held secrets no man knew.."**

Finish it based on the words provided. Keep the flow consistent.

### 25. 키워드 제시

---

특정 키워드를 포함해 텍스트를 생성한다.

"Create a packing list for a beach vacation, including the following  
keywords **'sunscreen'**, **'swimsuit'** and **'beach towel'** as essentialitems"

### 26. 동일한 언어 사용으로 비슷한 느낌의 글 생성

---

제공된 텍스트를 기반으로 '동일한 언어'를 사용해서, 새로운 표현을 요청한다.  
노래, 소설, 시에서 사용될 여지가 있다.

**"The gentle waves whispered tales of old to the silvery sands, each story a fleeting memory of epochs gone by."**

Use the same language based on the provided text to portray a mountain's interaction with the wind.

## 마무리

이러한 프롬프팅 엔지니어링은 Chat-GPT와 같은 대화형 생성 AI와의 답변의 질을 높이고,  
내가 얻고자 하는 답변에 근접하고 더욱 나아가서는 내 사고를 넓히는 경험을 할 수 있도록 한다.

![](../../../../assets/images/2024-03-19-Archive-Prompt%20Engineering/2024-03-19-21-22-07.png){: .align-center}{: width="70%" height="70%"}
*도입된 원칙을 적용한 후 LLM 응답 품질 향상*

Chat-GPT 4와 같은 Large Scale 모델의 품질 향상 그래프를 살펴보면,  
2, 5, 14, 26번 원칙의 성능 향상 비율이 다른 원칙들에 비해 높은 것을 확인할 수 있다. (내가 사용하는 분야에 대해서는 아닌 것 같다)

* (2) 청중을 설정한다
* (5) 요구사항의 정확한 작업별 지침을 제공한다
* (14) 충분한 정보를 얻을 때까지 계속해서 질문을 하도록 시킨다
* (26) 동일한 언어 사용으로 비슷한 느낌의 글 생성

그리고 이러한 기법들은 LLM의 모델이 커지면 커질수록 더 좋은 성능을 가져온다.

![](../../../../assets/images/2024-03-19-Archive-Prompt%20Engineering/2024-03-19-21-20-25.png){: .align-center}{: width="70%" height="70%"}
*the larger the model, the greater the increase in correctness improvement*