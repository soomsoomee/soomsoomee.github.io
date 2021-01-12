---
title: "[Pop Your Filter Bubble] 1. MIND(A Large-scale Dataset for news recommendation) 살펴보기"
layout: post
date: 2020-01-10 
tag: filter bubble
image: ../assets/images/project/filter_bubble/filterbubble.png
headerImage: true
projects: true
hidden: true # don't count this post in blog pagination
description: "'Pop Your Filter Bubble 프로젝트의 첫 번째 단계로 MIND 데이터셋을 살펴본다. 프로젝트에 사용할만할 데이터일까?"
category: project
author: soom
externalLink: false
---

'Pop Your Filter Bubble' 프로젝트의 첫 번째 과제는 데이터 구하기이다. 

https://paperswithcode.com/paper/mind-a-large-scale-dataset-for-news

Abstract

News recommendation is an important technique for personalized news service. Compared with product and movie recommendations which have been comprehensively studied, the research on news recommendation is much more limited, mainly due to the lack of a high-quality benchmark dataset. In this paper, we present a large-scale dataset named MIND for news recommendation. Constructed from the user click logs of Microsoft News, MIND contains 1 million users and more than 160k English news articles, each of which has rich textual content such as title, abstract and body. We demonstrate MIND a good testbed for news recommendation through a comparative study of several state-of-the-art news recommendation methods which are originally developed on different proprietary datasets. Our results show the performance of news recommendation highly relies on the quality of news content understanding and user interest modeling. Many natural language processing techniques such as effective text representation methods and pre-trained language models can effectively improve the performance of news recommendation. The MIND dataset will be available at https://msnews.github.io

1. Introduction
(1) 뉴스(news) 추천의 특징
* 뉴스 기사는 업데이트 속도가 매우 빠르다. cold-start problem이 특히 문제될 수 있음
* 뉴스기사는 텍스트적 정보(제목, 본문)을 풍부하게 포함한다. 기본적인 CF(Collaborative Filtering)처럼 유저, 아이템을 단순히 아이디로 포함하는 것보다 다양한 정보를 포함할 필요가 있다.
* 주로 명시적인 평점이 없고 클릭과 같은 암묵적인(implicit) 정보를 통해 유저의 선호가 표현된다. 

(2) MIND 데이터
* Microsoft News의 기사와 유저 로그를 포함한 데이터
* 100만 유저의 클릭 행동과 160,000개의 기사를 포함


2. Related Work
(1) News Recommendation

