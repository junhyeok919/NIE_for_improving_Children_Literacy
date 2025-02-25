### 🔖목차
- [5. 웹 서비스 구현](#5-웹-서비스-구현)
  - [5.1. 서비스 소개](#51-서비스-소개)
  - [5.2. 서비스 사용 형상 (UI)](#52-서비스-사용-형상-ui)

---

# 5. 웹 서비스 구현

## 5.1. 서비스 소개

- 앞서 연구한 2가지 핵심 요소 (이독성 지표, 뉴스레터 생성 GPT)를 바탕으로, 처음에 제기했던 신문활용교육의 한계점을 해소할 수 있는 서비스를 개발해보았습니다. 서비스 명칭은 BigKids이며, 서비스의 핵심 엔진은 크게 3가지로 구성했습니다.

### 💡BigKids 서비스의 핵심 엔진 3가지

**① 이독성 지표 제공**
- 뉴스 빅데이터에 대한 요인분석을 통해 신문기사의 '이독성'을 측정합니다. 이 지표를 활용해 사용자는 학습자 수준에 맞는 신문기사를 선택할 수 있습니다. 또한 본인이 입력한 신문기사가 학습자의 수준에 맞는지도 확인할 수 있습니다.
- <img src="https://github.com/user-attachments/assets/cdc54477-794e-413d-aaf0-502c91eecf27" width="70%">


**② 어린이 맞춤형 뉴스레터 제공**
- 사용자가 선택한 신문기사에 대해 어린이(청소년) 맞춤형 뉴스레터를 생성합니다. 이는 읽기 쉽게 풀어 쓴 글이고 어휘도 설명되어있기 때문에, 신문기사에 대한 거부감과 진입장벽을 낮추는 데 도움이 됩니다.
- 앞서 개발한 fine-tuned GPT를 기반으로 생성하기 때문에 인력과 비용을 획기적으로 절감할 수 있습니다.
- <img src="https://github.com/user-attachments/assets/9edd0377-d30f-400f-8f1e-9c1e74300001" width="70%">


**③ 신문기사에 어울리는 학습자료 제공**

- 선행연구와 기존 NIE 자료들을 참조해 4개 영역(독해력, 창의력, 어휘력 의사소통능력)의 학습자료를 구성했습니다. 또한 GPT 3.5에 prompt learning를 사용한 **퀴즈 생성 기능**을 추가했습니다. 
- 이와 같은 학습자료를 통해 전문지식이 없는 일반인도 가정에서 쉽게 신문활용교육(NIE) 및 학습이 가능하며, 교육현장에서도 다방면에서 활용이 가능할 것으로 예상합니다.
- <img src="https://github.com/user-attachments/assets/2c0fc5eb-6b99-4957-ab86-c259286357a2" width="70%">

## 5.2. 서비스 사용 형상 (UI)

- 위 기능들을 Figma를 사용해 프로토타입으로 구현했습니다. 사용자가 편리하게 사용할 수 있는 User Interface를 설계하고, 실제로 구동이 가능하도록 프레임워크를 구현했습니다.
- [여기](https://www.figma.com/proto/NFGfxoCi3jDeoNnQXzSXwq/BigKids-웹-프로토타입?page-id=0%3A1&node-id=215-213&p=f&viewport=33%2C330%2C0.04&t=Kdp2pzl532Cz8y6E-1&scaling=min-zoom&content-scaling=fixed&starting-point-node-id=215%3A213)를 클릭하시면 프로토타입 demo를 사용해보실 수 있습니다.

<img src="https://github.com/user-attachments/assets/e0dbe89a-7de8-4f6b-ad7f-7fefc0128b1d" width="70%">

