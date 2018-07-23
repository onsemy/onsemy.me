---
layout: post
title: Eclipse Gradle를 이용한 Unity Android Build
date: 2018-07-23 12:25:00
author: Jeongjin Oh
categories: Tech
tags: unity android build eclipse gradle
---

최근 회사에서 Build 담당이 되었다 (...) 시스템 구축하는 것을 좋아하긴 하지만 Build는 시간도 오래 걸리고 귀찮고 예측 불가능한 오류들이 많은 등의 손이 많이 가는 일이라 상당히 짜증이 나는 일이다~~(그래서 고경력자들이 기피하는 것 같다.)~~

그렇게 맡은 바 일을 충실히 잘 해내면 좋겠지만, 회사에서 퍼블리셔의 요청 사항으로 기존에 잘 굴러가던 `Gradle`기반의 `Android Studio` Project를 `Eclipse` Project로 변경하라는 작업이 내려왔다. 예전 같았으면 `Unity`나 `Google`을 욕하고 있었겠지만 지금은 많이 좋아져서 오히려 `Android Studio`로 Build 작업을 진행하는 것이 더 수월하다. 그래서 지금은 퍼블리셔를 욕하고 있다 (...)

어쨌든, `Android Studio`가 아닌 `Eclipse` Project로 바꾸는 일을 너무 복잡하게 생각했던 것 같다. 기존에는 `Unity`에서 `Eclipse ADT`용 Project를 뽑기 위해서는

![Build Settings에서 Android Target일 때](/images/2018-7-23-Unity-Android-Build-Through-Eclipse-With-Gradle/1.png)

위와 같이 Build System을 `ADT(Legacy)`로 놓고 해야 한다. 나도 처음에는 그렇게 진행했지만, `Eclipse`는 `Android`의 지원이 끊긴지 오래되었고, 관련 자료들은 없거나 다른 Platform으로 바뀌었고, 관련하여 오류가 발생해서 검색을 해도 현재는 쓸 수 없는 옛날 자료만 방대하게 나올 뿐이었다. 그렇게 며칠을 Build도 제대로 해보지 못한 채로 보내야 했다. 그렇게 스트레스는 날로 하늘을 찔렀고, 나의 머리카락도 후두둑 빠지기 시작했다 (...)

가장 큰 문제 중 하나가 기존 Android Studio에서 쓰고 있던 기능에 대한 Library의 부재/변경이었는데, 크게 두 가지가 나를 괴롭혔다.

1. MultiDex Support Library
2. Android Support Library (v4)

*2번*은 어찌어찌 `jar`파일을 구해서 적용을 했지만 1번은 애초에 답이 없었다. 내가 찾지 못한 것일 수도 있지만... 그래서 역으로 생각해 보았다.

> Eclipse에서 Android Studio Project를 불러올 순 없을까?

엄밀히 말하면 Android Studio Project는 불러올 수 없다. 대신 *Android Studio에서 쓰고 있는 `Gradle`을 불러올 수 있지 않을까?* 그래서 찾아보니 역시나! 이미 능력자(?)들이 Third-Party로 만들어서 `Eclipse Marketplace`에 올려두었다. *전세계 개발자들아! 고마워!*

그리하여, 대략 [이 링크](http://www.vogella.com/tutorials/EclipseGradle/article.html)를 타고 들어가서 따라서 해보면 된다. 현재의 것과 좀 다른 부분이 있지만 그 정도는 검색으로 때울 수 있을 것이다 (???)

Eclipse에서 Gradle Project를 불러오기 위해서는 Unity에서 Android Build System을 `Gradle(New)`로 변경 후 Build를 해야 한다.

이후 뽑혀져 나온 Directory에서 앞서 링크로부터 설정한대로 따라서 불러오면 되겠다.

한 가지 더 저 링크에서 설명되지 않은 부분이 있는데, Gradle Project의 Directory 내에 `gradle/wrapper/gradle-wrapper.properties`에 버전을 지정하지 않으면 Gradle Project를 불러올 때 Gradle Wrapper의 버전을 특정하지 못해서 불러올 수 없을 것이다.

![gradle-wrapper.properties](/images/2018-7-23-Unity-Android-Build-Through-Eclipse-With-Gradle/2.png)

같은 실수를 반복하지 않기 위해 일단 두서없이 정리를 해보았다.