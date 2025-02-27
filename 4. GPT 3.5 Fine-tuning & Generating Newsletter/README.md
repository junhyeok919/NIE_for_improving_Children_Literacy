### 🔖목차
- [4. GPT 3.5 Fine-tuning & 뉴스레터 생성 자동화](#4-gpt-35-fine-tuning--뉴스레터-생성-자동화)
  - [4.1. Prompt engineering을 통한 뉴스레터 생성](#41-prompt-engineering을-통한-뉴스레터-생성)
  - [4.2. Fine-tuning을 통한 뉴스레터 생성(Ver1)](#42-fine-tuning을-통한-뉴스레터-생성-ver1)
  - [4.3. Fine-tuning을 통한 뉴스레터 생성(Ver2)](#43-fine-tuning을-통한-뉴스레터-생성-ver2)
  - [4.4. 뉴스레터 적합성 검증](#44-뉴스레터-적합성-검증)

---

# 4. GPT 3.5 Fine-tuning & 뉴스레터 생성 자동화 

본 프로젝트의 두 번째 핵심 내용은 **생성형 AI를 사용해 신문기사 본문을 뉴스레터로 변환하는 것이었습니다. OpenAI의 API**를 사용하여 Prompt engineering 및 Fine-tuning을 반복 수행 후, 안정적으로 뉴스레터를 생성하는 기능을 개발했습니다. 

## 4.1. Prompt engineering을 통한 뉴스레터 생성

최초 시도는 Fine-tuning 없이 단순 Prompt engineering만을 사용했습니다. OpenAI API를 연결하여 저희가 확보한 뉴스 데이터셋에 대한 뉴스레터 생성을 시도했습니다.

**📍1. System prompt** : 뉴스레터 생성 작업에 적합한 성격과 역할을 입력함

- 문제-풀이-답 형태로 구성하는 CoT prompt skill을 참고하여 작성했습니다.
- 어린이들에게 친숙한 뉴스레터 형식으로 작성하기 위해 말투, 이모지에 대한 설정도 명시함

<img src="https://github.com/user-attachments/assets/a3755bf4-7fa3-4db2-b093-b9a25a2ba783" width="50%">

**📍2. User prompt :** 답변 생성에 참고할 수 있는 데이터를 입력함

- [뉴스레터 양식] : 실제 뉴스레터 중 정치, 경제, 국제, 사회 카테고리에 관련된 뉴스레터 40건을 샘플로 수집하고, GPT-3.5에 하나씩 예시로 입력함
- [일반적인 기사] : 완성해둔 최종 데이터셋에서 랜덤하게 40개의 신문기사를 선택해서 하나씩 입력함 (=뉴스레터로 변환해야 할 대상)
- [어려운 어휘] : NF-iDF로 산출한 어려운 어휘 중 상위 6개를 입력해서 해당 어휘에 대해서 설명하도록 지시함

**➡️출력 결과물**

- 답변 결과가 양호하긴 했지만, 반복 수행해보니 형식의 변동이 커서 일관적인 서비스 제공에 사용하기는 어려운 수준이라고 판단했습니다.

<img src="https://github.com/user-attachments/assets/d2e31f21-8918-4504-b458-24a200aea665" width="50%">

## 4.2. Fine-tuning을 통한 뉴스레터 생성 (Ver.1)

앞서 발생한 출력 변동성의 문제를 해결하기 위해, OpenAI에서 제공하는 Fine-tuning 기능을 사용했습니다. 이전과 달리 **User prompt / Assistant prompt 쌍을 정교하게 설계**하여 일관된 형태로 양질의 답변을 낼 수 있도록 모델을 튜닝했습니다. 

> - System prompt : GPT 모델의 성격, 수행할 역할 등을 명시
> - User prompt : (Assistant prompt와 쌍이 되도록) 명확한 input 데이터를 입력해야 함⭐
> - Assistant prompt : 모델의 출력을 조절하기 위해 명확한 output 데이터를 입력해야 함⭐

**📍1. System prompt**

- 앞선 prompt learning과 동일하게 입력함

**📍2. User prompt** 

- 완성해둔 최종 데이터셋에서 랜덤하게 40개의 신문 기사를 선택해 [일반적인 기사]로 알려줌

**📍3. Assistant prompt** 

- User prompt에 입력한 40개의 신문 기사에 대한 뉴스레터 40건을 직접 작성하여 [뉴스레터 양식]으로 알려줌
- 40건의 뉴스레터는 ChatGPT를 통해 초안을 생성 후, 직접 NIE 목적에 맞게 정제하여 작성함

**➡️출력 결과물**

- 이전보다 답변 결과가 좀 더 양호해졌지만, 어려운 어휘에 대한 설명이 부족하고, 출력 형식의 변동도 여전히 간헐적으로 발생하는 것을 확인했습니다.

<img src="https://github.com/user-attachments/assets/ec9f330d-cfb1-4d3f-98a1-79721677d9dd" width="65%">

## 4.3. Fine-tuning을 통한 뉴스레터 생성 (Ver.2)

답변 품질을 더욱 향상시키기 위해 다양한 요소를 조정했습니다. 인퍼런스를 반복하며 System prompt를 개선하고, tuning 데이터셋을 더욱 정교하게 수정했습니다. 추가로 하이퍼파라미터 튜닝도 수행했습니다.

**📍1. System prompt** 

- 출력 형식을 4가지 단락으로 고정시키도록 prompt를 수정함

<img src="https://github.com/user-attachments/assets/0172f93c-0248-4a81-a561-0cd7c1bb2074" width="55%">

**📍2. User prompt** 

- 이전과 동일하게 랜덤한 40개의 신문 기사를 [일반적인 기사]로 알려줌

**📍3. Assistant prompt** 

- 수정된 system prompt에 맞게 뉴스레터 40건을 다시 작성하여 [뉴스레터 양식]으로 알려줌
- Hallucination을 방지하기 위해 답변 평가 기준 6항목을 정의하고, 취약한 부분을 집중적으로 개선(예시 뉴스레터 재작성)함

<img src="https://github.com/user-attachments/assets/f84b52d1-71f4-4029-9ac1-3ee61982c3f9" width="50%">

**📍4. Hyperparameter Tuning :** 모델 답변 개선 과정에서 하이퍼파라미터를 조정하는 과정도 거쳤습니다. 파라미터별 조정 결과는 아래와 같습니다.

- `Temperature=0.9` : **모델의 자유도**와 관련된 파라미터로, 1에 가까워질수록 자유도가 올라가며 더욱 창의적인 문구가 생성됩니다. 1을 초과할 경우 제대로 된 결과물이 출력되지 않고, 지나치게 낮은 수(ex. 0.5)로 설정할 경우 뉴스레터의 요약이 부족함을 확인했습니다.
- `Max_tokens=1800` : **결과물의 출력 길이**와 관련된 파라미터입니다. 무작정 크게 설정할 경우 불필요하게 긴 [마무리] 파트가 출력되는 문제가 발생하는 것을 확인하고 조정했습니다.
- `Presence_penalty=0.3` : **새로운 대답을 반환하는 정도**를 나타내는 파라미터로, 0~2 중에서 높게 설정할수록 새로운 대답을 반환합니다. 어려운 신문 텍스트를 쉬운 말들로 새롭게 풀어쓰는 작업이 필요하기 때문에, 0보다는 크게 설정하도록 했습니다.
- `Frequency_penalty=0` : **동일한 대답을 반환하는 정도**를 나타내는 파라미터로, 0~2 중에서 높게 설정할수록 동일한 대답을 반환합니다. 쉬운 말들로 새롭게 풀어쓰는 작업이 필요하기 때문에, 초기값 그대로 설정했습니다.

**➡️출력 결과물**

- 이전보다 출력 형식의 변동이 눈에 띄게 줄어들었고, 어려운 어휘에 대한 설명도 빠뜨리지 않고 잘 설명해주는 것을 확인했습니다. 50회 인퍼런스 결과 전부 안정적인 출력을 보여 서비스 기능으로 활용하기 적합하다고 판단했습니다.

<img src="https://github.com/user-attachments/assets/1fea9163-860b-40e2-a9fc-a7ca8e92b467" width="65%">

## 4.4. 뉴스레터 적합성 검증

**📌뉴스레터의 이독성(독해 난이도)**

- ‘어린이 맞춤형 뉴스레터 생성’이라는 목적을 달성하기 위해서는, 출력 형태뿐만 아니라 생성된 **뉴스레터의 난이도** 또한 검증이 필요했습니다. 따라서, fine-tuned GPT를 통해 생성한 2,237건의 뉴스레터에 대하여 ‘신문 텍스트 이독성 지표’를 전부 측정했습니다. 그리고 이를 변환 이전의 신문 기사에 대한 이독성과 비교했습니다.
- 그 결과, 생성된 **뉴스레터의 독해 난이도가 원본 기사보다 더 낮은(=쉬운) 것을 검증**할 수 있었습니다. (세부 변수별 분포 역시 요인분석 때 확인한 가중치 방향성에 전부 부합함을 확인했습니다)

<img src="https://github.com/user-attachments/assets/2e9904bd-ab07-448a-b34f-db1b6edfba61" width="30%">

**📌뉴스레터 생성 비용** (2023년 11월 기준)

- 뉴스레터 1건 당 $0.015 (한화 약 20원) / 5~10초 소요
    - Fine-tuning된 GPT-3.5-Turbo 모델은 Input 1천 토큰 당 $0.003, Output 1천 토큰 당 $0.006가 청구됩니다. 1개의 기사를 입력하고 대응하는 어린이 뉴스레터를 출력하기 위한 평균 토큰은 대략 Input 2,000개, Output 1,000개입니다.
- Fine-tuning 비용 $2.7 (한화 약 3,500원)
    - GPT-3.5-Turbo 모델의 파인튜닝은 1천 토큰에 대해 $0.008가 청구됩니다. 현재 BigKids-GPT는 (일반 기사, 어린이 뉴스레터) 쌍 40건에 대해 약 30만 개의 토큰을 학습했습니다.

➡️ 위와 같이 뉴스레터의 독해 난이도와 생성 비용을 모두 검토해본 결과, 본 프로젝트에서 개발한 모델(BigKids-GPT)을 활용하면, NIE 서비스의 맞춤형 뉴스레터 생성 기능을 효율적으로 구현할 수 있음을 확인했습니다.

