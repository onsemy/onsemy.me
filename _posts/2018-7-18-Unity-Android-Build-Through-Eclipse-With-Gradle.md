---
layout: post
title: Eclipse Gradle를 이용한 Unity Android Build
date: 2018-07-18 12:36:00
author: Jeongjin Oh
categories: Tech
tags: unity android build eclipse gradle
---

최근 회사에서 Build 담당이 되었다 (...) 시스템 구축하는 것을 좋아하긴 하지만 Build는 시간도 오래 걸리고 귀찮고 예측 불가능한 오류들이 많은 등의 손이 많이 가는 일이라 상당히 짜증이 나는 일이다~~(그래서 고경력자들이 기피하는 것 같다.)~~

그렇게 맡은 바 일을 충실히 잘 해내면 좋겠지만, 회사에서 퍼블리셔의 요청 사항으로 기존에 잘 굴러가던 `Gradle`기반의 `Android Studio` Project를 `Eclipse` Project로 변경하라는 작업이 내려왔다. 예전 같았으면 `Unity`나 `Google`을 욕하고 있었겠지만 지금은 많이 좋아져서 오히려 `Android Studio`로 Build 작업을 진행하는 것이 더 수월하다. 그래서 지금은 퍼블리셔를 욕하고 있다 (...)

어쨌든, `Android Studio`가 아닌 `Eclipse` Project로 바꾸는 일을 너무 복잡하게 생각했던 것 같다. 기존에는 `Unity`에서 `Eclipse ADT`용 Project를 뽑기 위해서는 