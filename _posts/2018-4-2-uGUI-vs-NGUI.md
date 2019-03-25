---
layout: post
title: uGUI vs NGUI?
date: 2018-04-02 21:12:00
author: Jeongjin Oh
categories: Study
tags: uGUI Unity3D
---

이전에 N사 면접(최근에 봤던 두 N사)에서 들었던 질문 중 생각나는 질문에 대해서 정리를 해보면 좋을 것 같아서 흔적(?)을 남겨보고자 한다.

그 첫번째로 uGUI의 장단점과 실제 피부로 느꼈던 것, 채용 시장에서 주로 찾는 건 어떤 것인지 하나하나 알아보고자 한다. 사실 NGUI는 2014년에 ```몬스터 친구들 for Kakao``` 프로젝트에 잠시 참여하면서 2개월 동안 썼을 뿐이었고, 내 커리어의 대부분은 uGUI와 함께 했다. 그래서 NGUI와 관련하여 잘 모르는 부분이 존재할 것이다. NGUI는 잘 모르니 uGUI에 대해 정리해 보자면,

- Unity Engine (4.6 이후부터) 내에서 제공하는 UI System
- 추가 구매 없이 바로 사용 가능
- 직관적인 UI 구성요소 간의 Depth 조절
- Canvas 단위로 Draw Call이 관리됨
- Sprite Atlas 관리 (폴더 단위로도 가능)
- Particle Rendering 문제가 있음
- Tweening을 기본으로 지원하지 않음
- [소스 코드가 공개되어 있음](https://bitbucket.org/Unity-Technologies/ui)
- UI 확장 Asset도 쉽게 구할 수 있음
- 비공식 UI 확장 Component도 [소스가 공개되어 있음](https://bitbucket.org/UnityUIExtensions/unity-ui-extensions)

지금까지 써왔을 때 크게 불편한 것은 없었지만 위에서 언급한 것에서 불편했던 사항은 Third-Party Asset으로 어느정도 해결이 가능한 수준이다. 그렇다고 uGUI가 NGUI보다 더 좋다던가 추천한다던가를 쉽사리 이야기할 수 없을 것 같다.

최근에 구직 사이트에서의 자격요건이나 우대사항을 살펴보면 uGUI보다는 NGUI를 많이 찾는 것 같다. 체감상 대부분 게임 회사들이 NGUI로 UI를 구성하는 것 같다. uGUI가 나온지 3년이 넘었고 충분히 레퍼런스가 나오지 않았을까 싶기도 하지만 생각보다 게임업계가 보수적이라는 생각을 가지게 된다. 그럴만도 한 것이 그동안 NGUI 기반으로 만들어둔 자기 회사만의 레퍼런스가 있을텐데, 그걸 버리고 하기에는 바꾸는 비용에 대한 메리트가 없기 때문이 아닐까 싶다.

앞으로 어느 회사에서 어떤 작업을 할지 잘 모르겠지만 NGUI에 대해서도 어느 정도는 공부를 해둬야할 것 같다.