---
layout: post
title: AssetBundleManifest
date: 2019-08-27 12:31:00
author: Jeongjin Oh
category: Study
tags: Unity AssetBundle AssetBundleManifest
---

어느덧 회사에 입사한지 한달이 다 되었다. 그동안에 정말 여러 일이 있었다. 월급도 받았고. 혀튼 오늘은 지난 일요일의 과음과 전날 숙취로 괴로워하다 결국 몸살까지 난 기념으로 첫 연차를 썼다. 그리고 생각난 김에 글을 하나 써본다.

최근 회사에서 하고 있는 일은 기존에 사용하던 AssetBundle System을 개편(Renew)하는 작업을 하고 있다. 여러 굉장한 이유가 있지만(...) 그 중 하나가 기존의 시스템이 번들 버전이 달라버리면 무조건 전부(...) 받는 시스템이었기 때문이다. `UnityWebRequestAssetBundle`을 사용하는 것 같았는데 왜 전부 새로받는 것인지는 ~~스파게티 코드를 살펴볼 자신이 없어서~~ 잘 모르겠으나 어차피 AssetBundle의 다운로드, 로딩, 캐싱 등의 동작이 굉장히 복잡하게 설계되어 있어서 톱니바퀴처럼 착착 굴러가지 않는 이상 오동작의 여지가 많고, 무엇보다 유지보수가 불가능에 가까워서 새로 하고 있다.

이전에도 AssetBundle System을 만들어본 적이 있기 때문에 (신입 시절이었지만) 다시 한 번 리마인드 겸 손을 대보았다. 문제는 안한지 너무 오래되어서 어이없는 것으로 하루이틀을 날려먹은 것이다. 바로 `AssetBundleManifest` 덕분이다.

[문서](https://docs.unity3d.com/kr/2018.2/Manual/AssetBundles-Native.html) 상에서는 아래와 같이 되어있다. (2019년 8월 27일 2018.2 버전 기준)

![AssetBundleManifest](/images/2019-8-27-AssetBundleManifest/1.png)

```csharp
AssetBundleManifest manifest = assetBundle.LoadAsset<AssetBundleManifest>("AssetBundleManifest");
```

`"AssetBundleManifest"`가 Manifest를 불러오는 파일명인줄 알고 바꿔서 해도 계속 되지 않아서 도대체 뭘까 싶었는데... 알고보니 그냥 저 `"AssetBundleManifest"`를 고정으로 써야하는 것이었다. 이것은 내가 신입 때도 똑같이 겪었던 문제였음을 깨닫는데는 오랜 시간이 걸리지 않았다 (...) 아무리 4년 전이라지만 똑같은 실수를 반복하다니... 문서에 좀 명시를 해줬으면 어땠을까라는 아쉬움도 있지만 까먹고 똑같은 실수를 한 나도 잘못이다. 그래서 이렇게 글로 남겨본다. 누군가는 나처럼 똑같은 실수를 하지 않을까 싶기도 하기에 ㅎ...
