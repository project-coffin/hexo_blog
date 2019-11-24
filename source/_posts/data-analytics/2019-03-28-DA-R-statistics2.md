---
layout: post
title:  "[통계학#2] 추론 통계 - 표집 분포(Sampling Distribution)"
date: 2019-03-27 23:53:59
author: Roseline Song
categories: Data-Analytics
tags: R 데이터 강의 통계학
cover: "/assets/DataAnalysis.gif"
---

※ 강의 데이터 애널리틱스를 듣고 정리한 내용입니다.

<br>

### 추론 통계(Inferential Statistics)란?

**샘플**을 통해 **모집단의 특성**을 **추론**하는 것

<br>
<br>

### 표집 분포(Sampling Distribution)


하나의 모집단에서 여러 샘플을 뽑을 수 있다.
그리고 **각각의 샘플링에서 평균을 내어, 샘플들의 분포**를 만들 수 있다. 이런 분포를 '샘플링 분포'라고 한다. 이 샘플링 과정을 무한히 하면 우리가 평소에 보는 그래프처럼 자연스러운 분포를 만들 수 있다. 

<br>

이때 주의할 게 3가지가 있다. 

첫째, 샘플 분포는 '표본 분포'를 말하며 샘플링 분포(표집 분포)와는 다르다는 것이다. 표본 분포는 모집단에서 추출한 **한 샘플의 분포**를 말하는 것이고, 표집 분포는 여러 개 샘플들을 뽑아, 각 샘플의 평균에 대한 분포를 그린 것을 말한다. 

둘째, **표집 분포에서 각각의 포인트는 '값이 아니라 평균'이라는 것**이다. 

셋째, 표집 분포의 표준편차는 **표집 분포의 표준 오차(Standard Error)**라고 한다.

<br>
<br>

### Sampling Distribution의 특징 


**1. 표집 분포를 무한히 반복하면, 무조건 정규 분포**가 나온다

<br>

**2. 표준편차가 모집단의 표준편차보다 적다**

표준편차는 값이 '평균으로부터 떨어져 있는 정도'를 말한다. 표집 분포는 샘플들 각각의 평균을 분포로 만든 것이기 때문에 표집 분포의 표준오차는 모집단의 표준편차보다 적다. 

<br>
<br>

### SE(Standard Error) : 샘플 분포의 표준오차 (표준편차 x)


표준편차는 일반적으로 SD(Standard Diviation)라고 하지만, 표집 분포의 표준편차는 모집단과 구별하기 위해 SE(Standare Error)라고 부른다. 

**샘플 분포의 표준오차는 `표준편차/루트N`**이다. 루트N은 몇 번 샘플링 했는지를 의미한다. 분모가 커질수록 표준오차는 작아진다. 표준편차는 평균에서 떨어져있는 정도를 말하므로 샘플링 횟수가 많아질 수록 오차는 작아진다.

<br>
<br>