---
title: "[Pop Your Filter Bubble] 2. 정치기사 Text Classification을 활용하여 논조의 스펙트럼을 파악할 수 있을까?"
layout: post
date: 2020-01-10 
tag: 
- filter bubble
- recommendation system
image: ../assets/images/project/filter_bubble/filterbubble.png
headerImage: true
projects: true
hidden: true # don't count this post in blog pagination
description: "나와 생각이 '얼마나' 다른지까지 파악할 수 있을까?"
category: project
author: soom
externalLink: false
---

> **'Pop Your Filter Bubble' 프로젝트란?**    
필터버블(Filter Bubble)은 사용자에게 맞게 필터링된 정보만이 마치 거품(버블)처럼 사용자를 가둬버리는 현상을 말한다. 관심없는 정보, 싫어하는 정보는 저절로 걸러지고 사용자가 좋아할만한 정보만이 제공되면서 알고리즘이 만들어낸 정보에만 둘러싸이는 것이다. (출처: [필터버블의 덫](http://weekly.chosun.com/client/news/viw.asp?ctcd=c02&nNewsNumb=002509100003))  
추천 알고리즘의 발달로 우리는 점점 내가 좋아하는 것, 내가 익숙한 것에 쉽게 노출된다. 이것이 새로운 경험을 저해하는 장애물이 되는 것은 아닐까? 특히 '관점'을 담는 뉴스 기사에 추천 알고리즘을 적용하는 경우, 사용자들은 큰 그림을 보지 못하고 자기가 보고 싶은 것만 보는 '확증편향'에 빠지기 쉽다. 추천시스템을 역이용하여 나와 생각이 다른 기사를 함께 추천해주는 것은 어떨까?  

<br/><br/>

이상적인 이야기지만, 내가 읽고 있는 기사와 다른 논조의 기사를 스펙트럼처럼 보여줄 수 있으면 좋겠다는 생각이 들었다.
예를 들어 '우리나라 대기업의 상속세를 낮춰야 한다'는 논조의 기사를 읽고 있을때, 
비슷한 논조를 가진 기사부터 '우리나라 상속세는 높지 않은 편이다'라는 논조의 기사까지 입장의 정도에 따라 순서대로 보여주면 어떨까?  

같은 주제를 다루면서 나와 생각이 다른 기사를 추천 해주되, 미리 내 생각과 얼마나 다른지를 알 수 있으면 거부감도 적고, 해당 토픽에 대한 주장을 넓게 볼 수 있을 것이라 생각했다.
물론 사람이 판단하기에도 어려운 일이다. 그러나 text classification을 응용할 수 있을까 하여 
[Political Ideology Detection Using Recursive Neural Networks에 대한 논문 리뷰](https://soomsoomee.github.io/Political_Ideology-Detection-Using-Recursive-Neural-Networks/)도 하고,
논문 저자인 Mohit Lyyer 교수님께서 제공해주신 데이터셋을 활용하여 직접 분류 모델을 구축해보았다. 
다른 분야에 비해 '진보'와 '보수'라는 논조로 비교적 쉽게 분류될 수 있는 정치 기사에 한정해서라도, 논조의 '정도'를 파악해보고자 하는 욕심이 있다. 
