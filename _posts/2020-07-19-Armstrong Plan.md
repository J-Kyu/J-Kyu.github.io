---
layout: post
title: Project Armstrong
categories: [Armstrong]
tags: [Game, Project, Armstrong]
---
# Armstrong Plan

> * 2번째 Project는 Unity를 사용하여 개발할 조정 게임이다. 이 글은 해당 project의 계획을 설명하는 초안이다.
> * In Progress

## Concept

### Point

* 요즘 동아리 활동을 통해서 4인 조정을 열심히 하고 있다. 조정의 Key point는 화합이다. 여러명의 선수가 Catch와 finish 그리고 recovery를 정확하고 동일한 pace로 가져가야 배가 올바르게 나아갈 수 있다.
* Timing을 동일하게 가져오는 것을 재미 point로(기술) 조정(관점)에서 재해석 할 수 있을 것 같다.
* 2D Game

### Rowing Description

<center>
	<img src="/images/project_images/Armstrong/armstrong_explaination.png" style="zoom:30%;" />
</center>

## Creativity

### 화합 터치

* 이 게임의 가장 중심이 되는 기술은 가칭 "**화합 터치**"이다. 조건은 다음과 같다.
  1. Touch가 oar를 동작시켜야 한다.
  2. Recovery와 Catch, finish, turn을 인식한다.
  3. Rhythm Game 처럼, timing을 맞춰서 test가 될 수 있어야 한다.

| Oar Motion                                                   | Oar Description                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| <img src="/images/project_images/Armstrong/armstrong_OarMotion.png" alt="armstrong_explaination" style="zoom:30%;" /> | <img src="/images/project_images/Armstrong/armstrong_OarDescription.png" alt="armstrong_explaination" style="zoom:50%;" /> |



### 물결 방향

| Condition                                                    | Images                                                       |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| * 배의 움직임을 최소화하기 위해서 물결의 모션으로 표현한다. 조건은 다음과 같다.<br/><tab>1. 바탕은 파란색의 물을 표현하며, 물결은 검은 색 꼬불 선으로 통해서 표현한다.<br/><tab>2. 배의 방향 또한 물결의 꼬불 선의 방향을 통해서 표현한다.<br/><tab>3. 빨라질 수록 물결은 직선으로 뻗친다. | <img src="/images/project_images/Armstrong/armstrong_WaterWave.png" alt="armstrong_WaterWave" style="zoom:30%;" /> |

## Reinterpretation

> 아직은 미정이지만, 휴대폰 하나로 최대 4인까지 Play를 할 수 있는 **협동** 게임 혹은 시합 게임을 생각하고 있다.

### Rules

* 2가지 방법으로 게임을 운영할 수 있을 것 같다.
  1. Multi(협동): Rhythm Game 처럼, 가장 앞에 있는 사람의 motion에 맞춰서 rowing을 기록하는 방법
     * 혹은 Map에 따라, Computer Motion에 맞춰서 rowing
  2. Solo(시합): Map에 따라  recovery와 power, rate 요소를 대입하여 다른 PC와 대결.