---
title: "[Pop Your Filter Bubble] 1. MIND(A Large-scale Dataset for news recommendation) 살펴보기"
layout: post
date: 2020-01-10 
tag: 
- filter bubble
- recommendation system
image: ../assets/images/project/filter_bubble/filterbubble.png
headerImage: true
projects: true
hidden: true # don't count this post in blog pagination
description: "'Pop Your Filter Bubble 프로젝트의 첫 번째 단계로 MIND 데이터셋을 살펴본다. 프로젝트에 사용할만할 데이터일까?"
category: project
author: soom
externalLink: false
---

> **'Pop Your Filter Bubble' 프로젝트란?**    
필터버블(Filter Bubble)은 사용자에게 맞게 필터링된 정보만이 마치 거품(버블)처럼 사용자를 가둬버리는 현상을 말한다. 관심없는 정보, 싫어하는 정보는 저절로 걸러지고 사용자가 좋아할만한 정보만이 제공되면서 알고리즘이 만들어낸 정보에만 둘러싸이는 것이다. (출처: [필터버블의 덫](http://weekly.chosun.com/client/news/viw.asp?ctcd=c02&nNewsNumb=002509100003))  
추천 알고리즘의 발달로 우리는 점점 내가 좋아하는 것, 내가 익숙한 것에 쉽게 노출된다. 이것이 새로운 경험을 저해하는 장애물이 되는 것은 아닐까? 특히 '관점'을 담는 뉴스 기사에 추천 알고리즘을 적용하는 경우, 사용자들은 큰 그림을 보지 못하고 자기가 보고 싶은 것만 보는 '확증편향'에 빠지기 쉽다. 추천시스템을 역이용하여 나와 생각이 다른 기사를 함께 추천해주는 것은 어떨까?  

<br/><br/>
'Pop Your Filter Bubble' 프로젝트의 첫 번째 과제는 데이터 구하기이다. 대부분의 추천 서비스가 협업필터링(Collaborative Filtering)과 컨텐츠 기반 추천(Contents-based recommendation)을 섞은 하이브리드 추천시스템을 사용하고 있다. 따라서 이를 역이용하기 위해서는 협업필터링과 관련한 사용자 로그 데이터와 컨텐츠 기반 추천과 관련한 기사 데이터가 필요하다. Microsoft News에서 뉴스 기사 추천시스템을 위한 데이터([MIND: A Large-scale Dataset for News Recommendation](https://paperswithcode.com/paper/mind-a-large-scale-dataset-for-news))를 제공하고 있어, 이를 사용하기로 했다. 또한 MIND 데이터셋을 설명한 논문을 정리해보았다.  

---


> **'MIND: A Large-scale Dataset for News Recommendation' Abstract**  
News recommendation is an important technique for personalized news service. Compared with product and movie recommendations which have been comprehensively studied, the research on news recommendation is much more limited, mainly due to the lack of a high-quality benchmark dataset. In this paper, we present a large-scale dataset named MIND for news recommendation. Constructed from the user click logs of Microsoft News, MIND contains 1 million users and more than 160k English news articles, each of which has rich textual content such as title, abstract and body. We demonstrate MIND a good testbed for news recommendation through a comparative study of several state-of-the-art news recommendation methods which are originally developed on different proprietary datasets. Our results show the performance of news recommendation highly relies on the quality of news content understanding and user interest modeling. Many natural language processing techniques such as effective text representation methods and pre-trained language models can effectively improve the performance of news recommendation. The MIND dataset will be available at https://msnews.github.io  

<br/>

## Ⅰ. Introduction  

### 1.1 뉴스(news) 추천의 특징  

 * 뉴스 기사는 업데이트 속도가 매우 빠르다. cold-start problem이 특히 문제될 수 있음  
 * 뉴스 기사는 텍스트적 정보(제목, 본문)을 풍부하게 포함한다. 기본적인 CF(Collaborative Filtering)처럼 유저, 아이템을 단순히 아이디로 포함하는 것보다 다양한 정보를 포함할 필요가 있다.
 * 주로 명시적인 평점이 없고 클릭과 같은 암묵적인(implicit) 정보를 통해 유저의 선호가 표현된다. 

    
### 1.2. MIND 데이터  
 
 * Microsoft News의 기사와 유저 로그를 포함한 데이터
 * 100만 유저의 클릭 행동과 160,000개의 기사를 포함

<br/>

## Ⅱ.  Related Work    

### 2.1. News Recommendation  
 
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
column | 설명
------------ | -------------
Impression ID | 유저가 뉴스 페이지 접속 시 나오는 화면 ID
User ID | 익명화된 유저 아이디
Time | 접속 시간
History | 유저가 이전에 클릭한 기사 아이디를 시간 순서대로 정렬 
Impression | 유저가 뉴스 페이지 접속시 띄워지는 기사 아이디(nID)와, 유저의 클릭 여부(label)를 [(nID1, label1),(nID2, label2), ...] 형태로 기록 <br/> labe1=1인 경우 클릭한 것, label=0인 경우 클릭하지 않은 것  
     
  * 뉴스 기사 데이터: News ID, Category, SubCategory, Title, Abstract, URL, Title Entities, Abstract Entities
    ![news_data](/assets/images/project/filter_bubble/mindset_news.PNG)  
    * WikiData의 엔티티를 사용하여 title과 abstract에서 엔티티 추출
    
### 3.2 Dataset Analysis

![news data eda](/assets/images/project/filter_bubble/mindset_eda.PNG)  
* Figure2(d)의 뉴스 기사 survival tima을 보면, 84.5% 이상의 뉴스 생존 기간이 2일 미만이다. 영향력이 빨리 사라지는 뉴스의 특성상, cold-start problem이 중요할 수 밖에 없다. 

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

## 6. Conclusion and Discussion
 * 이미지, 비디오, 다른 언어로 된 뉴스 기사와 관련한 추천 연구로 확장
 * 클릭 로그 외에도 다양한 참여방식(읽었는지 여부, 댓글 등) 반영 가능성

