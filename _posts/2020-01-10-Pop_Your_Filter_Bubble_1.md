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
'Pop Your Filter Bubble' 프로젝트의 첫 번째 과제는 데이터 구하기이다. 대부분의 추천 서비스가 협업필터링(Collaborative Filtering)과 컨텐츠 기반 추천(Contents-based recommendation)을 섞은 하이브리드 추천시스템을 사용하고 있다. 따라서 이를 역이용하기 위해서는 협업필터링과 관련한 사용자 로그 데이터와 컨텐츠 기반 추천과 관련한 기사 데이터가 필요하다. Microsoft News에서 뉴스 기사 추천시스템을 위한 데이터([MIND: A Large-scale Dataset for News Recommendation](https://msnews.github.io/))를 제공하고 있어, 이를 사용하기로 했다. [MIND 데이터셋을 설명한 논문을 리뷰](https://soomsoomee.github.io/MIND/)하고, EDA를 진행하였다. 



