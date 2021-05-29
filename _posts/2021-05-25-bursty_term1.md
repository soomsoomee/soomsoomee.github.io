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

  

  <iframe width="560" height="310" src="https://youtu.be/lAq96T8FkTw" frameborder="0" allowfullscreen></iframe>
  <iframe width="560" height="310" src="https://youtu.be/NxTFlzBjS-4" frameborder="0" allowfullscreen></iframe>
 
 * 뉴스 기사 추천의 두 가지 포인트: 뉴스 기사의 텍스트적 성질 반영 + 유저의 행동 패턴 반영  
 * 과거에는 주로 뉴스의 카테고리 URL, 사용자의 인구적 특성, 지리적 특성을 반영하여 feature engineering  
 * 최근에는 다양한 deep learning 기법을 통해 뉴스 기사와 사용자의 특성을 학습. 그러나 대부분 공개적으로 사용하기 어려운 데이터셋을 사용.    
 * 예1) : 디노이징 오토인코더(denoising autoencoder) 통해 뉴스 기사 특성 학습, GRU 통해 유저의 클릭 패턴 학습 ([Okura et al., 2017](http://library.usc.edu.ph/ACM/KKD%202017/pdfs/p1933.pdf))    
 * 예2) 지식그래프를 활용한 CNN으로 뉴스 기사 임베딩 ([Wang et al., 2018](https://arxiv.org/abs/1801.08284))  
 * 예3) 기사를 제목, 분류, 본문 다양한 수준에서 활용하고, 유저의 클릭활동에 attention 적용하여 중요한 특성을 반영([Wu et al., 2019](https://arxiv.org/abs/1907.05576))


### 2.2. Existing Datasets

 * *Plista* dataset, *Adressa* dataset, Globo.com dataset, Yahoo! dataset 등이 있으나 대부분 영어가 아니거나, 텍스트 정보가 적거나, 유저수가 적다.  
 
<br/>

##  III.  MIND Dataset

### 3.1. Dataset Construction

 * Microsoft News 로그 데이터에서 랜덤으로 1만명 유저를 샘플링(2019.10.12 부터 2019.11.22.까지 총 6주 동안 최소 5개의 클릭수 이상 있는 유저)
 * 5주차를 train set(이전 4주차까지의 클릭 기록), 6주차를 test set(이전 5주차까지의 클릭 기록), 5주차의 마지막 날을 validate set으로 사용
 * 유저 데이터: Impression ID, User ID, Time, History, Impressions  
    * Impression ID:  유저가 뉴스 페이지 접속 시 나오는 화면 ID 
    * User ID:  익명화된 유저 아이디 
    * Time: 접속 시간 
    * History: 유저가 이전에 클릭한 기사 아이디를 시간 순서대로 정렬  
    * Impression: 유저가 뉴스 페이지 접속시 띄워지는 기사 아이디(nID)와, 유저의 클릭 여부(label)를 [(nID1, label1),(nID2, label2), ...] 형태로 기록 (labe1=1인 경우 클릭한 것, label=0인 경우 클릭하지 않은 것)    
     
     
  * 뉴스 기사 데이터: News ID, Category, SubCategory, Title, Abstract, URL, Title Entities, Abstract Entities
    ![news_data](/assets/images/project/filter_bubble/mindset_news.PNG)  
    * WikiData의 엔티티를 사용하여 title과 abstract에서 엔티티 추출
    
### 3.2 Dataset Analysis

![news data eda](/assets/images/project/filter_bubble/mindset_eda.PNG)  
* Figure2(d)의 뉴스 기사 survival time을 보면, 84.5% 이상의 뉴스 생존 기간이 2일 미만이다. 영향력이 빨리 사라지는 뉴스의 특성상, cold-start problem이 중요할 수 밖에 없다. 

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
  * TF-IDF, LDA보다 neural network 계열의 CNN, LSTM, Attention이 좋은 성능을 나타냄. 추천 태스크를 위한 특징 추출이 가능하기 때문이라고 보임.
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
