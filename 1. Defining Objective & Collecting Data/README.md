### 🔖목차
- [0. 프로젝트 배경](#0-프로젝트-배경)
  - [0.1. 주제 선정 배경](#01-주제-선정-배경)
  - [0.2. 선행 연구 조사](#02-선행-연구-조사)
- [1. 데이터 수집 & 전처리](#1-데이터-수집--전처리)
  - [1.1. 수집 대상](#11-수집-대상)
  - [1.2. 데이터 크롤링](#12-데이터-크롤링)
  - [1.3. 신문 텍스트 전처리](#13-신문-텍스트-전처리)

---

# 0. 프로젝트 배경

## 0.1. 주제 선정 배경

1. **어린이 및 청소년 세대 문해력 저하가 심각한 문제로 대두되고 있음**
    - 문해력 저하 및 장문 기피 현상에 대한 기사량 증가
    - OECD 국제학업성취도평가에서 ‘읽기’ 분야 순위가 하락세를 보이며, 기초적 읽기 역량에서 낮은 정답률을 보임
    - 교육부에서도 청소년 문해력 수준의 심각성을 인지하고, ‘기초 문해력 강화’를 위해 국어 교과  시수를 34시간 증대  
      ([≪’2022 개정 교육과정’ 총론 주요사항 발표≫](https://www.moe.go.kr/boardCnts/viewRenew.do?boardID=294&boardSeq=89671&lev=0&searchType=null&statusYN=W&page=1&s=moe&m=020402&opType=N) 참고)
2. **문해력 저하의 해결책으로서 ‘신문 활용 교육’은 꾸준한 관심과 수요가 존재해옴**
    - 김희정(2022), 이현주(2018) 등 다양한 연구에서 학습 자료로서 신문의 활용 가능성을 논의해오고 있음
    - 초등신문 활용 교육 도서가 베스트셀러 ([2023.08 기준](https://www.chosun.com/culture-life/book/2023/08/05/SH3AVJ7WUVFCBGDWDAT4S2TZN4/)) 1위를 차지하기도 하고, 신문 텍스트를 활용한 강의가 유아/청소년 온라인 강의 플랫폼(꾸끄)의 인기 목록을 차지함 
3. **하지만, NIE 교재나 전문가의 강의 없이는 일반인들이 신문 기사를 교육자료로 활용하기 어려움**
    - ‘어떤’ 신문이 교육자료로 활용 가능한지 판단하기 어려움 ➡️ 신문 빅데이터 분석을 통한 이독성(readability) 지표를 산출하자!
    - 교육자료로 쓸 신문을 골랐다고 해도, ‘어떻게‘ 교육에 활용할지 생각하기 어려움 ➡️ 생성형 AI를 활용해 어린이 맞춤형 뉴스레터 및 교육자료를 생성하자!

## 0.2. 선행 연구 조사

### 해외 자료

- Lexile 지수
    - 문장 복잡도(문장 길이, 문법적 복잡성), 어휘 난이도(로그 어휘 빈도, 학습자 어휘수준) 등
    - 범위 : 0~2000 / 공식 비공개
- Atos Book Level (=AR 지수)
    - Book level : 단어의 평균 길이, 난이도, 평균 문장 길이, 어휘 개수 등으로 판단 / 범위 : 0.1 ~ 16.9
    - Interest level : 책 내용 및 주제를 바탕으로 판단 / 범위 : LG ~ UG
- Flesch-Kincaid Readability Tests (FKRT)
    - Flesch Reading Ease (FRE) : 단어당 음절 수, 문장당 단어 수 → 점수 높을수록 읽기 쉬운 텍스트 / 범위 : 0~100 / 공식 공개
    - Flesch-Kincaid Grade Level (FKGL) : 단어당 음절 수, 문장당 단어 수 → 점수 높을수록 더 높은 학년 수준에 적합한 텍스트 / 범위 : 0.1 ~ 16.9 / 공식 공개
- Coh-metrix
    - 서사성, 응집성, 결속 구조, 어휘 다양성, 문법 복잡성 등 다양한 지표 제공

### 국내 자료

- 국립국어원 연구
    - 서상규. (2022). 2022년 국어 기초 어휘 선정 및 어휘 등급화 연구. 국립국어원, 1-125.
    - 김중섭. (2017). 국제 통용 한국어 표준 교육과정 적용 연구. 국립국어원, 1-503.
- 관련 학술 논문
    - 전지은. (2022). 신문 코퍼스의 어휘 난이도 분석 및 활용 연구. 언어사실과 관점, 56, 265-291.
    - 윤경선 & 이유미. (2014). 유아 한글 교육용 어휘 목록 선정을 위한 연구. 어문론집, 59, 65-84.
    - 김희정. (2022). 한국어 중급 학습자의 문해력 향상을 위한 읽기 교육 연구
    - 구민지. (2013). 한국어 읽기 교육을 위한 텍스트 난이도 측정법 연구

### 방향성 수립

- **한국어에는 ‘텍스트 난이도’를 측정하는 공식적인 지표가 부재하며,** 기존 연구에서 사용된 어휘 목록 기반의 난이도 측정 방식은 목록에 없는 새로운 단어가 등장했을 때 측정하기 곤란함. (난이도 과대평가되는 부분 발생)
    - → 한국어 문법, 문장 구조 등 **텍스트 자체의 속성**을 활용하여, **보다 일반화 가능한 형태로** 정량적 난이도를 산출하고자 함
    - → 실제 신문 기사 텍스트를 다량으로 수집 & 분석하여 '**신문 텍스트에 특화된 한국어 독해 난이도**'를 산출하고자 함
- 기존의 신문활용교육은 특정 기사에 맞게 학습 활동을 구상해야 하므로 많은 인력이 소요됨
    - → LLM(GPT 3.5 turbo)을 신문활용교육에 맞게 조정(tuning)하여, 보다 효율적인 신문활용교육 방안을 제시하고자 함
    - → 분석한 텍스트 속성 및 난이도 지표를 함께 결합하여 데이터 분석 결과를 실질적으로 활용하고자 함

</br>

---

</br>

# 1. 데이터 수집 & 전처리

## 1.1. 수집 대상

- 신문 기사의 성질을 대표할 수 있도록 **국내 4대 일간지의 최근 10개년**(2013-2022) **기사 본문**을 분석 대상으로 설정했습니다.
    - 중앙일보, 한겨레, 동아일보, 조선일보의 본문을 수집했습니다. (이후에 사용할 ‘물결21’ 신문 코퍼스 역시 동일한 4종의 신문기사로 이루어져 있어 일관성 측면에서도 적절하다고 판단했습니다)
    - 실제 서비스에서 수요가 있을 것으로 판단한 4개 카테고리 (정치, 경제, 사회, 국제)로 나누어 수집했습니다. (주요 카테고리 중 ’문화’ 면도 있었지만, 직접 눈으로 최근 뉴스들을 전부 확인해본 결과, 지나치게 사적인 이야기나 사설에 가까운 기사들이 대부분이었기에 제외했습니다)

## 1.2. 데이터 크롤링

- 1차로 각 신문사 포털에서 **BeautifulSoup 라이브러리**를 활용해 크롤링을 시도했습니다.
    - 그런데 기사 원문 전체가 잘 크롤링되지 않을 뿐더러, 뉴스 페이지에 연도별 필터가 없어서 공평한 기사 추출이 불가능했습니다. (최근 기사부터 수집되다보니, 12월의 기사만 수집되어 데이터 편향이 발생했습니다)
    - 또한, 조선일보의 경우는 저작권 상의 문제로 크롤링 자체가 어려운 상황이었습니다. 따라서, 조선일보를 경향신문으로 대체하기로 했습니다. (다른 여러 뉴스들 가운데 품질이나 발간량이 조선일보를 대체하기 가장 적합하다고 판단했습니다)
- 해결 방법을 찾던 중, **[빅카인즈](https://www.bigkinds.or.kr/)라는 플랫폼**에서 뉴스 검색에 용이한 필터들을 제공하는 것을 발견했습니다.
    - 일자/언론사별로 검색 필터를 제공할 뿐만 아니라, 광고나 주변 아이콘 등의 불필요한 텍스트 없이 기사 본문만 깔끔하게 출력되어 크롤링하기에 매우 적합하다고 판단했습니다.
    - 기사 본문에 대해 **Selenium을 통해 동적 크롤링**을 시도해본 결과 정상적으로 크롤링에 성공했습니다. 따라서 해당 방법을 반복하여 신문 기사 텍스트를 수집했습니다.
- 약 10일 간 웹 크롤링 작업을 수행하여, 최근 10개년(2013-2021)에 대한 자료를 수집 완료했습니다.
    - 기사의 대표성 확보를 위해 언론사별/월별 Top Keyword를 활용했습니다. 빅카인즈 API에서 제공하는 ‘TopN 키워드 API’를 사용해, 각 언론사/월마다 Top5 키워드를 추출했습니다. (역시 정치, 경제, 사회, 국제 4개 카테고리에 적용했습니다)
    - 각 연도별로 960건 (신문사 4종 * 카테고리 4개 * 키워드 Top5 * 12달)의 기사를 수집하여, 총 9,600건의 기사 본문 텍스트를 수집했습니다.

## 1.3. 신문 텍스트 전처리

- 수집 완료된 데이터셋을 점검해본 결과, **분석에 부적합한 기사들이 발견되어 정제 작업을 진행**했습니다.
    - 정제 대상 : NA, 포토기사, 인사이동, 부고, 사설, 행사 안내, 단순 수치, 광고성 기사 등
- 기사 본문에서 난이도 추출에 방해되는 요소를 제거했습니다.
    - 정제 대상 : 특수문자 및 기호, 한자 및 일본어, 이메일 주소, url 등
- 최종적으로 9,600건의 신문기사 본문을 모두 정제 후, **9,520행의 신문 기사 텍스트 데이터셋**을 확보했습니다.
<img src="https://github.com/user-attachments/assets/7605f142-24a6-46b8-942f-e80a1d385e38" width="70%">

