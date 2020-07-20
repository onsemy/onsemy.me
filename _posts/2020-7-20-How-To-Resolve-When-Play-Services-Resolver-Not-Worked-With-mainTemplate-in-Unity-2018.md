---
layout: post
title: Unity 2018에서 mainTemplate.gradle 사용 시 Play Services Resolver(Android Resolver)가 동작하지 않을 때 해결 방법
date: 2020-07-20 14:26:00
author: Jeongjin Oh
categories: Study
tags: Unity Android Resolver mainTemplate.gradle
---

요즘 회사에서 열심히 Build를 하고 있다. 그러던 와중에 드디어 Proguard로만 Dex를 제어하기에 한계에 다다라서 MultiDex를 사용하게 되었다. 어차피 Android 6.0으로 올리긴 해야해서 단순히 최소 Android Version만 바꾸면 되겠거니 했지만 운영상으로 당분간은 현재 Version을 유지하기로 함에 따라 현재 Android Version에서 MultiDex를 지원하는 방안을 모색해야 했다.

각종 문서나 Blog에서 친절하게 하는 방법은 알려주어서 설정은 가능했으나 정작 Build 후 실행 시 문제가 생겼다. 엉뚱한(?) Library가 없다는 것. 설마해서 Android Resolver가 돌지 않았나 싶어서 봤더니 역시나. mainTemplate.gradle이 설정되기만 하면 Resolver가 동작하지 않았다. 뭐가 문제일까 싶었다. 결론부터 말하면 Resolver가 동작하게끔 했다. 원인은 모른채로 말이다. 일단 아래처럼 설정해두면 될 것이다.

![](/images/2020-7-20-How-To-Resolve-When-Play-Services-Resolver-Not-Worked-With-mainTemplate-in-Unity-2018/1.png)

- *Patch mainTemplate.gradle*의 Check를 푼 후 OK를 누른다.

이렇게 하면 mainTemplate에 설정했던 MultiDexEnabled을 적용시키면서 Dependencies Resolving까지 가능할 것이다.

---

그리고 금방 해결될 줄 알았던 내 Razer Laptop은 오늘에서야 다시 내 손으로 돌아올 예정이다. 이번에는 정말 아무일도 없어야 하는데... 불안하지만 일단 QuickService로 받아서 빠르게 살펴볼 예정이다. 수습이 되면 바로 글로 정리해보겠다.
