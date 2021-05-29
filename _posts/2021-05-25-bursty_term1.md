---
title: "[논문 리뷰] Detecting bursty terms in computer science research "
layout: post
date: 2021-05-25
image: ../assets/images/markdown.jpg
headerImage: false
tag:
- emerging issue
- term burstiness
category: blog
author: soom
description: "Detecting bursty terms in computer science research 논문 리뷰"
---

<br/>

['Emerging Issue Detection'](https://soomsoomee.github.io/emerging_issue/) 프로젝트를 진행하면서, 이머징 이슈를 측정하는 방식으로 'term burstiness'를 공부해보기로 했다.
  term burstiness를 다룬 논문은 여럿 있는데, 그 중에서도 [이 연구](https://link.springer.com/article/10.1007/s11192-019-03307-5)가 향후 bursty해질 term을 '예측'한다는 관점에서 설명이 가장 잘 되어있는 것 같다. 이머징 이슈에서는 어떤 주제가 bursty해질 것인가에 대한 '예측'이 핵심이라고 생각한다.

> **Abstract**  
Research topics rise and fall in popularity over time, some more swiftly than others. The fastest rising topics are typically called bursts; for example “deep learning”, “internet of things” and “big data”. Being able to automatically detect and track bursty terms in the literature could give insight into how scientific thought evolves over time. In this paper, we take a trend detection algorithm from stock market analysis and apply it to over 30 years of computer science research abstracts, treating the prevalence of each term in the dataset like the price of a stock. Unlike previous work in this domain, we use the free text of abstracts and titles, resulting in a finer-grained analysis. We report a list of bursty terms, and then use historical data to build a classifier to predict whether they will rise or fall in popularity in the future, obtaining accuracy in the region of 80%. The proposed methodology can be applied to any time-ordered collection of text to yield past and present bursty terms and predict their probable fate.

<br/>

## Ⅰ. Burst Detection 선행연구

### 1.1 LDA 계열: 토픽(term의 집합)의 시작점을 찾고, 변화 양상을 살펴보는 모델

  * 말뭉치를 시간에 따라 조각으로 나누고, 각 조각에 LDA를 적용해 토픽을 도출. 그 다음 기간 별로 유사도 계산을 통해 토픽을 연결하고, 토픽의 시계열적 변화를 관찰.
  * 토픽의 시간에 따른 변화를 보면서, bursty해지는 구간도 발견 가능.
  * 한계: interpretability 낮음. 일관성 있게 기간별 토픽을 연결하는 것이 어려움.

### 1.2 Kleinberg 계열: busrty terms를 먼저 찾고, 이들을 그룹지어 토픽을 도출하는 모델

  * Kleinberg(2002)의 burst detection algorithm은 문서의 단어들이 non-bursty state와 busrty state를 반복한다고 가정. 이 알고리즘을 트위터, 뉴스 등에 적용한 다양한 후속 연구 존재.
  * 그러나, scientific litereature의 경우 Klenberg의 가정과는 달리 문서가 연속적으로 생성되지 않고 'batch'로 생성됨.(예: 저널 발간 주기) 트위터나 뉴스처럼 초 단위가 아니라, 일, 월, 년 등 더 긴 단위의 time step을 사용할 필요가 있음. 
  * 또한, scientific literature의 경우 기간에 따라 corpus의 특성과 내용이 매우 달라지는 특성을 가짐.
    

<br/>

## Ⅱ.  Moving Average Convergence Divergence(MACD)

### 2.1. EMA(Exponential Moving Average)
  
  SME(Simple Moving Average)가 시계열적 데이터의 노이즈를 줄이기 위해, 과거 값들을 동일한 가중치로 평균하는 것이라면, EMA는 최신 값에 가중치를 두어 과거값들을 평균한다.
  즉, 최신의 정보를 더 많이 반영하는 것이다. EMA에 대한 자세한 내용은 아래 동영상에 잘 설명되어 있다. 
  <p align="center"><iframe width="560" height="315" src="https://www.youtube.com/embed/lAq96T8FkTw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></p>
  <p align="center"><iframe width="560" height="315" src="https://www.youtube.com/embed/NxTFlzBjS-4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></p>


### 2.2. MACD

 * Moving Average는 수렴(Convergence)하고 발산(Divergence)하는 특징이 있다. 예를 들어, 1년 기준 장기 이동평균과, 1주일 기준 단기 이동평균이 있을 때, 1주일 이동평균선이 1년 이동평균선에서 멀어지는 경우, 지난 1주일의 추세가 1년의 추세와 다르게 나타나는 발산이 나타난다. 반대로, 1주일 이동평균선이 1년 이동평균선과 가까워지는 경우 지난 1주일의 추세는 1년의 추세와 유사하게 수렴한다고 할 수 있다. 이러한 **M**oving **A**verage의 **C**onvergence와 **D**ivergence를 분석하기 위한 방법이 MACD이다.

* MACD는 **(1) MACD선**, **(2) Signal선**, **(3) MACD선과 Signal 사이의 차이를 나타내는 히스토그램** 세 가지 요소로 구성된다.([참고](https://academy.binance.com/ko/articles/macd-indicator-explained))

<p align="center"><img src="/assets/images/emerging_issue/macd.PNG"></p>

  1. MACD: 단기 EMA - 장기 EMA. 단기 추세와 장기 추세 사이의 차이를 나타냄. MACD > 0 이면 단기적으로 장기 추세에 비해 상승세에 있는 것이고, MACD < 0 이면 단기적으로 장기 추세에 비해 하락세에 있는 것.
  2. Signal: MACD에 다시 EMA를 취한 것. 단기 추세와 장기 추세의 차이의 이동평균. 
  3. 히스토그램: MACD - Signal. MACD와 Signal의 교차점은, MACD의 단기 EMA와 장기 EMA 교차점보다 빠르게 나타나는 특성이 있음.
  
  * 관련 선행연구인 He and Parker(2010)은 MACD를 사용하여, PubMed에서 나타나는 MeSH(Medical Subject headings) terms의 발생 빈도를 계산함. 그러나 이 연구에서는 vocabulary를 한정시키지 않고 문서 전체에 등장하는 단어에 대해 MACD를 적용.
 
<br/>

##  III.  Materials and Methods

### 3.1. Dataset 

 * DBLP에서 1988-2017까지 Computer Science 주제의 260만개 article에 대한 제목과 abstract 사용.
 * 총 410만개 단어 중, 3년 연속 전체 문서의 0.02% 미만으로 등장한 단어를 제외하고 분석. 총 약 7만개의 단어 사용.
    
### 3.2 Normalisation

  * 최근으로 올 수록, 문서수가 많아지고, 제목이 길어지는 등 시계열적으로 corpus의 불균형이 존재하여 정규화 진행.
  * 연도별 단어 등장 횟수를 해당 연도의 문서 수와, 해당 연도의 문서 별 토큰수로 나누어 정규화.
  
### 3.3 Applying MACD

  * 총 31개의 timesteps에 대하여, (6, 12, 3) years의 하이퍼 파라미터 설정. 즉, 단기 EMA로 6년, 장기 EMA로 12년, MACD의 EMA로 3년 적용.
  * 위 하이퍼파라미터 하에서, 단어별 등장횟수의 MACD 히스토그램을 활용한 term burstiness는 아래와 같이 정의.
  
<br/>

## IV. Method
* 다양한 추천 알고리즘을 MIND에 적용하여 실험

### 4.1 General Recommendation Methods
 * LibFM(Rendle, 2012), DSSM(Huang et al., 2013), Wide&Deep(Cheng et al., 2016), DeepFM (Guo et al., 2017)

### 4.2 News Recommendation Methods
 * DFM (Lian et al., 2018), GRU (Okura et al., 2017), DKN (Wang et al., 2018), NPA (Wu et al., 2019b), NAML (Wu et al., 2019a), LSTUR (An et al., 2019), NRMS (Wu et al., 2019c)

<br/>

## V. Experiments

### 5.1 Experiments Settings

 * 뉴스 기사의 제목만 텍스트 정보로 포함
 * test set의 절반의 유저는 train set에 있는 유저 중 랜덤 샘플링
 * word embedding이 필요한 알고리즘의 경우 Glove를 우선적으로 사용
 * optimizer로 Adam 사용
 * 클릭 되지 않은 뉴스가 훨씬 많기에 negative sampling 사용
 * 각 실험마다 10번씩 반복

### 5.2 Performance Comparison
 ![performance comparison](/assets/images/project/filter_bubble/mind 실험 결과.PNG)   
 * 전반적으로 news-specific recommendation system의 성능이 높게 나타남. news-specific recommendation system은 유저와 뉴스 기사의 특징을 (피처 엔지니어링 하지 않고) end-to-end로 학습하기 때문인 것으로 보임.
 * news-specific recommendation system 중에서는 NRMS, LSTUR이 높은 성능을 보임.
 * AUC의 경우 unseen users에서 overlap users보다 성능이 낮게 나타자지만, 다른 메트릭에서는 큰 차이가 나타나지 않음. test set에 유저가 존재하지 않더라도, 유저의 클릭 특성이 잘 학습되면 준수한 성능을 보일 수 있을 것이라고 예상됨. 
 
 
### 5.3 News Content Understanding
 * 기본 실험에서 좋은 성능을 보인 모델(NAML, LSTUR) 등을 사용하여 다양한 요인을 바꾸어 실험 진행  
 
 
#### 5.3.1 News Representation Model
  * text representation 방법을 바꾸어 가며 실험  
  ![text representation](/assets/images/project/filter_bubble/mind_text_representation.PNG)  
  * TF-IDF, LDA보다 neural network 계열의 CNN, LS
  * TM, Attention이 좋은 성능을 나타냄. 추천 태스크를 위한 특징 추출이 가능하기 때문이라고 보임.
  * CNN보다는 문장의 전체적인 맥락을 파악할 수 있는 LSTM, Attention이 좋은 성능을 보임.
  * Attention은 CNN, LSTM의 성능을 높여줌.   
  
#### 5.3.2 Pre-trained Language Models
 * 기존 word-embedding을 BERT로 바꾸어 실험한 결과, 성능 향상.  
 ![text representation](/assets/images/project/filter_bubble/mind_bert.PNG)  
  
#### 5.3.3 Different News Information
   * title 외에 뉴스 기사의 다양한 텍스트 정보를 반영해가며 실험  
   ![other text](/assets/images/project/filter_bubble/mind_other_text.PNG)  
   * 기사 body가 title, abstract보다 높은 성능을 나타냄.
   * title, abstract, title과 같이 여러 수준의 정보를 같이 사용할 때 성능이 상승함.
   * 카테고리와 엔티티를 함께 사용할 경우 성능이 상승함. 특히 카테고리의 경우, 뉴스 분야에 따라 텍스트 특성이 달라질 수 있어서 유용한 것으로 보임.
   * concat 보다는 attentive multi-view 적용한 경우 전반적으로 성능이 높음.   
   
### 5.4 User Interest Modeling
 * 유저 클릭 정보에서 피처를 추출하는 다양한 방식 실험  
 ![other text](/assets/images/project/filter_bubble/mind_user_representaion.PNG) 
 * 유저 클릭 로그 반영 길이가 길어질수록 성능 향상  
 ![log length](/assets/images/project/filter_bubble/mind_log_length.PNG)
 
<br/>

##  VI. Conclusion and Discussion
 * 이미지, 비디오, 다른 언어로 된 뉴스 기사와 관련한 추천 연구로 확장
 * 클릭 로그 외에도 다양한 참여방식(읽었는지 여부, 댓글 등) 반영 가능성
 
 ---
 
 <br/>
 
## 느낀점
 * 뉴스 기사는 수명이 짧기 때문에 cold-start-prolbem을 보완하기 위한 텍스트 정보 반영이 중요한 것 같다.
 * 카테고리를 반영했을 때 성능이 올라간 것이 흥미롭다. 확실히 분야에 따라 각 단어가 가지는 맥락도 달라질 것이다.
 * BERT가 다른 임베딩 보다 높은 성능을 보였다. 역시 BERT를 써야 하나?? 대규모 데이터를 미리 학습한게 많은 도움이 되나보다. 
