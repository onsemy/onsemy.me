---
layout: post
title: Command PhaseScriptExecution failed with a nonzero exit code
date: 2022-01-13 00:14:00
author: Jeongjin Oh
category: Study
tags: Flutter pubspec.yaml assets
---

오랜만에 Study Category에 게시해보는 것 같다.

현재 다니고 있는 회사에서 Flutter를 사용하고 있다. 어제도 신나게 개발을 하고 있다가 다음과 같은 오류를 맞닥드렸다.

```log
Command PhaseScriptExecution failed with a nonzero exit code
```

Flutter에서 iOS Debug를 위해 실행했다가 나왔는데, 이전까지는 잘 되던 것이 갑자기 안되니 영문을 알 수 없었다. 나는 단지 내가 했던 작업을 병합하기 위해 현재까지 올라온 작업물을 내 작업 브랜치로 병합하고 최종 테스트를 해보고 있었을 뿐... 이전의 작업물이 좀 되어서 문제를 바로 파악하기도 어려웠다.

먼저 저 로그를 구글링 해봤다. 원인은 꽤나 다양했는데, 대부분은 `flutter clean` 후 다시 해보라는 것. 그렇게 쉽게 될 것이었으면 이렇게 남기지도 않았다. 그렇게 반나절 정도 시간이 흘러갈 즈음에 [하나의 글](https://khstar.tistory.com/entry/Flutter-iOS-%EC%95%B1-%EC%8B%A4%ED%96%89%EC%8B%9C-It-appears-that-your-application-still-contains-the-default-signing-identifier-%EC%98%A4%EB%A5%98)을 발견했다. 나의 상황과는 달랐지만 같은 오류 로그가 발생한 사례였는데,

> `pubspec.yaml`를 살펴보라!

라고 써있었다. 그리고 우리 프로젝트의 `pubspec.yaml`을 봤는데 겉으로 보기에는 별 이상이 없긴 했다. 그런데 Git에 수정 이력은 있었다. assets 폴더에 리소스를 추가하면서 함께 추가한 것이었다. 여기서부터 느낌이 쎄했다. 추가된 리스트를 보는데 뭔가 경로가 잘못된 것이 아닐까? 하면서 살펴봤으나 별로 특별해보이는 것은 없었다. 혹시나 하는 생각에 [Flutter 공식 문서](https://docs.flutter.dev/development/ui/assets-and-images#specifying-assets)를 찾아봤다. 첫 페이지를 보고 아뿔사!

![](/images/2022-1-13-Flutter-Error/1.png)

폴더일 경우 경로 끝에 `/`를 붙여야 하는데, `/`를 붙이지 않을 경우 폴더가 아닌 파일로 간주하기 때문에 불러오면서 오류가 발생한 것이다. 예상대로 추가된 경로들 중 붙지 않은게 많았다. 붙이고서 실행해보니 잘 동작한다! ~~와!~~ 이 회사 와서 처음 해보는 Flutter 였기에 이런 사소한(?) 것 때문에 시간을 잡아먹을 줄은 몰랐다. 그리고 병합 전에 미리 테스트를 하길 잘했다는 생각이 든다. 테스트 없이 병합하고서 누군가 오류 때문에 진행을 못할텐데, 내가 들인 시간보다 더 오래 걸릴 수도 있기 때문에 그나마 덜 바쁠 때 발견하고 해결해서 다행이다. 다시 한 번 테스트의 중요성을 깨달은 하루였다.

---

## _결론: 테스트하자. 두 번 하자. 계속 하자._

P.S: 그나저나 Flutter yaml 검사해주는 확장이 있을법 한데 없...나?
