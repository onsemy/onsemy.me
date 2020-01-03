---
layout: post
title: 다른 Package 이름으로 같은 내용의 APK를 설치하기 (feat. Facebook)
date: 2020-01-03 16:52:00
author: Jeongjin Oh
categories: Study
tags: Unity Android Facebook Different Package Name
---

어느 덧 이 회사에 입사한지 한 달이 지났다. 짧다면 짧고 길다면 길었을 이 시간에 업무를 보면서 마주했던 주요 이슈들에 대해 정리해보고 같은 어려움을 겪고 있는 사람들을 위해 공유할 수 있는 부분에 대해 공유하고자 한다.

회사 업무로 같은 컨텐츠의 APK지만 다른 Package Name(또는 Bundle ID)로 Build를 해야하는 경우가 생겨서 관련하여 작업하고 있었다. 지금까지 Unity에서 단순히 Package Name만 바꾸면 기기에서는 다른 App으로 인식하여 별도의 App으로 깔리는게 정석이었으나 이상하게도 우리 Project는 그러지 않았다. 처음에는 App Player(LDPlayer)의 문제로 생각했으나 그건 아니었고, 내가 알지 못하는 사이에 정책(또는 설정)이 바뀌었나 싶어서 각종 Unity의 설정을 뒤져보았으나 결국 알아내지 못하고 있었다.

그래서 일단 아예 새 Project를 만들어서 SampleScene만 있는 상태에서 Package Name만 바꿔서 Build하니 제대로 되었다. 일단 정책이나 설정에는 문제가 없는 것 같다. 설치할 때의 오류를 좀 더 자세히 보기 위해 Logcat을 열어보았다. 그런데 이걸 진즉에 했어야 했다. 바로 아래와 같이 자세히 나와있었기 때문이다.

```log
W	1611	system	PackageManager	Package couldn't be installed in /data/app/com.EXAMPLE.PACKAGE.NAME
W	1611	system	PackageManager	com.android.server.pm.PackageManagerException: Can't install because provider name com.facebook.app.FacebookContentProvider1234567890 (in package com.EXAMPLE.PACKAGE.NAME) is already used by com.SAMPLE.PACKAGE.NAME
```

우리 게임에는 Facebook Login이 연동되어 있었는데, 하나의 Facebook App으로 여러 Package(라고 해봤자 2개)로 Login을 연동하고자 했다. 그런데 이쪽에서 탈이 난 것. 기존에는 두 Package를 동시에 설치한 적이 없기 때문에 발견하지 못했다. 해당 Keyword로 검색해보니 authorities를 Unique하게 두면 된다고 한다. 바로 변경 후 TEST!

```xml
        <!-- android:exported default value is true -->
        <provider
            android:name="com.facebook.FacebookContentProvider"
            android:exported="true"
            android:authorities="com.facebook.app.FacebookContentProvider1234567890.CHANGED" />
```

결과는 성공적이었다! 이거 하나 때문에 삽질한 것을 생각하면 부들부들 떨리지만 이 맛에 개발하는 것이 아닐까 싶기도...

**결론: Log를 잘 보자.**
