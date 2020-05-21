---
layout: post
title: Google Play Games에서 `GamesNativeSDK - Can't register class...`가 Log에 나왔을 경우
date: 2020-05-21 14:19:00
author: Jeongjin Oh
categories: Study
tags: Unity GPGS Android
---

회사 업무를 하는 도중 Windows와 Android를 오가면서 Build를 해야할 일이 있었다. Windows Build 구성이 어느정도 마무리되어서 Android Build 구성이 제대로 동작하는지 확인하기 위해 Build를 했고 *MEMU*에서 실행했는데 아래와 같은 Log가 출력되면서 *Google Play Games*를 통한 Login이 동작하지 않음을 확인했다.

```log
05-20 18:38:35.745 28025 28025 E GamesNativeSDK: Can't register class com/google/android/gms/games/Games: an exception occurred.
...
```

일단 Google에 검색해보았다. 대부분은 Google Play Games의 Version이 너무 오래되어서 Update가 필요하다는 글이 많았다. 하지만 우리 Project에서 크게 바꾼 것은 없었고, 단지 Build를 새로 했을 뿐이었는데 Update가 필요하다니? 이상해서 Build PC에서 최신 Version으로 SVN Update를 받고 Build를 진행했는데 이건 또 잘 된다. 혹시나 ProGuard 문제가 아닐까 싶어서 Android Studio를 켜서 Login이 되는 APK와 안되는 APK를 비교해보았다.

![Android Studio](/images/2020-5-21-GamesNativeSDK-Cant-Register-Class/1.png)

역시나 뭔가 없다 했다. `com.google.android.gms.games`와 `com.google.android.gms.nearby`를 난독화에서 예외처리 했다. 그래도 안된다 젠장!

갈고리가 점점 많아지는 가운데 내 작업 Computer에 있는 다른 Project Directory에서 Build를 진행했더니 똑같은 문제가 발생했다. 그래서 일단 Build가 됐던 이전 Revision으로 되돌리고 다시 Build를 진행하니 된다. 그래서 SVN Log를 확인해보았다. 하지만 오동작을 할만한 의심가는 변경 사항은 없었다. 그래서 일단 퇴근 전에 내가 작업했던 Working Directory를 초기화하고 Unity에서 Re-import하도록 작업을 걸어두었다.

다음날 출근해서 Build를 걸었다. 잘 된다... 뭐지? 원인을 모른채 해결이 되어버렸다. 개발자 입장에서 제일 찜찜한 순간이다. 당장 급한 일이 없어서 원인을 좀 더 파악해보고 작업물을 올릴 예정이다.

---

세 줄 요약

1. Windows와 Android를 오가면서 CI Script를 수정하고 있었다.
2. 그러던 중 Android에서 실행하니 GPGS로의 Login이 안된다!
3. 여러 시도 끝에 작업물을 제외한 나머지 Revert 및 Unity의 Library Directory를 날려버리고 다시 Build를 하니 된다...
